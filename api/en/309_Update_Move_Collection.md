# Move and rename collection

### Overview

Move / rename WebDAV collection file.<br>
\* You can not change properties

### Required Privileges

write

### Restrictions

#### Common restriction

None

#### WebDAV restriction

Unpublished

<br>

### Request

#### Request URL

```
/{CellName}/{BoxName}/{ResourcePath}/
```

#### Request Method

MOVE

#### Request Query

##### Common Request Query

|Query Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|p_cookie_peer<br>|Cookie Authentication Value<br>|The cookie authentication value returned from the server during authentication<br>|No<br>|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br>|

##### WebDav Common Request Query

None

#### Request Header

##### Common Request Header

|Header Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|Method override function<br>|User-defined<br>|No<br>|Specifying this value in a request with the POST method indicates that the specified value is used as the method<br>|
|X-Override<br>|Header override function<br>|${OverwrittenHeaderName}:${Value}<br>|No<br>|The normal HTTP header value is overwritten. Specify multiple X-Override headers for the overwriting of multiple headers<br>|
|X-Personium-RequestKey<br>|RequestKey field value output in the event log<br>|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br>|No<br>|Supported in V 1.1.7 and later<br>|

##### Individual Request Header

|Header Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|Destination<br>|Change destination<br>|Absolute URI<br>|Yes<br>|Move / Specify the file name to change the name.<br>|
|Overwrite<br>|Overwrite<br>|"T" or "F"<br>|No<br>|Specify Overwrite available("T") and Can not overwrite("F").(The initial value is "F")<br>|
|Depth<br>|Mobile hierarchy<br>|"infinity"<br>|No<br>|Specify the depth of the moving collection hierarchy. (Initial value is infinite)<br>|
|Authorization<br>|Specifies authentication information in the OAuth 2.0 format<br>|Bearer {AccessToken}<br>|No<br>|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br>|

#### Request Body

None

#### Request Sample

None

<br>

### Response

#### Response Code

|Code<br>|Overview<br>|Notes<br>|
|:--|:--|:--|
|201<br>|Created<br>|Move or rename name success (create)<br>|
|204<br>|No Content<br>|Move or rename name success (Overwrite)<br>|

#### Response Header

|Header Name<br>|Overview<br>|Notes<br>|
|:--|:--|:--|
|Location<br>|Resource URL of the created collection<br>|Return only when collection can be created successfully<br>|
|ETag<br>|Resource version information<br>|Return only when collection can be created successfully<br>|
|Access-Control-Allow-Origin<br>|Cross domain communication permission header<br>|Return value fixed to "*"<br>|
|X-Personium-Version<br>|API version that the request is processed<br>|Version of the API used to process the request<br>|

#### Response Body

None

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

None<br><br>

### cURL Command

Change the collection name ("/" at the end is mandatory)

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{OldCollectionName}/" -X MOVE -i -H 'Destination:https://{UnitFQDN}/{CellName}/{BoxName}/{NewCollectionName}/' -H 'Authorization: Bearer {AccessToken}'
```

File name change

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{CollectionName}/{OldFileName}/" -X MOVE -i -H 'Destination:https://{UnitFQDN}/{CellName}/{BoxName}/{CollectionName}/{NewFileName}' -H 'Authorization: Bearer {AccessToken}'
```

File move

```sh
curl  "https://{UnitFQDN}/{CellName}/{BoxName}/{CollectionNameA}/{FileName}" -X MOVE -i -H 'Destination:https://{UnitFQDN}/{CellName}/{BoxName}/{CollectionNameB}/{FileName}' -H 'Authorization: Bearer {AccessToken}'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED