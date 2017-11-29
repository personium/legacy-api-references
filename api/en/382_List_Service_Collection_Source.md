# Get service collection source

### Overview

Acquire service source information

### Required Privileges

read

### Restrictions

* General Restrictions
    * None

* WebDAV Restrictions
    * Unpublished<br><br>

### Request

#### Request URL

```
/{CellName}/{BoxName}/{CollectionName}/__src/{ResourceName}
```

|Path<br>|Overview<br>|Notes<br>|
|:--|:--|:--|
|{CellName}<br>|Cell Name<br>|<br>|
|{BoxName}<br>|Box Name<br>|<br>|
|{CollectionName}<br>|Service Collection Name<br>|Valid values <br>Number of digits:1-128<br>Usable character types<br>alphanumeric character, period(.), under score(_), hyphen(-)<br>|
|{ResourceName}<br>|Resource name<br>|Valid values(limit) <br>Number of digits:1-128<br>Usable character types<br>alphanumeric character, period(.), under score(_), hyphen(-)<br>|

#### Request Method

GET

#### Request Query

##### Common Request Query

|Query Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|p_cookie_peer<br>|Cookie Authentication Value<br>|The cookie authentication value returned from the server during authentication<br>|No<br>|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br>|

#### Request Header

##### Common Request Header

|Header Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|Method override function<br>|User-defined<br>|No<br>|If you specify this value when requesting with the POST method, the specified value will be used as a method.<br>|
|X-Override<br>|Header override function<br>|${OverwrittenHeaderName}:${Value}<br>|No<br>|Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.<br>|
|X-Personium-RequestKey<br>|RequestKey field value output in the event log<br>|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br>|No<br>|Supported in V 1.1.7 and later<br>|

##### Service Collection Source Acquisition Specific Request Header

|Header Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|Authorization<br>|Specifies authentication information in the OAuth 2.0 format<br>|Bearer {AccessToken}<br>|No<br>|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br>|
|If-None-Match<br>|Specify the value of ETag, if there is no change, 304, if it is changed return the latest resource<br>|String<br>|No<br>|<br>|

#### Request Body

None

#### Request Sample

None

<br>

### Response

#### Response Code

200

#### Response Header

|Header Name<br>|Overview<br>|Notes<br>|
|:--|:--|:--|
|Content-Type<br>|Resource's Content-Type<br>|<br>|
|ETag<br>|Resource version information<br>|<br>|

#### Response Body

Return the contents of the file

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

None

<br>

### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{CollectionName}/__src/{ResourceName}" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED
