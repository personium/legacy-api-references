# Account Acquire

### Overview

Obtain existing Account information

### Required Privileges

auth-read

### Restrictions

* Always handles Content-Type in the request header as application/json
* Only accepts the request body in the JSON format
* Only application/json is supported for Content-Type in the request header and the JSON format for the response body
* Response body data is not ensured if atom or xml is specified in the $format query option, although it does not result in an error

<br>

### Request

#### Request URL

```
/{CellName}/__ctl/Account(Name='aoount_name')
```

or 

```
/{CellName}/__ctl/Account('{AccountName}')
```

#### Request Method

GET

#### Request Query

| Query Name<br>    | Overview<br>                    | Effective Value<br>                                                                | Required<br> | Notes<br>                                                                                                                |
|:-- |:-- |:-- |:-- |:-- |
| p_cookie_peer<br> | Cookie Authentication Value<br> | The cookie authentication value returned from the server during authentication<br> | No<br>       | Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br> |

[\$select  Query](406_Select_Query.html)

[\$expand  Query](405_Expand_Query.html)

[\$format  Query](404_Format_Query.html)

#### Request Header

| Header Name<br>            | Overview<br>                                                     | Effective Value<br>                                                                                        | Required<br> | Notes<br>                                                                                                                |
|:-- |:-- |:-- |:-- |:-- |
| X-HTTP-Method-Override<br> | Method override function<br>                                     | User-defined<br>                                                                                           | No<br>       | Specifying this value in a request with the POST method <br>indicates that the specified value is used as the method<br> |
| X-Override<br>             | Header override function<br>                                     | ${OverwrittenHeaderName}:${Value}<br>                                                                      | No<br>       | Overwrite normal HTTP header value. When overwriting multiple headers<br> specify multi X-Override headers<br>           |
| X-Personium-RequestKey<br> | RequestKey field value output in the event log<br>               | Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br> | No<br>       | PCS-${UNIXtime} by default<br>Supported in V 1.1.7 and later<br>                                                         |
| Authorization<br>          | Specifies authentication information in the OAuth 2.0 format<br> | Bearer {AccessToken}<br>                                                                                   | No<br>       | * Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br>                       |
| Accept<br>                 | Specifies the response body format<br>                           | application/json<br>                                                                                       | No<br>       | [application/json] by default<br>                                                                                        |
| If-None-Match<br>          | Specifies the target ETag value<br>                              | ETag value<br>                                                                                             | Yes<br>      | Not compatible<br>                                                                                                       |

#### Request Body

None

#### Request Sample

None

<br>

### Response

#### Response Code

200

#### Response Header

None

#### Response Body

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

#### Account specific response body

| Object<br> | Item Name<br>         | Type<br>   | Notes<br>                 |
|:-- |:-- |:-- |:-- |
| {3}<br>    | type<br>              | string<br> | CellCtl.Account<br>       |
| {2}<br>    | Name<br>              | string<br> | Account name<br>          |
| {2}<br>    | LastAuthenticated<br> | string<br> | default: null<br>         |
| {2}<br>    | Type<br>              | string<br> | Initial value:"basic"<br> |
| {2}<br>    | Cell<br>              | string<br> | default: null<br>         |

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

```JSON
{
  "d": {
    "results": {
      "__metadata": {
        "uri": "https://{UnitFQDN}/{CellName}/__ctl/Account('{AccountName}')",
        "etag": "W/\"1-1486462510467\"",
        "type": "CellCtl.Account"
      },
      "Name": "{AccountName}",
      "LastAuthenticated": null,
      "Type": "basic",
      "Cell": null,
      "__published": "/Date(1486462510467)/",
      "__updated": "/Date(1486462510467)/",
      "_Role": {
        "__deferred": {
          "uri": "https://{UnitFQDN}/{CellName}/__ctl/Account('{AccountName}')/_Role"
        }
      },
      "_ReceivedMessageRead": {
        "__deferred": {
          "uri": "https://{UnitFQDN}/{CellName}/__ctl/Account('{AccountName}')/_ReceivedMessageRead"
        }
      }
    }
  }
}
```

<br>

### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/Account('{AccountName}')" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED