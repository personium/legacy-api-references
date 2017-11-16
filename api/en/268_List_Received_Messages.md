# ReceivedMessage Acquire List

### Overview

acquire the List of ReceivedMessage

### Required Privileges

message or message-read

### Restrictions

* Always handles Content-Type in the request header as application/json
* Only accepts the request body in the JSON format
* Only application/json is supported for Content-Type in the request header and the JSON format for the response body
* Response body data is not ensured if atom or xml is specified in the $format query option, although it does not result in an error

<br>

### Request

#### Request URL

```
/{CellName}/__ctl/ReceivedMessage
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

##### OData Request Header

| Header Name<br>   | Overview<br>                           | Effective Value<br>  | Required<br> | Notes<br>                         |
|:-- |:-- |:-- |:-- |:-- |
| Accept<br>        | Specifies the response body format<br> | application/json<br> | No<br>       | [application/json] by default<br> |
| If-None-Match<br> | Specifies the target ETag value<br>    | ETag value<br>       | Yes<br>      | Not compatible<br>                |

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

##### Common

The response is a JSON object, the correspondence between the key (name) and type defined in the object (subobject) and the value are as follows

| Object<br> | Name (Key)<br>  | Type<br>   | Value<br>                                       |
|:-- |:-- |:-- |:-- |
| Root<br>   | d<br>           | object<br> | Object{1}<br>                                   |
| {1}<br>    | results<br>     | array<br>  | Array object {2}<br>                            |
| {2}<br>    | __metadata<br>  | object<br> | Object{3}<br>                                   |
| {3}<br>    | uri<br>         | string<br> | URL to the resource that was created<br>        |
| {3}<br>    | etag<br>        | string<br> | Etag value<br>                                  |
| {2}<br>    | __published<br> | string<br> | Creation date (UNIX time)<br>                   |
| {2}<br>    | __updated<br>   | string<br> | Update date (UNIX time)<br>                     |
| {1}<br>    | __count<br>     | string<br> | Get number of results in $inlinecount query<br> |

##### ReceivedMessage specific response body

| Object<br> | Name(Key)<br>             | Type<br>   | Value<br>                                                                                                                                                                                                                                                                                  |
|:-- |:-- |:-- |:-- |
| {3}<br>    | type<br>                  | string<br> | CellCtl.ReceivedMessage<br>                                                                                                                                                                                                                                                                |
| {2}<br>    | __id<br>                  | string<br> | ReceivedMessage ID<br>Return a 32 character string such as "b5d008e9092f489c8d3c574a768afc33" with UUID<br>                                                                                                                                                                                |
| {2}<br>    | _Box.Name<br>             | string<br> | BoxName for Relation<br>                                                                                                                                                                                                                                                                   |
| {2}<br>    | InReplyTo<br>             | string<br> | ID message you are replying<br>Return a 32 character string such as "b5d008e9092f489c8d3c574a768afc33" with UUID<br>                                                                                                                                                                       |
| {2}<br>    | From<br>                  | string<br> | From Cell URL<br>                                                                                                                                                                                                                                                                          |
| {2}<br>    | MulticastTo<br>           | string<br> | Destination Cell URL<br>it becomes CSV format if it is sent to the plurality of Cell<br>                                                                                                                                                                                                   |
| {2}<br>    | Type<br>                  | string<br> | Message type<br>message : message<br>relationship registration request(relation): req.relation.build<br>relationship deletion request(relation): req.relation.break<br>relationship registration request(role): req.role.grant<br>relationship deletion request(role): req.role.revoke<br> |
| {2}<br>    | Title<br>                 | string<br> | Message Title<br>                                                                                                                                                                                                                                                                          |
| {2}<br>    | Body<br>                  | string<br> | Message Body<br>                                                                                                                                                                                                                                                                           |
| {2}<br>    | Priority<br>              | string<br> | Priority<br>(high)1 - 5(low)<br>                                                                                                                                                                                                                                                           |
| {2}<br>    | Status<br>                | string<br> | Message Status<br>When Type is message<br> read: read<br> unread: unread<br>When type is req.relation.build or req.relation.break or req.role.grant or req.role.revoke <br> approved: approved <br> rejected: rejected <br> none: none<br>                                                 |
| {2}<br>    | RequestRelation<br>       | string<br> | Relation name or relation class URL or role name or role class URL, the registration request<br>Only when message type is other than message<br>                                                                                                                                           |
| {2}<br>    | RequestRelationTarget<br> | string<br> | CellURL of relationships<br>Only when message type is other than message<br>                                                                                                                                                                                                               |

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

```JSON
{
  "d": {
    "results": [
      {
        "__metadata": {
          "uri": "https://{UnitFQDN}/{CellName}/__ctl/ReceivedMessage('c87b42e10df846a9bee842225d1383fe')",
          "etag": "W/\"1-1486683974451\"",
          "type": "CellCtl.ReceivedMessage"
        },
        "__id": "c87b42e10df846a9bee842225d1383fe",
        "_Box.Name": "{BoxName}",
        "InReplyTo": "xnKXmd4TTZCw-bfSEw4f0AxnKXmd4TTZ",
        "From": "https://{UnitFQDN}/{CellName}/",
        "MulticastTo": null,
        "Type": "message",
        "Title": "Message Sample Title",
        "Body": "Message Sample Body",
        "Priority": 3,
        "Status": "unread",
        "RequestRelation": null,
        "RequestRelationTarget": null,
        "__published": "/Date(1486683974451)/",
        "__updated": "/Date(1486683974451)/",
        "_Box": {
          "__deferred": {
            "uri": "https://{UnitFQDN}/{CellName}/__ctl/ReceivedMessage('c87b42e10df846a9bee842225d1383fe')/_Box"
          }
        },
        "_AccountRead": {
          "__deferred": {
            "uri": "https://{UnitFQDN}/{CellName}/__ctl/ReceivedMessage('c87b42e10df846a9bee842225d1383fe')/_AccountRead"
          }
        }
      },
      {
        "__metadata": {
          "uri": "https://{UnitFQDN}/{CellName}/__ctl/ReceivedMessage('3afcc60e35fc49ee9a4e4f6c1ebee426')",
          "etag": "W/\"3-1486688634556\"",
          "type": "CellCtl.ReceivedMessage"
        },
        "__id": "3afcc60e35fc49ee9a4e4f6c1ebee426",
        "_Box.Name": null,
        "InReplyTo": "xnKXmd4TTZCw-bfSEw4f0AxnKXmd4TTZ",
        "From": "https://{UnitFQDN}/{CellName}/",
        "MulticastTo": null,
        "Type": "message",
        "Title": "Message Sample Title",
        "Body": "Message Sample Body",
        "Priority": 3,
        "Status": "read",
        "RequestRelation": null,
        "RequestRelationTarget": null,
        "__published": "/Date(1486638759669)/",
        "__updated": "/Date(1486688634556)/",
        "_Box": {
          "__deferred": {
            "uri": "https://{UnitFQDN}/{CellName}/__ctl/ReceivedMessage('3afcc60e35fc49ee9a4e4f6c1ebee426')/_Box"
          }
        },
        "_AccountRead": {
          "__deferred": {
            "uri": "https://{UnitFQDN}/{CellName}/__ctl/ReceivedMessage('3afcc60e35fc49ee9a4e4f6c1ebee426')/_AccountRead"
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
curl "https://{UnitFQDN}/{CellName}/__ctl/ReceivedMessage" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED