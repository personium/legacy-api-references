# Role\_$links delete

### Overview

Delete a list of OData resources associated with Role

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

##### Correlating with the Account

```
/{CellName}/__ctl/Role(Name='{RoleName}',_Box.Name='{BoxName}')/$links/_Account(Name='{AccountName}')
```

or

```
/{CellName}/__ctl/Role(Name='{RoleName}',_Box.Name='{BoxName}')/$links/_Account('{AccountName}')
```

or

```
/{CellName}/__ctl/Role(Name='{RoleName}')/$links/_Account(Name='{AccountName}')
```

or

```
/{CellName}/__ctl/Role(Name='{RoleName}')/$links/_Account('{AccountName}')
```

or

```
/{CellName}/__ctl/Role('{RoleName}')/$links/_Account(Name='{AccountName}')
```

or

```
/{CellName}/__ctl/Role('{RoleName}')/$links/_Account('{AccountName}')
```

##### Correlating with ExtCell

```
/{CellName}/__ctl/Role(Name='{RoleName}',_Box.Name='{BoxName}')/$links/_ExtCell(Url='{ExtCellURL}')
```

or

```
/{CellName}/__ctl/Role(Name='{RoleName}',_Box.Name='{BoxName}')/$links/_ExtCell('{ExtCellURL}')
```

or

```
/{CellName}/__ctl/Role('{RoleName}')/$links/_ExtCell(Url='{ExtCellURL}')
```

or

```
/{CellName}/__ctl/Role('{RoleName}')/$links/_ExtCell('{ExtCellURL}')
```

##### Correlating with ExtRole

```
/{CellName}/__ctl/Role(Name='{RoleName}',_Box.Name='{BoxName}')/$links/_ExtRole(ExtRole='{ExtRoleURL}',_Relation.Name='{RelationName}',_Relation._Box.Name='{BoxName}')
```

or

```
/{CellName}/__ctl/Role(Name='{RoleName}',_Box.Name='{BoxName}')/$links/_ExtRole(ExtRole='{ExtRoleURL}',_Relation.Name='{RelationName}')
```

or

```
/{CellName}/__ctl/Role(Name='{RoleName}',_Box.Name='{BoxName}')/$links/_ExtRole(ExtRole='{ExtRoleURL}')
```

or

```
/{CellName}/__ctl/Role(Name='{RoleName}',_Box.Name='{BoxName}')/$links/_ExtRole('{ExtRoleURL}')
```

or

```
/{CellName}/__ctl/Role(Name='{RoleName}')/$links/_ExtRole(ExtRole='{ExtRoleURL}',_Relation.Name='{RelationName}',_Relation._Box.Name='{BoxName}')
```

or

```
/{CellName}/__ctl/Role(Name='{RoleName}')/$links/_ExtRole(ExtRole='{ExtRoleURL}',_Relation.Name='{RelationName}')
```

or

```
/{CellName}/__ctl/Role(Name='{RoleName}')/$links/_ExtRole(ExtRole='{ExtRoleURL}')
```

or

```
/{CellName}/__ctl/Role(Name='{RoleName}')/$links/_ExtRole('{ExtRoleURL}')
```

or

```
/{CellName}/__ctl/Role('{RoleName}')/$links/_ExtRole(ExtRole='{ExtRoleURL}',_Relation.Name='{RelationName}',_Relation._Box.Name='{BoxName}')
```

or

```
/{CellName}/__ctl/Role('{RoleName}')/$links/_ExtRole(ExtRole='{ExtRoleURL}',_Relation.Name='{RelationName}')
```

or

```
/{CellName}/__ctl/Role('{RoleName}')/$links/_ExtRole(ExtRole='{ExtRoleURL}')
```

or

```
/{CellName}/__ctl/Role('{RoleName}')/$links/_ExtRole('{ExtRoleURL}')
```

##### Relation with linking

```
/{CellName}/__ctl/Role(Name='{RoleName}',_Box.Name='{BoxName}')/$links/_Relation(Name='{RelationName}',_Box.Name='{BoxName}')
```

or

```
/{CellName}/__ctl/Role(Name='{RoleName}',_Box.Name='{BoxName}')/$links/_Relation(Name='{RelationName}')
```

or

```
/{CellName}/__ctl/Role(Name='{RoleName}',_Box.Name='{BoxName}')/$links/_Relation('{RelationName}')
```

or

```
/{CellName}/__ctl/Role(Name='{RoleName}')/$links/_Relation(Name='{RelationName}',_Box.Name='{BoxName}')
```

or

```
/{CellName}/__ctl/Role(Name='{RoleName}')/$links/_Relation(Name='{RelationName}')
```

or

```
/{CellName}/__ctl/Role(Name='{RoleName}')/$links/_Relation('{RelationName}')
```

or

```
/{CellName}/__ctl/Role('{RoleName}')/$links/_Relation(Name='{RelationName}',_Box.Name='{BoxName}')
```

or

```
/{CellName}/__ctl/Role('{RoleName}')/$links/_Relation(Name='{RelationName}')
```

or

```
/{CellName}/__ctl/Role('{RoleName}')/$links/_Relation('{RelationName}')
```

If the \_Box.Name parameter is omitted, it is assumed that null is specified  
\* The ExCel key specifies the URL-encoded character string

#### Request Method

##### DELETE

#### Request Query

|Query Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|p_cookie_peer|Cookie Authentication Value|The cookie authentication value returned from the server during authentication|No|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used|

#### Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|Method override function|User-defined|No|If you specify this value when requesting with the POST method, the specified value will be used as a method.|
|X-Override|Header override function|${OverwrittenHeaderName}:${Value} override} $: $ {value}|No|Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.|
|X-Personium-RequestKey|RequestKey field value output in the event log|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters|No|PCS-${UNIXtime} by default|
|Authorization|Specifies authentication information in the OAuth 2.0 format|Bearer {AccessToken}|No|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API|
|If-Match|Specifies the target ETag value|ETag value|No|[*] by default|

#### Request Body

None

#### Request Sample

None

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

Refer to [Error Message List](004_Error_Messages.md)

#### Response Sample

None


### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/Role(Name='{RoleName}',_Box.Name='{BoxName}')/\$links/_Box('{BoxName}')" -X DELETE -i -H 'Authorization: Bearer {AccessToken}'
```


##### Copyright 2017 FUJITSU LIMITED
