# EntityType $links Registration

### Overview

Register $links information of EntityType

### Required Privileges

alter-schema

### Restrictions

None

### Request

#### Request URL

```
EntityType
/{CellName}/{BoxName}/{OdataCollecitonPath}/$metadata/EntityType(Name='{EntityTypeName}')/$links/_AssociationEnd
AssociationEnd
/{CellName}/{BoxName}/{OdataCollecitonPath}/$metadata/AssociationEnd(Name='{AssociationEndName}', _EntityType.Name='{EntityTypeName}')/$links/_AssociationEnd
```

#### Request Method

POST

#### Request Query

None

#### Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|Authorization|Specifies authentication information in the OAuth 2.0 format|Bearer {AccessToken}|No|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API|
|Accept|Specifies the response body format|application/json|No|[application/json] by default|

#### Request Body

|Item Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|uri|Link to AssociationEnd uri|Present AssociationEnd|Yes||

#### Request Sample

```JSON
{"uri": "https://{UnitFQDN}/{CellName}/{BoxName}/{OdataCollecitonPath}/$metadata/AssociationEnd(Name='{AssociationEndName}',_EntityType.Name=null)"}
```


### Response

#### Response Code

204

#### Response Header

|Item Name|Overview|Notes|
|:--|:--|:--|
|DataServiceVersion|Version information of ODataProtocol|Return only when AssociationEnd can be created successfully<br>Operation confirmed|

#### Response Body

None

#### Error Messages

Refer to [Error Message List](004_Error_Messages.md)

#### Response Sample

None


### cURL Command

EntityType

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{OdataCollecitonPath}/$metadata/EntityType(Name='{EntityTypeName}')/$links/_AssociationEnd" -X POST -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -H 'Accept:application/json'
-d '{"uri": "https://{UnitFQDN}/{CellName}/{BoxName}/{OdataCollecitonPath}/$metadata/AssociationEnd(Name='{AssociationEndName}_link',_EntityType.Name=null)"}'
```

AssociationEnd

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{OdataCollecitonPath}/$metadata/AssociationEnd(Name='{AssociationEndName}2',_EntityType.Name=Entity)/$links/_AssociationEnd" -X POST -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -H 'Accept:application/json' -d '{"uri": "https://{UnitFQDN}/{CellName}/{BoxName}/{OdataCollecitonPath}/$metadata/AssociationEnd(Name='{AssociationEndName}_link',_EntityType.Name=Entity2)"}'
```


###### Copyright 2017 FUJITSU LIMITED
