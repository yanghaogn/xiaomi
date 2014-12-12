# resources.json
http://slider.incubator.apache.org/docs/slider_specs/resource_specification.html
###失败策略
YARN containers hosting component instances may fail. This can happen because of

1. A problem in the configuration of the instance.
2. A problem in the app package
3. A problem (hardware, software or networking) in the server hosting the container
4. Conflict for a resource (usually a network port) between the component instance and another running program.
5. The server or the network connection to it failing.
6. The server being taken down for maintenance.
Slider reacts to a failed container by requesting a new container from YARN, preferably on a host that has already hosted an instance of that role. Once the container is allocated, slider will redeploy an instance of the component. As it may time for YARN to have the resources to allocate the container, it may take some time for the replacement to be instantiated.

Slider tracks failures in an attempt to differentiate problems in the application package or its configuration from those of the underlying servers. If a a component fails too many times then slider considers the application itself as failing, and halts.

This leads to the question: what is too many times?

The limits are defined in resources.json: 1. The duration of a failure window, a time period in which failures are counted. This duration can span days. 1. The maximum number of failures of any component in this time period.

The parameters defining the failure policy are as follows.

* yarn.container.failure.threshold

The threshold for failures. If set to "0" there are no limits on the number of times containers may fail.

* yarn.container.failure.window.days, yarn.container.failure.window.hours and `yarn.container.failure.window.minutes

These properties define the duration of the window; they are all combined so the window is, in minutes:
```
minutes + 60 * (hours + days * 24)
```
The initial window is measured from the start of the application master —once the duration of that window is exceeded, all failure counts are reset, and the window begins again.

If the AM itself fails, the failure counts are reset and and the window is restarted.

**Per-component and global failure thresholds**

The failure thresholds for individual components can be set independently

**Recommended values**

We recommend having a duration of a few hours for the window, and a large failure limit proportional to the the number of instances of that component

Why?

This will cover the loss of a large portion of the hardware of the cluster by trying to reinstantiate all the components. Yet, if a component does fail repeatedly, eventually slider will conclude that there is a problem and fail with the exit code 73, EXIT_DEPLOYMENT_FAILED.

Example

Here is a resource.json file for an HBase cluster
```
"resources": {
  "schema" : "http://example.org/specification/v2.0.0",
  "metadata" : { },
  "global" : {
    "yarn.container.failure.threshold" : "4", //可以继承
    "yarn.container.failure.window.hours" : "1'//失败的周期
  },
  "components" : {
    "slider-appmaster" : {
      "yarn.memory" : "256",
      "yarn.vcores" : "1",
      "yarn.component.instances" : "1"
    },
    "master" : {
      "yarn.role.priority" : "1",
      "yarn.memory" : "256",
      "yarn.vcores" : "1",
      "yarn.component.instances" : "2"
    },
    "worker" : {
      "yarn.role.priority" : "2",
      "yarn.memory" : "512",
      "yarn.container.failure.threshold" : "15",
      "yarn.vcores" : "2",
      "yarn.component.instances" : "10"
    }
  }
}
```
The window size is set to one hour: after that the counters are reset.

There is a global failure threshold of 4. As two instances of the HBase master are requested, the failure threshold per hour is double that of the number of masters.

There are ten worker components requested; the failure threshold for these components is overridden to be fifteen. This allows all workers to fail and the cluster to recover —but only another five failures would be tolerated for the remaining hour.

These failure thresholds are all heuristics. When initially configuring an application instance, low thresholds reduce the disruption caused by components which are frequently failing due to configuration problems.

In a production application, large failure thresholds and/or shorter windows ensures that the application is resilient to transient failures of the underlying YARN cluster and hardware.
###Using Labels
------

The resources.json file can be used to specify the labels to be used when allocating containers for the components. The details of the YARN Label feature can be found at YARN-796.

In summary:

* Nodes can be assigned one or more labels
* Capacity Queues can be defined with access to one or more labels
* Ensure application components are associated with appropriate label expressions
* Create the application using specific queue

This way, you can guarantee that a certain set of nodes are reserved for an application or for a component within an application.

Label expression is specified through property "yarn.label.expression". When no label expression is specified then it is assumed that only non-labeled nodes are used when allocating containers for component instances.

If label expression is specified for slider-appmaster then it also becomes the default label expression for all component. To take advantage of default label expression leave out the property (see HBASE_REGIONSERVER in the example). Label expression with empty string ("yarn.label.expression":"") means nodes without labels.

Example Here is a resource.json file for an HBase cluster which uses labels. The label for the application instance is "hbase1" and the label expression for the HBASE_MASTER components is "hbase1_master". HBASE_REGIONSERVER instances will automatically use label "hbase1". Alternatively, if you specify ("yarn.label.expression":"") for HBASE_REGIONSERVER then the containers will only be allocated on nodes with no labels.
```
{
    "schema": "http://example.org/specification/v2.0.0",
    "metadata": {
    },
    "global": {
    },
    "components": {
        "HBASE_MASTER": {
            "yarn.role.priority": "1",
            "yarn.component.instances": "1",
            //对应yarn label
            "yarn.label.expression":"hbase1_master"
        },
        "HBASE_REGIONSERVER": {
            "yarn.role.priority": "1",
            "yarn.component.instances": "1",
        },
        "slider-appmaster": {
            "yarn.label.expression":"hbase1"
        }
    }
}
```
为了试用label,你需要完成下面的操作

1. Create two labels, hbase1 and hbase1_master (use yarn rmadmin commands)
2. Assign the labels to nodes (use yarn rmadmin commands)
3. Perform refresh queue (yarn -refreshqueue)
4. Create a queue by defining it in the capacity scheduler config
5. Allow the queue to access to the labels and ensure that appropriate min/max capacity is assigned
6. Perform refresh queue (yarn -refreshqueue)
7. Create the Slider application against the above queue using parameter --queue while creating the application

#### 日志聚集

yarn-site.xml上的配置

Log aggregation at regular intervals for long running services (LRS) needs to be enabled at the YARN level before any application can exploit this functionality. To enable set the following property to a positive value of 3600 (in secs) or more. If set to a positive value less than 3600 (1 hour) this property defaults to 3600. To disable log aggregation set it to -1.
```
  <property>
      <name>yarn.nodemanager.log-aggregation.roll-monitoring-interval-seconds</name>
      <value>3600</value>
      <description>Defines how often NMs wake up to upload log files. The default value is -1. By default, the logs will be uploaded when the application is finished. By setting this configure, logs can be uploaded periodically when the application is running. The minimum rolling-interval-seconds can be set is 3600.</description>
  </property>
 ```
 
Subsequently every application owner has the flexibility to set the include and exclude patterns of file names that they intent to aggregate. In Slider the resources.json file can be used to specify the include and exclude patterns of files that need to be backed up under the default log directory of the application. These properties needs to be set at the global level as shown below -
```
{
    "schema": "http://example.org/specification/v2.0.0",
    "metadata": {
    },
    "global": {
        "yarn.log.include.patterns": "hbase*.*",
        "yarn.log.exclude.patterns": "hbase*.out"        
    },
    "components": {
        "HBASE_MASTER": {
            "yarn.role.priority": "1",
            "yarn.component.instances": "1",
        },
        "HBASE_REGIONSERVER": {
            "yarn.role.priority": "1",
            "yarn.component.instances": "1",
        },
        "slider-appmaster": {
        }
    }
}
```


















