# Box Acquire List

### Overview

Obtain a list of existing Box information

### Required Privileges

box-read

### Restrictions

* Accept in the request header is ignored
* Always handles Content-Type in the request header as application/json
* Only accepts the request body in the JSON format
* Only application/json is supported for Content-Type in the request header and the JSON format for the response body
* $formatQuery options ignored

<br>

### Request

#### Request URL

```
/{CellName}/__ctl/Box
```

#### Request Method

GET

#### Request Query

The following query parameters are available

| Query Name<br>    | Overview<br>                    | Effective Value<br>                                                                | Required<br> | Notes<br>                                                                                                                |
|:-- |:-- |:-- |:-- |:-- |
| p_cookie_peer<br> | Cookie Authentication Value<br> | The cookie authentication value returned from the server during authentication<br> | No<br>       | Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br> |

[\$select  Query](406_Select_Query.html)

[\$expand  Query](405_Expand_Query.html)

[\$format  Query](404_Format_Query.html)

[\$filter  Query](403_Filter_Query.html)

[\$inlinecount  Query](407_Inlinecount_Query.html)

[\$orderby  Query](400_Orderby_Query.html)

[\$top  Query](401_Top_Query.html)

[\$skip  Query](402_Skip_Query.html)

[Full-text Search (q) Query](408_Full_Text_Search_Query.html)

#### Request Header

| Header Name<br>            | Overview<br>                                                     | Effective Value<br>                                                                                        | Required<br> | Notes<br>                                                                                                         |
|:-- |:-- |:-- |:-- |:-- |
| X-HTTP-Method-Override<br> | Method override function<br>                                     | User-defined<br>                                                                                           | No<br>       | If you specify this value when requesting with the POST method, the specified value will be used as a method.<br> |
| X-Override<br>             | Header override function<br>                                     | ${OverwrittenHeaderName}:${Value}override} $: $ {value}<br>                                                | No<br>       | Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.<br>       |
| X-Personium-RequestKey<br> | RequestKey field value output in the event log<br>               | Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br> | No<br>       | PCS-${UNIXtime} by default<br>Supported in V 1.1.7 and later<br>                                                  |
| Authorization<br>          | Specifies authentication information in the OAuth 2.0 format<br> | Bearer {AccessToken}<br>                                                                                   | No<br>       | * Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br>                |
| Accept<br>                 | Specifies the response body format<br>                           | application/json<br>                                                                                       | No<br>       | [application/json] by default<br>                                                                                 |

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

#### Box specific response body

| Object<br> | Item Name<br> | Data Type<br> | Notes<br>        |
|:-- |:-- |:-- |:-- |
| {3}<br>    | type<br>      | string<br>    | CellCtl.Box <br> |
| {2}<br>    | Name<br>      | string<br>    | Box Name<br>     |
| {2}<br>    | Schema<br>    | string<br>    | Schema Name<br>  |

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

```JSON
{
  "d": {
    "results": [
      {
        "__metadata": {
          "uri": "https://{UnitFQDN}/{CellName}/__ctl/Box('{BoxName}')",
          "etag": "W/\"1-1486368212581\"",
          "type": "CellCtl.Box"
        },
        "Name": "{BoxName}",
        "Schema": null,
        "__published": "/Date(1486368212581)/",
        "__updated": "/Date(1486368212581)/",
        "_Role": {
          "__deferred": {
            "uri": "https://{UnitFQDN}/{CellName}/__ctl/Box('{BoxName}')/_Role"
          }
        },
        "_Relation": {
          "__deferred": {
            "uri": "https://{UnitFQDN}/{CellName}/__ctl/Box('{BoxName}')/_Relation"
          }
        },
        "_ReceivedMessage": {
          "__deferred": {
            "uri": "https://{UnitFQDN}/{CellName}/__ctl/Box('{BoxName}')/_ReceivedMessage"
          }
        },
        "_SentMessage": {
          "__deferred": {
            "uri": "https://{UnitFQDN}/{CellName}/__ctl/Box('{BoxName}')/_SentMessage"
          }
        }
      },
      {
        "__metadata": {
          "uri": "https://{UnitFQDN}/{CellName}/__ctl/Box('{BoxName}')",
          "etag": "W/\"1-1486461000154\"",
          "type": "CellCtl.Box"
        },
        "Name": "{BoxName}",
        "Schema": null,
        "__published": "/Date(1486461000154)/",
        "__updated": "/Date(1486461000154)/",
        "_Role": {
          "__deferred": {
            "uri": "https://{UnitFQDN}/{CellName}/__ctl/Box('{BoxName}')/_Role"
          }
        },
        "_Relation": {
          "__deferred": {
            "uri": "https://{UnitFQDN}/{CellName}/__ctl/Box('{BoxName}')/_Relation"
          }
        },
        "_ReceivedMessage": {
          "__deferred": {
            "uri": "https://{UnitFQDN}/{CellName}/__ctl/Box('{BoxName}')/_ReceivedMessage"
          }
        },
        "_SentMessage": {
          "__deferred": {
            "uri": "https://{UnitFQDN}/{CellName}/__ctl/Box('{BoxName}')/_SentMessage"
          }
        }
      }
    ]
  }
}
```

<br>

### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/Box" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED