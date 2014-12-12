
 #### TestJobImpl
 testReportDiagnostics()我们的存在
 原因：用户丢了
 ```
 java.lang.NullPointerException
	at org.apache.hadoop.mapreduce.v2.proto.MRProtos$JobReportProto$Builder.setUser(MRProtos.java:11270)
	at org.apache.hadoop.mapreduce.v2.api.records.impl.pb.JobReportPBImpl.setUser(JobReportPBImpl.java:217)
	at org.apache.hadoop.mapreduce.v2.util.MRBuilderUtils.newJobReport(MRBuilderUtils.java:73)
	at org.apache.hadoop.mapreduce.v2.app.job.impl.JobImpl.getReport(JobImpl.java:863)
 ```