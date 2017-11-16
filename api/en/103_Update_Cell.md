# Cell Update

### Overview

Updating existing cell information

### Required Privileges

Only unit users permitted

### Restrictions

* General Restrictions

    * None

* OData Restrictions

    * Always handles Content-Type in the request header as application/json
    * Only accepts the request body in the JSON format
    * Only application/json is supported for Content-Type in the request header and the JSON format for the response body
    * Response body data is not ensured if atom or xml is specified in the $format query option, although it does not result in an error

* OData Update Restrictions<

    * If-None-Match header is ignored
    * Name validation unimplemented

<br>

### Request

#### Request URL

```
/__ctl/Cell(Name='{CellName}')
```

or

```
/__ctl/Cell('{CellName}')
```

#### Request Method

PUT

#### Request Query

None

#### Request Header

##### Common Request Header

| Header Name<br>            | Overview<br>                                       | Effective Value<br>                                                                                        | Required<br> | Notes<br>                                                                                                                    |
|:-- |:-- |:-- |:-- |:-- |
| X-HTTP-Method-Override<br> | Method override function<br>                       | User-defined<br>                                                                                           | No<br>       | Specifying this value in a request with the POST method indicates that the specified value is used as the method<br>         |
| X-Override<br>             | Header override function<br>                       | ${OverwrittenHeaderName}:${Value}<br>                                                                      | No<br>       | The normal HTTP header value is overwritten. Specify multiple X-Override headers for the overwriting of multiple headers<br> |
| X-Personium-RequestKey<br> | RequestKey field value output in the event log<br> | Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br> | No<br>       | PCS-${UNIXtime} by default<br>Supported in V 1.1.7 and later<br>                                                             |

##### OData Common Request Header

| Header Name<br>   | Overview<br>                                                     | Effective Value<br>      | Required<br> | Notes<br>                                                                                          |
|:-- |:-- |:-- |:-- |:-- |
| Authorization<br> | Specifies authentication information in the OAuth 2.0 format<br> | Bearer {AccessToken}<br> | No<br>       | * Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br> |

##### OData Update Request Header

| Header Name<br>  | Overview<br>                           | Effective Value<br>  | Required<br> | Notes<br>                         |
|:-- |:-- |:-- |:-- |:-- |
| Content-Type<br> | Specifies the request body format<br>  | application/json<br> | No<br>       | [application/json] by default<br> |
| Accept<br>       | Specifies the response body format<br> | application/json<br> | No<br>       | [application/json] by default<br> |
| If-Match<br>     | Specifies the target ETag value<br>    | ETag value<br>       | No<br>       | [*] by default<br>                |

#### Request Body

##### Format

JSON

| Item Name<br> | Overview<br>  | Effective Value<br>                                                                                                                                                                                                        | Required<br> | Notes<br> |
|:-- |:-- |:-- |:-- |:-- |
| Name<br>      | Cell name<br> | Number of digits: 1 - 128<br>Character type: Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>However, the string cannot start with a single-byte hyphen ("-") or underscore ("_")<br>null<br> | Yes<br>      | <br>      |

#### Request Sample

```JSON
{"Name":"{CellName}"}
```

<br>

### Response

#### Response Code

204

#### Response Header

| Header Name<br>                 | Overview<br>                                     | Notes<br>                                          |
|:-- |:-- |:-- |
| ETag<br>                        | Resource version information<br>                 | <br>                                               |
| DataServiceVersion<br>          | OData version<br>                                | <br>                                               |
| Access-Control-Allow-Origin<br> | Cross domain communication permission header<br> | Return value fixed to "*"<br>                      |
| X-Personium-Version<br>         | Personium API version<br>                        | Version of the API used to process the request<br> |

#### Response Body

None

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

None

<br>

### cURL Command

```sh
curl "https://{UnitFQDN}/__ctl/Cell(Name='{CellName}')" -X PUT -i -H 'If-Match: *' -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"Name":"{CellName}"}'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED