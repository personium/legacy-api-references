# File registration update

## Overview

Register / update WebDav information.

### Required Privileges

write

### Restrictions

#### WebDAV restriction

Unpublished


## Request

### Request URL

```
/{CellName}/{BoxName}/{ResourcePath}
```

|Path|Overview|Notes|
|:--|:--|:--|
|{CellName}|Cell Name||
|{BoxName}|Box Name||
|{ResourcePath}|Path to resource|Valid values Number of digits:1-128<br>Usable character types<br>alphanumeric character, period(.), under score(_), hyphen(-)|

### Request Method

PUT

### Request Query

#### Common Request Query

|Query Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|p_cookie_peer|Cookie Authentication Value|The cookie authentication value returned from the server during authentication|No|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used|

### Request Header

#### Common Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|Method override function|User-defined|No|Specifying this value in a request with the POST method indicates that the specified value is used as the method|
|X-Override|Header override function|${OverwrittenHeaderName}:${Value}|No|The normal HTTP header value is overwritten. Specify multiple X-Override headers for the overwriting of multiple headers|
|X-Personium-RequestKey|RequestKey field value output in the event log|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters|No||

#### Individual Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|Authorization|Specifies authentication information in the OAuth 2.0 format|Bearer {AccessToken}|No|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API|
|If-Match|Specifies the target ETag value|ETag value|No|[*] by default|
|Content-Type|Specify the content format of the registration / update file|String|No|When registering and updating in SWF format<br>Content-Type:application/x-shockwave-flash<br>When registering / updating in PDF format<br>Content-Type:application/pdf<br>When registering and updating in JPG format<br>Content-Type:image/jpeg<br>When registering and updating in js format<br>Content-Type:application/x-javascript|

### Request Body

|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|
|Designate context information to be registered / updated as a request body in binary|The method specified in the Content-Type header|Yes||


## Response

### Response Code

|Code|Message|Overview|
|:--|:--|:--|
|201|Created|Successful registration|
|204|No Content|Update success|

### Response Header

|Header Name|Overview|Notes|
|:--|:--|:--|
|Content-Type|Format of data to be returned|Only when it failed at the time of update / creation, return it|
|ETag|Resource version information||

### Response Body

Only when it failed at the time of update / creation, return it

### Error Messages

Refer to [Error Message List](004_Error_Messages.md)


## cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{ResourceName}" -X PUT -i -H \
'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{[File contents]}'
```

