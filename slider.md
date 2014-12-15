# Slider
slder试图将计算框架平滑(slide)的迁移到YARN上。
####阅读引导
1. [Build YARN Apps on Hadoop with Apache Slider: Technical Preview Now Available](http://zh.hortonworks.com/blog/apache-slider-technical-preview-now-available/)
2. [命令](http://slider.incubator.apache.org/docs/manpage.html)
3. [架构](http://slider.incubator.apache.org/design/architecture.html)

####安装
1. 参考：[getting_started](http://slider.incubator.apache.org/docs/getting_started.html)  
 注意：配置请参考[Apache Slider Troubleshooting](http://slider.incubator.apache.org/docs/troubleshooting.html)
2. 编译  
 * 将`<hadoop.version>2.6.0-SNAPSHOT</hadoop.version>`改成`<hadoop.version>2.6.0</hadoop.version>`
 * mvn clean site:site site:stage package -DskipTests
3. 配置
 * zookeeper：127.0.0.1:2181 
4. 启动zookeeper  
 * bin/zkServer.sh start
 * bin/zkCli.sh -server 127.0.0.1:2181

####概念

1. component  
每个component对应application的一个组件，一个component的一个实例对应一个container，slider会为每个component取一个名字，在metainfo.xml中进行定义。每个application的资源都是基于component分配的
2. Slider AM
 1. The AM engine which handles all integration with external services, specifically YARN and any Slider clients
 2. A provider specific to deploying a class of applications.
 3. The Application State.
3. client：通过RPC，和AM或者YARN交互
4. agent
 1. The core provider在container上部署最小的agent, 然后, as the agent checks in to the agent provider's REST API, 执行这些命令：从hdfs下载包、解压这些包，然后按照模板并使用真实的配置通过python脚本启动
5. RPC接口  
SliderClusterAPI  
getJSONClusterStatus(): get the status of the application instance as a JSON document.  
flexCluster() update the desired count of role instances in the running application instance.  
stopCluster() stop the application instance
6. 

#### 技巧
1. 获取服务的host_port  
```
通过shel命令获取端口，或者通过GET从HTTP中获取服务详情
```



