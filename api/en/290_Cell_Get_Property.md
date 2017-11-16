# Acquire Properties

### Overview

Get cell properties

### Required Privileges

propfind

* In order to acquire ACL setting status, acl-read is required together

### Restrictions

Restrictions in V 1.0

* Ability to return a list of properties for which target resources can be returned
* A function that specifies the property to be returned in the response body

    * It becomes current allprop

<br>

### Request

#### Request URL

```
/{CellName}
```

#### Request Method

PROPFIND

#### Request Query

| Query Name<br>    | Overview<br>                    | Effective Value<br>                                                                | Required<br> | Notes<br>                                                                                                                |
|:-- |:-- |:-- |:-- |:-- |
| p_cookie_peer<br> | Cookie Authentication Value<br> | The cookie authentication value returned from the server during authentication<br> | No<br>       | Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br> |

#### Request Header

| Header Name<br>            | Overview<br>                                                     | Effective Value<br>                                                                                        | Required<br> | Notes<br>                                                                                                         |
|:-- |:-- |:-- |:-- |:-- |
| X-HTTP-Method-Override<br> | Method override function<br>                                     | User-defined<br>                                                                                           | No<br>       | If you specify this value when requesting with the POST method, the specified value will be used as a method.<br> |
| X-Override<br>             | Header override function<br>                                     | ${OverwrittenHeaderName}:${Value}<br>                                                                      | No<br>       | Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.<br>       |
| X-Personium-RequestKey<br> | RequestKey field value output in the event log<br>               | Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br> | No<br>       | PCS-${UNIXtime} by default<br>Supported in V 1.1.7 and later<br>                                                  |
| Authorization<br>          | Specifies authentication information in the OAuth 2.0 format<br> | Bearer {AccessToken}<br>                                                                                   | No<br>       | * Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br>                |
| Depth<br>                  | To get the hierarchy of a resource<br>                           | 0:Gets target resource only<br>1:Gets target and resources directly under the target<br>                   | Yes<br>      | <br>                                                                                                              |

#### Request Body

##### Namespace

| URI<br>  | Overview<br>         | Notes<br> |
|:-- |:-- |:-- |
| DAV:<br> | WebDAV Namespace<br> | D:<br>    |

\* Reference The prefixes are for making it easier to read the following table, but the use of these prefix strings is not ensured or requested.

##### Structure of XML

The body is XML and follows the following schema.

| Node name<br> | Namespace<br> | Node type<br> | Overview<br>                                                         | Notes<br>                                                                                                                                                   |
|:-- |:-- |:-- |:-- |:-- |
| propfind<br>  | D:<br>        | Element<br>   | Represents the root element of propfind, and allprop is a child.<br> | <br>                                                                                                                                                        |
| allprop<br>   | <br>          | Element<br>   | Represent setting to retrieve all properties<br>                     | allprop : get all properties<br>Even if the request body is empty, treat it as allprop<br>Elements other than allprop are not supported for v1.2 , v1.1<br> |

##### DTD notation

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

| Item Name<br>          | Overview<br>                      | Notes<br>                                                |
|:-- |:-- |:-- |
| Content-Type<br>       | Format of data to be returned<br> | <br>                                                     |
| DataServiceVersion<br> | OData Version<br>                 | Return only when Entity can be successfully acquired<br> |

#### Response Body

Namespace

| Item Name<br>             | Overview<br>                      | Notes<br> |
|:-- |:-- |:-- |
| Content-Type<br>          | Format of data to be returned<br> | D:<br>    |
| urn:x-personium:xmlns<br> | Personium namespace<br>           | p:<br>    |

\* Reference The prefixes are for making it easier to read the following table, but the use of these prefix strings is not ensured or requested.

##### Structure of XML

The body is XML and follows the following schema.

&#124;odata<br>

| Node name<br>        | Namespace<br> | Node type<br> | Overview<br>                                                                                                       | Notes<br>                                                                                                                                                                                                         |
|:-- |:-- |:-- |:-- |:-- |
| multistatus<br>      | D:<br>        | Element<br>   | Represents the route of multistatus and one or more responses are children<br>                                     | <br>                                                                                                                                                                                                              |
| response<br>         | D:<br>        | Element<br>   | Represents a response to resource acquisition, href and propstat are children<br>                                  | <br>                                                                                                                                                                                                              |
| href<br>             | D:<br>        | Element<br>   | Resource url<br>                                                                                                   | <br>                                                                                                                                                                                                              |
| propstat<br>         | D:<br>        | Element<br>   | Represents property information of a resource, status and prop are children<br>                                    | <br>                                                                                                                                                                                                              |
| prop<br>             | D:<br>        | Element<br>   | Property detailed information, creationdate, resourcetype, acl, and proppatch setting values are children<br>      | <br>                                                                                                                                                                                                              |
| creationdate<br>     | D:<br>        | Element<br>   | Resource creation time<br>                                                                                         | <br>                                                                                                                                                                                                              |
| getcontentlength<br> | D:<br>        | Element<br>   | Resource size<br>                                                                                                  | Resource is only file<br>                                                                                                                                                                                         |
| getcontenttype<br>   | p:<br>        | Element<br>   | Contenttype of resources<br>                                                                                       | Resource is only file<br>                                                                                                                                                                                         |
| getlastmodified<br>  | p:<br>        | Element<br>   | Resource update time<br>                                                                                           | <br>                                                                                                                                                                                                              |
| resourcetype<br>     | p:<br>        | Element<br>   | Represents the type of resource. Either one of collection, odata, or service is a child, or the child is empty<br> | <br>                                                                                                                                                                                                              |
| collection<br>       | p:<br>        | Element<br>   | Represents that the type of resource is a collection<br>Displays, if resource is collection<br>                    | <br>                                                                                                                                                                                                              |
| odata<br>            | p:<br>        | Element<br>   | Represents that the type of resource is a OData service collection<br>                                             | Displays, if resource is OData collection<br>                                                                                                                                                                     |
| service<br>          | p:<br>        | Element<br>   | Represents that the type of resource is a service collection<br>                                                   | Displays, if resource is service collection<br>                                                                                                                                                                   |
| acl<br>              | p:<br>        | Element<br>   | ACL setting set for resource<br>                                                                                   | In order to acquire the ACL setting, acl-read authority for the target resource is required<br>ACL Element Refer to the [cell level access control setting API](289_Cell_ACL.html) for the following contents<br> |
| base<br>             | p:<br>        | Element<br>   | ACL Privilege BaseURL<br>                                                                                          | When PROPFIND to Cell, default box ("__") resource URL<br>                                                                                                                                                        |
| status<br>           | D:<br>        | Element<br>   | Represents the response code of resource acquisition<br>                                                           | <br>                                                                                                                                                                                                              |

##### DTD notation

Namespace: D:

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

Namespace:p:

```dtd
<!ELEMENT odata EMPTY>
<!ELEMENT service EMPTY>
```

Namespace: xml:

```dtd
<!ATTLIST acl base CDATA #IMPLIED>
```

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

```xml
<multistatus xmlns="DAV:">
    <response>
        <href>https://{UnitFQDN}/{CellName}</href>
        <propstat>
            <prop>
                <creationdate>2017-02-03T01:27:31.130+0000</creationdate>
                <getlastmodified>Fri, 03 Feb 2017 01:27:31 GMT</getlastmodified>
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
curl "https://{UnitFQDN}/{CellName}" -X PROPFIND -i -H 'Depth:1' -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '<?xml version="1.0" encoding="utf-8"?><D:propfind xmlns:D="DAV:"><D:allpop/></D:propfind>'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED