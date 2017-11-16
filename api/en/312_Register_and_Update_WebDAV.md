# File registration update

### Overview

Register / update WebDav information.

### Required Privileges

write

### Restrictions

#### Common restriction

None

#### WebDAV restriction

Unpublished

<br>

### Request

#### Request URL

```
/{CellName}/{BoxName}/{ResourcePath}
```

| Path<br>           | Overview<br>         | Notes<br>                                                                                                                         |
|:-- |:-- |:-- |
| {CellName}<br>     | Cell Name<br>        | <br>                                                                                                                              |
| {BoxName}<br>      | Box Name<br>         | <br>                                                                                                                              |
| {ResourcePath}<br> | Path to resource<br> | Valid values Number of digits:1-128<br>Usable character types<br>alphanumeric character, period(.), under score(_), hyphen(-)<br> |

#### Request Method

PUT

#### Request Query

##### Common Request Query

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

##### Individual Request Header

| Header Name<br>   | Overview<br>                                                     | Effective Value<br>      | Required<br> | Notes<br>                                                                                                                                                                                                                                                                                                                                   |
|:-- |:-- |:-- |:-- |:-- |
| Authorization<br> | Specifies authentication information in the OAuth 2.0 format<br> | Bearer {AccessToken}<br> | No<br>       | * Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br>                                                                                                                                                                                                                                          |
| If-Match<br>      | Specifies the target ETag value<br>                              | ETag value<br>           | No<br>       | [*] by default<br>                                                                                                                                                                                                                                                                                                                          |
| Content-Type<br>  | Specify the content format of the registration / update file<br> | String<br>               | No<br>       | When registering and updating in SWF format<br>Content-Type:application/x-shockwave-flash<br>When registering / updating in PDF format<br>Content-Type:application/pdf<br>When registering and updating in JPG format<br>Content-Type:image/jpeg<br>When registering and updating in js format<br>Content-Type:application/x-javascript<br> |

#### Request Body

| Overview<br>                                                                             | Effective Value<br>                                 | Required<br> | Notes<br> |
|:-- |:-- |:-- |:-- |
| Designate context information to be registered / updated as a request body in binary<br> | The method specified in the Content-Type header<br> | Yes<br>      | <br>      |

#### Request Sample

None

<br>

### Response

#### Response Code

| Code<br> | Message<br>    | Overview<br>                |
|:-- |:-- |:-- |
| 201<br>  | Created<br>    | Successful registration<br> |
| 204<br>  | No Content<br> | Update success<br>          |

#### Response Header

| Header Name<br>  | Overview<br>                      | Notes<br>                                                           |
|:-- |:-- |:-- |
| Content-Type<br> | Format of data to be returned<br> | Only when it failed at the time of update / creation, return it<br> |
| ETag<br>         | Resource version information<br>  | <br>                                                                |

#### Response Body

Only when it failed at the time of update / creation, return it

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

None

<br>

### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{ResourceName}" -X PUT -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{[File contents]}'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED