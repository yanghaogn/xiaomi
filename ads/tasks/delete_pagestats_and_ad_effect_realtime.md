##ad_effect_realtime
####数据
1. 一个星期前type_id为NULL的数据
```
mysql> select count(*) from ad_effect_realtime where record_date < '2015-07-21 00:00:00' and type_id is NULL;  
+----------+
| count(*) |
+----------+
| 35662286 |
+----------+
1 row in set (18.57 sec)
```
2. 一个月前type_id为NULL的数据
```
mysql> select count(*) from ad_effect_realtime where record_date < '2015-06-21 00:00:00' and type_id is NULL;   
+----------+
| count(*) |
+----------+
| 16473483 |
+----------+
```

####结论
1. 备份15天前type_id为null以及TimeDimension为0的数据
```
mysqldump -uad_statistic_wr -pOTI1ZmFhYmZlZWEyNDIzZjgxYzBmMDFl -h10.105.20.11 miui_ad_statistic ad_effect_realtime where 'time_dimension = 0 and type_id is not null and record_date < '2015-07-12'' --skip-lock-tables > removeOldAdEffectRealTimeData/2015-07-12.dump
```
2. 清理15天前type_id为null以及TimeDimension为0的数据

####方案
1. 创建目录
```
hadoop dfs -mkdir /user/h_miui_ad/luoyan/ad_statistic/ad_effect_realtime/
hadoop dfs -mkdir /user/h_miui_ad/luoyan/ad_statistic/ad_effect_realtime/year=2015
hadoop dfs -mkdir /user/h_miui_ad/luoyan/ad_statistic/ad_effect_realtime/year=2015/month=07/
hadoop dfs -mkdir /user/h_miui_ad/luoyan/ad_statistic/ad_effect_realtime/year=2015/month=07/day=27/
```

##Page_Stats数据清理
####数据
1. 总量：
```
mysql> select count(*) from Page_Stats ;                      
+----------+
| count(*) |
+----------+
| 13685984 |
```
2. 各个TimeDimension的数据量
```
mysql> select TimeDimension, count(*) from Page_Stats group by TimeDimension ;                            
+---------------+----------+
| TimeDimension | count(*) |
+---------------+----------+
|            -2 |  6108763 |
|            -1 |  6223432 |
|             0 |  1151562 |
|             1 |   202227 |
+---------------+----------+
4 rows in set (10.31 sec)
```
3. 一个月前各个TimeDimension的数据量
```
mysql> select TimeDimension, count(*) from Page_Stats  where Date < '2015-06-27' group by TimeDimension;                                           
+---------------+----------+
| TimeDimension | count(*) |
+---------------+----------+
|            -2 |  5160028 |
|            -1 |  5158272 |
|             0 |  1027253 |
|             1 |   179153 |
+---------------+----------+
4 rows in set (11.74 sec)
```
4. 两个月前各个TimeDimension的数据量
```
mysql> select TimeDimension, count(*) from Page_Stats  where Date < '2015-05-27' group by TimeDimension;           
+---------------+----------+
| TimeDimension | count(*) |
+---------------+----------+
|            -2 |  3572282 |
|            -1 |  3572282 |
|             0 |   906871 |
|             1 |   157501 |
+---------------+----------+
4 rows in set (10.63 sec)
```
5. 三个月前各个TimeDimension的数据量
```
mysql> select TimeDimension, count(*) from Page_Stats  where Date < '2015-04-27' group by TimeDimension; 
+---------------+----------+
| TimeDimension | count(*) |
+---------------+----------+
|            -2 |  1933016 |
|            -1 |  1933016 |
|             0 |   800380 |
|             1 |   129018 |
+---------------+----------+
4 rows in set (10.09 sec)
```

####结论
1. 清理一个月前小时级统计数据
2. 清理三个月前小时级累加的数据