# Service Execute

### Overview

Execute the registered service

### Required Privileges

exec

### Restrictions

* General Restrictions
    * None

* WebDAV Restrictions
    * Unpublished

<br>

### Request

#### Request URL

```
/{CellName}/{BoxName}/{CollectionName}/{ServiceName}
```

|Path<br>|Overview<br>|Notes<br>|
|:--|:--|:--|
|{CellName}<br>|Cell Name<br>|<br>|
|{BoxName}<br>|Box Name<br>|<br>|
|{CollectionName}<br>|Service Collection Name<br>|Valid values <br>Number of digits:1-128<br>Usable character types<br>alphanumeric character, period(.), under score(_), hyphen(-)<br>|
|{ServiceName}<br>|Specify the name of the registered service<br>|Valid values(limit) <br>Number of digits:1-128<br>Usable character types<br>alphanumeric character, period(.), under score(_), hyphen(-)<br>|

#### Request Method

GET / POST / PUT / DELETE

#### Request Query

|Query Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|p_cookie_peer<br>|Cookie Authentication Value<br>|The cookie authentication value returned from the server during authentication<br>|No<br>|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br>|

#### Request Header

##### Common Request Header

|Header Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|Method override function<br>|User-defined<br>|No<br>|Specifying this value in a request with the POST method indicates that the specified value is used as the method<br>|
|X-Override<br>|Header override function<br>|${OverwrittenHeaderName}:${Value}<br>|No<br>|The normal HTTP header value is overwritten. Specify multiple X-Override headers for the overwriting of multiple headers<br>|
|X-Personium-RequestKey<br>|RequestKey field value output in the event log<br>|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br>|No<br>|Supported in V 1.1.7 and later<br>|

#### Request Body

None

#### Request Sample

None

<br>

### Response

#### Response Code

When the script is executed, the response code of the script is returned

#### Response Header

|Header Name<br>|Overview<br>|Notes<br>|
|:--|:--|:--|
|Content-Type<br>|Return script-dependent format<br>|text/html<br>|

#### Response Body

When the script is executed, the response body of the script is returned

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

None

<br>

### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{CollectionName}/{ServiceName}" -X GET -i -H "Authorization:Bearer {AccessToken}" -H "Accept:application/json"
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED
