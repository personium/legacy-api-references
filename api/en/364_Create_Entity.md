# Create Entity

### Overview

Create Entity of user data.

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
    * *When setting in request body and setting with DefaultValue (__published, __ updated is the latter timing)
    * For EntityType, you can create up to 400 DynamicProperty / DeclaredProperty / ComplexTypeProperty

<br>

### Request

#### Request URL

```
/{CellName}/{BoxName}/{ODataCollecitonName}/{EntityTypeName}
```

| Path<br>                  | Overview<br>        |
|:-- |:-- |
| {CellName}<br>            | Cell Name<br>       |
| {BoxName}<br>             | Box Name<br>        |
| {ODataCollecitonName}<br> | Collection Name<br> |
| {EntityTypeName}<br>      | EntityType name<br> |

#### Request Method

POST

#### Request Query

##### Common Request Query

| Query Name<br>    | Overview<br>                    | Effective Value<br>                                                                | Required<br> | Notes<br>                                                                                                                |
|:-- |:-- |:-- |:-- |:-- |
| p_cookie_peer<br> | Cookie Authentication Value<br> | The cookie authentication value returned from the server during authentication<br> | No<br>       | Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br> |

##### OData Common Request Query

None

#### Request Header

| Header Name<br>   | Overview<br>                                                     | Effective Value<br>      | Required<br> | Notes<br>                                                                                          |
|:-- |:-- |:-- |:-- |:-- |
| Authorization<br> | Specifies authentication information in the OAuth 2.0 format<br> | Bearer {AccessToken}<br> | No<br>       | * Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br> |
| Accept<br>        | Specifies the response body format<br>                           | application/json<br>     | No<br>       | [application/json] by default<br>Not compatible<br>                                                |
| Content-Type<br>  | Specifies the request body format<br>                            | application/json<br>     | No<br>       | When omitted, treat it as [application/json] <br>Not compatible<br>                                |

#### Request Body

| Item Name<br> | Overview<br> | Effective Value<br>                                                                                                                                                                                                       | Required<br> | Notes<br>                                                                            |
|:-- |:-- |:-- |:-- |:-- |
| __id<br>      | EntityID<br> | Number of digits: 1-200<br>Character type: Half size alphanumeric characters - (hyphen) and _ (underscore): (colon)<br> *However, - (hyphen), _ (underscore) and: (colon) can not be specified as the first character<br> | No<br>       | If not specified a unique ID will be assigned<br>Valid value check not supported<br> |

##### Property

Set up schema-defined properties and dynamic (schema-undefined) properties, up to 400 properties in total<br>
Contains the number of properties defined by ComplexType in the above

##### Schema-defined properties

| Item Name<br>                           | Overview<br>           | Effective Value<br>                                 | Required<br>                   | Notes<br> |
|:-- |:-- |:-- |:-- |:-- |
| Property associated with EntityType<br> | User defined item <br> | Based on DefaultValue of default value Property<br> | Based on Property Nullable<br> | <br>      |

##### Valid value of value of schema-defined property

| Data type<br>     | Effective Value<br>                                                                                                                                                                                                                                                                                                                                                                                                            |
|:-- |:-- |
| String<br>        | Number of digits: 0-51200 byte<br>Character type: When a control code is used as a value of a character string, return it in an escaped state at the time of acquisition<br>When "\" is used, it must be specified with "\\"<br>When an integer value, a decimal value, a boolean value, or a date type value is set in a property of a character string type, it is converted into a character string type and registered<br> |
| Integer value<br> | -2147483648 - 2147483647<br>                                                                                                                                                                                                                                                                                                                                                                                                   |
| Decimal point<br> | Number of digits in integer part: 1-5 digits<br>Number of digits in decimal part: 1-5 digits<br>                                                                                                                                                                                                                                                                                                                               |
| Boolean value<br> | true / false / null(treat null as false)<br>                                                                                                                                                                                                                                                                                                                                                                                   |
| Date<br>          | It is specified as a character string in the format of Date ([time of long type])<br>The valid value of [time of long type] is -6847804800000(1753-01-01T00:00:00.000Z)-253402300799999(9999-12-31T23:59:59.999Z)<br>In addition, you can specify the following as reserved words<br>SYSUTCDATETIME (): server time<br>                                                                                                        |

#### Dynamic (schema undefined) property

Set up schema-defined properties and dynamic (schema-undefined) properties, up to 400 properties in total<br>
Contains the number of properties defined by ComplexType in the above

##### Valid value of dynamic property's key

| Data type<br> | Effective Value<br>                                                                                                                                                                                                                                                                                                |
|:-- |:-- |
| String<br>    | Number of digits: 1-128 :<br>Character type: Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>_However, - (hyphen) and _ (underscore)_  can not be specified as the first character <br>_published, _updated is a reserved word, so it is not possible to specify the request body<br> |

##### Valid value of value of dynamic property

Same as valid value of value of schema-defined property<br>
Array, associative array can not be specified

#### Request Sample

```JSON
{"__id": "100-1_20101108-111352093","animalId": "100-1","name": "episode","startedAt": "2010-11-08","episodeType": "care","endedAt": "","outcome": "During treatment"}
```

```JSON
{"__id": "100-1_20101108-111352093","animalId": "100-1","name": "episode","update": "SYSUTCDATETIME()"}
```

```JSON
{"__id": "100-1_20101108-111352093","animalId": "100-1","name": "episode","update": "\/Date(1350451322147)\/"}
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

##### Common

The response is a JSON object, the correspondence between the key (name) and type defined in the object (subobject) and the value are as follows

| Object<br> | Name(Key)<br>   | Type<br>   | Value<br>                                       |
|:-- |:-- |:-- |:-- |
| Root<br>   | d<br>           | object<br> | Object{1}<br>                                   |
| {1}<br>    | results<br>     | array<br>  | Array object {2}<br>                            |
| {2}<br>    | __metadata<br>  | object<br> | Object{3}<br>                                   |
| {3}<br>    | uri<br>         | string<br> | URL to the resource that was created<br>        |
| {3}<br>    | etag<br>        | string<br> | Etag value<br>                                  |
| {3}<br>    | type<br>        | string<br> | UserData.{EntityTypeName}<br>                   |
| {2}<br>    | __id<br>        | string<br> | EntityID(__id)<br>                              |
| {2}<br>    | __published<br> | string<br> | Creation date (UNIX time)<br>                   |
| {2}<br>    | __updated<br>   | string<br> | Update date (UNIX time)<br>                     |
| {1}<br>    | __count<br>     | string<br> | Get number of results in $inlinecount query<br> |

In addition to the above, return dynamic user data specified at the time of registration

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

```JSON
{
  "d": {
    "results": {
      "__metadata": {
        "uri": "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/{EntityTypeName}('{EntityID}')",
        "etag": "W/\"1-1487662179733\"",
        "type": "UserData.{EntityTypeName}"
      },
      "__id": "{EntityID}",
      "__published": "/Date(1487662179733)/",
      "__updated": "/Date(1487662179733)/",
      "PetName": null,
      "animalId": "100-1",
      "name": "episode",
      "startedAt": "2010-11-08",
      "episodeType": "care",
      "endedAt": "",
      "outcome": "During treatment"
    }
  }
}
```

<br>

### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/{EntityTypeName}" -X POST -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"__id": "{EntityID}","animalId": "100-1","name": "episode","startedAt": "2010-11-08","episodeType": "care","endedAt": "","outcome": "During treatment"}'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED