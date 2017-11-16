# Delete Log File

### Overview

This API deletes the existing log files. It cannot delete the most recent log file.<br>
br />
If you exceed the maximum number of generations to hold rotate when the log file, the log file of the oldest is deleted.

### Required Privileges

log

### Restrictions

*  Log output of the internal event is not supported, Log output configuration is not supported
* log file name"default.log"(fixed)
* Set rotate size: 50MB
* Configure the log output label to "info""info"(fixed)(output for all INFO, WARN, ERROR)

<br>

### Request

#### Request URL

##### rotated log file

```
/{CellName}/__log/archive/{LogName}
```
