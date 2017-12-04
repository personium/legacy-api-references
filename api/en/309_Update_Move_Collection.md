# Move and rename collection

### Overview

Move / rename WebDAV collection file.  
\* You can not change properties

### Required Privileges

write

### Restrictions

#### Common restriction

None

#### WebDAV restriction

Unpublished


### Request

#### Request URL

```
/{CellName}/{BoxName}/{ResourcePath}/
```

#### Request Method

MOVE

#### Request Query

##### Common Request Query

|Query Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|p_cookie_peer|Cookie Authentication Value|The cookie authentication value returned from the server during authentication|No|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used|

##### WebDav Common Request Query

None

#### Request Header

##### Common Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|Method override function|User-defined|No|Specifying this value in a request with the POST method indicates that the specified value is used as the method|
|X-Override|Header override function|${OverwrittenHeaderName}:${Value}|No|The normal HTTP header value is overwritten. Specify multiple X-Override headers for the overwriting of multiple headers|
|X-Personium-RequestKey|RequestKey field value output in the event log|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters|No|Supported in V 1.1.7 and later|

##### Individual Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|Destination|Change destination|Absolute URI|Yes|Move / Specify the file name to change the name.|
|Overwrite|Overwrite|"T" or "F"|No|Specify Overwrite available("T") and Can not overwrite("F").(The initial value is "F")|
|Depth|Mobile hierarchy|"infinity"|No|Specify the depth of the moving collection hierarchy. (Initial value is infinite)|
|Authorization|Specifies authentication information in the OAuth 2.0 format|Bearer {AccessToken}|No|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API|

#### Request Body

None

#### Request Sample

None


### Response

#### Response Code

|Code|Overview|Notes|
|:--|:--|:--|
|201|Created|Move or rename name success (create)|
|204|No Content|Move or rename name success (Overwrite)|

#### Response Header

|Header Name|Overview|Notes|
|:--|:--|:--|
|Location|Resource URL of the created collection|Return only when collection can be created successfully|
|ETag|Resource version information|Return only when collection can be created successfully|
|Access-Control-Allow-Origin|Cross domain communication permission header|Return value fixed to "*"|
|X-Personium-Version|API version that the request is processed|Version of the API used to process the request|

#### Response Body

None

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

None

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


###### Copyright 2017 FUJITSU LIMITED
