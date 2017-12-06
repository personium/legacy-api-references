# $filter Query

### Overview

Use the $filter query to specify the search condition when retrieving the list

### Operator Support

|Operator|Meaning|Description example|Notes|
|:--|:--|:--|:--|
|eq|equal|For character strings : $filter=itemKey eq 'searchValue'<br>In case of numerical value : $filter=itemKey eq 10|Corresponds to character string, numeric value, boolean value, NULL|
|ne|Negative Equal|For character strings : $filter=itemKey ne 'searchValue'<br>In case of numerical value : $filter=itemKey ne 10|Corresponds to character string, numeric value, boolean value, NULL|
|gt|Greater than|$filter=itemKey gt 1000|Corresponds to character string, numeric value|
|ge|Greater equal|$filter=itemKey ge 1000|Corresponds to character string, numeric value|
|lt|Less than|$filter=itemKey lt 1000|Corresponds to character string, numeric value|
|le|Less equal|$filter=itemKey le 1000|Corresponds to character string, numeric value|
|and|AND|$filter=itemKey1 eq 'searchValue1' and itemKey2 eq 'searchValue2'||
|or|OR|$filter=itemKey1 eq 'searchValue1' or itemKey2 eq 'searchValue2'||
|()|Priority group|$filter=itemKey eq 'searchValue' or (itemKey gt 500 and itemKey lt 1500)|If parentheses are only one, parentheses are ignored|

### Support function

|Function|Overview|Example|Notes|
|:--|:--|:--|:--|
|startswith|Forward match|$filter=startswith(itemKey, 'searchValue')|Only for character string correspondence|
|substringof|Partial Match|$filter=substringof('searchValue1', itemKey1)|Only for character string correspondence<br>*partial matching of alphanumeric characters is not supported|

### Search specification by property type

#### Edm.String

Convert keywords specified for search to string type and search

#### Edm.Int32

An integer value or null can be specified  
If an integer value is specified as a string type, it is searched by converting it to a numeric type

#### Edm.Single

It can specify integer, fractional value or null  
If it is a string type integer, if decimal value is specified convert it to decimal value and search

#### Edm.Boolean

Only boolean values or null can be specified

### Notices

\*URL encoding required for query specification  
\*Valid values of the property to be searched are half-size alphanumeric characters, - (hyphen), \_ (underscore) valid.  
\*If you want to include a single quote in the search string, you can specify a single quote as a search string by describing two single quotes"''"  
\*If a property name that does not exist in $ filter is specified, search is performed ignoring the property name  
\*When \_\_updated, \_\_published is specified, it is specified by UNIX time (a number in parenthesis of "/Date()")

### cURL Command

Example: When acquiring a cell whose cell name is sample:

```sh
curl "https://{UnitFQDN}/__ctl/Cell?\$filter=Name%20eq%20'sample'" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```


###### Copyright 2017 FUJITSU LIMITED
