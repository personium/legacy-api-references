# \$filter Query

### Overview

Use the \$filter query to specify the search condition when retrieving the list

### Operator Support

|Operator<br>|Meaning<br>|Description example<br>|Notes<br>|
|:--|:--|:--|:--|
|eq<br>|equal<br>|For character strings : $filter=itemKey eq 'searchValue'<br>In case of numerical value : $filter=itemKey eq 10<br>|Corresponds to character string, numeric value, boolean value, NULL<br>|
|ne<br>|Negative Equal<br>|For character strings : $filter=itemKey ne 'searchValue'<br>In case of numerical value : $filter=itemKey ne 10<br>|Corresponds to character string, numeric value, boolean value, NULL<br>|
|gt<br>|Greater than<br>|$filter=itemKey gt 1000<br>|Corresponds to character string, numeric value<br>|
|ge<br>|Greater equal<br>|$filter=itemKey ge 1000<br>|Corresponds to character string, numeric value<br>|
|lt<br>|Less than<br>|$filter=itemKey lt 1000<br>|Corresponds to character string, numeric value<br>|
|le<br>|Less equal<br>|$filter=itemKey le 1000<br>|Corresponds to character string, numeric value<br>|
|and<br>|AND<br>|$filter=itemKey1 eq 'searchValue1' and itemKey2 eq 'searchValue2'<br>|<br>|
|or<br>|OR<br>|$filter=itemKey1 eq 'searchValue1' or itemKey2 eq 'searchValue2'<br>|<br>|
|()<br>|Priority group<br>|$filter=itemKey eq 'searchValue' or (itemKey gt 500 and itemKey lt 1500)<br>|If parentheses are only one, parentheses are ignored<br>|

### Support function

|Function<br>|Overview<br>|Example<br>|Notes<br>|
|:--|:--|:--|:--|
|startswith<br>|Forward match<br>|$filter=startswith(itemKey, 'searchValue')<br>|Only for character string correspondence<br>|
|substringof<br>|Partial Match<br>|$filter=substringof('searchValue1', itemKey1)<br>|Only for character string correspondence<br>*partial matching of alphanumeric characters is not supported<br>|

### Search specification by property type

#### Edm.String

Convert keywords specified for search to string type and search

#### Edm.Int32

An integer value or null can be specified<br>
If an integer value is specified as a string type, it is searched by converting it to a numeric type

#### Edm.Single

It can specify integer, fractional value or null<br>
If it is a string type integer, if decimal value is specified convert it to decimal value and search

#### Edm.Boolean

Only boolean values or null can be specified

### Notices

\*URL encoding required for query specification<br>
\*Valid values of the property to be searched are half-size alphanumeric characters, - (hyphen), \_ (underscore) valid.<br>
\*If you want to include a single quote in the search string, you can specify a single quote as a search string by describing two single quotes"''"<br>
\*If a property name that does not exist in \$ filter is specified, it is searched with it included in the search condition<br>
\*When \_\_updated, \_\_published is specified, it is specified by UNIX time (a number in parenthesis of "/Date()")

### cURL Command

Example: When acquiring a cell whose cell name is sample:

```sh
curl "https://{UnitFQDN}/__ctl/Cell?\$filter=Name%20eq%20'sample'" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED