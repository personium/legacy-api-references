# ComplexTypeProperty Registration

### Overview

Define the ComplexTypeProperty specified for user data

### Restrictions

* Always handles Content-Type in the request header as application/json
* Only accepts the request body in the JSON format
* Only application/json is supported for Content-Type in the request header and the JSON format for the response body
* Response body data is not ensured if atom or xml is specified in the $format query option, although it does not result in an error
* Individual restrictions

    * When user data of EntityType using the target ComplexType exists, it can be registered only when Nullable is true
    * Registerable only when ComplexType hierarchy number is within 5 hierarchies
    * Validity check of DefaultValue of Edm.DateTime is not properly done

* Restrictions stipulated as Personium specifications

    * For EntityType, you can create up to 400 DynamicProperty / DeclaredProperty / ComplexTypeProperty
    * Maximum number of registered types of ComplexTypeProperty of each hierarchy can be registered up to the following number of SimpleType
    * 2nd level: 50 pieces
    * 3rd level: 30 pieces
    * 4th level: 10 pieces
    * The maximum number of registered types of ComplexTypeProperty of each hierarchy can be registered up to the following number
    * 1st level: 20 pieces
    * 2nd level: 5 pieces
    * 3rd level: 2 pieces
    * 4th level: 0 pieces

### Required Privileges

alter-schema

<br>

### Request

#### Request URL

```
/{CellName}/{BoxName}/{ODataCollecitonName}/$metadata/ComplexTypeProperty
```

#### Request Method

POST

#### Request Query

| Query Name<br>    | Overview<br>                    | Effective Value<br>                                                                | Required<br> | Notes<br>                                                                                                                |
|:-- |:-- |:-- |:-- |:-- |
| p_cookie_peer<br> | Cookie Authentication Value<br> | The cookie authentication value returned from the server during authentication<br> | No<br>       | Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br> |

#### Request Header

##### Common Request Header

| Header Name<br>            | Overview<br>                                       | Effective Value<br>                                                                                        | Required<br> | Notes<br>                                                                                                                    |
|:-- |:-- |:-- |:-- |:-- |
| X-HTTP-Method-Override<br> | Method override function<br>                       | User-defined<br>                                                                                           | No<br>       | Specifying this value in a request with the POST method indicates that the specified value is used as the method<br>         |
| X-Override<br>             | Header override function<br>                       | ${OverwrittenHeaderName}:${Value}<br>                                                                      | No<br>       | The normal HTTP header value is overwritten. Specify multiple X-Override headers for the overwriting of multiple headers<br> |
| X-Personium-RequestKey<br> | RequestKey field value output in the event log<br> | Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br> | No<br>       | Supported in V 1.1.7 and later<br>                                                                                           |

##### OData Common Request Header

| Header Name<br>   | Overview<br>                                                     | Effective Value<br>      | Required<br> | Notes<br>                                                                                          |
|:-- |:-- |:-- |:-- |:-- |
| Authorization<br> | Specifies authentication information in the OAuth 2.0 format<br> | Bearer {AccessToken}<br> | No<br>       | * Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br> |

##### OData Create Request Header

| Header Name<br>  | Overview<br>                                 | Effective Value<br>  | Required<br> | Notes<br>                                         |
|:-- |:-- |:-- |:-- |:-- |
| Content-Type<br> | Specifies the request body format<br>        | application/json<br> | No<br>       | When omitted, treat it as [application/json] <br> |
| Accept <br>      | Specify the format of the response body <br> | application/json<br> | No<br>       | When omitted, treat it as [application/json] <br> |

#### Request Body

##### Format

JSON

| Item Name<br>         | Overview<br>                  | Effective Value<br>                                                                                                                                                                                                | Required<br> | Notes<br> |
|:-- |:-- |:-- |:-- |:-- |
| Name<br>              | ComplexTypeProperty name<br>  | Number of digits: 1 - 128<br>Character type: Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>However, the string cannot start with a single-byte hyphen ("-") or underscore ("_")<br> | Yes<br>      | <br>      |
| _ComplexType.Name<br> | ComplexType name attached<br> | Number of digits: 1 - 128<br>Character type: Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>However, the string cannot start with a single-byte hyphen ("-") or underscore ("_")<br> | Yes<br>      | <br>      |
| Type<br>              | Type definition<br>           | Edm.Boolean / Edm.String / Edm.Single / Edm.Int32 / Edm.DateTime / Registered ComplexType name<br>                                                                                                                 | Yes<br>      | <br>      |
| Nullable<br>          | Null value authorization<br>  | true / false<br>The default value is Null<br>                                                                                                                                                                      | No<br>       | <br>      |
| DefaultValue<br>      | Default value<br>             | See the table below<br>The default value is Null<br>                                                                                                                                                               | No<br>       | <br>      |
| CollectionKind<br>    | Array type<br>                | None / List<br>The default value is "None"<br>                                                                                                                                                                     | No<br>       | <br>      |

##### Valid values for DefaultValue

Valid values of DefaultValue differ depending on Type value (type definition), and also define items with different types of the following definitions with character strings

| Type Value<br>   | Effective Value<br>                                                                                                                                                                                                                                                                                                       |
|:-- |:-- |
| Edm.String<br>   | Number of digits: 0-51200 byte<br>When "\" is used, it must be specified with "\\"<br>                                                                                                                                                                                                                                    |
| Edm.Int32<br>    | -2147483648 - 2147483647<br>                                                                                                                                                                                                                                                                                              |
| Edm.Single<br>   | Number of digits in integer part: 1-5 digits<br>Number of digits in decimal part: 1-5 digits<br>                                                                                                                                                                                                                          |
| Edm.Boolean<br>  | true / false<br>                                                                                                                                                                                                                                                                                                          |
| Edm.DateTime<br> | It is specified as a character string in the format of Date ([time of long type])<br> The valid value of [time of long type] is -6847804800000(1753-01-01T00:00:00.000Z)-253402300799999(9999-12-31T23:59:59.999Z)<br>In addition, you can specify the following as reserved words<br> SYSUTCDATETIME (): server time<br> |

#### Request Sample

```JSON
{"Name": "{ComplexTypePropertyName}","_ComplexType.Name": "{ComplexTypeName}","Type": "Edm.String","Nullable": true,"DefaultValue": null,"CollectionKind": "None"}
```

<br>

### Response

#### Response Code

201

#### Response Header

##### Common Response Header

| Header Name<br>                 | Overview<br>                                     | Notes<br>                                          |
|:-- |:-- |:-- |
| Access-Control-Allow-Origin<br> | Cross domain communication permission header<br> | Return value fixed to "*"<br>                      |
| X-Personium-Version<br>         | API version that the request is processed<br>    | Version of the API used to process the request<br> |

##### OData Response Header

| Header Name<br>        | Overview<br>                             | Notes<br> |
|:-- |:-- |:-- |
| Content-Type<br>       | Format of data to be returned<br>        | <br>      |
| Location<br>           | URL to the resource that was created<br> | <br>      |
| DataServiceVersion<br> | OData version<br>                        | <br>      |
| ETag<br>               | Resource version information<br>         | <br>      |

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
| {2}<br>    | __published<br> | string<br> | Creation date (UNIX time)<br>                   |
| {2}<br>    | __updated<br>   | string<br> | Update date (UNIX time)<br>                     |
| {1}<br>    | __count<br>     | string<br> | Get number of results in $inlinecount query<br> |

##### Property specific response body

| Object<br> | Name(Key)<br>      | Type<br>    | Value<br>                              |
|:-- |:-- |:-- |:-- |
| {3}<br>    | type<br>           | string<br>  | ODataSvcSchema.ComplexTypeProperty<br> |
| {2}<br>    | Name<br>           | string<br>  | ComplexTypeProperty name<br>           |
| {2}<br>    | Type<br>           | string<br>  | Type definition<br>                    |
| {2}<br>    | Nullable<br>       | boolean<br> | Null value authorization<br>           |
| {2}<br>    | DefaultValue<br>   | string<br>  | Default value<br>                      |
| {2}<br>    | CollectionKind<br> | string<br>  | Array type<br>                         |

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

```JSON
{
  "d": {
    "results": {
      "__metadata": {
        "uri": "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/$metadata/ComplexTypeProperty(Name='{ComplexTypePropertyName}',_ComplexType.Name='{ComplexTypeName}')",
        "etag": "W/\"1-1487658277593\"",
        "type": "ODataSvcSchema.ComplexTypeProperty"
      },
      "Name": "{ComplexTypePropertyName}",
      "_ComplexType.Name": "{ComplexTypeName}",
      "Type": "Edm.String",
      "Nullable": true,
      "DefaultValue": null,
      "CollectionKind": "None",
      "__published": "/Date(1487658277593)/",
      "__updated": "/Date(1487658277593)/"
    }
  }
}
```

<br>

### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/\$metadata/ComplexTypeProperty" -X POST -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"Name": "{ComplexTypePropertyName}","_ComplexType.Name": "{ComplexTypeName}","Type": "Edm.String","Nullable": true,"DefaultValue": null,"CollectionKind": "None"}'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED