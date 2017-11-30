# Box Level Access Control Configuration

### Overview

Provides box level access control functions

### Required Privileges

write-acl

### Restrictions

* When ACL setting is done, the existing ACL setting is overwritten and updated
* The function to deny ACL configuration (deny)
* Acquisition of a list of privileges configurable by the ACL

<br>

### Request

#### Request URL

```
/{CellName}/{BoxName}
```

or

```
/{CellName}/{BoxName}/{ResourcePath}
```

|Path<br>|Overview<br>|Notes<br>|
|:--|:--|:--|
|{CellName}<br>|Cell Name<br>|<br>|
|{BoxName}<br>|Box Name<br>|<br>|
|{ResourcePath}<br>|Path to resource<br>|Valid values Number of digits:1-128<br>Usable character types<br>alphanumeric character, period(.), under score(_), hyphen(-)<br>|

#### Request Method

ACL

#### Request Query

None

#### Request Header

|Header Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|Method override function<br>|User-defined<br>|No<br>|Specifying this value in a request with the POST method indicates that the specified value is used as the method<br>|
|X-Override<br>|Header override function<br>|${OverwrittenHeaderName}:${Value}<br>|No<br>|The normal HTTP header value is overwritten. Specify multiple X-Override headers for the overwriting of multiple headers<br>|
|X-Personium-RequestKey<br>|RequestKey field value output in the event log<br>|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br>|No<br>|Supported in V 1.1.7 and later<br>|
|Authorization<br>|Specifies authentication information in the OAuth 2.0 format<br>|Bearer {AccessToken}<br>|No<br>|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br>|

#### Request Body

Namespace

|URI<br>|Overview<br>|Reference prefix<br>|
|:--|:--|:--|
|DAV:<br>|WebDAV Namespace<br>|D:<br>|
|urn:x-personium:xmlns<br>|Personium namespace<br>|p:<br>|

\* Reference The prefixes are for making it easier to read the following table, but the use of these prefix strings is not ensured or requested.

Structure of XML  
The body is XML and follows the following schema.  
See acl\_model ([access control model](../../user_guide/002_Access_Control.html)) for the contents of privilege setting under the privilege tag.

|Node name<br>|Namespace<br>|Node type<br>|Overview<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|acl<br>|D:<br>|Element<br>|Denotes the root of the ACL (Access Control List); one or more ace nodes will be its child<br>|<br>|
|base<br>|xml:<br>|Attributes<br>|Href Represents the base of the URL described in the tag and sets an arbitrary value as the attribute value. This attribute is optional.<br>|<br>|
|ace<br>|D:<br>|Element<br>|Denotes the privilege configuration target; href or all will be its child<br>|"invert", "deny", "protected", and "inherited" are not supported in V 1.1 systems<br>|
|principal<br>|D:<br>|Element<br>|Denotes the privilege configuration target; href or all will be its child<br>|<br>|
|grant<br>|D:<br>|Element<br>|Denotes the privilege grant setting; one or more privilege nodes will be its child node<br>|<br>|
|href<br>|D:<br>|Element<br>|Denotes the privilege configuration target role and is the text node to input the role resource URL<br>|Specify resource URL of privilege setting target role<br>It is possible to shorten the URL using the xml:base attribute setting in the acl element<br>|
|all<br>|D:<br>|Element<br>|All access entity privilege setting<br>|<br>|
|privilege<br>|D:<br>|Element<br>|Denotes the privilege setting; one of the following elements will be its child<br>|<br>|
|read<br>|D:<br>|Element<br>|Reference authority<br>|<br>|
|write<br>|D:<br>|Element<br>|Edit permission<br>|<br>|
|read-properties<br>|D:<br>|Element<br>|Property reference authority<br>|<br>|
|write-properties<br>|D:<br>|Element<br>|Edit property authority<br>|<br>|
|read-acl<br>|D:<br>|Element<br>|ACL setting reference authority<br>|<br>|
|write-acl<br>|D:<br>|Element<br>|ACL setting edit permission<br>|<br>|
|bind<br>|D:<br>|Element<br>|Unpublished<br>|V1.1 series, V1.2 series not supported<br>|
|unbind<br>|D:<br>|Element<br>|Unpublished<br>|V1.1 series, V1.2 series not supported<br>|
|exec<br>|D:<br>|Element<br>|Service execution authority<br>|<br>|

DTD notation

namespace D:

```dtd
<!ELEMENT acl (ace*) >
<!ELEMENT ace ((principal or invert), (grant or deny), protected?,inherited?)>
<!ELEMENT principal (href or all)>
<!ELEMENT principal (privilege*)>
<!ELEMENT href (#PCDATA)>
<!ELEMENT all EMPTY>
<!ELEMENT privilege (all or read or write or read-properties or write-properties or read-acl or write-acl or exec or bind or unbind)>
<!ELEMENT read EMPTY>
<!ELEMENT write EMPTY>
<!ELEMENT read-properties EMPTY>
<!ELEMENT write-properties EMPTY>
<!ELEMENT read-acl EMPTY>
<!ELEMENT write-acl EMPTY>
<!ELEMENT bind EMPTY>
<!ELEMENT unbind EMPTY>
<!ELEMENT exec EMPTY>
```

namespace p:

```dtd
<!ATTLIST acl requireSchemaAuthz (none or public or confidential) #IMPLIED>
<!ELEMENT exec EMPTY>   
```

namespace xml:

```dtd
<!ATTLIST acl base CDATA #IMPLIED>
```

#### Request Sample

```xml
<?xml version="1.0" encoding="utf-8" ?>
<D:acl xmlns:D="DAV:" xmlns:p="urn:x-personium:xmlns"
       xml:base="https://{UnitFQDN}/{CellName}/__role/{BoxName}/"
       p:requireSchemaAuthz="public">
    <D:ace>
        <D:principal>
            <D:all/>
        </D:principal>
        <D:grant>
            <D:privilege><D:read/></D:privilege>
        </D:grant>
    </D:ace>
    <D:ace>
        <D:principal>
            <D:href>role</D:href>
        </D:principal>
        <D:grant>
            <D:privilege><D:read/></D:privilege>
            <D:privilege><D:write/></D:privilege>
        </D:grant>
    </D:ace>
</D:acl>
```

<br>

### Response

#### Response Code

|Code<br>|Message<br>|Overview<br>|
|:--|:--|:--|
|200<br>|OK<br>|Success<br>|

#### Response Header

|Header Name<br>|Overview<br>|Notes<br>|
|:--|:--|:--|
|Content-Type<br>|Format of data to be returned<br>|Only when it failed at the time of update / creation, return it<br>|

#### Response Body

None

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

None

<br>

### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{CollectionName}" -X ACL -i
-H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d
'<?xml version="1.0" encoding="utf-8" ?>
 <D:acl xmlns:D="DAV:" xml:base="https://{UnitFQDN}/{CellName}/__role/{BoxName}/" xmlns:p="urn:x-personium:xmlns" p:requireSchemaAuthz="none">
  <D:ace>
   <D:principal>
    <D:href>doctor</D:href>
   </D:principal>
   <D:grant>
    <D:privilege>
     <D:read/>
    </D:privilege>
    <D:privilege>
     <D:write/>
    </D:privilege>
   </D:grant>
  </D:ace>
 </D:acl>'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED
