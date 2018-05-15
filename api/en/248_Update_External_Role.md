# Update ExtRole

## Overview

update existed ExtRole

### Required Privileges

auth

### Restrictions

* OData Restrictions
    * Accept in the request header is ignored
    * Always handles Content-Type in the request header as application/json
    * Only accepts the request body in the JSON format
    * Only application/json is supported for Content-Type in the request header and the JSON format for the response body
    * $formatQuery options ignored



## Request

### Request URL

```
/{CellName}/__ctl/ExtRole(ExtRole='{ExtRoleURL}',_Relation.Name='{RelationName}',_Relation._Box.Name='{BoxName}')
```

or

```
/{CellName}/__ctl/ExtRole(ExtRole='{ExtRoleURL}',_Relation.Name='{RelationName}')
```

If the \_Relation.\_Box.Name parameter is omitted, it is assumed that null is specified

### Request Method

PUT

### Request Query

|Query Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|p_cookie_peer|Cookie Authentication Value|The cookie authentication value returned from the server during authentication|No|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used|

### Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|Method override function|User-defined|No|If you specify this value when requesting with the POST method, the specified value will be used as a method.|
|X-Override|Header override function|${OverwrittenHeaderName}:${Value}|No|Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.|
|X-Personium-RequestKey|RequestKey field value output in the event log|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters|No|PCS-${UNIXtime} by default|
|Authorization|Specifies authentication information in the OAuth 2.0 format|Bearer {AccessToken}|No|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API|
|Content-Type|Specifies the request body format|application/json|No|[application/json] by default|
|Accept|Specifies the response body format|application/json|No|[application/json] by default|
|If-Match|Specifies the target ETag value|ETag value|No|[*] by default|

### Request Body

|Item Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|ExtRole|External Role Name(URL)|Number of digits: 1-1024<br>Follow URI format<br>scheme:http / https / urn<br>When associated with Box:/{Application CellName}/\_\_role/\_\_/{RoleName}<br>* However, if Schema information is not registered in Box, it is considered not to be associated with Box<br>When not associated with Box:/{CellName}/\_\_role/\_\_/{RoleName}|Yes||
|_Relation.Name|Relation name of relationbr|Number of digits: 1 - 128<br>Character type: Single-byte alphanumeric characters, hyphens ("-"), and underscores ("\_")<br> and +(plus) and :(colon)<br>However, the string cannot start with a underscore ("\_") or colon (:)<br>Relation which registered by Relation register API|Yes||
|_Relation._Box.Name|Box Name aassociated wirh Relation|Number of digits: 1 - 128<br>Character type: Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Box name associated with Relation registered by Relation registration API<br>null|No||

### Request Sample

```JSON
{
  "ExtRole": "https://{UnitFQDN}/{CellName}/__role/__/roletest",
  "_Relation.Name": "{RelationName}",
  "_Relation._Box.Name": "{BoxName}"
}
```


## Response

### Response Code

204

### Response Body

None

### Error Messages

Refer to [Error Message List](004_Error_Messages.md)

## cURL Command

```sh
curl curl "https://{UnitFQDN}/{CellName}/__ctl/ExtRole(ExtRole='https%3A%2F%2F{UnitFQDN}%2F{CellName}
%2F__role%2F__%2F{ExtRoleName}',_Relation.Name='{RelationName}',_Relation._Box.Name='{BoxName}')" \
-X PUT -i -H 'If-Match: *' -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' \
-d '{ "ExtRole": "https://{UnitFQDN}/{CellName}/__role/__/{ExtRoleName}", "_Relation.Name":"{RelationName}",\
"_Relation._Box.Name": "{BoxName}"}'
```


