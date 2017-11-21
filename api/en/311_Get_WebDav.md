# File acquisition

### Overview

Acquire specific WebDav information<br>
Depending on the ETag value specified in the If-None-Match header, the contents to be returned are different<br>
When specifying the range with the Range header, return the contents of the specified range

### Required Privileges

read

### Restrictions

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

GET

#### Request Query

| Query Name<br>    | Overview<br>                    | Effective Value<br>                                                                | Required<br> | Notes<br>                                                                                                                |
|:-- |:-- |:-- |:-- |:-- |
| p_cookie_peer<br> | Cookie Authentication Value<br> | The cookie authentication value returned from the server during authentication<br> | No<br>       | Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br> |

#### Request Header

##### Common Request Header

| Header Name<br>            | Overview<br>                                       | Effective Value<br>                                                                                        | Required<br> | Notes<br>                                                                                                         |
|:-- |:-- |:-- |:-- |:-- |
| X-HTTP-Method-Override<br> | Method override function<br>                       | User-defined<br>                                                                                           | No<br>       | If you specify this value when requesting with the POST method, the specified value will be used as a method.<br> |
| X-Override<br>             | Header override function<br>                       | ${OverwrittenHeaderName}:${Value}<br>                                                                      | No<br>       | Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.<br>       |
| X-Personium-RequestKey<br> | RequestKey field value output in the event log<br> | Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br> | No<br>       | Supported in V 1.1.7 and later<br>                                                                                |

##### Individual Request Header

| Header Name<br>   | Overview<br>                                                     | Effective Value<br>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Required<br> | Notes<br>                                                                                                                                                                                                                    |
|:-- |:-- |:-- |:-- |:-- |
| Authorization<br> | Specifies authentication information in the OAuth 2.0 format<br> | Bearer {AccessToken}<br>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | No<br>       | * Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br>                                                                                                                           |
| If-None-Match<br> | Specify the value of ETag<br>                                    | String<br>Specify the ETag value in the following format<br>"*"<br>or <br>"{Half-width number}-{Half-width number}"<br>                                                                                                                                                                                                                                                                                                                                                                                                                 | No<br>       | Example) When specifying the ETag value "1-1372742704414"<br> "1-1372742704414"<br>                                                                                                                                          |
| Range<br>         | Specify a range when acquiring a part of a resource<br>          | -Data type:String<br>[Assumption]<br>Start value of range specification starts at 0.When the size of the acquired file is Z (byte), the termination is Z-1 (byte)<br>1."Range: bytes={Half-width number}-{Half-width number}"<br>ex) "Range: bytes=10-20"<br>2."Range: bytes={Half-width number}-"<br> -> From the range start value to the end of the acquired file<br>ex) "Range: bytes=10-"<br>3."Range: bytes=-{Half-width number}"<br> -> Retrieve the specified minute from the end of the resource<br>ex) "Range: bytes=-20"<br> | No<br>       | -Specify a start value smaller than the size of the acquired file<br>-When the format of the specified Range header is incorrect, the Range header is ignored<br>[Limitations]<br>-Not supported for multi-part response<br> |

#### Request Body

None

#### Request Sample

None

<br>

### Response

#### Response Code

* If the If-None-Match header is not specified in the request, or if the ETag value of the If-None-Match header does not match the ETag of the resource stored in the WebDav in the request<br>
    (Including cases where the format of the specified ETag value is invalid)
* If the Range header is not specified in the request, or if the start value specified in the Range header in the request is larger than the termination value

| Code<br> | Message<br> | Overview<br>               |
|:-- |:-- |:-- |
| 200<br>  | OK<br>      | Successful acquisition<br> |

* When valid in the Range header of the request

| Code<br> | Message<br>         | Overview<br>                    |
|:-- |:-- |:-- |
| 206<br>  | Partial Content<br> | Partial acquisition success<br> |

* If the ETag value of the If-None-Match header in the request matches the ETag of the resource stored in WebDav

| Code<br> | Message<br>      | Overview<br>                          |
|:-- |:-- |:-- |
| 304<br>  | Not Modified<br> | The document has not been updated<br> |

#### Response Header

| Header Name<br>   | Overview<br>                                                                                    | Notes<br>                           |
|:-- |:-- |:-- |
| Content-Type<br>  | Content-Type of resource<br>                                                                    | Return only for status code 200<br> |
| ETag<br>          | Resource version information<br>                                                                | <br>                                |
| Accept-Ranges<br> | Indicates acceptance of byte range (range) request to resource<br>                              | <br>                                |
| Content-Range<br> | Indicates where the entity body corresponds to the part specified by the byte range request<br> | <br>                                |

In case of basic authentication error, return 400 + WWW-Authenticated: Basic header

#### Response Body

Return the contents of the file<br>
However, if the status code is 304, the response body is not returned<br>
If the status code is 206, the contents of the file specified in the Range header are returned

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

None

<br>

### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{CollectionName}" -X GET -i -H 'If-None-Match:"1-1372742704414"' -H 'Range:bytes=10-20 ' -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED