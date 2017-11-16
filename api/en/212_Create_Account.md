# Account Register

### Overview

register Account

### Required Privileges

auth

### Restrictions

* Always handles Content-Type in the request header as application/json
* Only accepts the request body in the JSON format
* Only application/json is supported for Content-Type in the request header and the JSON format for the response body
* Response body data is not ensured if atom or xml is specified in the $format query option, although it does not result in an error

<br>

### Request

#### Request URL

```
/{CellName}/__ctl/Account
```

#### Request Method

POST

#### Request Query

| Query Name<br>    | Overview<br>                    | Effective Value<br>                                                                | Required<br> | Notes<br>                                                                                                                |
|:-- |:-- |:-- |:-- |:-- |
| p_cookie_peer<br> | Cookie Authentication Value<br> | The cookie authentication value returned from the server during authentication<br> | No<br>       | Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br> |

#### Request Header

| Header Name<br>            | Overview<br>                                                     | Effective Value<br>                                                                                        | Required<br> | Notes<br>                                                                                                                   |
|:-- |:-- |:-- |:-- |:-- |
| X-HTTP-Method-Override<br> | Method override function<br>                                     | User-defined<br>                                                                                           | No<br>       | If you specify this value when requesting with the POST method, the specified value will be used as a method.<br>           |
| X-Override<br>             | Header override function<br>                                     | ${OverwrittenHeaderName}:${Value}<br>                                                                      | No<br>       | Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.<br>                 |
| X-Personium-RequestKey<br> | RequestKey field value output in the event log<br>               | Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br> | No<br>       | PCS-${UNIXtime} by default<br>Supported in V 1.1.7 and later<br>                                                            |
| Authorization<br>          | Specifies authentication information in the OAuth 2.0 format<br> | Bearer {AccessToken}<br>                                                                                   | No<br>       | * Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br>                          |
| Content-Type<br>           | Specifies the request body format<br>                            | application/json<br>                                                                                       | No<br>       | [application/json] by default<br>                                                                                           |
| Accept<br>                 | Specifies the response body format<br>                           | application/json<br>                                                                                       | No<br>       | [application/json] by default<br>                                                                                           |
| X-Personium-Credential<br> | Password<br>                                                     | String<br>                                                                                                 | No<br>       | Number of character:6 - 92<br>Character type: Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br> |

#### Request Body

| Header Name<br>       | Overview<br>                 | Effective Value<br>                                                                                                                                                                                                   | Required<br> | Notes<br>          |
|:-- |:-- |:-- |:-- |:-- |
| Name<br>              | Account Name<br>             | Number of digits: 1 - 128<br>Character type: Half size alphanumeric characters and following half-width symbol<br>-_!$*=^`{|}~.@<br>However, the first character cannot be a half-width symbol<br>                    | Yes<br>      | <br>               |
| Type<br>              | Account Type<br>             | basic(ID/PW authentication)<br>oidc:google(Google OpenID Connect authentication)                                                                                                                                      | No<br>       | default: basic<br> |
| LastAuthenticated<br> | last authentication date<br> | It is specified as a character string in the format of Date ([time of long type])<br>The valid value of [time of long type] is -6847804800000(1753-01-01T00:00:00.000Z)-253402300799999(9999-12-31T23:59:59.999Z)<br> | No<br>       | default: null<br>  |

#### Request Sample

ID/PWaccount for authentication

```JSON
{
  "Name": "{AccountName}"
}
```

Googleaccount for authentication

```JSON
{
  "Name": "{AccountName}","Type":"oidc:google"
}
```

ID/PW authentication +Googleaccount for authentication

```JSON
{
  "Name": "{AccountName}","Type":"basic oidc:google"
}
```

<br>

### Response

#### Response Code

201

#### Response Header

| Header Name<br>                 | Overview<br>                                     | Notes<br>                                          |
|:-- |:-- |:-- |
| Content-Type<br>                | Format of data to be returned<br>                | <br>                                               |
| Location<br>                    | URL to the resource that was created<br>         | <br>                                               |
| DataServiceVersion<br>          | OData version<br>                                | <br>                                               |
| ETag<br>                        | Resource version information<br>                 | <br>                                               |
| Access-Control-Allow-Origin<br> | Cross domain communication permission header<br> | Return value fixed to "*"<br>                      |
| X-Personium-Version<br>         | API version that the request is processed<br>    | Version of the API used to process the request<br> |

#### Response Body

| Object<br> | Item Name<br>   | Type<br>   | Notes<br>                                       |
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

| Object<br> | Item Name<br>         | Type<br>   | Notes<br>            |
|:-- |:-- |:-- |:-- |
| {3}<br>    | type<br>              | string<br> | CellCtl.Account<br>  |
| {2}<br>    | Name<br>              | string<br> | Account name<br>     |
| {2}<br>    | LastAuthenticated<br> | string<br> | default: null<br>    |
| {2}<br>    | Type<br>              | string<br> | default: "basic"<br> |
| {2}<br>    | Cell<br>              | string<br> | default: null<br>    |

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

ID/PWaccount for authentication

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
      "__updated": "/Date(1486462510467)/"
    }
  }
}
```

Googleaccount for authentication

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
      "Type": "oidc:google",
      "Cell": null,
      "__published": "/Date(1486462510467)/",
      "__updated": "/Date(1486462510467)/"
    }
  }
}
```

ID/PW authentication +Googleaccount for authentication

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
      "Type": "basic oidc:google",
      "Cell": null,
      "__published": "/Date(1486462510467)/",
      "__updated": "/Date(1486462510467)/"
    }
  }
}
```

<br>

### cURL Command

ID/PWaccount for authentication

```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/Account" -X POST -i -H 'X-Personium-Credential:password' -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"Name":"{AccountName}"}'
```

Googleaccount for authentication

```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/Account" -X POST -i -H 'X-Personium-Credential:password' -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"Name":"{AccountName}","Type":"oidc:google"}'
```

ID/PW authentication +Googleaccount for authentication

```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/Account" -X POST -i -H 'X-Personium-Credential:password' -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"Name":"{AccountName}","Type":"basic oidc:google"}'
```

<br>

###### Copyright 2017 FUJITSU LIMITED