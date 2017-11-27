# Role Delete

### Overview

Delete existing Role information

### Restrictions

* General Restrictions
    * None

* OData Restrictions
    * Accept in the request header is ignored
    * Always handles Content-Type in the request header as application/json
    * Only accepts the request body in the JSON format
    * Only application/json is supported for Content-Type in the request header and the JSON format for the response body
    * $formatQuery options ignored
    * Deletion will not be executed if the relationship setting exists in the deletion target.

### Required Privileges

auth

<br>

### Request

#### Request URL

```
/{CellName}/__ctl/Role(Name='{RoleName}',_Box.Name='{BoxName}')
```

or

```
/{CellName}/__ctl/Role(Name='{RoleName}')
```

or

```
/{CellName}/__ctl/Role('{RoleName}')
```

If the \_Box.Name parameter is omitted, it is assumed that null is specified

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

#### OData Request Header

|Header Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|Authorization<br>|Specifies authentication information in the OAuth 2.0 format<br>|Bearer {AccessToken}<br>|No<br>|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br>|

#### OData Create Request Header

|Header Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|Content-Type<br>|Specifies the request body format<br>|application/json<br>|No<br>|[application/json] by default<br>|
|Accept<br>|Specifies the response body format<br>|application/json<br>|No<br>|[application/json] by default<br>|

#### Request Body

None

#### Request Sample

None

<br>

### Response

#### Response Code

204

#### Response Header

None

#### Response Body

None

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

None

<br>

### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/Role(Name='{RoleName}',_Box.Name='{BoxName}')" -X DELETE -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED
