# Log File Acquire List

### Overview

Acquire the list of log files under the specified URL

### Required Privileges

log-read

### Restrictions

* Maximum depth of the hierarchy of the collection 50
* The maximum value of the child number of elements in each collection under 10,000

    * The collection immediately below the Box is regarded as the first level, and the collection is counted as the second level, the third level, Files to be created under the collection are not counted as hierarchies

* Log output of the internal event is not supported
*  Log output configuration is not supported. Log output configuration reference is not supported.
* Set the log file name as "default.log "
*  Rotate will be according to following default settings

    * Set rotate size: 50MB

* Configure the log output level to "info" (output for all INFO, WARN, ERROR).
* The file name when rotated is default.log. {Timestamp}. {Timestamp} is numbered by the time when it was rotated.

| Action<br>         | Archived log file<br>                                                                               | Description<br>                                                                            | Notes<br>                                                                                   |
|:-- |:-- |:-- |:-- |
| First Rotation<br> | archive/<br>default.log.1402910774659<br>                                                           | <br>Newly rotated file<br>                                                                 | <br>2014-06-16 18:26:14 +0900<br>                                                           |
| 2nd Rotation<br>   | archive/<br>default.log.1402910774659<br>default.log.1403910784659<br>                              | <br>the preceding roteted file<br>Newly rotated file<br>                                   | <br>2014-06-16 18:26:14 +0900<br>2014-06-28 08:13:04 +0900<br>                              |
| 3rd Rotation<br>   | archive/<br>default.log.1402910774659<br>default.log.1403910784659<br>default.log.1403910784659<br> | <br>the file before preceding file<br>the preceding roteted file<br>Newly rotated file<br> | <br>2014-06-16 18:26:14 +0900<br>2014-06-28 08:13:04 +0900<br>2014-07-09 21:59:44 +0900<br> |

<br>

### Request

#### Request URL

##### Recent log file

```
/{CellName}/__log/current
```

##### Log file that is rotated

```
/{CellName}/__log/archive
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

| URI<br>  | Overview<br>         | notes()prefix<br> |
|:-- |:-- |:-- |
| DAV:<br> | WebDAV Namespace<br> | D:<br>            |

\* Reference The prefixes are for making it easier to read the following table, but the use of these prefix strings is not ensured or requested.

##### Structure of XML

The body is XML and follows the following schema.

| Node name<br> | Namespace<br> | Node type<br> | Overview<br>                                                         | Notes<br>                                                                                                                                                   |
|:-- |:-- |:-- |:-- |:-- |
| propfind<br>  | D:<br>        | Element<br>   | Represents the root element of propfind, and allprop is a child.<br> | <br>                                                                                                                                                        |
| allprop<br>   | D:<br>        | Element<br>   | Represent setting to retrieve all properties<br>                     | allprop : get all properties<br>Even if the request body is empty, treat it as allprop<br>Elements other than allprop are not supported for v1.2 , v1.1<br> |

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

| Code<br> | Message<br>      | Overview<br>             |
|:-- |:-- |:-- |
| 207<br>  | Multi-Status<br> | when acquire succeed<br> |

#### Response Header

| Item Name<br>                   | Overview<br>                                     | Notes<br>                     |
|:-- |:-- |:-- |
| Content-Type<br>                | MimeType depending on Resource<br>               | "application/xml"<br>         |
| Access-Control-Allow-Origin<br> | Cross domain communication permission header<br> | Return value fixed to "*"<br> |
| X-Personium-Version<br>         | API version that the request is processed<br>    | Valid version                 |

#### Response Body

##### Namespace

| URI<br>                   | Overview<br>            | Reference Prefix<br> |
|:-- |:-- |:-- |
| DAV:<br>                  | WebDAV Namespace<br>    | D:<br>               |
| urn:x-personium:xmlns<br> | Personium namespace<br> | p:<br>               |

\* Reference The prefixes are for making it easier to read the following table, but the use of these prefix strings is not ensured or requested.

##### Structure of XML

The body is XML and follows the following schema.

| Node name<br>        | Namespace<br> | Node type<br> | Overview<br>                                                                                                         | Notes<br>                                                                                                                                                                                                    |
|:-- |:-- |:-- |:-- |:-- |
| multistatus<br>      | D:<br>        | Element<br>   | Represents the route of multistatus and one or more responses are children<br>                                       | <br>                                                                                                                                                                                                         |
| response<br>         | D:<br>        | Element<br>   | Represents a response to resource acquisition, href and propstat are children<br>                                    | <br>                                                                                                                                                                                                         |
| href<br>             | D:<br>        | Element<br>   | Resource url<br>                                                                                                     | <br>                                                                                                                                                                                                         |
| propstat<br>         | D:<br>        | Element<br>   | Represents property information of a resource, status and prop are children<br>                                      | <br>                                                                                                                                                                                                         |
| status<br>           | D:<br>        | Element<br>   | Represents the response code of resource acquisition<br>                                                             | <br>                                                                                                                                                                                                         |
| prop<br>             | D:<br>        | Element<br>   | Property detailed information, creationdate, resourcetype, acl, and proppatch setting values are children<br>        | <br>                                                                                                                                                                                                         |
| creationdate<br>     | D:<br>        | Element<br>   | Resource creation time<br>                                                                                           | <br>                                                                                                                                                                                                         |
| getcontentlength<br> | D:<br>        | Element<br>   | Resource size<br>                                                                                                    | Resource is only file<br>                                                                                                                                                                                    |
| getcontenttype<br>   | p:<br>        | Element<br>   | Contenttype of resources<br>                                                                                         | Resource is only file<br>                                                                                                                                                                                    |
| getlastmodified<br>  | p:<br>        | Element<br>   | Resource update time<br>                                                                                             | <br>                                                                                                                                                                                                         |
| resourcetype<br>     | p:<br>        | Element<br>   | Represents the type of the resource. <br>Eventually the collection, OData service will vary, child will be empty<br> | <br>                                                                                                                                                                                                         |
| collection<br>       | p:<br>        | Element<br>   | Represents that the type of resource is a collection<br>                                                             | Displays, if resource is collection<br>                                                                                                                                                                      |
| odata<br>            | p:<br>        | Element<br>   | Represents that the type of resource is a odata collection<br>                                                       | Displays, if resource is odata collection<br>                                                                                                                                                                |
| service<br>          | p:<br>        | Element<br>   | Represents that the type of resource is a service collection<br>                                                     | Displays, if resource is service collection<br>                                                                                                                                                              |
| acl<br>              | p:<br>        | Element<br>   | ACL setting set for resource<br>                                                                                     | In order to acquire the ACL setting, acl-read authority for the target resource is required. For contents below the ACL element, refer to the [cell level access control setting API](289_Cell_ACL.html)<br> |
| base<br>             | p:<br>        | Element<br>   | ACL Privilege BaseURL<br>                                                                                            | When PROPFIND to Cell, default box ("__") resource URL<br>                                                                                                                                                   |

##### DTD notation

##### Namespace: D:

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

##### Namespace:p:

```dtd
<!ELEMENT odata EMPTY>
<!ELEMENT service EMPTY>
```

##### Namespace: xml:

```dtd
<!ATTLIST acl base CDATA #IMPLIED>
```

#### Response Sample

##### resourcetype element

```xml
<?xml version="1.0" encoding="utf-8"?>
<multistatus xmlns="DAV:">
    <response>
        <href>https://{UnitFQDN}/{CellName}/__log/archive</href>
        <propstat>
            <prop>
                <creationdate>2017-02-03T01:27:31.093+0000</creationdate>
                <getlastmodified>Fri, 03 Feb 2017 01:27:31 GMT</getlastmodified>
                <resourcetype>
                    <collection/>
                </resourcetype>
            </prop>
            <status>HTTP/1.1 200 OK</status>
        </propstat>
    </response>
</multistatus>
```

In addition, it is planned to specify the file ZIP compaction flag at the time of log file rotation. By default (log configuration update API) is uncompressed.<br>
In this case, based on the log file compression flag, file name of href component and Mime Type of getcontenttype component will switch.

| ZIP compression flag<br> | File name of href component (ex:)<br> | Value of getcontenttype<br> | Notes<br>                                            |
|:-- |:-- |:-- |:-- |
| Uncompressed<br>         | default.log.1364460341902<br>         | text/csv<br>                | Applies the same even at the case of no rotation<br> |
| Compressed<br>           | default.log.1364460341902.zip<br>     | application/zip<br>         | <br>                                                 |

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/__log/archive" -X PROPFIND -i -H 'Depth:1' -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED