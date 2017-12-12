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


### Request

#### Request URL

```
/{CellName}/{BoxName}/{CollectionName}/{ServiceName}
```

|Path|Overview|Notes|
|:--|:--|:--|
|{CellName}|Cell Name||
|{BoxName}|Box Name||
|{CollectionName}|Service Collection Name|Valid values <br>Number of digits:1-128<br>Usable character types<br>alphanumeric character, period(.), under score(_), hyphen(-)|
|{ServiceName}|Specify the name of the registered service|Valid values(limit) <br>Number of digits:1-128<br>Usable character types<br>alphanumeric character, period(.), under score(_), hyphen(-)|

#### Request Method

GET / POST / PUT / DELETE

#### Request Query

|Query Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|p_cookie_peer|Cookie Authentication Value|The cookie authentication value returned from the server during authentication|No|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used|

#### Request Header

##### Common Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|Method override function|User-defined|No|Specifying this value in a request with the POST method indicates that the specified value is used as the method|
|X-Override|Header override function|${OverwrittenHeaderName}:${Value}|No|The normal HTTP header value is overwritten. Specify multiple X-Override headers for the overwriting of multiple headers|
|X-Personium-RequestKey|RequestKey field value output in the event log|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters|No||

#### Request Body

None

#### Request Sample

None


### Response

#### Response Code

When the script is executed, the response code of the script is returned

#### Response Header

|Header Name|Overview|Notes|
|:--|:--|:--|
|Content-Type|Return script-dependent format|text/html|

#### Response Body

When the script is executed, the response body of the script is returned

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

None


### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{CollectionName}/{ServiceName}" -X GET -i -H "Authorization:Bearer {AccessToken}" -H "Accept:application/json"
```


###### Copyright 2017 FUJITSU LIMITED
