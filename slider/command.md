# command
1. [manage](http://slider.incubator.apache.org/docs/manpage.html)

####resolve
```
$slider resolve --path /users/hbase/services/org-apache-slider --list
该命令在解析Zookeeper的目录/registry/users/yang/services/org-apache-slider
```
###Live Tests

When the `--live` flag is set, the application instance must be running or about to run for the probe to succeed. That is, either application is running (RUNNING) or in any of the states from which an application can start running. That means the service can be in any of the states `NEW`, `NEW_SAVING`, `SUBMITTED`, `ACCEPTED` or `RUNNING`

An application instance that is `FINISHED` or `FAILED` or `KILLED` is not considered to be live.

Note that probe does not check the liveness of the actually deployed application, merely that the application instance has been deployed

Return codes
```
 0 : application instance is running
-1 : application instance exists but is not running
69 : application instance is unknown
```
Example:
```
slider exists instance4 --live
```
When the `--state` flag is set, a specific YARN application state is checked for.

The allowed YARN states are:
```
  NEW: Application which was just created.
  NEW_SAVING: Application which is being saved.
  SUBMITTED: Application which has been submitted. 
  ACCEPTED: Application has been accepted by the scheduler
  RUNNING: Application which is currently running. 
  FINISHED:  Application which finished successfully. 
  FAILED: Application which failed.
  KILLED: Application which was terminated by a user or admin.
  ```
Example:
```
slider exists instance4 --state ACCEPTED
```
Return codes
```
 0 : application instance is running
-1 : application instance exists but is not in the desired state
69 : application instance is unknown
```
