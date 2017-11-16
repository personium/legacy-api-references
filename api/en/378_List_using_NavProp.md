# Get An Entity list via NavProp

### Overview

Get Entity list of user data via Navigation Property.

### Required Privileges

read

### Restrictions

None

<br>

### Request

#### Request URL

```
/{CellName}/{BoxName}/{ODataCollecitonName}/{EntityTypeName}('{EntityID}')/{NavigationPropertyName}
```

| Path<br>                     | Overview<br>                |
|:-- |:-- |
| {CellName}<br>               | Cell Name<br>               |
| {BoxName}<br>                | Box Name<br>                |
| {ODataCollecitonName}<br>    | Collection Name<br>         |
| {EntityTypeName}<br>         | EntityTypeName<br>          |
| {EntityID}<br>               | EntityID<br>                |
| {NavigationPropertyName}<br> | NavigationProperty name<br> |

NavigationProperty name that can be specified is limited to those having the following relation with EntitySet.

| From<br>   | To<br>     |
|:-- |:-- |
| 0 .. 1<br> | 0 .. 1<br> |
| 0 .. 1<br> | 1<br>      |
| 0 .. 1<br> | *<br>      |
| 1<br>      | 0 .. 1<br> |
| 1<br>      | 1<br>      |
| 1<br>      | *<br>      |
| *<br>      | 0 .. 1<br> |
| *<br>      | 1<br>      |
| *<br>      | *<br>      |

#### Request Method

GET

#### Request Query

The following query parameters are available

| Query Name<br>    | Overview<br>                    | Effective Value<br>                                                                | Required<br> | Notes<br>                                                                                                                |
|:-- |:-- |:-- |:-- |:-- |
| p_cookie_peer<br> | Cookie Authentication Value<br> | The cookie authentication value returned from the server during authentication<br> | No<br>       | Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br> |

[\$select  Query](406_Select_Query.html)

[\$expand  Query](405_Expand_Query.html)

[\$format  Query](404_Format_Query.html)

[\$filter  Query](403_Filter_Query.html)

[\$inlinecount  Query](407_Inlinecount_Query.html)

[\$orderby  Query](400_Orderby_Query.html)

[\$top  Query](401_Top_Query.html)

[\$skip  Query](402_Skip_Query.html)

[Full-text Search (q) Query](408_Full_Text_Search_Query.html)

#### Request Header

| Header Name<br>   | Overview<br>                                                     | Effective Value<br>      | Required<br> | Notes<br>                                                                                          |
|:-- |:-- |:-- |:-- |:-- |
| Authorization<br> | Specifies authentication information in the OAuth 2.0 format<br> | Bearer {AccessToken}<br> | No<br>       | * Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br> |
| Accept<br>        | Specifies the response body format<br>                           | application/json<br>     | No<br>       | [application/json] by default<br>                                                                  |

#### Request Body

None

#### Request Sample

None

<br>

### Response

#### Response Code

200

#### Response Header

| Item Name<br>          | Overview<br>                      | Notes<br>                                               |
|:-- |:-- |:-- |
| Content-Type<br>       | Format of data to be returned<br> | <br>                                                    |
| DataServiceVersion<br> | OData version information<br>     | Return only when Entity can be created successfully<br> |

#### Response Body

The response is a JSON object, the correspondence between the key (name) and type defined in the object (subobject) and the value is as follows

| Object<br> | Name(Key)<br>   | Type<br>   | Value<br>                                                                                      |
|:-- |:-- |:-- |:-- |
| Root<br>   | d<br>           | object<br> | Object{1}<br>                                                                                  |
| {1}<br>    | results<br>     | array<br>  | Array object {2}<br>                                                                           |
| {2}<br>    | __metadata<br>  | object<br> | Object{3}<br>                                                                                  |
| {3}<br>    | uri<br>         | string<br> | URL to the resource that was created<br>                                                       |
| {3}<br>    | etag<br>        | string<br> | Etag value<br>                                                                                 |
| {3}<br>    | type<br>        | string<br> | EntityType name<br>                                                                            |
| {2}<br>    | __id<br>        | string<br> | EntityID(__id)<br>                                                                             |
| {2}<br>    | __published<br> | string<br> | Creation date (UNIX time)<br>                                                                  |
| {2}<br>    | __updated<br>   | string<br> | Update date (UNIX time)<br>                                                                    |
| {2}<br>    | {NP name}<br>   | string<br> | Object{4}<br>It is returned only when Link is connected. {NP name}: NavigationPropert name<br> |
| {4}<br>    | __deferred<br>  | object<br> | Object{5}<br>                                                                                  |
| {5}<br>    | uri<br>         | string<br> | uri of the resource that has the relationship<br>                                              |
| {1}<br>    | __count<br>     | string<br> | Get number of results in $inlinecount query<br>                                                |

In addition to the above, return the schema-set item or the dynamic item specified at the time of registration

##### Numerical treatment

##### Decimal value(Edm.Single)

* The handling when acquiring UserOData in JSON format is as follows

    * The value that the decimal part such as 10.0 becomes 0 is returned as an integer value

##### Numerical value(Edm.Double)

\*Double type handling in Personium follows the Java Double specification

* The handling when acquiring UserOData in JSON format is as follows

    * The value that the decimal part such as 10.0 becomes 0 is returned as an integer value

* About the value to be returned

    * When the input value at the time of registration is a number having accuracy of double precision or more, data is registered by being rounded to double precision

        * Internally, it is managed as a floating point number, but at the time of output, it converts it to fixed point number representation within the range where no information drop occurs and outputs it<br>
            When output fixed-point number is used for input, the same number of inputs and original number is guaranteed

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

```JSON
{
  "d": {
    "results": [
      {
        "__metadata": {
          "uri": "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/{EntityTypeName}('{EntityID}')",
          "etag": "W/\"2-1487645572476\"",
          "type": "UserData.{EntityTypeName}"
        },
        "__id": "{EntityID}",
        "__published": "/Date(1487645572476)/",
        "__updated": "/Date(1487645572476)/",
        "TestProperty": null,
        "_TestEntity": {
          "__deferred": {
            "uri": "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/{EntityTypeName}('{EntityID}')/{NavigationPropertyName}"
          }
        }
      }
    ]
  }
}
```

<br>

### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/{EntityTypeName}('{EntityID}')/{NavigationPropertyName}" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED