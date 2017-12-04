# Delete Log File

### Overview

This API deletes the existing log files. It cannot delete the most recent log file.  
If you exceed the maximum number of generations to hold rotate when the log file, the log file of the oldest is deleted.

### Required Privileges

log

### Restrictions

* Log output of the internal event is not supported, Log output configuration is not supported
* log file name"default.log"(fixed)
* Set rotate size: 50MB
* Configure the log output label to "info"(fixed)(output for all INFO, WARN, ERROR)

<br>

### Request

#### Request URL

##### rotated log file

```
/{CellName}/__log/archive/{LogName}
```

\*{LogName} specifies the file name returned by the log file information acquisition API.

#### Request Method

DELETE

#### Request Query

|Query Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|p_cookie_peer<br>|Cookie Authentication Value<br>|The cookie authentication value returned from the server during authentication<br>|No<br>|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br>|

#### Request Header

|Header Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|Method override function<br>|User-defined<br>|No<br>|If you specify this value when requesting with the POST method, the specified value will be used as a method.<br>|
|X-Override<br>|Header override function<br>|${OverwrittenHeaderName}:${Value}<br>|No<br>|Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.<br>|
|X-Personium-RequestKey<br>|RequestKey field value output in the event log<br>|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br>|No<br>|PCS-${UNIXtime} by default<br>Supported in V 1.1.7 and later<br>|
|Authorization<br>|Specifies authentication information in the OAuth 2.0 format<br>|Bearer {AccessToken}<br>|No<br>|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br>|
|If-Match<br>|Specifies the target ETag value<br>|ETag value<br>|No<br>|[*] by default<br>|

#### Request Body

None

#### Request Sample

None

<br>

### Response

#### Response Code

|Code<br>|Message<br>|Overview<br>|
|:--|:--|:--|
|204<br>|No Content<br>|Delete success<br>|

#### Response Header

|Item Name<br>|Overview<br>|Notes<br>|
|:--|:--|:--|
|Content-Type<br>|application/json<br>|To be returned only if it fails to remove<br>|

#### Response Body

None

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

None

<br>

### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/__log/archive/{LogName}" -X DELETE -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED
