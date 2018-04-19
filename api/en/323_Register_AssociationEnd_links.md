# Link other objects with AssociationEnd

## Overview

Register link information of AssociationEnd of user data schema

### Required Privileges

alter-schema


## Request

### Request URL

```
/{CellName}/{BoxName}/{ODataCollecitonName}/$metadata/AssociationEnd(Name='{AssociationEndName}', 
_EntityType.Name='{EntityTypeName}')/$links/_AssociationEnd
```

### Request Method

POST

### Request Query

None

### Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|Authorization|Specifies authentication information in the OAuth 2.0 format|Bearer {AccessToken}|No|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API|
|Accept|Specifies the response body format|application/json|No|[application/json] by default|

### Request Body

|Item Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|uri|Link to AssociationEnd uri|Present AssociationEnd|Yes||

### Request Sample

```JSON
{"uri": "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/$metadata/AssociationEnd
(Name='{AssociationEndName}',_EntityType.Name='{EntityTypeName}')"}
```


## Response

### Response Code

204

### Response Header

|Item Name|Overview|Notes|
|:--|:--|:--|
|DataServiceVersion|Version information of ODataProtocol|Return only when AssociationEnd can be created successfully|

### Response Body

None

### Error Messages

Refer to [Error Message List](004_Error_Messages.md)

### Response Sample

None


## cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/\$metadata/AssociationEnd
(Name='{AssociationEndName}',_EntityType.Name='{EntityTypeName}')/\$links/_AssociationEnd" -X POST \
-i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -H 'Accept:application/json' \
-d "{\"uri\": \"https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/\$metadata/AssociationEnd
(Name='{AssociationEndName}',_EntityType.Name='{EntityTypeName}')\"}"
```


