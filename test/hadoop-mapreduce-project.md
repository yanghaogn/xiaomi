$cd hadoop-mapreduce-project  
$mvn -Dhadoop.log.dir=/home/yang/hadoop-core/build/test/logs  -Dtest.build.data=/home/yang/test.build.data -Dmaven.test.failure.ignore=true  test >test

使用>test可以将结果输出到文件test中


结果：工程/target/surefire-reports

 
####TestJobImpl
 testCheckAccess()我们的存在
 ```
testCheckAccess(org.apache.hadoop.mapreduce.v2.app.job.impl.TestJobImpl)  Time elapsed: 0.013 sec  <<< FAILURE!
java.lang.AssertionError: null
         at org.junit.Assert.fail(Assert.java:92)
         at org.junit.Assert.assertTrue(Assert.java:43)
         at org.junit.Assert.assertTrue(Assert.java:54)
         at org.apache.hadoop.mapreduce.v2.app.job.impl.TestJobImpl.testCheckAccess(TestJobImpl.java:539)

 ```
 原因，定位到JobImpl.checkAccess中，username为null,查找JobImpl.username的初始化过程

 以前： this.username = System.getProperty("user.name")  
 我们： this.username = conf.get(MRJobConfig.USER_NAME);
 
####TestMRJobs
Ref:[MAPREDUCE-5684](https://issues.apache.org/jira/browse/MAPREDUCE-5684)
```
Failed tests:
TestMRJobs.testFailingMapper:362 expected:<TIPFAILED> but was:<FAILED>

```
 

 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 

















