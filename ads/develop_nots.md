##staging测试

最近staging分支比较乱，经常出现编译错误，或者merge的代码被覆盖等情况，几个小小的建议，欢迎大家补充：

1.	每个人在自己的dev 分支进行代码改动

2.	在进入staging测试之前，把master分支最新代码merge到自己的dev分支，然后再把dev分支merge到staging分支

3.	Push代码之前确保staging分支没有编译错误

4.	部署之后如果出现问题，不建议reset代码，先定位原因之后再push新的改动

5.	对于thrift等common项目，也需要类似流程，同时记得把版本号设置成类似“xxx-songqiang-SNAPSHOT”的格式

6.	对于非常小的改动，也建议先在自己的dev分支修改，然后记得merge到staging分支

##shell
1. 测试
```
创建.debug文件
if test -f .debug ; then
    local emailList=yanghao3@xiaomi.com
    local year='2015'
    local month='07'
    local day='31'
fi
```

##git
####冲突解决
1. git rebase origin/master
2. 遇到冲突，切换到rebase分支
3. 解决冲突，git commit -m "merge master" -a
4. git rebase --continue
5. 如果提示"git add"，运行git status，如果没有变化，则运行git rebase --skip，然后重复步骤2，直到解决冲突

####合并
1. 将feature合并成一次提交

##mysql
1. updateBusinessLayout中，有大量的sql，每条sql用于查询一个app的信息，这些sql非常类似，如果将其中的like或者=，改成in将极大的节省sql查询次数，减轻服务器的负载

##Java
1. 不要违反的Java比较原语：[http://stackoverflow.com/questions/19325256/java-lang-illegalargumentexception-comparison-method-violates-its-general-contr](http://stackoverflow.com/questions/19325256/java-lang-illegalargumentexception-comparison-method-violates-its-general-contr)
2. 产生日期随机数，这是因为SimpleDateFormat不是线程安全的：http://stackoverflow.com/questions/6840803/simpledateformat-thread-safety
3. MD5加密：https://commons.apache.org/proper/commons-codec/apidocs/org/apache/commons/codec/digest/DigestUtils.html
4. 高并发性getHostByName，性能很差：[http://blog.csdn.net/shijun_zhang/article/details/6577426](http://blog.csdn.net/shijun_zhang/article/details/6577426)

## code review工具
arc land --remote infra --onto hadoop-2.4.0-mdh2

##maven
1. ubuntu上安装maven3:[https://software.intel.com/zh-cn/blogs/2014/03/10/ubuntu-maven-0](https://software.intel.com/zh-cn/blogs/2014/03/10/ubuntu-maven-0)
2. 

##工程
1. BI平台：添加指标后要给刘博文说，让那边添加新的map