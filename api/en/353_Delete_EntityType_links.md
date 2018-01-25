# EntityType $links remove

## Overview

Delete Entity Type $links information

### Required Privileges

alter-schema

### Restrictions

* Always handles Content-Type in the request header as application/json
* Only accepts the request body in the JSON format
* Only application/json is supported for Content-Type in the request header and the JSON format for the response body
* Response body data is not ensured if atom or xml is specified in the $format query option, although it does not result in an error


## Request

### Request URL

$links with AssociationEnd

```
/{CellName}/{BoxName}/{CollectionName}/EntityType('{EntityTypeName}')/$links
/_AssociationEnd(Name='{AssociationEndName}',_EntityType.Name='{EntityTypeName}')
or
/{CellName}/{BoxName}/{CollectionName}/EntityType('{EntityTypeName}')/$links
/_AssociationEnd(Name='{AssociationEndName}')
or
/{CellName}/{BoxName}/{CollectionName}/EntityType('{EntityTypeName}')/$links
/_AssociationEnd('{AssociationEndName}')
```

### Request Method

DELETE

### Request Query

None

### Request Header

#### Common Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|Method override function|User-defined|No|Specifying this value in a request with the POST method indicates that the specified value is used as the method|
|X-Override|Header override function|${OverwrittenHeaderName}:${Value}|No|The normal HTTP header value is overwritten. Specify multiple X-Override headers for the overwriting of multiple headers|
|X-Personium-RequestKey|RequestKey field value output in the event log|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters|No||

#### OData Common Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|Authorization|Specifies authentication information in the OAuth 2.0 format|Bearer {AccessToken}|No|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API|

#### OData Delete Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|If-Match|Specifies the target ETag value|ETag value|No|[*] by default|

### Request Body

None


## Response

### Response Code

204

### Response Header

#### Common Response Header

|Header Name|Overview|Notes|
|:--|:--|:--|
|X-Personium-Version|API version that the request is processed|Version of the API used to process the request|
|Access-Control-Allow-Origin|Cross domain communication permission header|Return value fixed to "*"|

#### OData Response Header

|Item Name|Overview|Notes|
|:--|:--|:--|
|DataServiceVersion|OData version information||

### Response Body

None

### Error Messages

Refer to [Error Message List](004_Error_Messages.md)


## cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{CollectionName}/$metadata/EntityType
(Name='{EntityTypeName}')/$links/_AssociationEnd(Name='a{AssociationEndName}',_EntityType.Name='{EntityTypeName}')" \
-X DELETE -i -H 'If-Match: *' -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```


