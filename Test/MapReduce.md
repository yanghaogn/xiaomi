* org.apache.hadoop.mapred.TestShuffleHandler 
 
 1. testMapFileAccess（）都存在
 ```
 Test 'org.apache.hadoop.mapred.TestShuffleHandler.testMapFileAccess' ignored
org.junit.internal.AssumptionViolatedException: got: <false>, expected: is <true>

	at org.junit.Assume.assumeThat(Assume.java:70)
	at org.junit.Assume.assumeTrue(Assume.java:39)
	at org.apache.hadoop.mapred.TestShuffleHandler.testMapFileAccess(TestShuffleHandler.java:518)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:39)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:25)
	at org.junit.runners.model.FrameworkMethod$1.runReflectiveCall(FrameworkMethod.java:45)
	at org.junit.internal.runners.model.ReflectiveCallable.run(ReflectiveCallable.java:15)
	at org.junit.runners.model.FrameworkMethod.invokeExplosively(FrameworkMethod.java:42)
	at org.junit.internal.runners.statements.InvokeMethod.evaluate(InvokeMethod.java:20)
	at org.junit.internal.runners.statements.FailOnTimeout$StatementThread.run(FailOnTimeout.java:62)
 ```
* 