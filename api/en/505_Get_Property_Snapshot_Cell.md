# Acquire Cell snapshot file setting

### Overview

Get Cell snapshot file property.

### Required Privileges

root

### Restrictions

* A function that specifies properties to be returned in the response body(Become current allprop)

<br>

### Request

#### Request URL

```
/{CellName}/__snapshot
```

or

```
/{CellName}/__snapshot/{FileName}
```

#### Request Method

PROPFIND

#### Request Query

|Query Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|p_cookie_peer<br>|Cookie Authentication Value<br>|The cookie authentication value returned from the server during authentication<br>|No<br>|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br>|

#### Request Header

|Header Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|Method override function<br>|User-defined<br>|No<br>|Specifying this value in a request with the POST method indicates that the specified value is used as the method<br>|
|X-Override<br>|Header override function<br>|${OverwrittenHeaderName}:${Value}<br>|No<br>|The normal HTTP header value is overwritten. Specify multiple X-Override headers for the overwriting of multiple headers<br>|
|X-Personium-RequestKey<br>|RequestKey field value output in the event log<br>|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br>|No<br>|PCS-${UNIXtime} by default<br>Supported in V 1.1.7 and later<br>|
|Authorization<br>|Specifies authentication information in the OAuth 2.0 format<br>|Bearer {AccessToken}<br>|No<br>|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br>|
|Depth<br>|To get the hierarchy of a resource<br>|0:The target resource itself <br>1:Gets target and resources directly under the target<br>|Yes<br>|<br>|

#### Request Body

Namespace

|URI<br>|Overview<br>|Reference prefix<br>|
|:--|:--|:--|
|DAV:<br>|WebDAV Namespace<br>|D:<br>|

\* Reference The prefixes are for making it easier to read the following table, but the use of these prefix strings is not ensured or requested.

Structure of XML

The body is XML and follows the following schema.

|Node name<br>|Namespace<br>|Node type<br>|Overview<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|propfind<br>|D:<br>|Element<br>|Represents the root element of propfind and allprop is a child <br>|<br>|
|allprop <br>|D:<br>|Element<br>|Represent setting to retrieve all properties <br>|allprop : get all properties<br>Even if the request body is empty, treat it as allprop<br>Elements other than allprop are not supported<br>|

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

|Code<br>|Message<br>|Overview<br>|
|:--|:--|:--|
|207<br>|Multi-Status<br>|Success<br>|

#### Response Header

|Header Name<br>|Overview<br>|Notes<br>|
|:--|:--|:--|
|Content-Type<br>|Format of data to be returned<br>|<br>|
|Access-Control-Allow-Origin<br>|Cross domain communication permission header<br>|Return value fixed to "*"<br>|
|X-Personium-Version<br>|API version that the request is processed<br>|Version of the API used to process the request<br>|

#### Response Body

Namespace

|URI<br>|Overview<br>|Reference prefix<br>|
|:--|:--|:--|
|DAV:<br>|WebDAV Namespace<br>|D:<br>|
|urn:x-personium:xmlns<br>|Personium namespace<br>|p:<br>|

\* Reference The prefixes are for making it easier to read the following table, but the use of these prefix strings is not ensured or requested.

Structure of XML

The body is XML and follows the following schema.

|Node name<br>|Namespace<br>|Node type<br>|Overview<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|multistatus<br>|D:<br>|Element<br>|Represents the route of multistatus and one or more responses are children<br>|<br>|
|response<br>|D:<br>|Element<br>|Represents a response to resource acquisition, href and propstat are children<br>|<br>|
|href<br>|D:<br>|Element<br>|Resource url<br>|<br>|
|propstat<br>|D:<br>|Element<br>|Represents property information of a resource, status and prop are children<br>|<br>|
|status<br>|D:<br>|Element<br>|Represents the response code of resource acquisition<br>|<br>|
|prop<br>|D:<br>|Element<br>|Property detailed information, creationdate, resourcetype, acl, and proppatch setting values are children<br>|<br>|
|creationdate<br>|D:<br>|Element<br>|Resource creation time<br>|<br>|
|getcontentlength<br>|D:<br>|Element<br>|Resource size<br>|Resource is only file<br>|
|getcontenttype<br>|D:<br>|Element<br>|Contenttype of resources<br>|Resource is only file<br>|
|getlastmodified<br>|D:<br>|Element<br>|Resource update time<br>|<br>|
|resourcetype<br>|D:<br>|Element<br>|Represents the type of the resource. <br>If collection is a child or child is empty<br>|<br>|
|collection<br>|D:<br>|Element<br>|Represents that the type of resource is a collection<br>|Displays, if resource is collection<br>|
|acl<br>|D:<br>|Element<br>|ACL setting set for resource<br>|ACL Element See the [ Cell Level Access Control Settings API ](289_Cell_ACL.html) for content below<br>|
|base<br>|xml:<br>|attribute<br>|ACL Privilege BaseURL<br>|When PROPFIND to Cell, default box ("__") resource URL<br>|

DTD notation

Namespace D:

```dtd
<!ELEMENT multistatus (response*)>
<!ELEMENT response (href, propstat)>
<!ELEMENT href (#PCDATA)>
<!ELEMENT propstat (status, prop)>
<!ELEMENT status (#PCDATA)>
<!ELEMENT prop (creationdate, resourcetype, ANY)>
<!ELEMENT creationdate (#PCDATA)>
<!ELEMENT getcontentlength (#PCDATA)>
<!ELEMENT getcontenttype (#PCDATA)>
<!ELEMENT getlastmodified (#PCDATA)>
<!ELEMENT resourcetype ((collection or EMPTY))>
<!ELEMENT collection EMPTY>
<!ELEMENT acl (ace*)>
```

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

```xml
<multistatus xmlns="DAV:">
    <response>
        <href>https://{UnitFQDN}/{CellName}/__snapshot/CellExport_2017_01.zip</href>
        <propstat>
            <prop>
                <creationdate>2017-02-15T01:52:34.635+0000</creationdate>
                <getcontentlength>2000000</getcontentlength>
                <getcontenttype>application/zip</getcontenttype>
                <getlastmodified>Wed, 15 Feb 2017 01:52:34 GMT</getlastmodified>
                <resourcetype/>
                <acl xmlns:p="urn:x-personium:xmlns"/>
            </prop>
            <status>HTTP/1.1 200 OK</status>
        </propstat>
    </response>
</multistatus>
```

<br>

### cURL Sample

```sh
curl "https://{UnitFQDN}/{CellName}/__snapshot/CellExport_2017_01.zip" -X PROPFIND -i  -H 'Depth:0' -H 'Authorization: Bearer {AccessToken}' -d '<?xml version="1.0" encoding="utf-8"?><D:propfind xmlns:D="DAV:"><D:allprop/></D:propfind>'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED
