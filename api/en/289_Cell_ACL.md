# Cell Level Access Control Configuration

### Overview

Provides cell level access control functions.

### Required Privileges

acl

### Restrictions

Updating the ACL configuration overwrites existing ACL settings.<

* Restrictions in V 1.0

    * The function to deny ACL configuration (deny)
    * Acquisition of a list of privileges configurable by the ACL

<br>

### Request

#### Request URL

```
/{CellName}
```

#### Request Method

ACL

#### Request Query

Common Request Query

| Query Name<br>    | Overview<br>                    | Effective Value<br>                                                                | Required<br> | Notes<br>                                                                                                                |
|:-- |:-- |:-- |:-- |:-- |
| p_cookie_peer<br> | Cookie Authentication Value<br> | The cookie authentication value returned from the server during authentication<br> | No<br>       | Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br> |

#### Common Request Header

| Header Name<br>            | Overview<br>                                       | Effective Value<br>                                                                                        | Required<br> | Notes<br>                                                                                                         |
|:-- |:-- |:-- |:-- |:-- |
| X-HTTP-Method-Override<br> | Method override function<br>                       | User-defined<br>                                                                                           | No<br>       | If you specify this value when requesting with the POST method, the specified value will be used as a method.<br> |
| X-Override<br>             | Header override function<br>                       | ${OverwrittenHeaderName}:${Value}<br>                                                                      | No<br>       | Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.<br>       |
| X-Personium-RequestKey<br> | RequestKey field value output in the event log<br> | Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br> | No<br>       | PCS-${UNIXtime} by default<br>Supported in V 1.1.7 and later<br>                                                  |

#### Request Header

| Header Name<br>   | Overview<br>                                                     | Effective Value<br>      | Required<br> | Notes<br>                                                                                          |
|:-- |:-- |:-- |:-- |:-- |
| Authorization<br> | Specifies authentication information in the OAuth 2.0 format<br> | Bearer {AccessToken}<br> | No<br>       | * Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br> |

#### Request Body

#### Namespace

| URI<br>                   | Overview<br>            | Notes (prefix)<br> |
|:-- |:-- |:-- |
| DAV:<br>                  | WebDAV Namespace<br>    | D:<br>             |
| urn:x-personium:xmlns<br> | Personium namespace<br> | p:<br>             |

\* Reference The prefixes are for making it easier to read the following table, but the use of these prefix strings is not ensured or requested.

#### Structure of XML

The body is XML and follows the following schema.<br><br>
For details on the privilege settings within the privilege tag, please refer to the acl\_model ([Access Control Model](../../user_guide/002_Access_Control.html)).

| Node name<br>    | Namespace<br> | Node type<br> | Overview<br>                                                                                            | Notes<br>                                                                                                                                                            |
|:-- |:-- |:-- |:-- |:-- |
| acl<br>          | D:<br>        | Element<br>   | Denotes the root of the ACL (Access Control List); one or more ace nodes will be its child<br>          | <br>                                                                                                                                                                 |
| base<br>         | D:<br>        | Element<br>   | Denotes an ACE (Access Control Element); a pair of principal and grant will be its child<br>            | <br>                                                                                                                                                                 |
| ace<br>          | D:<br>        | Element<br>   | Denotes the privilege configuration target; href or all will be its child<br>                           | "invert", "deny", "protected", and "inherited" are not supported in V 1.1 systems<br>                                                                                |
| principal<br>    | D:<br>        | Element<br>   | Denotes the privilege configuration target; href or all will be its child<br>                           | <br>                                                                                                                                                                 |
| grant<br>        | D:<br>        | Element<br>   | Denotes the privilege grant setting; one or more privilege nodes will be its child node<br>             | <br>                                                                                                                                                                 |
| href<br>         | D:<br>        | Element<br>   | Denotes the privilege configuration target role and is the text node to input the role resource URL<br> | Specify the resource URL of the privilege configuration target role<br>It is possible to shorten the URL using the xml:base attribute setting in the acl element<br> |
| all<br>          | D:<br>        | Element<br>   | All access entity privilege setting<br>                                                                 | The setting for all roles and non-authorized access entities (without the Authorization header)<br>                                                                  |
| privilege<br>    | D:<br>        | Element<br>   | Denotes the privilege setting; one of the following elements will be its child<br>                      | <br>                                                                                                                                                                 |
| root<br>         | p:<br>        | Element<br>   | All privileges<br>                                                                                      | <br>                                                                                                                                                                 |
| auth<br>         | p:<br>        | Element<br>   | Authentication management API editing and viewing privileges<br>                                        | <br>                                                                                                                                                                 |
| auth-read<br>    | p:<br>        | Element<br>   | Authentication management API viewing privileges<br>                                                    | <br>                                                                                                                                                                 |
| message<br>      | p:<br>        | Element<br>   | Message management API editing and viewing privileges<br>                                               | <br>                                                                                                                                                                 |
| message-read<br> | p:<br>        | Element<br>   | Message management API viewing privileges<br>                                                           | <br>                                                                                                                                                                 |
| event<br>        | p:<br>        | Element<br>   | Event management API editing and view privileges<br>                                                    | <br>                                                                                                                                                                 |
| event-read<br>   | p:<br>        | Element<br>   | Event management API viewing privileges<br>                                                             | <br>                                                                                                                                                                 |
| log<br>          | p:<br>        | Element<br>   | Event bus log API editing and viewing privileges<br>                                                    | <br>                                                                                                                                                                 |
| log-read<br>     | p:<br>        | Element<br>   | Event bus log API viewing privileges<br>                                                                | <br>                                                                                                                                                                 |
| social<br>       | p:<br>        | Element<br>   | Relation management API editing and viewing privileges<br>                                              | <br>                                                                                                                                                                 |
| social-read<br>  | p:<br>        | Element<br>   | Relation management API viewing privileges<br>                                                          | <br>                                                                                                                                                                 |
| box<br>          | p:<br>        | Element<br>   | Box management API editing and viewing privileges<br>                                                   | <br>                                                                                                                                                                 |
| box-read<br>     | p:<br>        | Element<br>   | Box management API viewing privileges<br>                                                               | <br>                                                                                                                                                                 |
| box-install<br>  | p:<br>        | Element<br>   | Box installation execution privileges * supported by V 1.2.3<br>                                        | <br>                                                                                                                                                                 |
| box-export<br>   | p:<br>        | Element<br>   | Box export execution privileges<br>                                                                     | Unsupported (Configuration disabled)<br>                                                                                                                             |
| acl<br>          | p:<br>        | Element<br>   | ACL management API editing and viewing privileges<br>                                                   | <br>                                                                                                                                                                 |
| acl-read<br>     | p:<br>        | Element<br>   | ACL management API viewing privileges<br>                                                               | <br>                                                                                                                                                                 |
| propfind<br>     | p:<br>        | Element<br>   | Property acquisition API viewing privileges<br>                                                         | <br>                                                                                                                                                                 |

##### DTD notation

Namespace: D:

```dtd
<!ELEMENT acl (ace*) >
<!ATTLIST acl base CDATA #IMPLIED>
<!ELEMENT ace ((principal or invert), (grant or deny), protected?,inherited?)>
<!ELEMENT principal (href or all)>
<!ELEMENT principal (privilege+)>
<!ELEMENT href (#PCDATA)>
<!ELEMENT all EMPTY>
<!ELEMENT privilege (root or auth or auth-read or message or message-read or event or event-read or social or social-read or box or box-read or acl or acl-read or propfind)>
```

Namespace:xml:

```dtd
<!ATTLIST acl base CDATA #IMPLIED>
```

Namespace: p:

```dtd
<!ELEMENT root EMPTY>
<!ELEMENT auth EMPTY>
<!ELEMENT auth-read EMPTY>
<!ELEMENT message EMPTY>
<!ELEMENT message-read EMPTY>
<!ELEMENT event EMPTY>
<!ELEMENT event-read EMPTY>
<!ELEMENT log EMPTY>
<!ELEMENT log-read EMPTY>
<!ELEMENT social EMPTY>
<!ELEMENT social-read EMPTY>
<!ELEMENT box EMPTY>
<!ELEMENT box-read EMPTY>
<!ELEMENT box-install EMPTY>
<!ELEMENT box-export EMPTY>
<!ELEMENT acl EMPTY>
<!ELEMENT acl-read EMPTY>
<!ELEMENT propfind EMPTY>
```

#### Request Sample

```xml
<?xml version="1.0" encoding="utf-8" ?>
<D:acl xmlns:D="DAV:" xmlns:p="urn:x-personium:xmlns" xml:base="https://example.com/testcell1/__role/box1/">
    <D:ace>
        <D:principal>
            <D:all/>
        </D:principal>
        <D:grant>
           <D:privilege><p:auth/></D:privilege>
           <D:privilege><p:box/></D:privilege>
        </D:grant>
    </D:ace>
    <D:ace>
        <D:principal>
            <D:href>role</D:href>
        </D:principal>
        <D:grant>
            <D:privilege><p:root/></D:privilege>
        </D:grant>
    </D:ace>
</D:acl>        
```

<br>

### Response

#### Response Code

| Code<br> | Message<br> | Overview<br> |
|:-- |:-- |:-- |
| 200<br>  | OK<br>      | Success<br>  |

#### Response Header

| Item Name<br>    | Overview<br>                      | Notes<br> |
|:-- |:-- |:-- |
| Content-Type<br> | Format of data to be returned<br> | <br>      |

#### Response Body

None

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}" -X ACL -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '<?xml version="1.0" encoding="utf-8" ?><D:acl xmlns:D="DAV:" xmlns:p="urn:x-personium:xmlns" xml:base="http://{UnitFQDN}/{CellName}/__role/{BoxName}/">  <D:ace><D:principal><D:href>{RoleName}</D:href></D:principal><D:grant><D:privilege><p:box-read/></D:privilege><D:privilege><p:auth/></D:privilege></D:grant></D:ace></D:acl>'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED