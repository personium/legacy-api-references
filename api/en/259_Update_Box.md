# Box Update

### Overview

Update existing Box information

### Required Privileges

None

### Restrictions

* Accept in the request header is ignored
* Always handles Content-Type in the request header as application/json
* Only accepts the request body in the JSON format
* Only application/json is supported for Content-Type in the request header and the JSON format for the response body
* $formatQuery options ignored

<br>

### Request

#### Request URL

```
/{CellName}/__ctl/Box('{BoxName}')
```

or

```
/{CellName}/__ctl/Box(Name='{BoxName}')
```

#### Request Method

PUT

#### Request Query

|Query Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|p_cookie_peer<br>|Cookie Authentication Value<br>|The cookie authentication value returned from the server during authentication<br>|No<br>|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br>|

#### Request Header

|Header Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|Method override function<br>|User-defined<br>|No<br>|If you specify this value when requesting with the POST method, the specified value will be used as a method.<br>|
|X-Override<br>|Header override function<br>|${OverwrittenHeaderName}:${Value}override} $: $ {value}<br>|No<br>|Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.<br>|
|X-Personium-RequestKey<br>|RequestKey field value output in the event log<br>|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br>|No<br>|PCS-${UNIXtime} by default<br>Supported in V 1.1.7 and later<br>|
|Authorization<br>|Specifies authentication information in the OAuth 2.0 format<br>|Bearer {AccessToken}<br>|No<br>|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br>|
|Contents-Type<br>|Specifies the request body format<br>|application/json<br>|No<br>|[application/json] by default<br>|
|Accept<br>|Specifies the response body format<br>|application/json<br>|No<br>|[application/json] by default<br>|
|If-Match<br>|Specifies the target ETag value<br>|ETag value<br>|Yes<br>|<br>|

#### Request Body

##### Format

JSON

|Item Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|Name<br>|Box Name<br>|Number of digits: 1 - 128<br>Character type: Single-byte alphanumeric characters, hyphens ("-"), and underscores ("\_")<br>However, the string cannot start with a single-byte hyphen ("-") or underscore ("\_")<br>null<br>|Yes<br>|<br>|
|Schema<br>|Schema Name<br>|Number of digits: 1 - 128<br>Character type: Single-byte alphanumeric characters, hyphens ("-"), and underscores ("\_")<br>However, the string cannot start with a single-byte hyphen ("-") or underscore ("\_")<br>null<br>|No<br>|<br>|

#### Request Sample

```JSON
{"Name":"{BoxName}", "Schema":"https://{UnitFQDN}/{CellName}/"}
```

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
curl "https://{UnitFQDN}/{CellName}/__ctl/Box('{BoxName}')" -X PUT -i -H 'If-Match: *' -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"Name":"{BoxName}"}'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED
