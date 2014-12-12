# YARN

1. [Simplifying user-logs management and access in YARN](http://zh.hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/)  
通过配置，可以将作业的日志聚集在HDFS上。聚集是在NM上完成的，而日志的自动删除是由MapReduce JobHistoryServer中的AggregatedLogDeletionService完成的
2. [YARN框架学习系列](http://zh.hortonworks.com/get-started/yarn/)

# 组件
###proxyserver
当你点击[yang:8088/cluster](http://yang:8088/cluster)看到应用列表时，点击某个应用对应的Traking UI，跳转到[yang:8088/proxy/application_1418178618443_0004/](http://yang:8088/proxy/application_1418178618443_0004/)，你看到的内容就是通过proxyserver从AM或者JobHistoryServer上获取来的信息，按照官方文档说，这样可以增加服务的安全性
client <——>proxy<——>AM or HistoryServer
