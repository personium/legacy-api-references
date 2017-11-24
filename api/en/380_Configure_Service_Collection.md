# Service Collection Source Settings

### Overview

Apply service collection source settings

### Restrictions

Unpublished

### Required Privileges

write-properties

<br>

### Request

#### Request URL

```
/{CellName}/{BoxName}/{CollectionName}
```

| Path<br>             | Overview<br>                | Notes<br>                                                                                                                             |
|:-- |:-- |:-- |
| {CellName}<br>       | Cell Name<br>               | <br>                                                                                                                                  |
| {BoxName}<br>        | Box Name<br>                | <br>                                                                                                                                  |
| {CollectionName}<br> | Service Collection Name<br> | Valid values <br>Number of digits:1-128<br>Usable character types<br>alphanumeric character, period(.), under score(_), hyphen(-)<br> |

#### Request Method

PROPPATCH

#### Request Query

##### Common Request Query

| Query Name<br>    | Overview<br>                    | Effective Value<br>                                                                | Required<br> | Notes<br>                                                                                                                |
|:-- |:-- |:-- |:-- |:-- |
| p_cookie_peer<br> | Cookie Authentication Value<br> | The cookie authentication value returned from the server during authentication<br> | No<br>       | Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br> |

#### Request Header

##### Common Request Header

| Header Name<br>            | Overview<br>                                       | Effective Value<br>                                                                                        | Required<br> | Notes<br>                                                                                                         |
|:-- |:-- |:-- |:-- |:-- |
| X-HTTP-Method-Override<br> | Method override function<br>                       | User-defined<br>                                                                                           | No<br>       | If you specify this value when requesting with the POST method, the specified value will be used as a method.<br> |
| X-Override<br>             | Header override function<br>                       | ${OverwrittenHeaderName}:${Value}<br>                                                                      | No<br>       | Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.<br>       |
| X-Personium-RequestKey<br> | RequestKey field value output in the event log<br> | Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br> | No<br>       | Supported in V 1.1.7 and later<br>                                                                                |

##### Service Collection Settings Specific Request Header

| Header Name<br>   | Overview<br>                                                     | Effective Value<br>      | Required<br> | Notes<br>                                                                                          |
|:-- |:-- |:-- |:-- |:-- |
| Authorization<br> | Specifies authentication information in the OAuth 2.0 format<br> | Bearer {AccessToken}<br> | No<br>       | * Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br> |
| Content-Type<br>  | Specify content format<br>                                       | application/xml<br>      | No<br>       | <br>                                                                                               |
| Accept<br>        | Specify acceptable media types in response<br>                   | application/xml<br>      | No<br>       | <br>                                                                                               |

#### Request Body

Namespace

| URI<br>                                 | Overview<br>                | reference prefix<br> |
|:-- |:-- |:-- |
| DAV:<br>                                | WebDAV Namespace<br>        | D:<br>               |
| urn:x-personium:xmlns<br>               | Personium API namespace<br> | p:<br>               |

\* Reference The prefixes are for making it easier to read the following table, but the use of these prefix strings is not ensured or requested.

Structure of XML<br>
The body is XML and follows the following schema.

| Node name<br>      | Namespace<br> | Node type<br> | Overview<br>                                                                     | Notes<br>                                                                               |
|:-- |:-- |:-- |:-- |:-- |
| propertyupdate<br> | D:<br>        | Element<br>   | It represents the root of propertyupdate, and set and remove are children<br>    | <br>                                                                                    |
| set<br>            | D:<br>        | Element<br>   | Represents a property setting, and one or more props are children<br>            | <br>                                                                                    |
| remove<br>         | D:<br>        | Element<br>   | Represents a property deletion setting, and one or more props are children<br>   | <br>                                                                                    |
| prop<br>           | D:<br>        | Element<br>   | Represents a property value, and one or more arbitrary elements are children<br> | When set: Child node name is key<br>When remove: Delete with child node name as key<br> |

DTD notation

```dtd
<!ELEMENT propertyupdate (set, remove)
<!ELEMENT set (prop*) >
<!ELEMENT remove (prop*) >
<!ELEMENT prop ANY>
```

#### Service collection setting specific definition

| Node name<br> | Namespace<br> | Node type<br>  | Overview<br>                                                                                                                       | Notes<br>                                                                                                             |
|:-- |:-- |:-- |:-- |:-- |
| service <br>  | p:<br>        | element<br>    | It represents a service setting, and one or more multiple path elements are children<br>                                           | <br>                                                                                                                  |
| language <br> | p:<br>        | Attributes<br> | It represents the service source language setting, and "JavaScript" is fixed as attribute value<br>                                | <br>                                                                                                                  |
| subject <br>  | p:<br>        | Attributes<br> | It represents the service subject setting and sets the Account name registered in the cell belonging to as the attribute value<br> | Execute with the Role privilege attached to the Account in which the Personium API in the logic is set up<br>         |
| path <br>     | p:<br>        | Element<br>    | Represents a service collection setting.<br>                                                                                       | <br>                                                                                                                  |
| name <br>     | p:<br>        | Attributes<br> | Represents a service call name and an arbitrary character as an attribute value<br>                                                | This setting value becomes the "__src/" path name immediately under the request URL when the service is executed.<br> |
| src<br>       | p:<br>        | Attributes<br> | Represents the service source file name, and sets the file name deployed under __src as the attribute value<br>                    | <br>                                                                                                                  |

DTD notation

```dtd
<!ELEMENT service (path*)>
<!ATTLIST service language CDATA "JavaScript">
<!ATTLIST service subject CDATA #IMPLIED>
<!ELEMENT path EMPTY>
<!ATTLIST path name CDATA #REQUIRED>
<!ATTLIST path src CDATA #REQUIRED>
```

#### Request Sample

```xml
<D:propertyupdate xmlns:D="DAV:"
    xmlns:p="urn:x-personium:xmlns">
    <D:set>
        <D:prop>
          <p:service language="JavaScript">
            <p:path name="${name}" src="${src}"/>
          </p:service>
        </D:prop>
    </D:set>
</D:propertyupdate>
```

<br>

### Response

#### Response Code

207

#### Response Header

None

#### Response Body

Namespace

| URI<br>  | Overview<br>         | reference prefix<br> |
|:-- |:-- |:-- |
| DAV:<br> | WebDAV Namespace<br> | D:<br>               |

\* Reference The prefixes are for making it easier to read the following table, but the use of these prefix strings is not ensured or requested.

Structure of XML<br>
The body is XML and follows the following schema.

| Node name<br>    | Namespace<br> | Node type<br> | Overview<br>                                                                   | Notes<br>                                                                                                                         |
|:-- |:-- |:-- |:-- |:-- |
| multistatus <br> | D:<br>        | Element<br>   | Represents the route of multistatus and one or more responses are children<br> | <br>                                                                                                                              |
| response<br>     | D:<br>        | Element<br>   | Represents the contents of multistatus, and href and propstat are children<br> | <br>                                                                                                                              |
| href<br>         | D:<br>        | Element<br>   | URL of the resource that executed PROPPATCH<br>                                | <br>                                                                                                                              |
| propstat <br>    | D:<br>        | Element<br>   | Represents property setting result, prop and status are children<br>           | <br>                                                                                                                              |
| prop <br>        | D:<br>        | Element<br>   | Represents property setting contents<br>                                       | Display the result of resource setting as follows<br>Successful setting: set key and value<br>Deleted Successful: Deleted key<br> |
| status<br>       | D:<br>        | Element<br>   | Property setting status code<br>                                               | In the case of setting success 200 (OK) is returned<br>                                                                           |

DTD notation

```dtd
<!ELEMENT multistatus (response*)>
<!ELEMENT response (href, propstat)>
<!ELEMENT href (#PCDATA)>
<!ELEMENT propstat (prop, status)>
<!ELEMENT prop ANY>
<!ELEMENT status (#PCDATA)>
```

#### Response Sample

```xml
<multistatus xmlns="DAV:">
    <response>
        <href>https://{UnitFQDN}/{CellName}/{BoxName}/{CollectionName}</href>
        <propstat>
            <prop>
                <p:service language="JavaScript" xmlns:p="urn:x-personium:xmlns" xmlns:D="DAV:">
                    <p:path name="sample" src="sample.js"/>
                </p:service>
            </prop>
            <status>HTTP/1.1 200 OK</status>
        </propstat>
    </response>
</multistatus>
```

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{CollectionName}" -X PROPPATCH -i -H "Authorization:Bearer {AccessToken}" -H "Accept:application/json" -d "<?xml version=\"1.0\" encoding=\"utf-8\" ?><D:propertyupdate xmlns:D=\"DAV:\" xmlns:p=\"urn:x-personium:xmlns\"><D:set><D:prop><p:service language=\"JavaScript\"><p:path name=\"sample\" src=\"sample.js\"/></p:service></D:prop></D:set></D:propertyupdate>"
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED
