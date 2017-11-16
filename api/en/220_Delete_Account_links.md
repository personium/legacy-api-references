# Account\_\$links delete

### Overview

Delete a list of OData resources associated with Account

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

link with Role

```
/{CellName}/__ctl/Account(Name='{AccountName}')/$links/_Role(Name='{RoleName}',_Box.Name='{BoxName}')
```

or 

```
/{CellName}/__ctl/Account(Name='{AccountName}')/$links/_Role(Name='{RoleName}')
```

or 

```
/{CellName}/__ctl/Account(Name='{AccountName}')/$links/_Role('{RoleName}')
```

or 

```
/{CellName}/__ctl/Account('{AccountName}')/$links/_Role(Name='{RoleName}',_Box.Name='{BoxName}')
```

or 

```
/{CellName}/__ctl/Account('{AccountName}')/$links/_Role(Name='{RoleName}')
```

or 

```
/{CellName}/__ctl/Account('{AccountName}')/$links/_Role('{RoleName}')
```

#### Request Method

DELETE

#### Request Query

| Query Name<br>    | Overview<br>                    | Effective Value<br>                                                                | Required<br> | Notes<br>                                                                                                                |
|:-- |:-- |:-- |:-- |:-- |
| p_cookie_peer<br> | Cookie Authentication Value<br> | The cookie authentication value returned from the server during authentication<br> | No<br>       | Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br> |

#### Request Header

| Header Name<br>            | Overview<br>                                                     | Effective Value<br>                                                                                        | Required<br> | Notes<br>                                                                                                         |
|:-- |:-- |:-- |:-- |:-- |
| X-HTTP-Method-Override<br> | Method override function<br>                                     | User-defined<br>                                                                                           | No<br>       | If you specify this value when requesting with the POST method, the specified value will be used as a method.<br> |
| X-Override<br>             | Header override function<br>                                     | ${OverwrittenHeaderName}:${Value}<br>                                                                      | No<br>       | Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.<br>       |
| X-Personium-RequestKey<br> | RequestKey field value output in the event log<br>               | Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br> | No<br>       | PCS-${UNIXtime} by default<br>Supported in V 1.1.7 and later<br>                                                  |
| Authorization<br>          | Specifies authentication information in the OAuth 2.0 format<br> | Bearer {AccessToken}<br>                                                                                   | No<br>       | * Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br>                |
| If-Match<br>               | Specifies the target ETag value<br>                              | ETag value<br>                                                                                             | No<br>       | <br>                                                                                                              |

#### Request Body

None

#### Request Sample

None

<br>

### Response

#### Response Code

204

#### Response Header

| Header Name<br>                 | Overview<br>                                     | Notes<br>                                          |
|:-- |:-- |:-- |
| DataServiceVersion<br>          | OData version<br>                                | <br>                                               |
| Access-Control-Allow-Origin<br> | Cross domain communication permission header<br> | Return value fixed to "*"<br>                      |
| X-Personium-Version<br>         | API version that the request is processed<br>    | Version of the API used to process the request<br> |

#### Response Body

None

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

None<br><br>

### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/Account('{AccountName}')/\$links/_Role('{RoleName}')" -X DELETE -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED