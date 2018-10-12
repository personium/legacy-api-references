# Delete Log File

## Overview

This API deletes the existing log files. It cannot delete the most recent log file.  
If you exceed the maximum number of generations to hold rotate when the log file, the log file of the oldest is deleted.

### Required Privileges

log

### Restrictions

* Log output of the internal event is not supported, Log output configuration is not supported
* log file name"default.log"(fixed)
* Set rotate size: 50MB
* Configure the log output label to "info"(fixed)(output for all INFO, WARN, ERROR)


## Request

### Request URL

#### rotated log file

```
/{CellName}/__log/archive/{LogName}
```

\*{LogName} specifies the file name returned by the log file information acquisition API.

### Request Method

DELETE

### Request Query

|Query Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|p_cookie_peer|Cookie Authentication Value|The cookie authentication value returned from the server during authentication|No|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used|

### Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|Method override function|User-defined|No|If you specify this value when requesting with the POST method, the specified value will be used as a method.|
|X-Override|Header override function|${OverwrittenHeaderName}:${Value}|No|Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.|
|X-Personium-RequestKey|RequestKey field value output in the event log|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters|No|PCS-${UNIXtime} by default|
|Authorization|Specifies authentication information in the OAuth 2.0 format|Bearer {AccessToken}|No|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API|
|If-Match|Specifies the target ETag value|ETag value|No|[*] by default|

### Request Body

None


## Response

### Response Code

|Code|Message|Overview|
|:--|:--|:--|
|204|No Content|Delete success|

### Response Header

|Item Name|Overview|Notes|
|:--|:--|:--|
|Content-Type|application/json|To be returned only if it fails to remove|

### Response Body

None

### Error Messages

Refer to [Error Message List](004_Error_Messages.md)


## cURL Command

```sh
curl "{CellURL}/__log/archive/{LogName}" -X DELETE -i -H \
'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```

