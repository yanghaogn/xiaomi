#### environment
1. /etc/hosts中的`127.0.1.1 yang`改成`127.0.0.1 yang`，这是因为运行过程中，unittest没法找到`127.0.1.1`的位置
2. mvn test运行参数
```
-Dhadoop.log.dir=/home/user/hadoop-core/build/test/logs  -Dtest.build.data=/home/yang/test.build.data -Dmaven.test.failure.ignore=true 
```
 
#### TestJobHistoryUtils
 testGetHistoryDirsForCleaning()均存在
 原因，System.getProperty("test.build.data")为null
 ```
 Caused by: java.lang.NullPointerException
	at java.io.File.<init>(File.java:222)
	at org.apache.hadoop.mapreduce.v2.jobhistory.TestJobHistoryUtils.<clinit>(TestJobHistoryUtils.java:40)
 ```


