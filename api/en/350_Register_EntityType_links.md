# EntityType \$links Registration

### Overview

Register \$links information of EntityType

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