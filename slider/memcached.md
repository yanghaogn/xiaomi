# memcached
**启动zookeeper**
```
$zkServer.sh start
```
**jmemcached包来源**  
如果直接下载jmemcached-cli-1.0.0.jar、jmemcached-core-1.0.0.jar,最后jmemcached启动不起来，因为启动时或者运行过程中缺少class文件，需要自己编译
```
$hg clone https://code.google.com/p/jmemcache-daemon/
$cd jmemcache-daemon
$mvn clean package -Pdist,native -DskipTests -Ptar
$mkdir ~/jmemcached-1.0.0
$cp cli/target/jmemcached-cli-1.0.1-SNAPSHOT-main.jar ~/jmemcached-1.0.0
$cp core/target/jmemcached-core-1.0.1-SNAPSHOT.jar ~/jmemcached-1.0.0
$cd ~/jmemcached-1.0.0
$tar -cvf jmemcached-1.0.0.tar *
将该包拷贝到memcached/package/files下
```
之后按照官方文档操作

**install**
```
$bin/slider install-package --name MEMCACHED --package app-packages/jmemcached-1.0.0.zip
```
**更新包**
```
$bin/slider install-package --name MEMCACHED --package app-packages/jmemcached-1.0.0.zip --replacepkg
```
**create**
```
$bin/slider create memcached1 --template tmp/memcached/appConfig.json --resources tmp/memcached/resources.json --queue default 

```
**更新服务器数目**
MEMCACHED: metainfo.xml中进行定义，
```
$bin/slider flex memcached1 --component MEMCACHED 8 //将该memcached1的MEMCACHED设置成8
```
**服务的状态**  
如果想查看为live的应用，需要在后面指定--live
An application instance that is FINISHED or FAILED or KILLED is not considered to be live.
```
$slider exists memcached1 --live
or
$slider list memcached1 --live
```
####端口号获取
1. 参考 http://slider.incubator.apache.org/docs/slider_specs/specifying_exports.html

####patch
#####出错处理
某些时候报错，会调用`slider/slider-agent/src/main/python/resource_management/core/shell.py`
里面的` err_msg = Logger.get_protected_text(("Execution of '%s' returned %d. %s") % (command[-1], code, out))`，而如果传入参数的字节大小大于127,则会报错，这样会隐藏本来的错误

在执行命令前加上LANG=C









