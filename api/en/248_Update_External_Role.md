# Extcell Update

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
/{CellName}/__ctl/ExtRole(ExtRole='{ExtRoleURL}',_Relation.Name='{RelationName}',_Relation._Box.Name='{BoxName}')
```

or 

```
/{CellName}/__ctl/ExtRole(ExtRole='{ExtRoleURL}',_Relation.Name='{RelationName}')
```

If the \_Relation.\_Box.Name parameter is omitted, it is assumed that null is specified

#### Request Method

PUT

#### Request Query

| Query Name<br>    | Overview<br>                    | Effective Value<br>                                                                | Required<br> | Notes<br>                                                                                                                |
|:-- |:-- |:-- |:-- |:-- |
| p_cookie_peer<br> | Cookie Authentication Value<br> | The cookie authentication value returned from the server during authentication<br> | No<br>       | Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br> |

#### Request Header

| Header Name<br>            | Overview<br>                                                     | Effective Value<br>                                                                                        | Required<br> | Notes<br>                                                                                                         |
|:-- |:-- |:-- |:-- |:-- |
| X-HTTP-Method-Override<br> | Method override function<br>                                     | User-defined<br>                                                                                           | No<br>       | If you specify this value when requesting with the POST method, the specified value will be used as a method.<br> |
| X-Override<br>             | Header override function<br>                                     | ${OverwrittenHeaderName}:${Value}<br>                                                                      | No<br>       | Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.<br>       |
| X-Personium-RequestKey<br> | RequestKey field value output in the event log<br>               | Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br> | No<br><br>   | PCS-${UNIXtime} by default<br>Supported in V 1.1.7 and later<br>                                                  |
| Authorization<br>          | Specifies authentication information in the OAuth 2.0 format<br> | Bearer {AccessToken}<br>                                                                                   | No<br>       | * Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br>                |
| Content-Type<br>           | Specifies the request body format<br>                            | application/json<br>                                                                                       | No<br>       | [application/json] by default<br>                                                                                 |
| Accept<br>                 | Specifies the response body format<br>                           | application/json<br>                                                                                       | No<br>       | [application/json] by default<br>                                                                                 |
| If-Match<br>               | Specifies the target ETag value<br>                              | ETag value<br>                                                                                             | No<br>       | [*] by default<br>                                                                                                |

#### Request Body

| Item Name<br>           | Overview<br>                           | Effective Value<br>                                                                                                                                                                                                                                                                                                                   | Required<br> | Notes<br> |
|:-- |:-- |:-- |:-- |:-- |
| ExtRole<br>             | External Role Name(URL)<br>            | Number of digits: 1-1024<br>Follow URI format<br>scheme:http / https / urn<br>When associated with Box:/{Application CellName}/__role/__/{RoleName}<br>* However, if Schema information is not registered in Box, it is considered not to be associated with Box<br>When not associated with Box:/{CellName}/__role/__/{RoleName}<br> | Yes<br>      | <br>      |
| _Relation.Name<br>      | Relation name of relationbr            | Number of digits: 1 - 128<br>Character type: Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br> and +(plus) and :(colon)<br>However, the string cannot start with a underscore ("_") or colon (:)<br>Relation which registered by Relation register API<br>null<br>                                        | Yes<br>      | <br>      |
| _Relation._Box.Name<br> | Box Name aassociated wirh Relation<br> | Number of digits: 1 - 128<br>Character type: Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Box name associated with Relation registered by Relation registration API<br>null<br>                                                                                                                       | No<br>       | <br>      |

#### Request Sample

```JSON
{
  "ExtRole": "https://{UnitFQDN}/{CellName}/__role/__/roletest",
  "_Relation.Name": "{RelationName}",
  "_Relation._Box.Name": "{BoxName}"
}
```

<br>

### Response

#### Response Code

204

#### Response Body

None

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

### cURL Command

```sh
curl curl "https://{UnitFQDN}/{CellName}/__ctl/ExtRole(ExtRole='https%3A%2F%2F{UnitFQDN}%2F{CellName}%2F__role%2F__%2F{ExtRoleName}',_Relation.Name='{RelationName}',_Relation._Box.Name='{BoxName}')" -X PUT -i -H 'If-Match: *' -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{ "ExtRole": "https://{UnitFQDN}/{CellName}/__role/__/{ExtRoleName}", "_Relation.Name":"{RelationName}","_Relation._Box.Name": "{BoxName}"}'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED