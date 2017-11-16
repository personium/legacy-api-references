# Change Properties

### Overview

change Cell properties

### Required Privileges

Only unit users permitted

### Restrictions

Unpublished

<br>

### Request

#### Request URL

```
/{CellName}
```

#### Request Method

PROPPATCH

#### Request Query

| Query Name<br>    | Overview<br>                    | Effective Value<br>                                                                | Required<br> | Notes<br>                                                                                                                |
|:-- |:-- |:-- |:-- |:-- |
| p_cookie_peer<br> | Cookie Authentication Value<br> | The cookie authentication value returned from the server during authentication<br> | No<br>       | Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br> |

#### Request Header

| Header Name<br>            | Overview<br>                                                     | Effective Value<br>                   | Required<br> | Notes<br>                                                                                                         |
|:-- |:-- |:-- |:-- |:-- |
| X-HTTP-Method-Override<br> | Method override function<br>                                     | User-defined<br>                      | No<br>       | If you specify this value when requesting with the POST method, the specified value will be used as a method.<br> |
| X-Override<br>             | Header override function<br>                                     | ${OverwrittenHeaderName}:${Value}<br> | No<br>       | Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.<br>       |
| Authorization<br>          | Specifies authentication information in the OAuth 2.0 format<br> | Bearer {AccessToken}<br>              | No<br>       | * Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br>                |
| Content-Type<br>           | Specify content format<br>                                       | application / xml<br>                 | No<br>       | <br>                                                                                                              |
| Accept<br>                 | Specify acceptable media types in response<br>                   | application / xml<br>                 | No<br>       | <br>                                                                                                              |

#### Request Body

| Item Name<br>                           | Namespace<br> | Overview<br>                                  | Required<br> | Effective Value<br>                       | Notes<br>                                              |
|:-- |:-- |:-- |:-- |:-- |:-- |
| DAV:<br>                                | <br>          | XML namespace setting<br>                     | Yes<br>      | "DAV:"<br>                                | <br>                                                   |
| urn: x-personium: xmlns<br>             | <br>          | XML namespace setting<br>                     | Yes<br>      | "Urn: x-personium: xmlns"<br>             | <br>                                                   |
| http://www.w3.com/standards/z39.50/<br> | <br>          | XML namespace setting<br>                     | Yes<br>      | "Http://www.w3.com/standards/z39.50/"<br> | <br>                                                   |
| propertyupdate<br>                      | DAV:<br>      | propertyupdate (Access Control List) root<br> | Yes<br>      |                                           | <br>                                                   |
| set<br>                                 | DAV:<br>      | set property<br>                              | No<br>       | <! ELEMENT set (prop *)><br>              | <br>                                                   |
| remove<br>                              | DAV:<br>      | remove property<br>                           | No<br>       | <! ELEMENT set (prop *)><br>              | <br>                                                   |
| prop<br>                                | DAV:<br>      | remove property value<br>                     | No<br>       | <! ELEMENT prop ANY><br>                  | Delete using the XML tag specified as ANY as a key<br> |
| prop<br>                                | DAV:<br>      | set property value<br>                        | No<br>       | <! ELEMENT prop ANY><br>                  | The XML tag specified as ANY is the key<br>            |

#### Request Sample

```xml
<D:propertyupdate xmlns:D="DAV:"
    xmlns:p="urn:x-personium:xmlns"
    xmlns:Z="http://www.w3.com/standards/z39.50/">
    <D:set>
        <D:prop>
            <Z:Author>Author1 update</Z:Author>
            <p:hoge>fuga</p:hoge>
        </D:prop>
    </D:set>
    <D:remove>
        <D:prop>
            <Z:Author/>
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
| 207<br>  | MULTI_STATUS<br> | Success<br>  |

#### Response Header

None

#### Response Body

| Item Name<br>   | Namespace<br> | Overview<br>                                    | Notes<br>                                                                                                          |
|:-- |:-- |:-- |:-- |
| multistatus<br> | DAV:<br>      | Response Body route<br>                         | <! ELEMENT multistatus (response *)><br>                                                                           |
| response<br>    | DAV:<br>      | response route<br>                              | <! ELEMENT response (href, propstat)><br>                                                                          |
| href<br>        | DAV:<br>      | URL of the resource that executed PROPPATCH<br> | URL of the resource that executed PROPPATCH<br>                                                                    |
| propstat<br>    | DAV:<br>      | rproperty setting result<br>                    | <! ELEMENT propstat (prop, status)><br>                                                                            |
| prop<br>        | DAV:<br>      | property setting contents<br>                   | display resource setting result as following <br>success: setting key and value<br>delete success: deleted key<br> |
| status<br>      | DAV:<br>      | Property setting status code<br>                | In the case of setting success 200 (OK) is returned<br>                                                            |

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

```xml
<multistatus xmlns="DAV:">
    <response>
        <href>https://{UnitFQDN}/{CellName}/</href>
        <propstat>
            <prop>
                <Z:Author xmlns:p="urn:x-personium:xmlns" xmlns:D="DAV:" xmlns:Z="http://www.w3.com/standards/z39.50/">${author1}</Z:Author>
                <p:hoge xmlns:p="urn:x-personium:xmlns" xmlns:D="DAV:" xmlns:Z="http://www.w3.com/standards/z39.50/">${hoge}</p:hoge>
                <Z:Author xmlns:p="urn:x-personium:xmlns" xmlns:D="DAV:" xmlns:Z="http://www.w3.com/standards/z39.50/"/>
                <p:hoge xmlns:p="urn:x-personium:xmlns" xmlns:D="DAV:" xmlns:Z="http://www.w3.com/standards/z39.50/"/>
            </prop>
            <status>HTTP/1.1 200 OK</status>
        </propstat>
    </response>
</multistatus>
```

<br>

### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}" -X PROPPATCH -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '<?xml version="1.0" encoding="utf-8" ?>
<D:propertyupdate xmlns:D="DAV:" xmlns:p="urn:x-personium:xmlns" xmlns:Z="http://www.w3.com/standards/z39.50/"><D:set><D:prop><Z:Author>${author1}</Z:Author>
<p:hoge>${hoge}</p:hoge></D:prop></D:set><D:remove><D:prop><Z:Author/><p:hoge/></D:prop></D:remove></D:propertyupdate>'
```

<br>

###### Copyright 2017 FUJITSU LIMITED