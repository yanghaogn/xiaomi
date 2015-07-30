##staging测试

最近staging分支比较乱，经常出现编译错误，或者merge的代码被覆盖等情况，几个小小的建议，欢迎大家补充：

1.	每个人在自己的dev 分支进行代码改动

2.	在进入staging测试之前，把master分支最新代码merge到自己的dev分支，然后再把dev分支merge到staging分支

3.	Push代码之前确保staging分支没有编译错误

4.	部署之后如果出现问题，不建议reset代码，先定位原因之后再push新的改动

5.	对于thrift等common项目，也需要类似流程，同时记得把版本号设置成类似“xxx-songqiang-SNAPSHOT”的格式

6.	对于非常小的改动，也建议先在自己的dev分支修改，然后记得merge到staging分支

##git
####冲突解决
1. git rebase origin/master
2. 遇到冲突，切换到rebase分支
3. 解决冲突，git commit -m "merge master" -a
4. git rebase --continue
5. 如果提示"git add"，运行git status，如果没有变化，则运行git rebase --skip，然后重复步骤2，直到解决冲突

####合并
1. 将feature合并成一次提交
git merge --no-ff <branch>
##mysql
1. 重复则更新记录，不重复就插入https://dev.mysql.com/doc/refman/5.0/en/insert-on-duplicate.html
```
insert into ad_type (virtual_media_type,billing_type,billing_type_name,placement_type,placement_type_name,media_type,media_type_name,sub_media_type,sub_media_type_name,new_media_type,new_media_type_name) values (21704,2,'CPC',17,'信息流广告-应用',4,'一点资讯',-1,'null',4,'一点资讯') on duplicate key update virtual_media_type=21704,billing_type=2,billing_type_name='CPC',placement_type=17,placement_type_name='信息流广告-应用',media_type=4,media_type_name='一点资讯',sub_media_type=-1,sub_media_type_name='null',new_media_type=4,new_media_type_name='一点资讯';
```

##Java
1. 不要违反的Java比较原语：[http://stackoverflow.com/questions/19325256/java-lang-illegalargumentexception-comparison-method-violates-its-general-contr](http://stackoverflow.com/questions/19325256/java-lang-illegalargumentexception-comparison-method-violates-its-general-contr)
2. 产生日期随机数，这是因为SimpleDateFormat不是线程安全的：http://stackoverflow.com/questions/6840803/simpledateformat-thread-safety

## code review工具
arc land --remote infra --onto hadoop-2.4.0-mdh2

