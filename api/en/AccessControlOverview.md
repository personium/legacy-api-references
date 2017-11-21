# Access control model

## Overview

Access control to the access subject by ACL is done by applying WebDAV ACL to role based access control. 

By setting the ACL with the ACL method for Cell, Box, Collection, etc., access right to that resource can be set. 

Basically, it conforms to the specification of WebDAV ACL, but this page will explain Personium original specifications. 

ACL settings are defined in XML. 

## ACL

AThe access control to the access subject by the ACL is done by applying [ WebDAV ACL ](http://www.ietf.org/rfc/rfc3744) to role based access control.

By setting the ACL with the ACL method for Cell, Box, Collection, etc., access right to that resource can be set. 

Basically, it conforms to the specification of WebDAV ACL, but this page will explain Personium original specifications. 

ACL settings are defined in XML. 

* ### ACL component in Personium 
* The Personium ACL consists of the following elements. 
* | col 1        | col 2                                                                |
    | -- | -- |
    | Element name | Contents                                                             |
    | ace          | Represents an access entity that sets ACLs and a set of their grants |
    | pricipal     | Define the access subject of the ACL to be set up                    |
    | grant        | Define one or more privileges to be given to principal               |
    | privilege    | Define permission settings                                           |

### Limitations

In Personium V1 series, there are the following restrictions.

ÅEThe ACL is updated by overwriting the existing setting

ÅESet up deprivation of authority by deny element

ÅEAcquire a list of authorizations that can be granted

### Object

The target of ACL setting of Personium is a resource, and it is set by the ACL method to the URL of each resource.

In the case of a path of a cell, it becomes a cell level ACL, and in case of a path under a box, it becomes a box level ACL.

These two have different Privileges (authorities) that can be set, and they will not affect each other.

| col 1          | col 2                                                     | col 3                                                                                                           |
| -- | -- | -- |
|                | Object                                                    | Target resource                                                                                                 |
| Cell Level ACL | Setting to the cell,

Control CRUD of cell control object | Cell                                                                                                            |
| Box Level ACL  | Control CRUD of Box control object                        | Box, WebDAV collection, OData collection, Service collection<br>
Directory and file under the WebDAV collection |

## ace

The target access agent is defined as a Principal element, and the authorization is defined as a grant element. You can set multiple ace elements. 

* ### Principal 

Although Principal has "human or computational actor" in the WebDAV ACL, it is defined as a role in Personium. 

The role of Personium is a concept close to "Group" in WebDAV ACL. 

###  (1) all 

As defined in the WebDAV ACL, setting the all element in pricipal gives the authorization setting, <br>
It can also be defined for all roles and unauthorized access agents (without Authorization header). 

Since it is authority for all access agents to the resource, caution is required when using it. 

| col 1                        |
| -- |
| principal:all

privilege:all |

With the above setting, all operations are released to all accesses accessed. 

However, if schema authentication level setting is done, that checking will take effect. 

####  Setting example 

Give read access to all access subjects. 

| col 1                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| -- |
| <D:acl xmlns:D="DAV:" xml:base="http://fqdn/testcell1/__role/box1/"

&nbsp;&nbsp;&nbsp;&nbsp;xmlns:p="urn:x-personium:xmlns"

&nbsp;&nbsp;&nbsp;&nbsp;p:requireSchemaAuthz="none">

<D:ace>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:principal>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</D:all>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</D:principal>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:grant>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:privilege><D:read/></D:privilege>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</D:grant>

&nbsp;&nbsp;&nbsp;&nbsp;</D:ace>

</D:acl> |

###  (2) Roll 

To define a role for the target access agent, enclose it in an href element and set the role resource URL. <br>
For specifications on role resources see Role Issue. 

Note that the role resource URL that can be set can not specify a role resource URL different from the cell URL of the ACL setting target. 

####  Setting example 

- Describe all role resource URLs to be set in Principal 

(Example of giving read and write privileges to doctor attached to box 1 and giving read only to guest attached to box 2) 

| col 1                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| -- |
| <D:acl xmlns:D="DAV:"

&nbsp;&nbsp;&nbsp;&nbsp;xmlns:p="urn:x-personium:xmlns"

&nbsp;&nbsp;&nbsp;&nbsp;p:requireSchemaAuthz="none">

&nbsp;&nbsp;&nbsp;&nbsp;<D:ace>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:principal>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:href>http://fqdn/testcell1/__role/box1/doctor</D:href>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</D:principal>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:grant>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:privilege><D:read/></D:privilege>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:privilege><D:write/></D:privilege>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</D:grant>

&nbsp;&nbsp;&nbsp;&nbsp;</D:ace>

<D:ace>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:principal>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:href>http://fqdn/testcell1/__role/box2/guest</D:href>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</D:principal>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:grant>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:privilege><D:read/></D:privilege>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</D:grant>

&nbsp;&nbsp;&nbsp;&nbsp;</D:ace>

</D:acl> |

Omission of description by xml: base 

By describing the role resource URL up to the box in the xml: base attribute of the acl element, omit the description of the role resource URL set in Principal can do. 

(Example of giving read and write privileges to doctor attached to box 1 and giving read only to guest attached to box 2) 

| col 1                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| -- |
| <D:acl xmlns:D="DAV:" xml:base="http://fqdn/testcell1/__role/box1/"

&nbsp;&nbsp;&nbsp;&nbsp;xmlns:p="urn:x-personium:xmlns"

&nbsp;&nbsp;&nbsp;&nbsp;p:requireSchemaAuthz="none">

&nbsp;&nbsp;&nbsp;&nbsp;<D:ace>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:principal>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:href>doctor</D:href>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</D:principal>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:grant>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:privilege><D:read/></D:privilege>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:privilege><D:write/></D:privilege>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</D:grant>

&nbsp;&nbsp;&nbsp;&nbsp;</D:ace>

<D:ace>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:principal>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:href>../box2/guest</D:href>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</D:principal>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:grant>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:privilege><D:read/></D:privilege>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</D:grant>

&nbsp;&nbsp;&nbsp;&nbsp;</D:ace>

</D:acl> |

\* Like guest, when setting a role attached to a box different from the xml: base box, it is possible to describe relative paths as described above.

#### Output with PROPFIND

When outputting the ACL setting with PROPFIND, the xml: base attribute is output as follows.

* In the case of PROPFIND to a cell, the URL of the main box
* In the case of PROPFIND under Box, the URL to that Box

## grant/Privilege

The authority is set with the element defined by Personium, and set by enclosing it in the Privilege element in the grant element.<br>
In addition, you can specify multiple Privilege elements in the grant element.

* ### Cell Level ACL Privilege

    For the set cell, specify execution authority of the cell control object and execution authority of the method.<br>
    When the authority located at the higher level is set, it has the authority belonging to the lower rank. (Eg message has the authority of message-read)

    | Authority name | Target cell control object       | Top authority name | Methods that can be executed        |
    | -- | -- | -- | -- |
    | root           | It has all the authorities below | -                  | All                                 |
    | auth           | Account, Role, ExtRole           | root               | PUT, POST, DELETE, GET, OPTIONS     |
    | auth-read      | Account, Role, ExtRole           | auth               | GET, OPTIONS                        |
    | message        | RecievedMessage, SentMessage     | root               | POST, DELETE, GET, OPTIONS          |
    | message-read   | RecievedMessage, SentMessage     | message            | GET, OPTIONS                        |
    | event          | event                            | root               | PUT, POST, DELETE, GET, OPTIONS     |
    | event-read     | event                            | event              | GET, OPTIONS                        |
    | log            | log                              | root               | PUT, POST, DELETE, GET, OPTIONS     |
    | log-read       | log                              | log                | GET, OPTIONS                        |
    | social         | Relation, ExtCell                | root               | PUT, POST, DELETE, GET, OPTIONS     |
    | social-read    | Relation, ExtCell                | social             | GET, OPTIONS                        |
    | box            | Box                              | root               | PUT, POST, DELETE, GET, OPTIONS     |
    | box-read       | Box                              | box                | GET, OPTIONS                        |
    | box-install    | Box                              | box                | MKCOL(Barfile installation)         |
    | box-export     | Box                              | root               | GET(Barfile export) * Not supported |
    | acl            | Cell                             | root               | ACL                                 |
    | acl-read       | Cell                             | acl                | Display of ACL setting of PROPFIND  |
    | propfind       | Cell                             | root               | PROPFIND                            |

    #### Setting Example

    | col 1                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
    | -- |
    | <?xml version="1.0" encoding="utf-8" ?>

    <D:acl xmlns:D="DAV:" xmlns:p="urn:x-personium:xmlns" xml:base="https://example.com/cell/box">

    &nbsp;&nbsp;&nbsp;&nbsp;<D:ace>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:principal>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:href>role10</D:href>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</D:principal>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:grant>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:privilege><p:root/></D:privilege>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</D:grant>

    &nbsp;&nbsp;&nbsp;&nbsp;</D:ace>

    &nbsp;&nbsp;&nbsp;&nbsp;<D:ace>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:principal>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:href>../box2/role13</D:href>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</D:principal>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:grant>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:privilege><p:social/></D:privilege>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</D:grant>

    &nbsp;&nbsp;&nbsp;&nbsp;</D:ace>

    &nbsp;&nbsp;&nbsp;&nbsp;<D:ace>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:principal>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:href>role15</D:href>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</D:principal>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:grant>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:privilege><p:acl/></D:privilege>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</D:grant>

    &nbsp;&nbsp;&nbsp;&nbsp;</D:ace>

    </D:acl> |

    ### 

    ### Box level ACL Privilege

    Specify the execution authority of the method for the resources below the set box.<br>
    Box level Privilege is basically defined along WebDAV ACL.<br>
    When the authority located at the higher level is set, it has the authority belonging to the lower rank.(Example: read also has the authority of read-properties)

    | Authority name   | Target cell control object                                           | Top authority name | Methods that can be executed       |
    | -- | -- | -- | -- |
    | all              | It has all the authorities below                                     | p:root             | All                                |
    | read             | Has read permission. It does not include read-acl.                   | all                | GET, OPTIONS                       |
    | write            | Has write authority. It does not include write-acl.                  | all                | PUT, POST, DELETE, MKCOL           |
    | read-properties  | Has the right to read properties.                                    | read               | PROPFIND                           |
    | write-properties | Have authority to write properties.                                  | write              | PROPPATCH                          |
    | read-acl         | It has read authority of ACL.                                        | all                | Display of ACL setting of PROPFIND |
    | write-acl        | Has authority to write ACL.                                          | all                | ACL                                |
    | write-content    | Has authority to write content.                                      | write              | * Not supported                    |
    | bind             | Has bind authority.                                                  | write              | * Not supported                    |
    | unbind           | Has unbind authority.                                                | write              | * Not supported                    |
    | exec             | Has service execution authority. * Personium original implementation | all                | -                                  |

    #### Setting Example

    | col 1                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
    | -- |
    | <D:acl xmlns:D="DAV:" xml:base="http://fqdn/testcell1/__role/box1/"

    &nbsp;&nbsp;&nbsp;&nbsp;xmlns:p="urn:x-personium:xmlns"

    &nbsp;&nbsp;&nbsp;&nbsp;p:requireSchemaAuthz="none">

    &nbsp;&nbsp;&nbsp;&nbsp;<D:ace>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:principal>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:href>doctor</D:href>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</D:principal>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:grant>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:privilege><D:read/></D:privilege>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:privilege><D:write/></D:privilege>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</D:grant>

    &nbsp;&nbsp;&nbsp;&nbsp;</D:ace>

    <D:ace>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:principal>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:href>../box2/guest</D:href>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</D:principal>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:grant>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:privilege><D:read/></D:privilege>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</D:grant>

    &nbsp;&nbsp;&nbsp;&nbsp;</D:ace>

    </D:acl> |

    ### ACL inheritance

    For box level ACL setting, the setting of the parent (upper directory) of the accessed resource is applied back to the cell.<br>
    The ACL setting for the access subject is inherited in the form in which the parent setting is added.<br>
    Therefore, the following points need to be noted when setting ACL.
    * When strong permission of all etc. etc is set for parent, even if restriction is applied to child, it becomes invalid

    #### Examples of ACL inheritance and applied privileges

    Example resource

    | col 1             | col 2         | col 3                                       |
    | -- | -- | -- |
    | Resource type     | Resource name | Resource URL                                |
    | Cell              | cell          | https://fqdn/cell                           |
    | Box               | box           | https://fqdn/cell/box                       |
    | WebDAV Collection | webdav        | https://fqdn/cell/box/webdav                |
    | Directory         | directory     | https://fqdn/cell/box/webdav/directory      |
    | File              | file          | https://fqdn/cell/box/webdav/directory/file |

    After applying the following ACL setting to the above resource, the authorities applied when accessing each resource are as follows.

    | col 1               | col 2          | col 3                                      |
    | -- | -- | -- |
    | Resources to access | Permission set | Applicable authority                       |
    | Cell                | auth-read      | auth-read                                  |
    | Box                 | read-acl       | auth-read, read-acl                        |
    | WebDAV Collection   | read           | auth-read, read-acl, read                  |
    | Directory           | no settings    | auth-read, read-acl, read                  |
    | File                | read-propaties | auth-read, read-acl, read-properties, read |

## Schema authority request level

For access control to the application according to the schema authority request level,

the request level is set by the RequireSchemaAuthz attribute of the ACL element at the time of setting the ACL.

For schema authentication specifications, see "Schema authentication (application authentication)" in the [authentication model](../../user_guide/003_Auth.html).

* ### Setting value

    Schema privilege request level value

    | col 1         | col 2                                                                                            |
    | -- | -- |
    | Level value   | Contents                                                                                         |
    | none(Default) | Accessible without schema authentication                                                         |
    | public        | Accessible if schema authentication result is OK                                                 |
    | confidential  | When the schema authentication result is OK and the special role confidentialClient is available |

    #### Setting Example

    Sample Schema Privilege Request Level Setting ACL

    | col 1                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
    | -- |
    | <D:acl xmlns:D="DAV:" xml:base="http://localhost:8080/testcell1/__role/box1/"

    &nbsp;&nbsp;&nbsp;&nbsp;xmlns:p="urn:x-personium:xmlns"

    &nbsp;&nbsp;&nbsp;&nbsp;p:requireSchemaAuthz="{Level value}">

    &nbsp;&nbsp;&nbsp;&nbsp;<D:ace>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:principal>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:all/>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</D:principal>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:grant>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:privilege><D:read/></D:privilege>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<D:privilege><D:write/></D:privilege>

    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</D:grant>

    &nbsp;&nbsp;&nbsp;&nbsp;</D:ace>

    </D:acl> |

    ### Inheritance of schema authority request level setting

    The schema authority request level setting is applied by setting the parent (upper directory) of the accessed resource back to the box only
    when the accessed resource is not set.
    * If the schema authority request level is not set, it is treated as none, but it is not inherited if explicitly set to none.
    * Inheritance is confirmed back to the Box, but when the schema level setting is performed on the intermediate resource, the setting value becomes effective.

    #### Schema privilege Request level inheritance and example of authority to be applied

    The schema authority request level applied when accessing each resource after setting the following schema authority request level to the above resource is as follows.

    Example resource

    | col 1             | col 2         | col 3                                       |
    | -- | -- | -- |
    | Resource type     | Resource name | Resource URL                                |
    | Cell              | cell          | https://fqdn/cell                           |
    | Box               | box           | https://fqdn/cell/box                       |
    | WebDAV Collection | webdav        | https://fqdn/cell/box/webdav                |
    | Directory         | directory     | https://fqdn/cell/box/webdav/directory      |
    | File              | file          | https://fqdn/cell/box/webdav/directory/file |

    After applying the following ACL setting to the above resource, the authorities applied when accessing each resource are as follows.

    | col 1               | col 2        | col 3               |
    | -- | -- | -- |
    | Resources to access | Set level    | Applicable level    |
    | Box                 | confidential | confidential &nbsp; |
    | WebDAV Collection   | public       | public              |
    | Directory           | no settings  | public              |
    | File                | none         | none                |
