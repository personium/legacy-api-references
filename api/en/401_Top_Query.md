# \$top  Query

### Overview

By specifying an integer value in the \$top query, it is possible to limit the number of data listings to be acquired<br>
If the number of registered data is less than the number of acquired data, all data is acquired<br>
If the number of registered data exceeds the number of acquisitions, acquire from the beginning of the registered data up to the number of acquisitions<br>
Because the data obtained by the \$top query is not sorted, you need to specify a sort condition such as a \$ orderby query

\*If you specify anything other than an integer value in \$top, return "400 Bad Request"

### Request Query

```
$top={number}
```

| Value<br>    | Overview<br>                                                     | Effective Value<br>                                                                                  | Notes<br> |
|:-- |:-- |:-- |:-- |
| {number}<br> | Specify the number of entities included in the returned feed<br> | Half size numeric 0-10000 (default: 25) <br> $expand 0-100 (default: 25) when query is specified<br> | <br>      |

### cURL Command

Example: When acquiring 10 cells:

```sh
curl "https://{UnitFQDN}/__ctl/Cell?\$top=10" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```

### Restrictions

* Considering processing performance, use with the $skip query<br>
    In this case, the recommended value of the $top query is 50 or less<br><br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED