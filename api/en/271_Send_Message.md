# SendMessage

### Overview

send message

### Required Privileges

message

### Restrictions

* Always handles Content-Type in the request header as application/json
* Only accepts the request body in the JSON format
* Only application/json is supported for Content-Type in the request header and the JSON format for the response body
* Response body data is not ensured if atom or xml is specified in the $format query option, although it does not result in an error

<br>

### Request

#### Request URL

```
/{CellName}/__message/send
```

#### Request Method

POST

#### Request Query

None

#### Request Header

| Header Name<br>            | Overview<br>                                                     | Effective Value<br>                   | Required<br> | Notes<br>                                                                                                         |
|:-- |:-- |:-- |:-- |:-- |
| X-HTTP-Method-Override<br> | Method override function<br>                                     | User-defined<br>                      | No<br>       | If you specify this value when requesting with the POST method, the specified value will be used as a method.<br> |
| X-Override<br>             | Header override function<br>                                     | ${OverwrittenHeaderName}:${Value}<br> | No<br>       | Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.<br>       |
| Authorization<br>          | Specifies authentication information in the OAuth 2.0 format<br> | Bearer {AccessToken}<br>              | No<br>       | * Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br>                |
| Content-Type<br>           | Specifies the request body format<br>                            | application/json<br>                  | No<br>       | [application/json] by default<br>                                                                                 |
| Accept<br>                 | Specifies the response body format<br>                           | application/json<br>                  | No<br>       | [application/json] by default<br>                                                                                 |

#### Request Body

JSON

| Item Name<br>             | Overview<br>                                      | Effective Value<br>                                                                                                                                                                                                  | Required<br> | Notes<br>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|:-- |:-- |:-- |:-- |:-- |
| BoxBound<br>              | Linking with Box, whether possible or not<br>     | true / false<br>The default value is false<br>                                                                                                                                                                       | No<br>       | Send a token you have schema authentication is set to true this item when you tie in Box (not implemented)<br>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| InReplyTo<br>             | Message ID of the reply message<br>               | Number of digits: 32<br>null<br>                                                                                                                                                                                     | No<br>       | <br>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| To<br>                    | Destination cell URL<br>                          | URL format<br>null<br>                                                                                                                                                                                               | * 1<br>      | If you want to send message to multiple cells specified in CSV format,<br>*1 required either Relation or To<br>Maximum 1000 cell URL destination can be specified in Relation or To<br>                                                                                                                                                                                                                                                                                                                                                                                                       |
| ToRelation<br>            | Relationship names to be sent<br>                 | Number of digits: 1 - 128<br>Character type: Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_"), plus, :<br>However, the string cannot start with a underscore ("_") or colon (:)<br>null<br> | * 1<br>      | *1 required either Relation or To<br>Maximum 1000 cell URL destination can be specified in Relation or To<br>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Type<br>                  | Message type<br>                                  | message<br>req.relation.build<br>req.relation.break<br>req.role.grant<br>req.role.revoke<br>                                                                                                                         | No<br>       | The message is treated as a default<br>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| Title<br>                 | Message Title<br>                                 | Number of digits: 256< characters or less<br>                                                                                                                                                                        | No<br>       | The default is treated as an empty string<br>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Body<br>                  | Message Body<br>                                  | Number of digits: 64Kbyte following<br>                                                                                                                                                                              | No<br>       | The default is treated as an empty string<br>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Priority<br>              | Priority<br>                                      | 1~5<br>                                                                                                                                                                                                              | No<br>       | The default is treated as 3<br>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| RequestRelation<br>       | Information of requested relation to register<br> | URL format<br>null<br>                                                                                                                                                                                               | * 2<br>      | *2 Required when message type is other than message<br>Relation name or relation class URL or role name or role class URL, the registration request<br>When only the relation name is specified, it is regarded as a relative URL from the following URL<br>BoxBound is true:[target Box schema URL]__relation/__/<br>BoxBound is false:[destination cell URL]__relation/__/<br>When only the role name is specified, it is regarded as a relative URL from the following URL<br>BoxBound is true:[target Box schema URL]__role/__/<br>BoxBound is false:[destination cell URL]__role/__/<br> |
| RequestRelationTarget<br> | Cell URL that connects the relationship<br>       | URL format<br>null<br>                                                                                                                                                                                               | * 2<br>      | *2 Required when message type is other than message<br>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |

#### Request Sample

```JSON
{
  "BoxBound": true,
  "InReplyTo": "hnKXm44TTZCw-bfSEw4f0A",
  "To": "https://{UnitFQDN}/{TargetCellName}",
  "ToRelation": null,
  "Type": "req.relation.build",
  "Title": "Friend request",
  "Body": "Thank you for the friend approval",
  "Priority": 3,
  "RequestRelation": "https://{UnitFQDN}/{AppCellName}/__relation/__/{RelationName}",
  "RequestRelationTarget": "https://{UnitFQDN}/{CellName}"
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

##### SentMessage specific response body

| Object<br> | Name (Key)<br>            | Type<br>   | Value<br>                                                                                                                                                                                                                                                                                 |
|:-- |:-- |:-- |:-- |
| {3}<br>    | type<br>                  | string<br> | CellCtl.ReceivedMessage<br>                                                                                                                                                                                                                                                               |
| {2}<br>    | __id<br>                  | string<br> | ReceivedMessage ID<br>Return a 32 character string such as "b5d008e9092f489c8d3c574a768afc33" with UUID<br>                                                                                                                                                                               |
| {2}<br>    | InReplyTo<br>             | string<br> | ID message you are replying<br>Return a 32 character string such as "b5d008e9092f489c8d3c574a768afc33" with UUID<br>                                                                                                                                                                      |
| {2}<br>    | To<br>                    | string<br> | Destination cell URL<br>                                                                                                                                                                                                                                                                  |
| {2}<br>    | ToRelation<br>            | string<br> | Relationship names to be sent<br>                                                                                                                                                                                                                                                         |
| {2}<br>    | Type<br>                  | string<br> | Message type<br>message: message<br>relationship registration request(relation): req.relation.build<br>relationship deletion request(relation): req.relation.break<br>relationship registration request(role): req.role.grant<br>relationship deletion request(role): req.role.revoke<br> |
| {2}<br>    | Title<br>                 | string<br> | Message Title<br>                                                                                                                                                                                                                                                                         |
| {2}<br>    | Body<br>                  | string<br> | Message Body<br>                                                                                                                                                                                                                                                                          |
| {2}<br>    | Priority<br>              | string<br> | Priority<br>(high)1 - 5(low)<br>                                                                                                                                                                                                                                                          |
| {2}<br>    | RequestRelation<br>       | string<br> | Relation name or relation class URL or role name or role class URL, the registration request<br>Only when message type is other than message<br>                                                                                                                                          |
| {2}<br>    | RequestRelationTarget<br> | string<br> | CellURL of relationships<br>Only when message type is other than message<br>                                                                                                                                                                                                              |
| {2}<br>    | _Box.Name<br>             | string<br> | BoxName for Relation<br>                                                                                                                                                                                                                                                                  |
| {2}<br>    | Result<br>                | array<br>  | Transmission result of each destination Cell<br>Array object {4}<br>                                                                                                                                                                                                                      |
| {4}<br>    | To<br>                    | string<br> | Destination cell URL<br>                                                                                                                                                                                                                                                                  |
| {4}<br>    | Code<br>                  | string<br> | Response Code<br>                                                                                                                                                                                                                                                                         |
| {4}<br>    | Reason<br>                | string<br> | Detailed message<br>                                                                                                                                                                                                                                                                      |

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

```JSON
{
  "d": {
    "results": {
      "__metadata": {
        "uri": "https://{UnitFQDN}/{CellName}/__ctl/SentMessage('3afcc60e35fc49ee9a4e4f6c1ebee426')",
        "etag": "W/\"1-1486638759524\"",
        "type": "CellCtl.SentMessage"
      },
      "__id": "3afcc60e35fc49ee9a4e4f6c1ebee426",
      "InReplyTo": "xnKXmd4TTZCw-bfSEw4f0AxnKXmd4TTZ",
      "To": "https://{UnitFQDN}/{CellName}",
      "ToRelation": null,
      "Type": "message",
      "Title": "Message Sample Title",
      "Body": "Message Sample Body",
      "Priority": 3,
      "RequestRelation": null,
      "RequestRelationTarget": null,
      "_Box.Name": null,
      "Result": [
        {
          "To": "https://{UnitFQDN}/{CellName}/",
          "Code": "201",
          "Reason": "Created."
        }
      ],
      "__published": "/Date(1486638759524)/",
      "__updated": "/Date(1486638759524)/"
    }
  }
}
```

<br>

### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/__message/send" -X POST -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"BoxBound":false,"InReplyTo":"xnKXmd4TTZCw-bfSEw4f0AxnKXmd4TTZ","To":"https://{UnitFQDN}/{CellName}","Type":"message","Title":"Message Sample Title","Body":"Message Sample Body","Priority":3}'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED