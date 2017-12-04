# Role\_$links registration

### Overview

Link Role to the OData resource specified by $links

* Account
* ExtCell
* ExtRole
* Relation

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

##### Association with the account

```
/{CellName}/__ctl/Role(Name='{RoleName}',_Box.Name='{BoxName}')/$links/_Account
```

or

```
/{CellName}/__ctl/Role(Name='{RoleName}')/$links/_Account
```

or

```
/{CellName}/__ctl/Role('{RoleName}')/$links/_Account
```

##### Association with the ExtCell

```
/{CellName}/__ctl/Role(Name='{RoleName}',_Box.Name='{BoxName}')/$links/_ExtCell
```

or

```
/{CellName}/__ctl/Role(Name='{RoleName}')/$links/_ExtCell
```

or

```
/{CellName}/__ctl/Role('{RoleName}')/$links/_ExtCell
```

##### Association with the ExtRole

```
/{CellName}/__ctl/Role(Name='{RoleName}',_Box.Name='{BoxName}')/$links/_ExtRole
```

or

```
/{CellName}/__ctl/Role(Name='{RoleName}')/$links/_ExtRole
```

or

```
/{CellName}/__ctl/Role('{RoleName}')/$links/_ExtRole
```

##### Association with the relation

```
/{CellName}/__ctl/Role(Name='{RoleName}',_Box.Name='{BoxName}')/$links/_Relation
```

or

```
/{CellName}/__ctl/Role(Name='{RoleName}')/$links/_Relation
```

or

```
/{CellName}/__ctl/Role('{RoleName}')/$links/_Relation
```

If the \_Box.Name parameter is omitted, it is assumed that null is specified

#### Request Method

POST

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
|Content-Type<br>|Specifies the request body format<br>|application/json<br>|No<br>|[application/json] by default<br>|
|Accept<br>|Specifies the response body format<br>|application/json<br>|No<br>|[application/json] by default<br>|

#### Request Body

##### Format

JSON

|Item Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|uri<br>|URI of the OData resource to be linked<br>|Number of digits: 1-1024<br>Follow URI format<br>scheme:http / https / urn<br>|Yes<br>|<br>|

#### Request Sample

```JSON
{"uri":"https://{UnitFQDN}/Cell/__ctl/Box('{BoxName}')"}
```

<br>

### Response

#### Response Code

204

#### Response Header

|Header Name<br>|Overview<br>|Notes<br>|
|:--|:--|:--|
|DataServiceVersion<br>|OData version<br>|<br>|
|Access-Control-Allow-Origin<br>|Cross domain communication permission header<br>|Return value fixed to "*"<br>|
|X-Personium-Version<br>|API version that the request is processed<br>|Version of the API used to process the request<br>|

#### Response Body

None

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

None<br><br>

### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/Role('{RoleName}')/\$links/_Box" -X POST -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d "{\"uri\":\"https://{UnitFQDN}/{CellName}/__ctl/Box('{BoxName}')\"}"
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED
