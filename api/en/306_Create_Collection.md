# Create Collection

### Overview

Create a collection

### Required Privileges

write

### Restrictions

Common restriction

* None

WebDAV restriction

* Unpublished

<br>

### Request

#### Request URL

```
/{CellName}/{BoxName}/{CollectionName}
```

| Path<br>             | Overview<br>        | Notes<br>                                                                                                                         |
|:-- |:-- |:-- |
| {CellName}<br>       | Cell Name<br>       | <br>                                                                                                                              |
| {BoxName}<br>        | Box Name<br>        | <br>                                                                                                                              |
| {CollectionName}<br> | Collection Name<br> | Valid values Number of digits:1-128<br>Usable character types<br>alphanumeric character, period(.), under score(_), hyphen(-)<br> |

#### Request Method

MKCOL

#### Request Query

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

#### Request Body

##### Namespace

| URI<br>                   | Overview<br>            | Reference prefix<br> |
|:-- |:-- |:-- |
| DAV:<br>                  | WebDAV Namespace<br>    | D:<br>               |
| urn:x-personium:xmlns<br> | Personium namespace<br> | p:<br>               |

\* Reference The prefixes are for making it easier to read the following table, but the use of these prefix strings is not ensured or requested.

##### Structure of XML

The body is XML and follows the following schema.

| Node name<br>    | Namespace<br> | Node type<br> | Overview<br>                                                                                  | Notes<br>                                                                         |
|:-- |:-- |:-- |:-- |:-- |
| mkcol <br>       | D:<br>        | Element<br>   | Represents the root element of mkcol, set is a child<br>                                      | <br>                                                                              |
| set<br>          | D:<br>        | Element<br>   | Property setting, prop is a child <br>                                                        | <br>                                                                              |
| prop<br>         | D:<br>        | Element<br>   | It represents a property setting value, and resourcetype is a child <br>                      | <br>                                                                              |
| resourcetype<br> | D:<br>        | Element<br>   | It represents a resource type setting, and one of collection / odata / service is a child<br> | <br>                                                                              |
| collection<br>   | D:<br>        | Element<br>   | Represent a collection<br>                                                                    | If only the collection node is specified WebDAV collection creation becomes<br>   |
| odata<br>        | p:<br>        | Element<br>   | Represents an OData collection<br>                                                            | If collection node and odata node are specified OData collection creation<br>     |
| service<br>      | p:<br>        | Element<br>   | Represents a Service collection<br>                                                           | If collection node and service node are specified Service collection creation<br> |

##### DTD notation

##### Namespace D:

```dtd
<!ELEMENT mkcol (set) >
<!ELEMENT set (prop) >
<!ELEMENT prop (resourcetype) >
<!ELEMENT resourcetype (collection or odata or service) >
<!ELEMENT collection EMPTY>       
```

##### Namespace p:

```dtd
<!ELEMENT odata EMPTY>
<!ELEMENT service EMPTY>
```

#### Request Sample

Create WebDAV collection

```xml
<?xml version="1.0" encoding="utf-8"?>
<D:mkcol xmlns:D="DAV:" xmlns:p="urn:x-personium:xmlns">
  <D:set>
    <D:prop>
      <D:resourcetype>
        <D:collection/>
      </D:resourcetype>
    </D:prop>
  </D:set>
</D:mkcol>
```

Create OData collection

```xml
<?xml version="1.0" encoding="utf-8"?>
<D:mkcol xmlns:D="DAV:" xmlns:p="urn:x-personium:xmlns">
  <D:set>
    <D:prop>
      <D:resourcetype>
        <D:collection/>
        <p:odata/>
      </D:resourcetype>
    </D:prop>
  </D:set>
</D:mkcol>
```

Create Service Collection

```xml
<?xml version="1.0" encoding="utf-8"?>
<D:mkcol xmlns:D="DAV:" xmlns:p="urn:x-personium:xmlns">
  <D:set>
    <D:prop>
      <D:resourcetype>
        <D:collection/>
        <p:service/>
      </D:resourcetype>
    </D:prop>
  </D:set>
</D:mkcol>
```

<br>

### Response

#### Response Code

201

#### Response Header

##### Common Response Header

| Header Name<br>                 | Overview<br>                                     | Notes<br>                                          |
|:-- |:-- |:-- |
| Access-Control-Allow-Origin<br> | Cross domain communication permission header<br> | Return value fixed to "*"<br>                      |
| X-Personium-Version<br>         | API version that the request is processed<br>    | Version of the API used to process the request<br> |

##### WebDAVCommon Response Header

| Header Name<br> | Overview<br>                     | Notes<br>                                                   |
|:-- |:-- |:-- |
| ETag<br>        | Resource version information<br> | Return only when collection can be created successfully<br> |

#### Response Body

None

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

None

<br>

### cURL Command

Create WebDAV collection

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{CollectionName}" -X MKCOL -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '<?xml version="1.0" encoding="utf-8"?><D:mkcol xmlns:D="DAV:" xmlns:p="urn:x-personium:xmlns"><D:set><D:prop><D:resourcetype><D:collection/></D:resourcetype></D:prop></D:set></D:mkcol>'
```

Create OData collection

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{CollectionName}" -X MKCOL -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '<?xml version="1.0" encoding="utf-8"?><D:mkcol xmlns:D="DAV:" xmlns:p="urn:x-personium:xmlns"><D:set><D:prop><D:resourcetype><D:collection/><p:odata/></D:resourcetype></D:prop></D:set></D:mkcol>'
```

Create Service Collection

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{CollectionName}" -X MKCOL -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '<?xml version="1.0" encoding="utf-8"?><D:mkcol xmlns:D="DAV:" xmlns:p="urn:x-personium:xmlns"><D:set><D:prop><D:resourcetype><D:collection/><p:service/></D:resourcetype></D:prop></D:set></D:mkcol>'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED