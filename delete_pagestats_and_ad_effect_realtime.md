##ad_effect_realtime
####数据
1. 可以清理一个星期前type_id为null的数据
![](ad_effect_realtime一个星期前type_id的数据量.png)
####清理
可以清理一个星期前type_id为null的数据量
#Page_Stats数据清理
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