# \$select  Query

### Overview

When specifying the property to return, use the \$ select query<br>
If omitted, return all the acquired properties<br>
However, the following properties are always returned without specifying them in \$select

* __id
* __published
* __updated
* __metadata

\*If you specify a property name that does not exist in \$select, ignore the specified item<br>
\*If the value of the Dynamic property specified by \$select is NULL, you can not get the property value<br>
\*Specify the property name without enclosing it with "'" (single quote)

### Request Query

```
$select={propertyName}
```

\*When omitted, it is \$select = *

| Path<br>           | Overview<br>                                                                               |
|:-- |:-- |
| {PropertyName}<br> | Property name to return<br>To specify more than one, specify it with a comma separator<br> |

### cURL Command

Example: When returning only Name property when acquiring Box list:

```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/Box?\$select=Name" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```

Example: When returning all the properties when acquiring the Box list:

```
curl "https://{UnitFQDN}/{CellName}/__ctl/Box?\$select=*" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED