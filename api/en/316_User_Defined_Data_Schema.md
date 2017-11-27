# Get schema (\$ metadata)

### Overview

Retrieve schema information for schema definition of user data

### Required Privileges

read

### Restrictions

None

<br>

### Request

#### Request URL

```
/{CellName}/{BoxName}/{OdataCollectionName}/$metadata
```

#### Request Method

GET

#### Request Query

Common Request Query

|Query Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|p_cookie_peer<br>|Cookie Authentication Value<br>|The cookie authentication value returned from the server during authentication<br>|No<br>|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br>|

#### Request Header

##### Common Request Header

|Header Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|Method override function<br>|User-defined<br>|No<br>|Specifying this value in a request with the POST method indicates that the specified value is used as the method<br>|
|X-Override<br>|Header override function<br>|${OverwrittenHeaderName}:${Value}<br>|No<br>|The normal HTTP header value is overwritten. Specify multiple X-Override headers for the overwriting of multiple headers<br>|
|X-Personium-RequestKey<br>|RequestKey field value output in the event log<br>|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br>|No<br>|Supported in V 1.1.7 and later<br>|

#### Request Body

None

#### Request Sample

None

<br>

### Response

#### Response Code

200

#### Response Header

|Header Name<br>|Overview<br>|Notes<br>|
|:--|:--|:--|
|Content-Type<br>|Format of data to be returned<br>|<br>|
|DataServiceVersion<br>|OData version information<br>|<br>|
|Access-Control-Allow-Origin<br>|Cross domain communication permission header<br>|Return value fixed to "*"<br>|
|X-Personium-Version<br>|API version that the request is processed<br>|Version of the API used to process the request<br>|

#### Response Body

Response sample reference

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

```xml
<?xml version="1.0" encoding="utf-8"?>
<edmx:Edmx Version="1.0" xmlns:edmx="http://schemas.microsoft.com/ado/2007/06/edmx" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns:p="urn:x-personium:xmlns">
  <edmx:DataServices m:DataServiceVersion="1.0">
    <Schema xmlns="http://schemas.microsoft.com/ado/2006/04/edm" Namespace="UserData">
      <ComplexType Name="TestComplexType">
        <Property Name="TestComplexTypeProperty" Type="Edm.String" Nullable="true"></Property>
      </ComplexType>
      <EntityType Name="TestEntity" OpenType="true">
        <Key>
          <PropertyRef Name="__id"></PropertyRef>
        </Key>
        <Property Name="__id" Type="Edm.String" Nullable="false" DefaultValue="UUID()" p:Format="regEx('^[a-zA-Z0-9][a-zA-Z0-9-_:]{0,199}$')"></Property>
        <Property Name="__published" Type="Edm.DateTime" Nullable="false" DefaultValue="SYSUTCDATETIME()" Precision="3"></Property>
        <Property Name="__updated" Type="Edm.DateTime" Nullable="false" DefaultValue="SYSUTCDATETIME()" Precision="3"></Property>
        <Property Name="TestProperty" Type="Edm.String" Nullable="true"></Property>
        <NavigationProperty Name="_TestEntity" Relationship="UserData.TestEntity-TestEntity-assoc" FromRole="TestEntity:TestAssociationEndFrom" ToRole="TestEntity:TestAssociationEndTo"></NavigationProperty>
      </EntityType>
      <Association Name="TestEntity-TestEntity-assoc">
        <End Role="TestEntity:TestAssociationEndFrom" Type="UserData.TestEntity" Multiplicity="1"></End>
        <End Role="TestEntity:TestAssociationEndTo" Type="UserData.TestEntity" Multiplicity="0..1"></End>
      </Association>
      <EntityContainer Name="UserData" m:IsDefaultEntityContainer="true">
        <EntitySet Name="TestEntity" EntityType="UserData.TestEntity"></EntitySet>
        <AssociationSet Name="TestEntity-TestEntity-assoc" Association="UserData.TestEntity-TestEntity-assoc">
          <End Role="TestEntity:TestAssociationEndFrom" EntitySet="TestEntity"></End>
          <End Role="TestEntity:TestAssociationEndTo" EntitySet="TestEntity"></End>
        </AssociationSet>
      </EntityContainer>
    </Schema>
  </edmx:DataServices>
</edmx:Edmx>
```

<br>

### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{OdataCollectionName}/\$metadata" -X GET -i -H 'Authorization: Bearer {AccessToken}'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED