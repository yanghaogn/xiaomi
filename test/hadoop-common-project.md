# hadoop-common-project
$cd  hadoop-common-project  
$mvn -Dhadoop.log.dir=/home/yang/hadoop-core/build/test/logs  -Dtest.build.data=/home/yang/test.build.data -Dmaven.test.failure.ignore=true  test

有Error，但是没有fail的test