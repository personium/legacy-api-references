# Schema acquisition (\$metadata)

### Overview

Retrieve schema information

### Required Privileges

read

### Restrictions

* The normal type of the response body is XML format, and the response header Content-Type is application/xml
* The abnormality system of the response body is assumed to be JSON format, and the Content-Type of the response header is set to application/json
* If $format is atomsvc or Accept header is application/atomsvc+xml, Schema's Atom ServiceDocument is returned
* If $format is not atomsvc and Accept header is not application/atomsvc+xml, EDMX of user data is returned
* ComplexType and Documentation tags do not correspond and do not return

<br>

### Request

#### Request URL

```
/{CellName}/{BoxName}/{odataname}/$metadata
```

#### Request Method

GET

#### Request Query

##### Common Query

| Query Name<br>    | Overview<br>                    | Effective Value<br>                                                                | Required<br> | Notes<br>                                                                                                                |
|:-- |:-- |:-- |:-- |:-- |
| p_cookie_peer<br> | Cookie Authentication Value<br> | The cookie authentication value returned from the server during authentication<br> | No<br>       | Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br> |

##### Individual Query

If atomsvc is specified for \$format, return Schema's Atom ServiceDocument<br>
Ignore others

#### Request Header

##### Common Request Header

| Header Name<br>            | Overview<br>                                       | Effective Value<br>                                                                                        | Required<br> | Notes<br>                                                                                                                    |
|:-- |:-- |:-- |:-- |:-- |
| X-HTTP-Method-Override<br> | Method override function<br>                       | User-defined<br>                                                                                           | No<br>       | Specifying this value in a request with the POST method indicates that the specified value is used as the method<br>         |
| X-Override<br>             | Header override function<br>                       | ${OverwrittenHeaderName}:${Value}<br>                                                                      | No<br>       | The normal HTTP header value is overwritten. Specify multiple X-Override headers for the overwriting of multiple headers<br> |
| X-Personium-RequestKey<br> | RequestKey field value output in the event log<br> | Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br> | No<br>       | Supported in V 1.1.7 and later<br>                                                                                           |

##### Individual request header

| Header Name<br>   | Overview<br>                                                     | Effective Value<br>                            | Required<br> | Notes<br>                                                                                          |
|:-- |:-- |:-- |:-- |:-- |
| Accept<br>        | Format of data to be returned<br>                                | application/atomsvc+xml<br>application/xml<br> | No<br>       | If not specified, the schema information of the user data schema will be acquired<br>              |
| Authorization<br> | Specifies authentication information in the OAuth 2.0 format<br> | Bearer {Authentication token}<br>              | No<br>       | * Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br> |

#### Request Body

None

#### Request Sample

None

<br>

### Response

#### Response Code

200

#### Response Header

##### Common Response Header

| Header Name<br>                 | Overview<br>                                     | Notes<br>                                                 |
|:-- |:-- |:-- |
| Access-Control-Allow-Origin<br> | Cross domain communication permission header<br> | Return value fixed to "*"<br>                             |
| X-Personium-Version<br>         | API version that the request is processed<br>    | If not specified, the latest API version is specified<br> |

##### Schema acquisition specific response header

| Header Name<br>        | Overview<br>                      | Notes<br> |
|:-- |:-- |:-- |
| Content-Type<br>       | Format of data to be returned<br> | <br>      |
| DataServiceVersion<br> | OData version<br>                 | <br>      |

#### Response Body

##### For user data

| URI<br>                                                            | Overview<br>                             | Reference prefix<br> |
|:-- |:-- |:-- |
| http://schemas.microsoft.com/ado/2007/06/edmx<br>                  | edmx namespace<br>                       | edmx:<br>            |
| http://schemas.microsoft.com/ado/2007/08/dataservices<br>          | WCF Data Services namespace<br>          | d:<br>               |
| http://schemas.microsoft.com/ado/2007/08/dataservices/metadata<br> | WCF Data Services metadata namespace<br> | m:<br>               |
| urn:x-personium:xmlns<br>                                          | Personium namespace <br>                 | p:<br>               |
| http://schemas.microsoft.com/ado/2006/04/edm<br>                   | scheme namespace<br>                     | -<br>                |

\* Reference The prefixes are for making it easier to read the following table, but the use of these prefix strings is not ensured or requested.

##### Structure of XML

The body is XML (edmx) and follows the following schema.

| Node name<br>                | Namespace<br> | Node type<br>  | Overview<br>                                                                                                                                                | Notes<br>                                                       |
|:-- |:-- |:-- |:-- |:-- |
| Edmx <br>                    | edmx:<br>     | Element<br>    | Represents the root of the EDMX document and always has one DataServices element in the child<br>                                                           | <br>                                                            |
| Version<br>                  | edmx:<br>     | Attributes<br> | Represents the version of the EDMX document. The attribute value is always '1.0'<br>                                                                        | <br>                                                            |
| DataServices<br>             | edmx:<br>     | Element<br>    | Represents a data service, with multiple Schema elements for children<br>                                                                                   | <br>                                                            |
| DataServiceVersion<br>       | m:<br>        | Attributes<br> | Represents the version of the data service. The attribute value is always '1.0'<br>                                                                         | <br>                                                            |
| Schema<br>                   | -<br>         | Element<br>    | It represents the top-level element of the CSDL document, Association / ComplexType <br>EntityType / EntityContainer / Having elements of Documentation<br> | <br>                                                            |
| Namespace<br>                | -<br>         | Attributes<br> | A mandatory attribute representing the name of the schema.<br>                                                                                              | You can not use System, Transient, or Edm as the value<br>      |
| ComplexType<br>              | -<br>         | Element<br>    | It represents a complex type, and has a child Documentation / Property element<br>                                                                          | <br>                                                            |
| Name<br>                     | -<br>         | Attributes<br> | Represents a compound schema name, mandatory attribute<br>                                                                                                  | Required only if ComplexType exists in the OData collection<br> |
| Abstract<br>                 | -<br>         | Attributes<br> | Represents data in the form of a property<br>                                                                                                               | Displayed only when Abstract exists<br>                         |
| Documentation<br>            | -<br>         | Element<br>    | Represent a compound type document<br>                                                                                                                      | Display only when it exists<br>                                 |
| EntityType<br>               | -<br>         | Element<br>    | It represents an entity type and has children elements<br>Documentation / HasStream / BaseType / Key / Property / NavigationProperty<br>                    | <br>                                                            |
| HasStream<br>                | -<br>         | Attributes<br> | Represents whether an entity is associated with a media resource stream<br>                                                                                 | <br>                                                            |
| BaseType<br>                 | -<br>         | Attributes<br> | Represents the basic type of an entity type<br>                                                                                                             | <br>                                                            |
| Documentation<br>            | -<br>         | Element<br>    | Represent document of entity type<br>                                                                                                                       | <br>                                                            |
| Key<br>                      | -<br>         | Element<br>    | It represents a key of an entity, and one or more PropertyRefs are always children<br>                                                                      | <br>                                                            |
| PropertyRef<br>              | -<br>         | Element<br>    | Represents the reference property of a key<br>                                                                                                              | <br>                                                            |
| Name<br>                     | -<br>         | Attributes<br> | Represents the name of the property being referenced and is a required attribute<br>                                                                        | <br>                                                            |
| NavigationProperty<br>       | -<br>         | Element<br>    | Represents a NavigationProperty<br>                                                                                                                         | <br>                                                            |
| Name<br>                     | -<br>         | Attributes<br> | It represents the name of the NavigationProperty and it is a required attribute<br>                                                                         | <br>                                                            |
| Documentation<br>            | -<br>         | Element<br>    | Represents a document of NavigationProperty<br>                                                                                                             | <br>                                                            |
| Relationship<br>             | -<br>         | Attributes<br> | Represents the name of the association within the scope of the model and is a required attribute<br>                                                        | <br>                                                            |
| FromRole<br>                 | -<br>         | Attributes<br> | Represents a referencing role<br>                                                                                                                           | <br>                                                            |
| ToRole<br>                   | -<br>         | Attributes<br> | Represents a referenced role<br>                                                                                                                            | <br>                                                            |
| Association<br>              | -<br>         | Element<br>    | It represents an Association, and has child element of Documentation / End<br>                                                                              | <br>                                                            |
| Name<br>                     | -<br>         | Attributes<br> | Represents an AssociationName, mandatory attribute<br>                                                                                                      | <br>                                                            |
| Documentation<br>            | -<br>         | Element<br>    | Represent document of Association<br>                                                                                                                       | Display only when it exists<br>                                 |
| End<br>                      | -<br>         | Element<br>    | Represents the name of the AssociationEnd, mandatory attribute<br>                                                                                          | <br>                                                            |
| Role<br>                     | -<br>         | Attributes<br> | Represents the name of the AssociationEnd, mandatory attribute<br>                                                                                          | <br>                                                            |
| Type<br>                     | -<br>         | Attributes<br> | Represents the type of AssociationEnd, mandatory attribute<br>                                                                                              | <br>                                                            |
| Multiplicity<br>             | -<br>         | Attributes<br> | Represents the multiplicity of AssociationEnd, mandatory attribute<br>                                                                                      | <br>                                                            |
| EntityContainer<br>          | -<br>         | Element<br>    | It represents an entity container and has child elements <br>Documentation / EntitySet / FunctionImport / AssociationSet<br>                                | <br>                                                            |
| Name<br>                     | -<br>         | Attributes<br> | Represents an EntityContainer name, mandatory attribute<br>                                                                                                 | <br>                                                            |
| IsDefaultEntityContainer<br> | m:<br>        | Attributes<br> | Represents the default EntityContainer flag<br>                                                                                                             | <br>                                                            |
| Documentation<br>            | -<br>         | Element<br>    | Represent document of document of EntityContainer<br>                                                                                                       | <br>                                                            |
| EntitySet<br>                | -<br>         | Element<br>    | Represents an EntitySet and has child elements of Documentation<br>                                                                                         | <br>                                                            |
| Documentation<br>            | -<br>         | Element<br>    | Represent document of document of EntityContainer<br>                                                                                                       | <br>                                                            |
| Name<br>                     | -<br>         | Attributes<br> | Represents an EntitySet and has child elements of Documentation<br>                                                                                         | <br>                                                            |
| EntityType<br>               | -<br>         | Attributes<br> | Represents the EntityType of an entity set<br>                                                                                                              | <br>                                                            |
| Name<br>                     | -<br>         | Attributes<br> | Represents an EntitySet name, mandatory attribute<br>                                                                                                       | <br>                                                            |
| FunctionImport<br>           | -<br>         | Element<br>    | Represents a FunctionImport (model declaration function)<br>                                                                                                | <br>                                                            |
| Name<br>                     | -<br>         | Attributes<br> | Represents a FunctionImport name, mandatory attribute<br>                                                                                                   | <br>                                                            |
| EntitySet<br>                | -<br>         | Attributes<br> | Represents an entity set of FunctionImport and has elements of Documentation as children<br>                                                                | Repeat for existing entity sets<br>                             |
| ReturnType<br>               | -<br>         | Attributes<br> | Represents the return type<br>                                                                                                                              | <br>                                                            |
| HttpMethod<br>               | m:<br>        | Attributes<br> | Represent method, mandatory attribute<br>                                                                                                                   | When FunctionImport exists<br>                                  |
| Documentation<br>            | -<br>         | Element<br>    | Represent document of FunctionImport<br>                                                                                                                    | <br>                                                            |
| Parameter<br>                | -<br>         | Element<br>    | Represents FunctionImport arguments<br>                                                                                                                     | <br>                                                            |
| Name<br>                     | -<br>         | Attributes<br> | It represents the argument name of the FunctionImport, and mandatory items<br>                                                                              | When FunctionImport, Parameter exists<br>                       |
| Type<br>                     | -<br>         | Attributes<br> | It represents the argument type of FunctionImport, and mandatory items<br>                                                                                  | When FunctionImport, Parameter exists<br>                       |
| Mode<br>                     | -<br>         | Attributes<br> | Represents the mode of the FunctionImport argument, valid values: 'In', 'Out', or 'InOut'<br>                                                               | <br>                                                            |
| Documentation<br>            | -<br>         | Element<br>    | Represent document of FunctionImport argument<br>                                                                                                           | Display only when it exists<br>                                 |
| AssociationSet<br>           | -<br>         | Element<br>    | Represents an AssociationSet and has Documentation and two Ends in its child<br>                                                                            | <br>                                                            |
| Name<br>                     | -<br>         | Attributes<br> | Represents an AssociationSet name, mandatory attribute<br>                                                                                                  | <br>                                                            |
| Association<br>              | -<br>         | Attributes<br> | Represents an AssociationSet association, mandatory attributes<br>                                                                                          | <br>                                                            |
| Documentation<br>            | -<br>         | Element<br>    | Represent document of AssociationSet<br>                                                                                                                    | Display only when it exists<br>                                 |
| End<br>                      | -<br>         | Element<br>    | Represents an AssociationSetEnd, and essential elements<br>                                                                                                 | <br>                                                            |
| Role<br>                     | -<br>         | Attributes<br> | Represents a role, mandatory attributes<br>                                                                                                                 | <br>                                                            |
| EntitySet<br>                | -<br>         | Attributes<br> | Representing an EntitySet, mandatory attributes<br>                                                                                                         | <br>                                                            |

##### Property

| Node name<br>      | Namespace<br> | Node type<br>  | Overview<br>                                                  | Notes<br>                                                                                 |
|:-- |:-- |:-- |:-- |:-- |
| Property<br>       | -<br>         | Element<br>    | Represents a Property of an EntityType<br>                    | <br>                                                                                      |
| Name<br>           | -<br>         | Attributes<br> | Represents the name of the Property<br>                       | <br>                                                                                      |
| Type<br>           | -<br>         | Attributes<br> | Represents the type of Property value<br>                     | <br>                                                                                      |
| Nullable<br>       | -<br>         | Attributes<br> | Indicates whether null value can be assigned<br>              | <br>                                                                                      |
| MaxLength<br>      | -<br>         | Attributes<br> | Represents the maximum allowable length of a Property<br>     | <br>                                                                                      |
| DefaultValue<br>   | -<br>         | Attributes<br> | Represents the default value of a Property<br>                | <br>                                                                                      |
| Precision<br>      | -<br>         | Attributes<br> | Represents the number of significant digits of a Property<br> | <br>                                                                                      |
| Scale<br>          | -<br>         | Attributes<br> | Represents the number of decimal places of Property<br>       | <br>                                                                                      |
| CollectionKind<br> | -<br>         | Attributes<br> | Represents array type of Property<br>                         | List if the collection is an array, otherwise None<br>It is not displayed in case of None |
| Format<br>         | p:<br>        | Attributes<br> | Represents the character format of the Property<br>           | <br>                                                                                      |
| IsDeclared<br>     | p:<br>        | Attributes<br> | Represents whether it is a static Property or not<br>         | It is displayed with false for dynamic properties, not for static Properties<br>          |

##### Documentation

Not compatible

| Node name<br>       | Namespace<br> | Node type<br> | Overview<br>                             | Notes<br> |
|:-- |:-- |:-- |:-- |:-- |
| Summary<br>         | -<br>         | Element<br>   | Represents a summary of the Document<br> | <br>      |
| LongDescription<br> | -<br>         | Element<br>   | Represent Document details<br>           | <br>      |

##### DTD notation

namespace:edmx:

```dtd
<!ELEMENT Edmx (DataServices)>
<!ATTLIST Edmx Version CDDATA "1.0">
<!ELEMENT DataServices (Schema*)>
```

namespace: m:

```dtd
<!ATTLIST DataServices DataServiceVersion CDDATA "1.0">
<!ATTLIST FunctionImport HttpMethod CDDATA #IMPLIED>
<!ATTLIST EntityContainer IsDefaultEntityContainer CDDATA "true">
```

namespace:http://schemas.microsoft.com/ado/2006/04/edm

```dtd
<!ELEMENT Schema (Association*,ComplexType*,EntityType*,EntityContainer*)>
<!ATTLIST Schema Namespace CDDATA #REQUIRED>
<!ELEMENT ComplexType (Documentation?,Property*)>
<!ATTLIST ComplexType Name CDDATA #IMPLIED
                      Abstract CDDATA #IMPLIED>
<!ELEMENT Documentation (Summary?,LongDescription?)>
<!ELEMENT Summary (#PCDATA)>
<!ELEMENT LongDescription (#PCDATA)>
<!ELEMENT Property EMPTY>
<!ATTLIST Property Name CDDATA #REQUIRED
                   Type CDDATA #REQUIRED
                   Nullable CDDATA (true|false) #REQUIRED
                   MaxLength CDDATA #IMPLIED
                   DefaultValue CDDATA #IMPLIED
                   Precision CDDATA #IMPLIED
                   Scale CDDATA #IMPLIED
                   CollectionKind (List|None) #IMPLIED>
<!ELEMENT EntityType (Documentation?,Key/Property*/NavigationProperty*)>
<!ATTLIST EntityType OpenType (true|false) #REQUIRED
                     HasStream CDDATA #IMPLIED
                     BaseType CDDATA #IMPLIED>
<!ELEMENT Key (PropertyRef*)>
<!ELEMENT PropertyRef ENPTY>
<!ATTLIST PropertyRef Name CDDATA #REQUIRED>
<!ELEMENT NavigationProperty (Documentation?)>
<!ATTLIST PropertyRef Name CDDATA #REQUIRED
                      Relationship CDDATA #REQUIRED
                      FromRole CDDATA #REQUIRED
                      ToRole  CDDATA #REQUIRED>
<!ELEMENT Association (Documentation?|End+)>
<!ATTLIST Association Name CDDATA #REQUIRED>
<!ELEMENT End ENPTY>
<!ATTLIST End Role CDDATA #REQUIRED
          Type CDDATA #REQUIRED
          Multiplicity ("1","0..1","*") #REQUIRED>
<!ELEMENT EntityContainer (Documentation?|EntitySet*|FunctionImport*|AssociationSet*)>
<!ATTLIST FunctionImport Name CDDATA #REQUIRED
                         EntitySet CDDATA #IMPLIED
                         ReturnType CDDATA #IMPLIED>
<!ELEMENT Parameter (Documentation?)>
<!ATTLIST Parameter Name CDDATA #REQUIRED
                    Type CDDATA #REQUIRED
                    Mode  ("In","Out","InOut") #IMPLIED>
<!ELEMENT AssociationSet (Documentation?|End+)>
<!ATTLIST AssociationSet Name CDDATA #REQUIRED
                         Association CDDATA #REQUIRED>
<!ELEMENT End (Documentation?)>
<!ATTLIST End Role CDDATA #IMPLIED
              EntitySet CDDATA #REQUIRED>
```

namespace:p:

```dtd
<!ATTLIST Property Format CDDATA #IMPLIED>
```

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

##### For Schema's Atom ServiceDocument

Return the following fixedly.

```xml
<?xml version="1.0" encoding="utf-8"?>
<service xmlns="http://www.w3.org/2007/app" xml:base="https://demo.Personium/kouroki/TestBox/TestOData/$metadata/" xmlns:atom="http://www.w3.org/2005/Atom" xmlns:app="http://www.w3.org/2007/app">
  <workspace>
    <atom:title>Default</atom:title>
    <collection href="EntityType">
      <atom:title>EntityType</atom:title>
    </collection>
    <collection href="AssociationEnd">
      <atom:title>AssociationEnd</atom:title>
    </collection>
    <collection href="ComplexTypeProperty">
      <atom:title>ComplexTypeProperty</atom:title>
    </collection>
    <collection href="Property">
      <atom:title>Property</atom:title>
    </collection>
    <collection href="ComplexType">
      <atom:title>ComplexType</atom:title>
    </collection>
  </workspace>
</service>
```

##### For user data

```xml
<?xml version="1.0" encoding="utf-8"?>
<edmx:Edmx Version="1.0" xmlns:edmx="http://schemas.microsoft.com/ado/2007/06/edmx" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns:p="urn:x-personium:xmlns">
  <edmx:DataServices m:DataServiceVersion="1.0">
    <Schema xmlns="http://schemas.microsoft.com/ado/2006/04/edm" Namespace="UserData">
      <ComplexType Name="Address"></ComplexType>
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
        <EntitySet Name="animal" EntityType="UserData.animal"></EntitySet>
        <EntitySet Name="Profile" EntityType="UserData.Profile"></EntitySet>
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

##### For Schema's Atom ServiceDocument

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/\$metadata" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept:application/atomsvc+xml'
```

##### For user data

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/\$metadata" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept:application/xml'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED