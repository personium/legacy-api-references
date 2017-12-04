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


### Request

#### Request URL

```
/{CellName}/__ctl/ReceivedMessage
```

#### Request Method

GET

#### Request Query

The following query parameters are available

|Query Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|p_cookie_peer|Cookie Authentication Value|The cookie authentication value returned from the server during authentication|No|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used|

[$select  Query](406_Select_Query.html)

[$expand  Query](405_Expand_Query.html)

[$format  Query](404_Format_Query.html)

[$filter  Query](403_Filter_Query.html)

[$inlinecount  Query](407_Inlinecount_Query.html)

[$orderby  Query](400_Orderby_Query.html)

[$top  Query](401_Top_Query.html)

[$skip  Query](402_Skip_Query.html)

[Full-text Search (q) Query](408_Full_Text_Search_Query.html)

#### Request Header

##### Common Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|Method override function|User-defined|No|Specifying this value in a request with the POST method indicates that the specified value is used as the method|
|X-Override|Header override function|${OverwrittenHeaderName}:${Value}|No|The normal HTTP header value is overwritten. Specify multiple X-Override headers for the overwriting of multiple headers|
|X-Personium-RequestKey|RequestKey field value output in the event log|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters|No|PCS-${UNIXtime} by default<br>Supported in V 1.1.7 and later|

##### OData Common Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|Authorization|Specifies authentication information in the OAuth 2.0 format|Bearer {AccessToken}|No|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API|

##### OData Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|Accept|Specifies the response body format|application/json|No|[application/json] by default|
|If-None-Match|Specifies the target ETag value|ETag value|Yes|Not compatible|

#### Request Body

None

#### Request Sample

None


### Response

#### Response Code

200

#### Response Header

None

#### Response Body

##### Common

The response is a JSON object, the correspondence between the key (name) and type defined in the object (subobject) and the value are as follows

|Object|Name (Key)|Type|Value|
|:--|:--|:--|:--|
|Root|d|object|Object{1}|
|{1}|results|array|Array object {2}|
|{2}|__metadata|object|Object{3}|
|{3}|uri|string|URL to the resource that was created|
|{3}|etag|string|Etag value|
|{2}|__published|string|Creation date (UNIX time)|
|{2}|__updated|string|Update date (UNIX time)|
|{1}|__count|string|Get number of results in $inlinecount query|

##### ReceivedMessage specific response body

|Object|Name(Key)|Type|Value|
|:--|:--|:--|:--|
|{3}|type|string|CellCtl.ReceivedMessage|
|{2}|__id|string|ReceivedMessage ID<br>Return a 32 character string such as "b5d008e9092f489c8d3c574a768afc33" with UUID|
|{2}|_Box.Name|string|BoxName for Relation|
|{2}|InReplyTo|string|ID message you are replying<br>Return a 32 character string such as "b5d008e9092f489c8d3c574a768afc33" with UUID|
|{2}|From|string|From Cell URL|
|{2}|MulticastTo|string|Destination Cell URL<br>it becomes CSV format if it is sent to the plurality of Cell|
|{2}|Type|string|Message type<br>message : message<br>relationship registration request(relation): req.relation.build<br>relationship deletion request(relation): req.relation.break<br>relationship registration request(role): req.role.grant<br>relationship deletion request(role): req.role.revoke|
|{2}|Title|string|Message Title|
|{2}|Body|string|Message Body|
|{2}|Priority|string|Priority<br>(high)1 - 5(low)|
|{2}|Status|string|Message Status<br>When Type is message<br> read: read<br> unread: unread<br>When type is req.relation.build or req.relation.break or req.role.grant or req.role.revoke <br> approved: approved <br> rejected: rejected <br> none: none|
|{2}|RequestRelation|string|Relation name or relation class URL or role name or role class URL, the registration request<br>Only when message type is other than message|
|{2}|RequestRelationTarget|string|CellURL of relationships<br>Only when message type is other than message|

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


### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/ReceivedMessage" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```


###### Copyright 2017 FUJITSU LIMITED