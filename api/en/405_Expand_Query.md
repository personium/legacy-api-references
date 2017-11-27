# \$expand  Query

### Overview

By specifying the name of NavigationProperty in the \$expand query, it is possible to expand related information and acquire it.<br>
The expansion of related information at the time of list acquisition is expanded up to 100 cases.<br>
Deployment of related information at the time of acquiring one case will expand up to 10000 cases.<br>
The sort order of related data acquired by \$expand is as follows.

|Relation<br>|Sort conditions<br>|order<br>|
|:--|:--|:--|
|0 .. 1:0 .. 1<br>|Entity creation date and time of the navigation property to be expanded<br>|Descending order<br>|
|0 .. 1: *<br>|Entity creation date and time of the navigation property to be expanded<br>|Descending order<br>|
|*: 0 .. 1<br>|Entity creation date and time of the navigation property to be expanded<br>|Descending order<br>|
|_:_ <br>|Link information creation date and time with the navigation property to be expanded<br>|Descending order<br>|

\*When Multiplicity is & quot;1", the sort result is the same as "0..1".

### Request Query

```
$expand={NavigationPropertyName}
```

|Path<br>|Overview<br>|
|:--|:--|
|{NavigationPropertyName}<br>|Navigation property name to expand<br>To specify more than one, specify it with a comma separator<br>|

\*If you specify a NavigationProperty name that does not exist in \$expand, return "400 Bad Request"

##### Navigation property Specifiable number

You can specify up to 2 navigation properties when acquiring list<br>
You can specify up to 10 navigation properties when acquiring one case<br>
\*If you exceed the number of navigation properties that can be specified return "400 Bad Request"

### cURL Command

Expand and acquire information associated with navigation properties

```
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/{EntityTypeName}('{EntityID}')?\$expand={NavigationPropertyName}" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```

### Restrictions

* Expansion of related information can be done in one level only
* Add __count as an item in the related information list (not supported) to indicate whether the expanded related information has returned all cases<br><br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED