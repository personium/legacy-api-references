# Entity \$links deleted

### Overview

Delete \$links information with Entity of user data

### Required Privileges

None

### Restrictions

* Always handles Content-Type in the request header as application/json
* Only accepts the request body in the JSON format
* Only application/json is supported for Content-Type in the request header and the JSON format for the response body
* Response body data is not ensured if atom or xml is specified in the $format query option, although it does not result in an error

<br>

### Request

#### Request URL

##### \$links with user data

```
/{CellName}/{BoxName}/{CollectionName}/{EntityTypeName}('{EntityID}')/$links/_{EntityTypeName}('{EntityID}')
```

#### Request Method

DELETE

#### Request Query

##### Common Request Query

| Header Name<br>   | Overview<br>                    | Effective Value<br>                                                                | Required<br> | Notes<br>                                                                                                        |
|:-- |:-- |:-- |:-- |:-- |
| p_cookie_peer<br> | Cookie Authentication Value<br> | The cookie authentication value returned from the server during authentication<br> | No<br>       | Valid only when no Authorization header is specified Specify when using authentication information of cookie<br> |

##### OData Common Request Query

None

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

##### ODataDelete request header

| Header Name<br> | Overview<br>                        | Effective Value<br> | Required<br> | Notes<br>          |
|:-- |:-- |:-- |:-- |:-- |
| If-Match<br>    | Specifies the target ETag value<br> | ETag value<br>      | No<br>       | [*] by default<br> |

#### Request Body

##### Format

JSON

| Item Name<br> | Overview<br>                               | Effective Value<br>                                                            | Required<br> | Notes<br> |
|:-- |:-- |:-- |:-- |:-- |
| uri<br>       | URI of the OData resource to be linked<br> | Number of digits: 1-1024<br>Follow URI format<br>scheme:http / https / urn<br> | Yes<br>      | <br>      |

#### Request Sample

None<br><br>

### Response

#### Response Code

204

#### Response Header

##### Common Response Header

| Header Name<br>                 | Overview<br>                                     | Notes<br>                                          |
|:-- |:-- |:-- |
| Access-Control-Allow-Origin<br> | Cross domain communication permission header<br> | Return value fixed to "*"<br>                      |
| X-Personium-Version<br>         | API version that the request is processed<br>    | Version of the API used to process the request<br> |

##### OData \$links Response Header

| Header Name<br>        | Overview<br>      | Notes<br> |
|:-- |:-- |:-- |
| DataServiceVersion<br> | OData version<br> | <br>      |

#### Response Body

None

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

None

<br>

### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{CollectionName}/{EntityTypeName}('{EntityID}')/\$links/_{EntityTypeName}('{EntityID}')" -X DELETE -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED