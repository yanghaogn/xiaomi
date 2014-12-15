# am重试

####受影响的参数
1. yarn-site.xml
```
yarn.resourcemanager.am.max-attempts
```
2. $SLIDER_CONF_DIR/
```
slider.yarn.restart.limit//设置成0表示不受影响
```
3. ~~resources.json~~不受影，因为一旦AM重启，计数被重置了
```
    "yarn.container.failure.threshold":"5",
    "yarn.container.failure.window.days":"1",
    "yarn.container.failure.window.hours":"1",
    "yarn.container.failure.window.minutes":"1"
```

####失败后不让component重启
appConfig.json  


~~"slider.am.restart.supported" : "false"~~该参数不再起作用

1. test kill the container，会重启container  
```
$slider kill-container memcached1 --id container_1418384158768_0004_01_000001
```
2. 测试自杀，不会重启container
```
$slider am-suicide memcached1 --exitcode 1  --message "test"
```
3. 吐槽
```
启动，kill SliderAppMaster,都重启
启动，1. kill -6 SliderAppMaster，只am重启
        2. kill SliderAppMaster,只am重启
```
4. 

