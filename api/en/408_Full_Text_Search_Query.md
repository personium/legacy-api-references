# Full-text Search (q)  Query

### Overview

Use full-text search (q) query when specifying full-text search keyword when acquiring list<br>
Including complex type data, all values are searched

\*URL encoding required for query specification<br>
\*When specifying \_\_updated, \_published values, specify them by UNIX time(a number in parenthesis of "/Date()")<br>
\*Items in \_metadata are not subject to search

### Request Query

```
q={SearchKeyword}
```

| Item<br>            | Overview<br>              | Effective Value<br>              | Notes<br> |
|:-- |:-- |:-- |:-- |
| {SearchKeyword}<br> | Specify search string<br> | Number of digits: 1-255 byte<br> | <br>      |

### Type to be searched

The data types to be searched are shown below

| Data type<br>    | Search target<br> | Notes<br>                 |
|:-- |:-- |:-- |
| Edm.String<br>   | Yes<br>           | <br>                      |
| Edm.Boolean<br>  | Yes<br>           | <br>                      |
| Edm.Single<br>   | Yes<br>           | <br>                      |
| Edm.Int32<br>    | Yes<br>           | <br>                      |
| Edm.Double<br>   | Yes<br>           | Dynamic property only<br> |
| Edm.DateTime<br> | No<br>            | <br>                      |

### Search Specification

* Half space blank

    * Treat as delimiter
    * "(Double quote), it is not treated as one word
    * Example) The following specification searches for data including the keywords "Pochi" and "Tama"

        ```
        q=Pochi%20Tama
        q="Pochi%20Tama"
        ```

* Half size alphanumeric characters

    * Can not search for partial matches within keywords
    * Not distinguish between capital letters and small letters

* Double-byte character

    * Searchable for partial matches
    * Not distinguish between capital letters and small letters

### cURL Command

Example: When acquiring a cell list, when acquiring a cell that matches the keyword "sample":

```sh
curl "https://{UnitFQDN}/__ctl/Cell?q=sample" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED