####场景
用户在运行大量作业时，Hadoop默认是按照FIFO的顺序调度这些作业的，这能满足绝大多数的应用场景。而如果某用户想让作业J<sub>1</sub>运行的时候，同时运行其他作业。这时默认调度器就不能很好的满足用户对服务质量的需求。我们通过给Hadoop增加patch，让用户指定参数限制Job的并发性，已达到用户对作业运行顺序的目的。  
#### 目的
用户可以通过参数MRJobConfig.JOB_RUNNING_MAP_LIMIT或者MRJobConfig.JOB_RUNNING_REDUCE_LIMIT，分别限制单个MapReduce作业中同时运行的mapper和reducer的数目
#### 讨论
**方案一** ~~在RM端进行限制，即MapReduceAM将资源请求全部发放给RM，然后由RM对资源进行限制，这样会增加RM的负载~~

**方案二** 在AM端进行限制，这样用户在指定参数后，AM只向RM申请限定的资源量，优点是减少了RM的负载，缺点是在进行资源请求的时候会引入一定的延时。在资源充足的时候，如果一个任务运行结束的时候，如果采用方案一，则资源会立马分配给application，但是本方案会增加一个AM向RM请求资源量的过程，而这个过程会照成延迟。经过后面的实验测试，虽然会引入这个延时，但是仍然能满足服务质量的需求。
#### 实现
#####分析
在进行调度的时候，资源调度过程是向下面这描述的，即map的资源直接发送给RM，而Reduce的资源先放入pending队列，在满足条件的时候，再将资源的，而schedule队列里面的请求全部发送给了RM，因此
```
  /*
  Vocabulary Used: 
  pending -> requests which are NOT yet sent to RM
  scheduled -> requests which are sent to RM but not yet assigned
  assigned -> requests which are assigned to a container
  completed -> request corresponding to which container has completed
  
  Lifecycle of map
  scheduled->assigned->completed
  
  Lifecycle of reduce
  pending->scheduled->assigned->completed
  
  Maps are scheduled as soon as their requests are received. Reduces are 
  added to the pending and are ramped up (added to scheduled) based 
  on completed maps and current availability in the cluster.
  */
```
##### 过程
1. heartbeat()
 * getResources()：返回分配的container
    * 向RM发起资源请求，病获取分配的container和结束的container
    * 获取已经分配的contaner
    * 根据已经结束的container，移除assgned队列中的元素
 * 分发container
 * 调度reduce
2. 

```
生成Pending队列和scheduling队列，在heartbeat()和handleEvent()处将pending队列的数据移动到scheduling队列
在调度reduce的时候，计算map总的需求资源量时加上pendingMap队列的资源，避免死锁
``` 
3. reduce限制
```
调度时，加上reduce数目的限制，并且在处理reduce资源请求的时候，计算map需求资源的时候，得将pendingMap的资源加进来
```
4. 链接 
[MAPREDUCE-6176](https://issues.apache.org/jira/browse/MAPREDUCE-6176)
5. 资源请求语义  
比如map资源需求量是5，不加限制的时候，其请求语义
```
{Priority: 20, Capability: <memory:1024, vCores:1>, # Containers: 5, Location: *, Relax Locality: true}
{Priority: 20, Capability: <memory:1024, vCores:1>, # Containers: 5, Location: /default-rack, Relax Locality: true}
{Priority: 20, Capability: <memory:1024, vCores:1>, # Containers: 1, Location: h0, Relax Locality: true}
{Priority: 20, Capability: <memory:1024, vCores:1>, # Containers: 1, Location: h1, Relax Locality: true}
{Priority: 20, Capability: <memory:1024, vCores:1>, # Containers: 1, Location: h2, Relax Locality: true}
{Priority: 20, Capability: <memory:1024, vCores:1>, # Containers: 1, Location: h3, Relax Locality: true}
{Priority: 20, Capability: <memory:1024, vCores:1>, # Containers: 1, Location: h4, Relax Locality: true}
```
限制为3时，我们的方案是将被限制的资源放入pending队列，所以请求语义是  
```
{Priority: 20, Capability: <memory:1024, vCores:1>, # Containers: 3, Location: *, Relax Locality: true}
{Priority: 20, Capability: <memory:1024, vCores:1>, # Containers: 3, Location: /default-rack, Relax Locality: true}
{Priority: 20, Capability: <memory:1024, vCores:1>, # Containers: 1, Location: h0, Relax Locality: true}
{Priority: 20, Capability: <memory:1024, vCores:1>, # Containers: 1, Location: h1, Relax Locality: true}
{Priority: 20, Capability: <memory:1024, vCores:1>, # Containers: 1, Location: h2, Relax Locality: true}
```
社区的方案，是在请求的时候限制资源的数目，所以请求语义是
```
{Priority: 20, Capability: <memory:1024, vCores:1>, # Containers: 3, Location: *, Relax Locality: true}
{Priority: 20, Capability: <memory:1024, vCores:1>, # Containers: 5, Location: /default-rack, Relax Locality: true}
{Priority: 20, Capability: <memory:1024, vCores:1>, # Containers: 1, Location: h0, Relax Locality: true}
{Priority: 20, Capability: <memory:1024, vCores:1>, # Containers: 1, Location: h1, Relax Locality: true}
{Priority: 20, Capability: <memory:1024, vCores:1>, # Containers: 1, Location: h2, Relax Locality: true}
{Priority: 20, Capability: <memory:1024, vCores:1>, # Containers: 1, Location: h3, Relax Locality: true}
{Priority: 20, Capability: <memory:1024, vCores:1>, # Containers: 1, Location: h4, Relax Locality: true}
```

####实验

##### 单个任务的延时
mapred-site.xml中参数yarn.app.mapreduce.am.scheduler.heartbeat.interval-ms可以指定am向RM发送心跳的周期  

| | 1000 | 500 |
| -- | -- | -- |
| 原来 |  | 1s内 |
| 我们 |3s  | 2s |
| 社区 | 3-4s | 2s |

##### 限制
1. 功能  
限制map和reduce数目的功能能得到满足
2. 
























