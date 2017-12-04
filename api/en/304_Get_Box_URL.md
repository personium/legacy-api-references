# Get BoxURL

### Overview

It is a resource used to obtain the URL of Box. The application that performed the schema authentication redirects to the Box URL for the schema - authenticated application by accessing this resource with an access token.  
An application that does not support schema authentication redirects to the URL of the corresponding Box by giving schema url as a parameter.

### Required Privileges

Access control of this resource depends on ACL of Box route. It is necessary to satisfy the following two points.

* If the requireSchemaAuthz attribute of the Box root ACL is not none (public, confidential) it must be schema authenticated
* The user can read the Box route. (User authentication is unnecessary when the Box route is open to the public)

\*For the requireSchemaAuthz attribute of the ACL, see "Schema Privilege Request Level" in the [access control model](../../user_guide/002_Access_Control.html).

### Restrictions

None

<br>

### Request

#### Request URL

```
/{CellName}/__box
```

#### Request Method

GET

#### Request Query

|Query Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|p_cookie_peer<br>|Cookie Authentication Value<br>|The cookie authentication value returned from the server during authentication<br>|No<br>|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br>|
|schema<br>|App cell URL<br>|URL format<br>|No<br>|Number of digits: 1-1024<br>Follow URI format<br>|

#### Request Header

|Header Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|Method override function<br>|User-defined<br>|No<br>|If you specify this value when requesting with the POST method, the specified value will be used as a method.<br>|
|X-Override<br>|Header override function<br>|${OverwrittenHeaderName}:${Value} override} $: $ {value}<br>|No<br>|Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.<br>|
|X-Personium-RequestKey<br>|RequestKey field value output in the event log<br>|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br>|No<br>|Supported in V 1.1.7 and later<br>|
|Authorization<br>|Specifies authentication information in the OAuth 2.0 format<br>|Bearer {AccessToken}<br>|No<br>|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br>|

#### Request Body

None

#### Request Sample

None

<br>

### Response

#### Response Code

|Code<br>|Message<br>|Overview<br>|
|:--|:--|:--|
|200<br>|FOUND<br>|Acquisition success<br>|

#### Response Header

|Header Name<br>|Overview<br>|Notes<br>|
|:--|:--|:--|
|Location<br>|URL for Box metadata acquisition API<br>|Depending on the request specification method, the following values are obtained<br>Specify schema query<br>chema Box URL corresponding to the app cell URL specified in the query<br>Specify only Authorization header without schema query<br>Box URL corresponding to the schema URL included in the token<br>|

Location sample

```
Location:https://{UnitFQDN}/{CellName}/{BoxName}
```

#### Response Body

None

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

```
Location:https://{UnitFQDN}/{CellName}/{BoxName}
```

<br>

### cURL Command

#### Schema authenticated

```sh
curl "https://{UnitFQDN}/{CellName}/__box" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```

#### Schema authentication not supported

```sh
curl "https://{UnitFQDN}/{CellName}/__box?schema=https://{UnitFQDN}/{CellName}/" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED
