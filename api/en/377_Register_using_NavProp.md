# Entity NavProp registration

### Overview

Create entity Entity via Navigation Property.

### Required Privileges

write

### Restrictions

* Always handles Content-Type in the request header as application/json
* Only accepts the request body in the JSON format
* Only application/json is supported for Content-Type in the request header and the JSON format for the response body
* Response body data is not ensured if atom or xml is specified in the $format query option, although it does not result in an error
* User data restrictions

    * Property scope of Edm.DateTime type is not properly checked
    * Array of Edm.DateTime type is not supported
    * If SYSUTCDATETIME () is specified as the property of Edm.DateTime type, the set system time may be different
    * When setting in request body and setting with DefaultValue (__published, __ updated is the latter timing)
    * For EntityType, you can create up to 400 DynamicProperty / DeclaredProperty / ComplexTypeProperty

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
| {EntityTypeName}<br>         | EntityType name<br>         |
| {EntityID}<br>               | EntityID<br>                |
| {NavigationPropertyName}<br> | NavigationProperty name<br> |

NavigationProperty name that can be specified is limited to those having the following relation with EntitySet.

| From<br>   | To<br> |
|:-- |:-- |
| 0 .. 1<br> | 1<br>  |
| 0 .. 1<br> | *<br>  |
| 1<br>      | 1<br>  |
| 1<br>      | *<br>  |
| *<br>      | *<br>  |

#### Request Method

POST<br><br>

#### Request Query

##### Common Request Query

| Query Name<br>    | Overview<br>                    | Effective Value<br>                                                                | Required<br> | Notes<br>                                                                                                                |
|:-- |:-- |:-- |:-- |:-- |
| p_cookie_peer<br> | Cookie Authentication Value<br> | The cookie authentication value returned from the server during authentication<br> | No<br>       | Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br> |

#### Request Header

| Header Name<br>   | Overview<br>                                                     | Effective Value<br>      | Required<br> | Notes<br>                                                                                                        |
|:-- |:-- |:-- |:-- |:-- |
| Authorization<br> | Specifies authentication information in the OAuth 2.0 format<br> | Bearer {AccessToken}<br> | No<br>       | * Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br>Not tested<br> |
| Accept<br>        | Specifies the response body format<br>                           | application/json<br>     | No<br>       | [application/json] by default<br>Not compatible<br>                                                              |
| Content-Type<br>  | Specifies the request body format<br>                            | application/json<br>     | No<br>       | [application/json] by default<br>Not compatible<br>                                                              |

#### Request Body

| Item Name<br> | Overview<br> | Effective Value<br>                                                                                                      | Required<br> | Notes<br>                                                                            |
|:-- |:-- |:-- |:-- |:-- |
| __id<br>      | EntityID<br> | Number of digits: 1-200<br>Character type: Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br> | No<br>       | If not specified a unique ID will be assigned<br>Valid value check unimplemented<br> |

* _It is possible to register dynamic user data in addition to _id

    * Only character string can be set
    * If a numeric value, boolean value, null is specified, it is treated as a character string
    * Can not specify Hash value
    * Up to 400 user data can be specified
    * _published, _updated is a reserved word, so it is not possible to specify the request body

#### Request Sample

```JSON
{"__id": "100-1_20101108-111352093","animalId": "100-1","name": "episode","startedAt": "2010-11-08","episodeType": "care","endedAt": "","outcome": "During treatment"}
```

<br>

### Response

#### Response Code

201

#### Response Header

| Item Name<br>          | Overview<br>                       | Notes<br>                                               |
|:-- |:-- |:-- |
| Content-Type<br>       | Format of data to be returned<br>  | <br>                                                    |
| Location<br>           | Resource URL of created Entity<br> | Return only when Entity can be created successfully<br> |
| DataServiceVersion<br> | OData version information<br>      | Return only when Entity can be created successfully<br> |
| ETag<br>               | Resource version information<br>   | Return only when Entity can be created successfully<br> |

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
| {5}<br>    | uri<br>         | string<br> | uri of the resource that has the relationship<br>Not tested<br>                                |
| {1}<br>    | __count<br>     | string<br> | Get number of results in $inlinecount query<br>                                                |

Return items that were schema-set other than the above, or dynamic items specified at registration

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

```JSON
{
  "d": {
    "results": {
      "__metadata": {
        "uri": "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/{EntityTypeName}('100-1_20101108-111352093')",
        "etag": "W/\"1-1487929403469\"",
        "type": "UserData.{EntityTypeName}"
      },
      "__id": "100-1_20101108-111352093",
      "__published": "/Date(1487929403469)/",
      "__updated": "/Date(1487929403469)/",
      "PetName": null,
      "{NavigationPropertyName}": {
        "__deferred": {
          "uri": "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/{EntityTypeName}('100-1_20101108-111352093')/{NavigationPropertyName}"
        }
      }
    }
  }
}
```

<br>

### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/{EntityTypeName}('{EntityID}')/{NavigationPropertyName}" -X POST -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"__id": "100-1_20101108-111352093"}'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED