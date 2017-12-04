# Cell snapshot file delete

### Overview

Delete an existing Cell snapshot file

### Required Privileges

root

### Restrictions

* None


### Request

#### Request URL

```
/{CellName}/__snapshot/{FileName}
```

#### Request Method

DELETE

#### Request Query

|Query Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|p_cookie_peer|Cookie Authentication Value|The cookie authentication value returned from the server during authentication|No|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used|

#### Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|Method override function|User-defined|No|Specifying this value in a request with the POST method indicates that the specified value is used as the method|
|X-Override|Header override function|${OverwrittenHeaderName}:${Value}|No|The normal HTTP header value is overwritten. Specify multiple X-Override headers for the overwriting of multiple headers|
|X-Personium-RequestKey|RequestKey field value output in the event log|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters|No|PCS-${UNIXtime} by default|
|Authorization|Specifies authentication information in the OAuth 2.0 format|Bearer {AccessToken}|No|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API|
|If-Match|Specifies the target ETag value|ETag value|No|[*] by default|

#### Request Body

None

#### Request Sample

None


### Response

#### Response Code

|Code|Message|Overview|
|:--|:--|:--|
|204|No Content|Deletion success|

#### Response Header

|Header Name|Overview|Notes|
|:--|:--|:--|
|Content-Type|application/json|To be returned only if it fails to remove|
|Access-Control-Allow-Origin|Cross domain communication permission header|Return value fixed to "*"|
|X-Personium-Version|API version that the request is processed|Version of the API used to process the request|

#### Response Body

None

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

None


### cURL Sample

```sh
curl "https://{UnitFQDN}/{CellName}/__snapshot/{FileName}" -X DELETE -i -H 'Authorization: Bearer {AccessToken}'
```


###### Copyright 2017 FUJITSU LIMITED
