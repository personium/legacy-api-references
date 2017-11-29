# EntityType $links Registration

### Overview

Register $links information of EntityType

### Required Privileges

alter-schema

### Restrictions

None<br><br>

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

|Header Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|Authorization<br>|Specifies authentication information in the OAuth 2.0 format<br>|Bearer {AccessToken}<br>|No<br>|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br>|
|Accept<br>|Specifies the response body format<br>|application/json<br>|No<br>|[application/json] by default<br>|

#### Request Body

|Item Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|uri<br>|Link to AssociationEnd uri<br>|Present AssociationEnd<br>|Yes<br>|<br>|

#### Request Sample

```JSON
{"uri": "https://{UnitFQDN}/{CellName}/{BoxName}/{OdataCollecitonPath}/$metadata/AssociationEnd(Name='{AssociationEndName}',_EntityType.Name=null)"}
```

<br>

### Response

#### Response Code

204

#### Response Header

|Item Name<br>|Overview<br>|Notes<br>|
|:--|:--|:--|
|DataServiceVersion<br>|Version information of ODataProtocol<br>|Return only when AssociationEnd can be created successfully<br>Operation confirmed<br>|

#### Response Body

None

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

None

<br>

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

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED
