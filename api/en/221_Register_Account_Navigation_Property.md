# Registration via Account\_NavProp

### Overview

register Role via Account Navigation Property

### Required Privileges

write

### Restrictions

* Always handles Content-Type in the request header as application/json
* Only accepts the request body in the JSON format
* Only application/json is supported for Content-Type in the request header and the JSON format for the response body
* Response body data is not ensured if atom or xml is specified in the $format query option, although it does not result in an error

<br>

### Request

#### Request URL

##### navigationProperty to Role

```
/{CellName}/__ctl/Account(Name='{AccountName}')/_Role
```

or 

```
/{CellName}/__ctl/Account('{AccountName}')/_Role
```

#### Request Method

POST

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
| Content-Type<br>           | Specifies the request body format<br>                            | application/json<br>                                                                                       | No<br>       | [application/json] by default<br>                                                                                 |
| Accept<br>                 | Specifies the response body format<br>                           | application/json<br>                                                                                       | No<br>       | [application/json] by default<br>                                                                                 |

#### Request Body

When registering Role

| Item Name<br> | Overview<br> | Effective Value<br>                                                                                                                                                             | Required<br> | Notes<br> |
|:-- |:-- |:-- |:-- |:-- |
| Name          | Account Name | Character type: Half size alphanumeric characters and following half-width symbol (-_!$*=^`{|}~.@) <br>However, the first character can not be specified as the first character | Yes          | <br>      |

#### Request Sample

```JSON
{
  "Name": "{AccountName}"
}
```

<br>

### Response

#### Response Code

201

#### Response Header

| Item Name<br>                   | Overview<br>                                     | Notes<br>                                          |
|:-- |:-- |:-- |
| X-Personium-Version<br>         | API version that the request is processed<br>    | Version of the API used to process the request<br> |
| Access-Control-Allow-Origin<br> | Cross domain communication permission header<br> | Return value fixed to "*"<br>                      |
| Content-Type<br>                | Format of data to be returned<br>                | <br>                                               |
| Location<br>                    | URL to the resource that was created<br>         | <br>                                               |
| ETag<br>                        | Resource version information<br>                 | <br>                                               |
| DataServiceVersion<br>          | OData version<br>                                | <br>                                               |

#### Response Body

| Item Name<br>                       | Overview<br>                             | Notes<br> |
|:-- |:-- |:-- |
| d<br>                               | <br>                                     | <br>      |
| d / results<br>                     | <br>                                     | <br>      |
| d / results / __published<br>       | Created date<br>                         | <br>      |
| d / results / __updated<br>         | Updated date<br>                         | <br>      |
| d / results / __metadata<br>        | <br>                                     | <br>      |
| d / results / __metadata / etag<br> | ETag value<br>                           | <br>      |
| d / results / __metadata / uri<br>  | URL to the resource that was created<br> | <br>      |
| d / results / __metadata / type<br> | EntityType<br>                           | <br>      |
| d / results / Name<br>              | Role Name<br>                            | <br>      |
| d / results / _Box.Name<br>         | Box name to be related<br>               | <br>      |

##### When registered Account

Account specific response body

| Object<br> | Item Name<br> | Data Type<br> | Notes<br>           |
|:-- |:-- |:-- |:-- |
| {2}<br>    | Name<br>      | string<br>    | Account name<br>    |
| {2}<br>    | Cell<br>      | string<br>    | null<br>            |
| {2}<br>    | Type<br>      | string<br>    | basic<br>           |
| {3}<br>    | Type<br>      | string<br>    | CellCtl.Account<br> |

#### Response Sample

```JSON
{
  "d": {
    "results": {
      "Name": "{AccountName}",
      "__published": "/Date(1349355810698)/",
      "Cell": null,
      "__updated": "/Date(1349355810698)/",
      "Type": "basic",
      "__metadata": {
        "etag": "1-1349355810698",
        "type": "CellCtl.Account",
        "uri": "https://{UnitFQDN}/{CellName}/__ctl/Account('{AccountName}')"
      }
    }
  }
}   
```

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

### cURL Command

##### Register via Account and Role navigationProperty

```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/Account('acount_name')/_Role" -X POST -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"Name":"{RoleName}"}'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED