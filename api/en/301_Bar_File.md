# bar File

bar is the file format specified as the request body of the box installation API. <br>The bar file stores various data located in Odata/WebDAV/Service defined under the box and is archived in the ZIP format. <br>Normally, archived data definitions under the archived box are exported from Personium through the box export API (unimplemented).

### Specifications

The file format shall be ZIP, including the ZIP64 format. <br>ZIP file encryption is not supported.

#### Directory Structure

The following example shows the directory structure in the bar file. <br>If directories and files indicated as "Required" do not exist when the box is installed, an error of no required directory/file (400 Bad Request) is returned. <br>An error also occurs if the bar file is not structured in the following sequence.

```
bar/
 |
 +-- 00_meta/  *Required
 |    |
 |    +-- 00_manifest.json  *Required
 |    +-- 10_relations.json
 |    +-- 20_roles.json
 |    +-- 30_extroles.json
 |    +-- 70_$links.json
 |    +-- 90_rootprops.xml  *Required
 |
 +-- 90_contents/     *The directory name under the same as the collection name
      |
      +-- {OData}/    *Required for OData collection in rootprops.xml
      |    |
      |    +-- 00_$metadata.xml    *Required
      |    +-- 10_odatarelations.json
      |    |
      |    +-- 90_data/
      |         |
      |         +-- {EntityType}/
      |              |
      |              +-- {1.json}
      |
      +-- {Service}/
      |    |
      |    +-- {src.js}
      |
      +-- {dir1}/
           |
           +-- {dir1-1}/
                |
                +-- {userdata1-2.jpg}
                |
                +-- {dir2}/
                     |
                     +-- [userdata1-2.jpg}
```

#### Bar File Version Control

The bar file is upgraded if the data structure is changed in response to enhancement etc., and backward compatibility can no longer be ensured.

* Not upgraded at the level of file addition
* Upgraded if the file format or file name is changed or deleted

### File List

#### 00\_manifest.json

The file describing the information of the box to be installed

| Item Name<br>   | Overview<br>                 | Effective Value<br>                                                                                                                                                                                                                    | Required<br> | Notes<br>                                                                                               |
|:-- |:-- |:-- |:-- |:-- |
| bar_version<br> | bar file version<br>         | Valid version<br>Revised every time the bar file format is changed<br>                                                                                                                                                                 | Yes<br>      | Currently "1"<br>                                                                                       |
| box_version<br> | Box version<br>              | Valid version<br>Revised every time the box format is changed<br>                                                                                                                                                                      | Yes<br>      | Any string can be specified but "1" is recommended (for the provision of the box revision function)<br> |
| DefaultPath<br> | Box name in the bar file<br> | Number of digits: 1 to 128<br>Character type: Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>However, the string cannot start with a single-byte hyphen ("-") or underscore ("_")<br>null is invalid<br> | Yes<br>      | <br>                                                                                                    |
| schema<br>      | Schema name<br>              | Number of digits: 1 to 1024<br>Conforming to the URI format (schema: http / https / urn)<br>null is invalid<br>                                                                                                                        | Yes<br>      | <br>                                                                                                    |

##### Samples

```JSON
{
  "bar_version": "1",
  "box_version": "1",
  "DefaultPath": "{BoxName}",
  "schema": "http://app1.example.com"
}
```

#### 10\_relations.json

The file describing the information of the relations to be installed<br>\* For items whose "Effective Value" column shows "-", refer to the Relation request body

| Item Name<br>      | Overview<br>      | Effective Value<br> | Required<br> | Notes<br> |
|:-- |:-- |:-- |:-- |:-- |
| Relations <br>     | Relation list<br> | <br>                | Yes<br>      | <br>      |
| Relations/Name<br> | Relation name<br> | -<br>               | Yes<br>      | <br>      |

##### Samples

```JSON
{
  "Relations": [
    {
      "Name": "relation1"
    }
  ]
}
```

#### 20\_roles.json

The file describing the information of the roles to be installed<br>\* For items whose "Effective Value" column shows "-", refer to the Role request body

| Item Name<br>  | Overview<br>      | Effective Value<br> | Required<br> | Notes<br> |
|:-- |:-- |:-- |:-- |:-- |
| Roles <br>     | Relation list<br> | <br>                | Yes<br>      | <br>      |
| Roles/Name<br> | Relation name<br> | -<br>               | Yes<br>      | <br>      |

##### Samples

```JSON
{
  "Roles": [
    {
      "Name": "role1"
    }
  ]
}
```

#### 30\_extroles.json

The file describing the information of the ExtRoles to be installed<br>\* For items whose "Effective Value" column shows "-", refer to the ExtRole request body

| Item Name<br>               | Overview<br>                       | Effective Value<br> | Required<br> | Notes<br>              |
|:-- |:-- |:-- |:-- |:-- |
| ExtRoles<br>                | ExtRole list<br>                   | <br>                | Yes<br>      | <br>                   |
| ExtRoles/ExtRole<br>        | Reference destination role URI<br> | -<br>               | Yes<br>      | null is invalid *1<br> |
| ExtRoles/_Relation.Name<br> | Relation name<br>                  | -<br>               | Yes<br>      | null is invalid<br>    |

(*1) Converted to the role class URL during export<br>[https://{UnitFQDN}/cell1/\_\_role/box/staff](https://%7BUnitFQDN%7D/cell1/__role/box/staff) -> [https://{UnitFQDN}/cell1/\_\_role/\_\_/staff](https://%7BUnitFQDN%7D/cell1/__role/__/staff)

##### Samples

```JSON
{
  "ExtRoles": [
    {
      "ExtRole": "https://{UnitFQDN}/{CellName}/__role/__/role2",
      "_Relation.Name": "Relation1"
    }
  ]
}
```

#### 70\_\$links.json

The file describing the data relation information of the \$links to be installed

| Item Name<br>      | Overview<br>                        | Effective Value<br>                    | Required<br> | Notes<br>                                                |
|:-- |:-- |:-- |:-- |:-- |
| Links<br>          | $links list<br>                     | <br>                                   | Yes<br>      | <br>                                                     |
| Links/FromType<br> | Reference source data type<br>      | "Relation"<br>"Role"<br>"ExtRole" <br> | Yes<br>      | null is invalid<br>                                      |
| Links/FromName<br> | Reference source data name<br>      | - (Array format *1)<br>                | Yes<br>      | null is invalid<br> (Example: {"Name":"relation1"}])<br> |
| Links/ToType<br>   | Reference destination data type<br> | "Relation"<br>"Role"<br>"ExtRole"<br>  | Yes<br><br>  | null is invalid<br><br>                                  |
| Links/ToName<br>   | Reference destination data name<br> | - (Array format *1)<br>                | Yes<br><br>  | null is invalid<br> (Example: {"Name":"role"}])<br>      |

\*1 The list format is applied for ExtRoles because relation information is also required. However, the key name of the specified JSON data is fixed to "Name". (Restriction)

##### Samples

```JSON
{
  "Links": [
    {
      "FromType": "Relation",
      "FromName":
        {
          "Name": "relation1"
        },
      "ToType": "Role",
      "ToName":
        {
          "Name": "role1"
        }
    },
    {
      "FromType": "Role",
      "FromName":
        {
          "Name": "role1"
        },
      "ToType": "ExtRole",
      "ToName": {
          "ExtRole": "https://{UnitFQDN}/{CellName}/__role/__/role2",
          "_Relation.Name": "Relation1"
        }
    }
  ]
}
```

#### 90\_rootprops.xml

Shows the XML data acquired with the PROPFIND method for all hierarchical levels under the box to be export to the bar file. <br>For details on XML data, refer to [get file setting API(PROPFIND)](307_Get_Property.html). <br>The URL of the box to be installed is described as "Personium-box:/". <br>When the bar file is installed, the installation targets include all data

* resourcetype: Sets the collection type
* acl: Sets privileges
* Other: Set in PROPPATCH

##### Samples

```xml
<multistatus xmlns="DAV:">
    <response>
        <href>Personium-box:/</href>
        <propstat>
           <prop>
              <resourcetype>
                  <collection/>
              </resourcetype>
              <acl xml:base="https://{UnitFQDN}/cell/__role/__/" xmlns:p="urn:x-personium:xmlns">
                  <ace>
                      <principal>
                          <href>admin</href>
                      </principal>
                      <grant>
                          <privilege>
                              <all/>
                          </privilege>
                      </grant>
                  </ace>
              </acl>
          </prop>
      </propstat>
  </response>
  <response>
      <href>Personium-box:/odata</href>
      <propstat>
          <prop>
              <resourcetype>
                  <collection/>
                  <p:service xmlns:p="urn:x-personium:xmlns"/>
              </resourcetype>
              <acl xml:base="https://{UnitFQDN}/cell/__role/__/" xmlns:p="urn:x-personium:xmlns">
                  <ace>
                      <principal>
                          <href>user</href>
                      </principal>
                      <grant>
                          <privilege>
                              <read/>
                          </privilege>
                          <privilege>
                              <write/>
                          </privilege>
                          <privilege>
                              <read-properties/>
                          </privilege>
                      </grant>
                  </ace>
              </acl>
          </prop>
      </propstat>
  </response>
  <response>
      <href>Personium-box:/dav</href>
      <propstat>
          <prop>
              <resourcetype>
                  <collection/>
              </resourcetype>
              <acl xml:base="https://{UnitFQDN}/cell/__role/__/" xmlns:p="urn:x-personium:xmlns">
                  <ace>
                      <principal>
                          <href>user</href>
                      </principal>
                      <grant>
                          <privilege>
                              <read/>
                          </privilege>
                          <privilege>
                              <write/>
                          </privilege>
                          <privilege>
                              <read-properties/>
                          </privilege>
                      </grant>
                  </ace>
              </acl>
          </prop>
      </propstat>
  </response>
  <response>
      <href>Personium-box:/dav/testdavfile.txt</href>
      <propstat>
          <prop>
              <getcontenttype>text/plain</getcontenttype>
          </prop>
      </propstat>
  </response>
  <response>
      <href>Personium-box:/service</href>
      <propstat>
          <prop>
              <resourcetype>
                  <collection/>
                  <p:service xmlns:p="urn:x-personium:xmlns"/>
              </resourcetype>
             <acl xml:base="https://{UnitFQDN}/cell/__role/__/" xmlns:p="urn:x-personium:xmlns"/>
              <p:service language="JavaScript" xmlns:D="DAV:" xmlns:p="urn:x-personium:xmlns" \
              xmlns:Z="http://www.w3.com/standards/z39.50/">
                  <p:path name="ehr" src="ehr.js"/>
                  <p:path name="ehr_connector" src="ehr_connector.js"/>
              </p:service>
            </prop>
        </propstat>
    </response>
    <response>
        <href>Personium-box:/service/__src</href>
        <propstat>
            <prop>
                <resourcetype>
                    <collection/>
                </resourcetype>
                <acl xml:base="https://{UnitFQDN}/cell/__role/__/"\
                 xmlns:p="urn:x-personium:xmlns"/>
            </prop>
        </propstat>
    </response>
    <response>
        <href>Personium-box:/service/__src/ehr.js</href>
        <propstat>
            <prop>
                <getcontenttype>text/javascript</getcontenttype>
            </prop>
        </propstat>
    </response>
    <response>
        <href>Personium-box:/service/__src/ehr_connector.js</href>
        <propstat>
            <prop>
                <getcontenttype>text/javascript</getcontenttype>
            </prop>
        </propstat>
    </response>
</multistatus>
```

#### contents/{OData}/

Details not contributed yet for the following files

* 90_data / {EntityType}/1.json

##### 00\_\$metadata.xml

Shows user OData schema definitions. This data is the XML data acquired by \$metadata for the collection for Odata during the export to the bar file. <br>For details on XML data, refer to [schema acquisition (\$metadata)](316_User_Defined_Data_Schema.html). <br>When the box is installed, the installation targets include the contents under the Schema tag. <br>Even if user OData schema definitions do not exist, the file itself exists.

Sample in the case of no schema definitions

```xml
<Edmx: Edmx Version = '1 .0 'xmlns: edmx =' http://schemas.microsoft.com/ado/2007/06/edmx \
'xmlns: d =' http://schemas.microsoft.com/ado/2007 / 08/dataservices' xmlns:\
 m = 'http://schemas.microsoft.com/ado/2007/08/dataservices/metadata' xmlns: p = 'urn: x-personium: xmlns'>
  <edmx:DataServices m:DataServiceVersion='1.0'>
    <Schema Xmlns='http://schemas.microsoft.com/ado/2006/04/edm' Namespace='UserData'>
      <EntityContainer Name='UserData' m:IsDefaultEntityContainer='true'/>
    </ Schema>
  </ Edmx: DataServices>
</ Edmx: Edmx>
```

##### 10\_odatarelations.json

The file describing the data-related information of \$links of the user data to be installed<br>At the user OData schema level, AssociationEnd relations are defined in 00\_\$metadata.xml, and in this file, relations to actual user data are defined.

| Item Name<br>      | Overview<br>                            | Effective Value<br>     | Required<br> | Notes<br>                                                    |
|:-- |:-- |:-- |:-- |:-- |
| Links<br>          | $links list<br>                         | <br>                    | Yes<br>      | <br>                                                         |
| Links/FromType<br> | Reference source data type<br>          | - <br>                  | Yes<br>      | null is invalid<br>                                          |
| Links/FromId<br>   | Reference source user data ID<br>       | - (Array format *1)<br> | Yes<br>      | null is invalid<br> (Example: {"FromId":"fujitsu_taro"})<br> |
| Links/ToType<br>   | Reference destination data type<br>     | -<br>                   | Yes<br><br>  | null is invalid<br><br>                                      |
| Links/ToId <br>    | Reference destination user data ID <br> | - (Array format *1)<br> | Yes<br><br>  | null is invalid<br> (Example: {"ToId":"fujitsu_hanako"})<br> |

\*1 The array format is applied in consideration of the future support for composite primary keys.

##### Samples

```JSON
{
  "Links": [
    {
      "FromType": "Keeper",
      "FromId": {
        "usercode": "001",
        "Name": "Peru_Taro"
      },
      "ToType": "Animal",
      "ToId": {
        "shopcode": "001",
        "Name": "pochi"
      }
    },
    {
      "FromType": "Keeper",
      "FromId": {
        "usercode": "002",
        "Name": "Peru_Hanako"
      },
      "ToType": "Animal",
      "ToId": {
        "shopcode": "002",
        "Name": "tama"
      }
    }
  ]
}
```

#### 90\_data / {EntityType} / {1.json}

Stores user data in the JSON format one by one.

```JSON
{
    "__id": "{EntityName}",
    "name": "pochi",
    "address": {
        "country": "japan",
        "city": "tokyo"
    }
}
```

#### contents / {webdavbcol} /

\* Not contributed yet

#### contents/Service/{src.js}

Registers the source file stored in bar/90\_contents/{Service}/{src.js} with {Service}/\_\_src/{src.js} under the installation destination box.

* Unable to be executed as a service if no collection {Service} definitions (PROPPATCH) exist in bar/00_meta/90_rootprops.xml
* {src.js} cannot be registered if no {Service}/__src definitions exist in bar/00_meta/90_rootprops.xml
* Content-Type in {src.js} to be set according to the {Service}/__src/{src.js} definition in bar/00_meta/90_rootprops.xml

Sample of 90\_rootprops.xml for service registration

```xml
<multistatus xmlns="DAV:">
    <response>
        <href>Personium-box:/service</href>
        <propstat>
            <prop>
                <resourcetype>
                    <collection/>
                    <p:service xmlns:p="urn:x-personium:xmlns"/>
                </resourcetype>
                <acl xml:base="https://{UnitFQDN}/cell/__role/__/"\
                xmlns:p="urn:x-personium:xmlns"/>
                <p:service language="JavaScript" xmlns:D="DAV:" xmlns:p="urn:x-personium:xmlns" \
                xmlns:Z="http://www.w3.com/standards/z39.50/">
                    <p:path name="ehr" src="ehr.js"/>
                    <p:path name="ehr_connector" src="ehr_connector.js"/>
                </p:service>
            </prop>
        </propstat>
    </response>
    <response>
        <href>Personium-box:/service/__src</href>
        <propstat>
            <prop>
                <resourcetype>
                    <collection/>
                </resourcetype>
                <acl xml:base="https://{UnitFQDN}/cell/__role/__/"\
                 xmlns:p="urn:x-personium:xmlns"/>
            </prop>
        </propstat>
    </response>
    <response>
        <href>Personium-box:/service/__src/ehr.js</href>
        <propstat>
            <prop>
                <getcontenttype>text/javascript</getcontenttype>
            </prop>
        </propstat>
    </response>
    <response>
        <href>Personium-box:/service/__src/ehr_connector.js</href>
        <propstat>
            <prop>
                <getcontenttype>text/javascript</getcontenttype>
            </prop>
        </propstat>
    </response>
</multistatus>
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED