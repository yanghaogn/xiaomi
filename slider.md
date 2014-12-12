# Slider
slder试图将计算框架平滑(slide)的迁移到YARN上。
####阅读引导
1. [Build YARN Apps on Hadoop with Apache Slider: Technical Preview Now Available](http://zh.hortonworks.com/blog/apache-slider-technical-preview-now-available/)
2. [命令](http://slider.incubator.apache.org/docs/manpage.html)

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


#### 技巧
1. 获取服务的host_port  
```
通过shel命令获取端口，或者通过GET从HTTP中获取服务详情
```
2. 


