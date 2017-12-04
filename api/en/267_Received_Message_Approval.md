# Message status change (unread, read, approve)

### Overview
* approve Message
	* When the message type to be approved is message  
	change message status to read/unread
	* When the type of the message to approve is req.relation.build or req.role.grant  
	According to the values of RequestRelation and RequestRelationTarget of the message to be approved, register the relation and change the state of the message to approval/rejection
	* When the type of the message to approve is req.relation.break or req.role.revoke  
	According to the values of RequestRelation and RequestRelationTarget of the message to be approved, delete the relation and change the state of the message to approval/rejection

### Required Privileges

message  
social(Requires only relationship registration/deletion approval)

### Restrictions

* Always handles Content-Type in the request header as application/json
* Only accepts the request body in the JSON format
* Only application/json is supported for Content-Type in the request header and the JSON format for the response body
* Response body data is not ensured if atom or xml is specified in the $format query option, although it does not result in an error


### Request

#### Request URL

```
/{CellName}/__message/received/{MessageID}
```

#### Request Method

POST

#### Request Query

|Query Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|p_cookie_peer|Cookie Authentication Value|The cookie authentication value returned from the server during authentication|No|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used|

#### Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|Method override function|User-defined|No|If you specify this value when requesting with the POST method, the specified value will be used as a method.|
|X-Override|Header override function|${OverwrittenHeaderName}:${Value}|No|Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.|
|X-Personium-RequestKey|RequestKey field value output in the event log|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters|No|PCS-${UNIXtime} by default<br>Supported in V 1.1.7 and later|
|Authorization|Specifies authentication information in the OAuth 2.0 format|Bearer {AccessToken}|No|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API|
|Content-Type|Specifies the request body format|application/json|No|[application/json] by default|
|Accept|Specifies the response body format|application/json|No|[application/json] by default|

#### Request Body

JSON

|Item Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|Command|Message Command|When Type is message<br> read: read<br> unread: unread<br>When type is req.relation.build or req.relation.break or req.role.grant or req.role.revoke<br> approved: approved <br> rejected: rejected <br>However, if you have already changed to approved or rejected, you can not approve it|Yes||

#### Request Sample

```JSON
{"Command": "approved"}
```


### Response

#### Response Code

204

#### Response Header

|Item Name|Overview|Notes|
|:--|:--|:--|
|ETag|Resource version information||
|DataServiceVersion|OData version||
|Access-Control-Allow-Origin|Cross domain communication permission header|Return value fixed to "*"|
|X-Personium-Version|API version that the request is processed|Version of the API used to process the request|

#### Response Body

None

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

None


### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/__message/received/{MessageID}" -X POST -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"Command": "approved"}'
```


###### Copyright 2017 FUJITSU LIMITED
