结果：/home/yang/xiaomi/hadoop-common/hadoop-mapreduce-project/hadoop-mapreduce-client/hadoop-mapreduce-client-app/target/surefire-reports
#### org.apache.hadoop.mapreduce.v2.app.MRAppBenchmark
benchmark1() ，都存在"java.lang.NullPointerException"  
原因：队列丢了
解决方案
```
--- a/hadoop-mapreduce-project/hadoop-mapreduce-client/hadoop-mapreduce-client-core/src/main/java/org/apache/hadoop/mapreduce/jobhistory/JobQueueChangeEvent.java
+++ b/hadoop-mapreduce-project/hadoop-mapreduce-client/hadoop-mapreduce-client-core/src/main/java/org/apache/hadoop/mapreduce/jobhistory/JobQueueChangeEvent.java
@@ -27,7 +27,7 @@
   
   public JobQueueChangeEvent(JobID id, String queueName) {
     datum.jobid = new Utf8(id.toString());
-    datum.jobQueueName = new Utf8(queueName);
+    datum.jobQueueName = new Utf8(queueName==null?"":queueName);
   }
```

#### org.apache.hadoop.mapred.TestShuffleHandler
 
 testMapFileAccess（） ，都存在
 /
 ```
 testMapFileAccess(org.apache.hadoop.mapred.TestShuffleHandler)  Time elapsed: 0.122 sec  <<< FAILURE!
java.lang.AssertionError: null
at org.junit.Assert.fail(Assert.java:92)
at org.junit.Assert.assertTrue(Assert.java:43)
at org.junit.Assert.assertTrue(Assert.java:54)
at org.apache.hadoop.mapred.TestShuffleHandler.testMapFileAccess(TestShuffleHandler.java:594)

 ```
 
 ####org.apache.hadoop.mapreduce.v2.app.job.impl.TestJobImpl
 testCheckAccess()
 ```
 TestJobImpl.testCheckAccess:539 null
 ```
 
####TestMRJobsn
```
Failed tests:
TestMRJobs.testFailingMapper:362 expected:<TIPFAILED> but was:<FAILED>

```
 

 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
