# ExtRole Registration

### Overview

register ExtRole

### Restrictions

* General Restrictions

    * None

* OData Restrictions

    * Accept in the request header is ignored
    * Always handles Content-Type in the request header as application/json
    * Only accepts the request body in the JSON format
    * Only application/json is supported for Content-Type in the request header and the JSON format for the response body
    * $formatQuery options ignored

### Required Privileges

auth

<br>

### Request

#### Request URL

```
/{CellName}/__ctl/ExtRole
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

| Item Name<br>           | Overview<br>                           | Effective Value<br>                                                                                                                                                                                                                                                                                                                   | Required<br> | Notes<br> |
|:-- |:-- |:-- |:-- |:-- |
| ExtRole<br>             | External Role Name(URL)<br>            | Number of digits: 1-1024<br>Follow URI format<br>scheme:http / https / urn<br>When associated with Box:/{Application CellName}/__role/__/{RoleName}<br>* However, if Schema information is not registered in Box, it is considered not to be associated with Box<br>When not associated with Box:/{CellName}/__role/__/{RoleName}<br> | Yes<br>      | <br>      |
| _Relation.Name<br>      | Relation name of relationbr            | Number of digits: 1 - 128<br>Character type: Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br> and +(plus) and :(colon)<br>However, the string cannot start with a underscore ("_") or colon (:)<br>Relation which registered by Relation register API<br>null<br>                                        | Yes<br>      | <br>      |
| _Relation._Box.Name<br> | Box Name aassociated wirh Relation<br> | Number of digits: 1 - 128<br>Character type: Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Box name associated with Relation registered by Relation registration API<br>null<br>                                                                                                                       | No<br>       | <br>      |

#### Request Sample

```JSON
{
  "ExtRole": "https://{UnitFQDN}/{CellName}/__role/__/RoleName",
  "_Relation.Name": "{RelationName}",
  "_Relation._Box.Name": "{BoxName}"
}
```

<br>

### Response

#### Response Code

201

#### Response Header

| Item Name<br>                   | Overview<br>                                     | Notes<br>                                          |
|:-- |:-- |:-- |
| Access-Control-Allow-Origin<br> | Cross domain communication permission header<br> | Return value fixed to "*"<br>                      |
| X-Personium-Version<br>         | API version that the request is processed<br>    | Version of the API used to process the request<br> |

#### OData Response Header

| Item Name<br>          | Overview<br>                             | Notes<br> |
|:-- |:-- |:-- |
| Content-Type<br>       | Format of data to be returned<br>        | <br>      |
| Location<br>           | URL to the resource that was created<br> | <br>      |
| DataServiceVersion<br> | OData version<br>                        | <br>      |
| ETag<br>               | Resource version information<br>         | <br>      |

#### Response Body

The response is a JSON object, the correspondence between the key (name) and type defined in the object (subobject) and the value are as follows

| Object<br> | Item Name<br>   | Data Type<br> | Notes<br>                                       |
|:-- |:-- |:-- |:-- |
| Root<br>   | d<br>           | object<br>    | Object{1}<br>                                   |
| {1}<br>    | results<br>     | array<br>     | Array object {2}<br>                            |
| {2}<br>    | __metadata<br>  | object<br>    | Object{3}<br>                                   |
| {3}<br>    | uri<br>         | string<br>    | URL to the resource that was created<br>        |
| {3}<br>    | etag<br>        | string<br>    | Etag value<br>                                  |
| {2}<br>    | __published<br> | string<br>    | Creation date (UNIX time)<br>                   |
| {2}<br>    | __updated<br>   | string<br>    | Update date (UNIX time)<br>                     |
| {1}<br>    | __count<br>     | string<br>    | Get number of results in $inlinecount query<br> |

#### ExtRole specific response body

| Object<br> | Item Name<br>           | Data Type<br> | Notes<br>                              |
|:-- |:-- |:-- |:-- |
| {3}<br>    | type<br>                | string<br>    | CellCtl.ExtRole<br>                    |
| {2}<br>    | ExtRole<br>             | string<br>    | External Role URL<br>                  |
| {2}<br>    | _Relation.Name<br>      | string<br>    | Relation Name<br>                      |
| {2}<br>    | _Relation._Box.Name<br> | string<br>    | Box Name aassociated wirh Relation<br> |

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

```JSON
{
  "d": {
    "results": {
      "__published": "/Date(1371010428917)/",
      "_Relation._Box.Name": null,
      "__updated": "/Date(1371010428917)/",
      "_Relation.Name": "{RelationName}",
      "__metadata": {
        "etag": "W/\"1-1371010428917\"",
        "type": "CellCtl.ExtRole",
        "uri": "https://{UnitFQDN}/{CellName}/__ctl/ExtRole(ExtRole='https://{UnitFQDN}/{CellName}/__role/__/roletest',_Relation.Name='relation',_Relation._Box.Name=null)"
      },
      "ExtRole": "https://{UnitFQDN}/{CellName}/__role/__/RoleName"
    }
  }
}
{
  "d": {
    "results": {
      "__metadata": {
        "uri": "https://{UnitFQDN}/{CellName}/__ctl/ExtRole(ExtRole='https://{UnitFQDN}/{CellName}/__role/__/{ExtRoleName}',_Relation.Name='{RelationName}',_Relation._Box.Name='{BoxName}')",
        "etag": "W/\"1-1486717404966\"",
        "type": "CellCtl.ExtRole"
      },
      "ExtRole": "https://{UnitFQDN}/{CellName}/__role/__/{ExtRoleName}",
      "_Relation.Name": "{RelationName}",
      "_Relation._Box.Name": "{BoxName}",
      "__published": "/Date(1486717404966)/",
      "__updated": "/Date(1486717404966)/"
    }
  }
}
```

<br>

### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/ExtRole" -X POST -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{ "ExtRole": "https://{UnitFQDN}/{CellName}/__role/__/{ExtRoleName}", "_Relation.Name": "{RelationName}", "_Relation._Box.Name": "{BoxName}"}'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED