# hadoop-yarn-project

#### TestApplicationClientProtocolOnHA

基本全错
```
org.apache.hadoop.yarn.exceptions.YarnRuntimeException: java.io.IOException: ResourceManager failed to start. Final state is STOPPED
	at org.apache.hadoop.yarn.server.MiniYARNCluster.startResourceManager(MiniYARNCluster.java:326)
	at org.apache.hadoop.yarn.server.MiniYARNCluster.access$500(MiniYARNCluster.java:93)
	at org.apache.hadoop.yarn.server.MiniYARNCluster$ResourceManagerWrapper.serviceStart(MiniYARNCluster.java:446)
	at org.apache.hadoop.service.AbstractService.start(AbstractService.java:193)
	at org.apache.hadoop.service.CompositeService.serviceStart(CompositeService.java:120)
	at org.apache.hadoop.service.AbstractService.start(AbstractService.java:193)
	at org.apache.hadoop.yarn.client.ProtocolHATestBase.startHACluster(ProtocolHATestBase.java:277)
	at org.apache.hadoop.yarn.client.TestApplicationClientProtocolOnHA.initiate(TestApplicationClientProtocolOnHA.java:54)
	at sun.reflect.GeneratedMethodAccessor10.invoke(Unknown Source)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:25)
	at org.junit.runners.model.FrameworkMethod$1.runReflectiveCall(FrameworkMethod.java:45)
	at org.junit.internal.runners.model.ReflectiveCallable.run(ReflectiveCallable.java:15)
	at org.junit.runners.model.FrameworkMethod.invokeExplosively(FrameworkMethod.java:42)
	at org.junit.internal.runners.statements.RunBefores.evaluate(RunBefores.java:27)
	at org.junit.internal.runners.statements.RunAfters.evaluate(RunAfters.java:30)
	at org.junit.rules.TestWatchman$1.evaluate(TestWatchman.java:53)
```

