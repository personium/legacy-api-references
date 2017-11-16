# AssociationEnd \$links Registration

### Overview

Register link information of AssociationEnd of user data schema

### Required Privileges

alter-schema

### Restrictions

None

<br>

### Request

#### Request URL

```
/{CellName}/{BoxName}/{ODataCollecitonName}/$metadata/AssociationEnd(Name='{AssociationEndName}', _EntityType.Name='{EntityTypeName}')/$links/_AssociationEnd
```

#### Request Method

POST

#### Request Query