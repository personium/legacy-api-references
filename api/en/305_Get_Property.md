# Acquisition of collection settings

### Overview

Get collection settings

### Required Privileges

read-properties

* When acquiring ACL setting status, read-acl is required together

### Restrictions

Common restriction

* None

WebDAV restriction

* Unpublished

Restriction on V1.0 series

* Function to specify properties to be returned in the response body (it becomes current allprop)

<br>

### Request

#### Request URL

```
/{CellName}/{BoxName}
```

or

```
/{CellName}/{BoxName}/{CollectionName}
```

| Path<br>             | Overview<br>        | Notes<br>                                                                                                                         |
|:-- |:-- |:-- |
| {CellName}<br>       | Cell Name<br>       | <br>                                                                                                                              |
| {BoxName}<br>        | Box Name<br>        | <br>                                                                                                                              |
| {CollectionName}<br> | Collection Name<br> | Valid values Number of digits:1-128<br>Usable character types<br>alphanumeric character, period(.), under score(_), hyphen(-)<br> |

#### Request Method

PROPFIND

#### Request Query

##### Common Request Query

| Query Name<br>    | Overview<br>                    | Effective Value<br>                                                                | Required<br> | Notes<br>                                                                                                                |
|:-- |:-- |:-- |:-- |:-- |
| p_cookie_peer<br> | Cookie Authentication Value<br> | The cookie authentication value returned from the server during authentication<br> | No<br>       | Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br> |

##### WebDav Common Request Query

None

#### Request Header

##### Common Request Header

| Header Name<br>            | Overview<br>                                       | Effective Value<br>                                                                                        | Required<br> | Notes<br>                                                                                                                    |
|:-- |:-- |:-- |:-- |:-- |
| X-HTTP-Method-Override<br> | Method override function<br>                       | User-defined<br>                                                                                           | No<br>       | Specifying this value in a request with the POST method indicates that the specified value is used as the method<br>         |
| X-Override<br>             | Header override function<br>                       | ${OverwrittenHeaderName}:${Value}<br>                                                                      | No<br>       | The normal HTTP header value is overwritten. Specify multiple X-Override headers for the overwriting of multiple headers<br> |
| X-Personium-RequestKey<br> | RequestKey field value output in the event log<br> | Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br> | No<br>       | Supported in V 1.1.7 and later<br>                                                                                           |

##### Individual Request Header

| Header Name<br>   | Overview<br>                                                     | Effective Value<br>                                                                    | Required<br> | Notes<br>                                                                                          |
|:-- |:-- |:-- |:-- |:-- |
| Authorization<br> | Specifies authentication information in the OAuth 2.0 format<br> | Bearer {AccessToken}<br>                                                               | No<br>       | * Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br> |
| Depth<br>         | Resource hierarchy to be acquired<br>                            | 0: Resource itself itself <br> 1: Resource of interest and resource right under it<br> | Yes<br>      | <br>                                                                                               |

#### Request Body

Namespace

| URI<br>  | Overview<br>         | Reference prefix<br> |
|:-- |:-- |:-- |
| DAV:<br> | WebDAV Namespace<br> | D:<br>               |

\* Reference The prefixes are for making it easier to read the following table, but the use of these prefix strings is not ensured or requested.

Structure of XML

The body is XML and follows the following schema.

| Node name<br> | Namespace<br> | Node type<br> | Overview<br>                                                       | Notes<br>                                                                                                                                                                 |
|:-- |:-- |:-- |:-- |:-- |
| propfind<br>  | D:<br>        | Element<br>   | Represents the root element of propfind and allprop is a child<br> | <br>                                                                                                                                                                      |
| allprop <br>  | D:<br>        | Element<br>   | Obtain all properties Get<br>                                      | Allprop... get all properties<br>Even if the request body is empty, treat it as allprop<br>Elements other than allprop are not supported for v1.2 series, v1.1 series<br> |

DTD notation

```dtd
<!ELEMENT propfind (allprop) >
<!ELEMENT allprop ENPTY >
```

#### Request Sample

```xml
<?xml version="1.0" encoding="utf-8"?>
<D:propfind xmlns:D="DAV:">
  <D:allprop/>
</D:propfind>
```

<br>

### Response

#### Response Code

| Code<br> | Message<br>      | Overview<br> |
|:-- |:-- |:-- |
| 207<br>  | Multi-Status<br> | Success<br>  |

#### Response Header

| Header Name<br>        | Overview<br>                      | Notes<br>                                                |
|:-- |:-- |:-- |
| Content-Type<br>       | Format of data to be returned<br> | <br>                                                     |
| DataServiceVersion<br> | OData version information<br>     | Return only when Entity can be successfully acquired<br> |

#### Response Body

Namespace

| URI<br>                   | Overview<br>            | Reference prefix<br> |
|:-- |:-- |:-- |
| DAV:<br>                  | WebDAV Namespace<br>    | D:<br>               |
| urn:x-personium:xmlns<br> | Personium namespace<br> | p:<br>               |

\* Reference The prefixes are for making it easier to read the following table, but the use of these prefix strings is not ensured or requested.

Structure of XML

The body is XML and follows the following schema.

| Node name<br>        | Namespace<br> | Node type<br>  | Overview<br>                                                                                                    | Notes<br>                                                                                                                                                                                               |
|:-- |:-- |:-- |:-- |:-- |
| multistatus<br>      | D:<br>        | Element<br>    | Represents the route of multistatus and one or more responses are children<br>                                  | <br>                                                                                                                                                                                                    |
| response<br>         | D:<br>        | Element<br>    | Represents a response to resource acquisition, href and propstat are children<br>                               | <br>                                                                                                                                                                                                    |
| href<br>             | D:<br>        | Element<br>    | Resource url<br>                                                                                                | <br>                                                                                                                                                                                                    |
| propstat<br>         | D:<br>        | Element<br>    | Represents property information of a resource, status and prop are children<br>                                 | <br>                                                                                                                                                                                                    |
| status<br>           | D:<br>        | Element<br>    | Represents the response code of resource acquisition<br>                                                        | <br>                                                                                                                                                                                                    |
| prop<br>             | D:<br>        | Element<br>    | Property detailed information, creationdate, resourcetype, acl, and proppatch setting values are children<br>   | <br>                                                                                                                                                                                                    |
| creationdate<br>     | D:<br>        | Element<br>    | Resource creation time<br>                                                                                      | <br>                                                                                                                                                                                                    |
| getcontentlength<br> | D:<br>        | Element<br>    | Resource size<br>                                                                                               | Only when the resource is a file<br>                                                                                                                                                                    |
| getcontenttype<br>   | D:<br>        | Element<br>    | Resource contenttype<br>                                                                                        | Only when the resource is a file<br>                                                                                                                                                                    |
| getlastmodified<br>  | D:<br>        | Element<br>    | Resource update time<br>                                                                                        | <br>                                                                                                                                                                                                    |
| resourcetype<br>     | D:<br>        | Element<br>    | Represents the type of resource. <br> collection, either odata or service is a child, or the child is empty<br> | <br>                                                                                                                                                                                                    |
| collection<br>       | D:<br>        | Element<br>    | Represents that the resource type is a collection<br>                                                           | If the resource is WebDAV, only this element is displayed<br>                                                                                                                                           |
| odata<br>            | p:<br>        | Element<br>    | Represents that the resource type is an OData collection<br>                                                    | For OData collection display<br>                                                                                                                                                                        |
| service<br>          | p:<br>        | Element<br>    | Represents that the resource type is a service collection<br>                                                   | For Service collection Display<br>                                                                                                                                                                      |
| acl<br>              | D:<br>        | Element<br>    | ACL setting set for resource<br>                                                                                | In order to acquire the ACL setting, acl-read authority for the target resource is required. <br> ACL element For content below, see the [cell level access control setting API](289_Cell_ACL.html)<br> |
| base<br>             | xml:<br>      | Attributes<br> | ACL Privilege BaseURL<br>                                                                                       | In the case of PROPFIND to Cell, the resource URL of the default box ("__")<br>                                                                                                                         |

DTD notation

namespace D:

```dtd
<!ELEMENT multistatus (response*)>
<!ELEMENT response (href, propstat)>
<!ELEMENT href (#PCDATA)>
<!ELEMENT propstat (status, prop)>
<!ELEMENT status (#PCDATA)>
<!ELEMENT prop (creationdate, resourcetype, acl, ANY)>
<!ELEMENT creationdate (#PCDATA)>
<!ELEMENT getcontentlength (#PCDATA)>
<!ELEMENT getcontenttype (#PCDATA)>
<!ELEMENT getlastmodified (#PCDATA)>
<!ELEMENT resourcetype ((collection, (odata or service) or EMPTY))>
<!ELEMENT collection EMPTY>
<!ELEMENT acl (ace*)>
```

namespace p:

```dtd
<!ELEMENT odata EMPTY>
<!ELEMENT service EMPTY>
```

namespace xml:

```dtd
<!ATTLIST acl base CDATA #IMPLIED>
```

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

```xml
<multistatus xmlns="DAV:">
    <response>
        <href>https://{UnitFQDN}/{CellName}/{BoxName}</href>
        <propstat>
            <prop>
                <creationdate>2017-02-15T01:52:34.635+0000</creationdate>
                <getlastmodified>Wed, 15 Feb 2017 01:52:34 GMT</getlastmodified>
                <resourcetype>
                    <collection/>
                </resourcetype>
                <acl xmlns:p="urn:x-personium:xmlns"/>
            </prop>
            <status>HTTP/1.1 200 OK</status>
        </propstat>
    </response>
</multistatus>
```

<br>

### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{CollectionName}" -X PROPFIND -i  -H 'Depth:1' -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '<?xml version="1.0" encoding="utf-8"?><D:propfind xmlns:D="DAV:"><D:allprop/></D:propfind>'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED