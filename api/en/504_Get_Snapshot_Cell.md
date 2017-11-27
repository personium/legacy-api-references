# Retrieve Cell snapshot file

### Overview

Obtain a Cell snapshot file.<br>The content to be returned varies depending on the ETag value specified in the If-None-Match header.

### Required Privileges

root

### Restrictions

* None

<br>

### Request

#### Request URL

```
/{CellName}/__snapshot/{FileName}
```

#### Request Method

GET

#### Request Query

|Query Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|p_cookie_peer<br>|Cookie Authentication Value<br>|The cookie authentication value returned from the server during authentication<br>|No<br>|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br>|

#### Request Header

|Header Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|Method override function<br>|User-defined<br>|No<br>|When this value is specified at the time of request in the POST method, the specified value is used as a method.<br>|
|X-Override<br>|Header override function<br>|${OverwrittenHeaderName}:${Value}<br>|No<br>|Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.<br>|
|X-Personium-RequestKey<br>|RequestKey field value output in the event log<br>|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br>|No<br>|PCS-${UNIXtime} by default<br>Supported in V 1.1.7 and later<br>|
|Authorization<br>|Specifies authentication information in the OAuth 2.0 format<br>|Bearer {AccessToken}<br>|No<br>|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br>|
|If-None-Match<br>|Specify the value of ETag<br>|String<br>Specify the ETag value in the following format<br>"*"or "{Half size} - {Half size}"<br>|No<br>|Example) When specifying ETag value "1-1372742704414"<br>"1-1372742704414"<br>|

#### Request Body

None

#### Request Sample

None

<br>

### Response

#### Response Code

* If the If-None-Match header is not specified in the request, or if the ETag value of the If-None-Match header does not match the ETag of the resource stored in WebDav in the request<br>(Including cases where the format of the specified ETag value is invalid)

|Code|Message|Overview|
|:--|:--|:--|
|200|OK|Acquisition success|

* If the ETag value of the If-None-Match header in the request matches the ETag of the resource stored in WebDav

|Code|Message|Overview|
|:--|:--|:--|
|304|Not Modified|The document has not been updated|

#### Response Header

|Header Name<br>|Overview<br>|Notes<br>|
|:--|:--|:--|
|Content-Type<br>|Resource Content-Type<br>|Return only for status code 200<br>|
|ETag<br>|Resource version information<br>|<br>|
|Access-Control-Allow-Origin<br>|Cross domain communication permission header<br>|Return value fixed to "*"<br>|
|X-Personium-Version<br>|API version that the request is processed<br>|Version of the API used to process the request<br>|

#### Response Body

Return the contents of the file<br><br>
However, when the status code is 304, the response body is not returned

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

None

<br>

### cURL Sample

```sh
curl "https://{UnitFQDN}/{CellName}/__snapshot/{FileName}" -X POST -i -H 'Authorization: Bearer {AccessToken}'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED