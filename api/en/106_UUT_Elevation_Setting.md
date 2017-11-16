# ULUUT elevation setting PROPPATCH

### Overview

This API changes the UUT (Unit User Token) upgraded settings.

### Required Privileges

Only unit users permitted

### Restrictions

Unpublished<br><br>

### Request

#### Request URL

```
/{CellName}
```

| Path<br>       | Overview<br>  |
|:-- |:-- |
| {CellName}<br> | Cell Name<br> |

#### Request Method

PROPPATCH

#### Request Query

None

#### Request Header

| Header Name<br>            | Overview<br>                                                     | Effective Value<br>                   | Required<br> | Notes<br>                                                                                                         |
|:-- |:-- |:-- |:-- |:-- |
| X-HTTP-Method-Override<br> | Method override function<br>                                     | User-defined<br>                      | No<br>       | If you specify this value when requesting with the POST method, the specified value will be used as a method.<br> |
| X-Override<br>             | Header override function<br>                                     | ${OverwrittenHeaderName}:${Value}<br> | No<br>       | Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.<br>       |
| Authorization<br>          | Specifies authentication information in the OAuth 2.0 format<br> | Bearer {AccessToken}<br>              | No<br>       | * Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br>                |
| Content-Type<br>           | Specify content format<br>                                       | application / xml<br>                 | No<br>       | <br>                                                                                                              |
| Accept<br>                 | Specify acceptable media types in response<br>                   | application / xml<br>                 | No<br>       | <br>                                                                                                              |

#### Request Body

| Item Name<br>                           | Namespace<br> | Overview<br>                                  | Required<br> | Effective Value<br>                                     | Notes<br>                                                  |
|:-- |:-- |:-- |:-- |:-- |:-- |
| DAV:<br>                                | <br>          | XML namespace setting<br>                     | Yes<br>      | "DAV:"<br>                                              | <br>                                                       |
| urn: x-personium: xmlns<br>             | <br>          | XML namespace setting<br>                     | Yes<br>      | "Urn: x-personium: xmlns"<br>                           | <br>                                                       |
| http://www.w3.com/standards/z39.50/<br> | <br>          | XML namespace setting<br>                     | Yes<br>      | "http://www.w3.com/standards/z39.50/"<br>               | <br>                                                       |
| propertyupdate<br>                      | DAV:<br>      | propertyupdate (Access Control List) root<br> | Yes<br>      |                                                         | <br>                                                       |
| set<br>                                 | DAV:<br>      | set property<br>                              | No<br>       | <! ELEMENT set (prop *)><br>                            | <br>                                                       |
| remove<br>                              | DAV:<br>      | remove property<br>                           | No<br>       | <! ELEMENT set (prop *)><br>                            | <br>                                                       |
| prop<br>                                | DAV:<br>      | remove property value<br>                     | No<br>       | <! ELEMENT prop ANY><br>                                | Delete using the XML tag specified as ANY as a key<br>     |
| prop<br>                                | DAV:<br>      | set property value<br>                        | No<br>       | <! ELEMENT prop ANY><br>                                | The XML tag specified as ANY is the key<br>                |
| ownerRepresentativeAccounts<br>         | <br>          | Upgrade setting<br>                           | Yes<br>      | <! ELEMENT ownerRepresentativeAccounts (account *)><br> | <br>                                                       |
| account<br>                             | <br>          | Account setting for upgrade<br>               | Yes<br>      | <! ELEMENT account ANY><br>                             | Specify an account name that allows upgrade as a value<br> |

#### Structure of XML

##### The body is XML and follows the following schema.

| Node name<br>      | Namespace<br> | Node type<br> | Overview<br>                                                                     | Notes<br>                                                                               |
|:-- |:-- |:-- |:-- |:-- |
| propertyupdate<br> | D:<br>        | Element<br>   | It represents the root of propertyupdate, and set and remove are children <br>   | <br>                                                                                    |
| set<br>            | D:<br>        | Element<br>   | Represents a property setting, and one or more props are children <br>           | <br>                                                                                    |
| remove<br>         | D:<br>        | Element<br>   | Represents a property deletion setting, and one or more props are children<br>   | <br>                                                                                    |
| prop<br>           | D:<br>        | Element<br>   | Represents a property value, and one or more arbitrary elements are children<br> | When set: Child node name is key<br>When remove: Delete with child node name as key<br> |

#### DTD notation

```dtd
<!ELEMENT propertyupdate (set, remove) >
<!ELEMENT set (prop*) >
<!ELEMENT remove (prop*) >
<!ELEMENT prop ANY>
```

#### 

##### The body is XML and follows the following schema.

| Node name<br>                    | Namespace<br> | Node type<br> | Overview<br>                                                                         | Notes<br> |
|:-- |:-- |:-- |:-- |:-- |
| ownerRepresentativeAccounts <br> | p:<br>        | Element<br>   | Represents an elevation setting list, one or more account elements are children<br>  | <br>      |
| account <br>                     | p:<br>        | Element<br>   | Describe account setting for promotion and describe account name to be promoted <br> | <br>      |

#### DTD notation

```dtd
<!ELEMENT ownerRepresentativeAccounts (account*)>
<!ELEMENT account (#PCDATA)>
```

#### Request Sample

```xml
<D:propertyupdate xmlns:D="DAV:" xmlns:p="urn:x-personium:xmlns">
  <D:set>
    <D:prop>
      <p:ownerRepresentativeAccounts><p:account>account1</p:account><p:account>account2</p:account></p:ownerRepresentativeAccounts>
    </D:prop>
  </D:set>
</D:propertyupdate>
```

<br>

### Response

#### Response Code

| Code<br> | Message<br>      | Overview<br> |
|:-- |:-- |:-- |
| 207<br>  | MULTI_STATUS<br> | Success<br>  |

#### Response Header

Unpublished

#### Response Body

Namespace

| URI<br>         | Overview<br>         | Notes (prefix)<br> |
|:-- |:-- |:-- |
| multistatus<br> | WebDAV Namespace<br> | D:<br>             |

#### Structure of XML

##### The body is XML and follows the following schema.

| Node name<br>    | Namespace<br> | Node type<br> | Overview<br>                                                                   | Notes<br>                                                                                                                    |
|:-- |:-- |:-- |:-- |:-- |
| multistatus <br> | D:<br>        | Element<br>   | Represents the route of multistatus and one or more responses are children<br> | <br>                                                                                                                         |
| response <br>    | D:<br>        | Element<br>   | Represents the contents of multistatus, and href and propstat are children<br> | <br>                                                                                                                         |
| href<br>         | D:<br>        | Element<br>   | URL of the resource that executed PROPPATCH<br>                                | <br>                                                                                                                         |
| propstat <br>    | D:<br>        | Element<br>   | Represents property setting result, prop and status are children<br>           | <br>                                                                                                                         |
| prop<br>         | D:<br>        | Element<br>   | Represents property setting contents<br>                                       | Display the result of resource setting as follows Setting Successful: Set key and value Deleted Successful: Deleted key<<br> |
| status<br>       | D:<br>        | Element<br>   | Property setting status code<br>                                               | In the case of setting success 200 (OK) is returned<br>                                                                      |

#### DTD notation

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
        <href>http://localhost:9998/testcell1/box1/patchcol</href>
        <propstat>
            <prop>
                <Z:Author xmlns:p="urn:x-personium:xmlns" xmlns:D="DAV:" xmlns:Z="http://www.w3.com/standards/z39.50/">Author1 update</Z:Author>
                <p:hoge xmlns:D="DAV:" xmlns:p="urn:x-personium:xmlns" xmlns:Z="http://www.w3.com/standards/z39.50/">fuga</p:hoge>
                <Z:Author xmlns:p="urn:x-personium:xmlns" xmlns:D="DAV:" xmlns:Z="http://www.w3.com/standards/z39.50/"/>
                <p:hoge xmlns:D="DAV:" xmlns:p="urn:x-personium:xmlns" xmlns:Z="http://www.w3.com/standards/z39.50/"/>
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
curl "https://{UnitFQDN}/cell -X PROPPATCH" -H 'Authorization: Bearer {AccessToken}' -d '<?xml version="1.0" encoding="utf-8" ?>
<D:propertyupdate xmlns:D="DAV:" xmlns:p="urn:x-personium:xmlns" xmlns:Z="http://www.w3.com/standards/z39.50/">
<D:set><D:prop><p:requireSchemaAuthz>confidential</p:requireSchemaAuthz></D:prop></D:set></D:propertyupdate>'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED