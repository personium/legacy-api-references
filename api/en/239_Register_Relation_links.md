# Relation\_$links registration

### Overview

Link Relation to the OData resource specified by $links<br>Get a list of OData resources linked with following

* Box
* ExtCell
* ExtRole
* Role

Only when the \_Box.Name described in the request URL is null (or if it is not specified) can be associated with Box

### Required Privileges

None

### Restrictions

* Accept in the request header is ignored
* Always handles Content-Type in the request header as application/json
* Only accepts the request body in the JSON format
* Only application/json is supported for Content-Type in the request header and the JSON format for the response body
* $formatQuery options ignored


### Request

#### Request URL

##### Correlating with Box

```
/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/$links/_Box
```

or

```
/{CellName}/__ctl/Relation(Name='{RelationName}')/$links/_Box
```

or

```
/{CellName}/__ctl/Relation('{RelationName}')/$links/_Box
```

##### Correlating with ExtCell

```
/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/$links/_ExtCell
```

or

```
/{CellName}/__ctl/Relation(Name='{RelationName}')/$links/_ExtCell
```

or

```
/{CellName}/__ctl/Relation('{RelationName}')/$links/_ExtCell
```

##### Correlating with ExtRole

```
/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/$links/_ExtRole
```

or

```
/{CellName}/__ctl/Relation(Name='{RelationName}')/$links/_ExtRole
```

or

```
/{CellName}/__ctl/Relation('{RelationName}')/$links/_ExtRole
```

##### Correlating with the role

```
/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/$links/_Role
```

or

```
/{CellName}/__ctl/Relation(Name='{RelationName}')/$links/_Role
```

or

```
/{CellName}/__ctl/Relation('{RelationName}')/$links/_Role
```

If the \_Box.Name parameter is omitted, it is assumed that null is specified

#### Request Method

POST

#### Request Query

|Query Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|p_cookie_peer|Cookie Authentication Value|The cookie authentication value returned from the server during authentication|No|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used|

#### Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|Method override function|User-defined|No|If you specify this value when requesting with the POST method, the specified value will be used as a method.|
|X-Override|Header override function|${OverwrittenHeaderName}:${Value}|No|Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.|
|X-Personium-RequestKey|RequestKey field value output in the event log|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters|No|PCS-${UNIXtime} by default<br>Supported in V 1.1.7 and later|
|Authorization|Specifies authentication information in the OAuth 2.0 format|Bearer {AccessToken}|No|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API|
|Content-Type|Specifies the request body format|application/json|No|[application/json] by default|
|Accept|Specifies the response body format|application/json|No|[application/json] by default|

#### Request Body

##### Format

JSON

|Item Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|uri|URI of the OData resource to be linked|Number of digits: 1-1024<br>Follow URI format<br>scheme:http / https / urn|Yes||

#### Request Sample

```JSON
{"uri":"https://{UnitFQDN}/{CellName}/__ctl/Role(Name='{RoleName}',_Box.Name='{BoxName}')"}
```


### Response

#### Response Code

204

#### Response Header

|Header Name|Overview|Notes|
|:--|:--|:--|
|DataServiceVersion|OData version||
|Access-Control-Allow-Origin|Cross domain communication permission header|Return value fixed to "*"|
|X-Personium-Version|API version that the request is processed|Version of the API used to process the request|

#### Response Body

None

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

None


### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/\$links/_Role" -X POST -i  -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d "{\"uri\":\"https://{UnitFQDN}/{CellName}/__ctl/Role(Name='{RoleName}',_Box.Name='{BoxName}')\"}"
```


###### Copyright 2017 FUJITSU LIMITED
