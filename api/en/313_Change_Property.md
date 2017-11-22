# File setting change

### Overview

Change properties

### Required Privileges

write-properties

### Restrictions

Unpublished

<br>

### Request

#### Request URL

```
/{CellName}
```

or

```
/{CellName}/{BoxName}
```

or

```
/{CellName}/{BoxName}/{ResourcePath}
```

| Path<br>           | Overview<br>         | Notes<br>                                                                                                                         |
|:-- |:-- |:-- |
| {CellName}<br>     | Cell Name<br>        | <br>                                                                                                                              |
| {BoxName}<br>      | Box Name<br>         | <br>                                                                                                                              |
| {ResourcePath}<br> | Path to resource<br> | Valid values Number of digits:1-128<br>Usable character types<br>alphanumeric character, period(.), under score(_), hyphen(-)<br> |

#### Request Method

PROPPATCH

#### Request Query

##### Common Request Query

| Query Name<br>    | Overview<br>                    | Effective Value<br>                                                                | Required<br> | Notes<br>                                                                                                                |
|:-- |:-- |:-- |:-- |:-- |
| p_cookie_peer<br> | Cookie Authentication Value<br> | The cookie authentication value returned from the server during authentication<br> | No<br>       | Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br> |

#### Request Header

##### Common Request Header

| Header Name<br>            | Overview<br>                                       | Effective Value<br>                                                                                        | Required<br> | Notes<br>                                                                                                                    |
|:-- |:-- |:-- |:-- |:-- |
| X-HTTP-Method-Override<br> | Method override function<br>                       | User-defined<br>                                                                                           | No<br>       | Specifying this value in a request with the POST method indicates that the specified value is used as the method<br>         |
| X-Override<br>             | Header override function<br>                       | ${OverwrittenHeaderName}:${Value}<br>                                                                      | No<br>       | The normal HTTP header value is overwritten. Specify multiple X-Override headers for the overwriting of multiple headers<br> |
| X-Personium-RequestKey<br> | RequestKey field value output in the event log<br> | Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br> | No<br>       | Supported in V 1.1.7 and later<br>                                                                                           |

##### Individual Request Header

| Header Name<br>   | Overview<br>                                                     | Effective Value<br>      | Required<br> | Notes<br>                                                                                          |
|:-- |:-- |:-- |:-- |:-- |
| Authorization<br> | Specifies authentication information in the OAuth 2.0 format<br> | Bearer {AccessToken}<br> | No<br>       | * Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br> |

| Header Name<br>  | Overview<br>                                   | Effective Value<br> | Required<br> | Notes<br> |
|:-- |:-- |:-- |:-- |:-- |
| Content-Type<br> | Specify content format<br>                     | application/xml<br> | No<br>       | <br>      |
| Accept<br>       | Specify acceptable media types in response<br> | application/xml<br> | No<br>       | <br>      |

#### Request Body

##### Namespace

| URI<br>                                 | Overview<br>                    | Reference prefix<br> |
|:-- |:-- |:-- |
| DAV:<br>                                | WebDAV Namespace<br>            | D:<br>               |
| urn:x-personium:xmlns<br>               | Personium namespace<br>         | p:<br>               |

\* Reference The prefixes are for making it easier to read the following table, but the use of these prefix strings is not ensured or requested.

##### Structure of XML

| Node name<br>      | Namespace<br> | Node type<br> | Overview<br>                                                                     | Notes<br>                                                                               |
|:-- |:-- |:-- |:-- |:-- |
| propertyupdate<br> | D:<br>        | Element<br>   | It represents the root of propertyupdate, and set and remove are children<br>    | <br>                                                                                    |
| set<br>            | D:<br>        | Element<br>   | Represents a property setting, and one or more props are children<br>            | <br>                                                                                    |
| remove<br>         | D:<br>        | Element<br>   | Represents a property deletion setting, and one or more props are children<br>   | <br>                                                                                    |
| prop<br>           | D:<br>        | Element<br>   | Represents a property value, and one or more arbitrary elements are children<br> | When set: Child node name is key<br>When remove: Delete with child node name as key<br> |

##### DTD notation

```dtd
<!ELEMENT propertyupdate (set, remove) >
<!ELEMENT set (prop*) >
<!ELEMENT remove (prop*) >
<!ELEMENT prop ANY>
```

#### Request Sample

```xml
<D:propertyupdate xmlns:D="DAV:"
    xmlns:p="urn:x-personium:xmlns">
    <D:set>
        <D:prop>
            <p:hoge>fuga</p:hoge>
        </D:prop>
    </D:set>
    <D:remove>
        <D:prop>
            <p:hoge/>
        </D:prop>
    </D:remove>
</D:propertyupdate>
```

<br>

### Response

#### Response Code

| Code<br> | Message<br>      | Overview<br> |
|:-- |:-- |:-- |
| 207<br>  | Multi-Status<br> | Success<br>  |

#### Response Header

Unpublished

#### Response Body

##### Namespace

| URI<br>  | Overview<br>         | Reference prefix<br> |
|:-- |:-- |:-- |
| DAV:<br> | WebDAV Namespace<br> | D:<br>               |

\* Reference The prefixes are for making it easier to read the following table, but the use of these prefix strings is not ensured or requested.

##### Structure of XML

The body is XML and follows the following schema.

| Node name<br>   | Namespace<br> | Node type<br> | Overview<br>                                                                   | Notes<br>                                                                                                                            |
|:-- |:-- |:-- |:-- |:-- |
| multistatus<br> | D:<br>        | Element<br>   | Represents the route of multistatus and one or more responses are children<br> | <br>                                                                                                                                 |
| response<br>    | D:<br>        | Element<br>   | Represents the contents of multistatus, and href and propstat are children<br> | <br>                                                                                                                                 |
| href<br>        | D:<br>        | Element<br>   | URL of the resource that executed PROPPATCH<br>                                | <br>                                                                                                                                 |
| propstat<br>    | D:<br>        | Element<br>   | Represents property setting result, prop and status are children<br>           | <br>                                                                                                                                 |
| prop<br>        | D:<br>        | Element<br>   | Represents property setting contentsbr                                         | Display the result of resource setting as follows <br> Setting Succeeded: Set Key and Value <br> Deleted Successful: Deleted Key<br> |
| status<br>      | D:<br>        | Element<br>   | Property setting status code<br>                                               | In the case of setting success 200 (OK) is returned<br>                                                                              |

##### DTD notation

##### namespace D:

```dtd
<!ELEMENT multistatus (response*)>
<!ELEMENT response (href, propstat)>
<!ELEMENT href (#PCDATA)>
<!ELEMENT propstat (prop, status)>
<!ELEMENT prop ANY>
<!ELEMENT status (#PCDATA)   
```

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

```xml
<multistatus xmlns="DAV:">
    <response>
        <href>https://{CellName}/{BoxName}/{ResourcePath}</href>
        <propstat>
            <prop>
                <p:hoge xmlns:p="urn:x-personium:xmlns" xmlns:D="DAV:">foo</p:hoge>
                <p:hoge xmlns:p="urn:x-personium:xmlns" xmlns:D="DAV:"/>
            </prop>
            <status>HTTP/1.1 200 OK</status>
        </propstat>
    </response>
</multistatus>
```

<br>

### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{ResourcePath}' -X PROPPATCH -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '<?xml version="1.0" encoding="utf-8" ?><D:propertyupdate xmlns:D="DAV:" xmlns:p="urn:x-personium:xmlns"><D:set><D:prop><p:hoge>${hoge}</p:hoge></D:prop></D:set><D:remove><D:prop><p:hoge/></D:prop></D:remove></D:propertyupdate>'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED
