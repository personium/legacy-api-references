# \$inlinecount  Query

### Overview

Use the \$inlinecount query to specify the number of retrieved results when retrieving a list<br>
If omitted, do not include the number of acquisition results in response

### Effective Value

| Value<br>    | Overview<br>                                         | Example<br>               | Notes<br> |
|:-- |:-- |:-- |:-- |
| allpages<br> | Include number of results obtained<br>               | $inlinecount=allpages<br> | <br>      |
| none<br>     | Do not include the number of acquisition results<br> | $inlinecount=none<br>     | <br>      |

### cURL Command

Example: Obtaining the cell list When acquiring the cell count including the number of results:

```sh
curl "https://{UnitFQDN}/__ctl/Cell?\$inlinecount=allpages" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED