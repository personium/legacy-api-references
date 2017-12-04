# AssociationEnd Registration

### Overview

Register user data AssociationEnd

### Required Privileges

alter-schema

### Restrictions

* Always handles Content-Type in the request header as application/json
* Only accepts the request body in the JSON format
* Only application/json is supported for Content-Type in the request header and the JSON format for the response body
* Response body data is not ensured if atom or xml is specified in the $format query option, although it does not result in an error
* AssociationEnd restriction
    * When AssociationEnd specifies"1"in Multiplicity, it operates as "0..1".

<br>

### Request

#### Request URL

```
/{CellName}/{BoxName}/{ODataCollecitonName}/$metadata/AssociationEnd
```

#### Request Method

POST

#### Request Query

|Query Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|p_cookie_peer<br>|Cookie Authentication Value<br>|The cookie authentication value returned from the server during authentication<br>|No<br>|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br>|

#### Request Header

##### Common Request Header

|Header Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|Method override function<br>|User-defined<br>|No<br>|Specifying this value in a request with the POST method indicates that the specified value is used as the method<br>|
|X-Override<br>|Header override function<br>|${OverwrittenHeaderName}:${Value}<br>|No<br>|The normal HTTP header value is overwritten. Specify multiple X-Override headers for the overwriting of multiple headers<br>|
|X-Personium-RequestKey<br>|RequestKey field value output in the event log<br>|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br>|No<br>|Supported in V 1.1.7 and later<br>|

##### OData Common Request Header

|Header Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|Authorization<br>|Specifies authentication information in the OAuth 2.0 format<br>|Bearer {AccessToken}<br>|No<br>|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br>|

##### OData Create Request Header

|Header Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|Content-Type<br>|Specifies the request body format<br>|application/json<br>|No<br>|[application/json] by default<br>|
|Accept<br>|Specifies the response body format<br>|application/json<br>|No<br>|[application/json] by default<br>|

#### Request Body

##### Format

JSON

|Item Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|Name<br>|AssociationName<br>|Number of digits: 1 - 128<br>Character type: Single-byte alphanumeric characters, hyphens ("-"), and underscores ("\_")<br>However, the string cannot start with a single-byte hyphen ("-") or underscore ("\_")<br>|Yes<br>|<br>|
|Multiplicity<br>|Multiplicity<br>|"0 .. 1" / "1" / "*"<br>|Yes<br>|<br>|
|_EntityType.Name<br>|EntityType name of relation target<br>|Number of digits: 1 - 128<br>Character type: Single-byte alphanumeric characters, hyphens ("-"), and underscores ("\_")<br>However, the string cannot start with a single-byte hyphen ("-") or underscore ("\_")<br>Explanation: Registered EntityType by EntityType registration API<br>|Yes<br>|<br>|

#### Request Sample

```JSON
{
   "Name": "{AssociationEndName}",
  "Multiplicity": "{Multiplicity}",
  "_EntityType.Name": "{EntityTypeName}"
}
```

<br>

### Response

#### Response Code

201

#### Response Header

##### Common Response Header

|Header Name<br>|Overview<br>|Notes<br>|
|:--|:--|:--|
|Access-Control-Allow-Origin<br>|Cross domain communication permission header<br>|Return value fixed to "*"<br>|
|X-Personium-Version<br>|API version that the request is processed<br>|Version of the API used to process the request<br>|

##### OData Response Header

|Header Name<br>|Overview<br>|Notes<br>|
|:--|:--|:--|
|Content-Type<br>|Format of data to be returned<br>|<br>|
|Location<br>|URL to the resource that was created<br>|<br>|
|DataServiceVersion<br>|OData version<br>|<br>|
|ETag<br>|Resource version information<br>|<br>|

#### Response Body

##### Common

The response is a JSON object, the correspondence between the key (name) and type defined in the object (subobject) and the value are as follows

|Object<br>|Name(Key)<br>|Type<br>|Value<br>|
|:--|:--|:--|:--|
|Root<br>|d<br>|object<br>|Object{1}<br>|
|{1}<br>|results<br>|array<br>|Array object {2}<br>|
|{2}<br>|__metadata<br>|object<br>|Object{3}<br>|
|{3}<br>|uri<br>|string<br>|URL to the resource that was created<br>|
|{3}<br>|etag<br>|string<br>|Etag value<br>|
|{2}<br>|__published<br>|string<br>|Creation date (UNIX time)<br>|
|{2}<br>|__updated<br>|string<br>|Update date (UNIX time)<br>|
|{1}<br>|__count<br>|string<br>|Get number of results in $inlinecount query<br>|

##### AssociationEnd Individual response body

|Object<br>|Name(Key)<br>|Type<br>|Value<br>|
|:--|:--|:--|:--|
|{3}<br>|type<br>|string<br>|ODataSvcSchema.AssociationEnd<br>|
|{2}<br>|Name<br>|string<br>|AssociationEndName<br>|
|{2}<br>|Multiplicity<br>|string<br>|Multiplicity<br>|
|{2}<br>|_EntityType.Name<br>|string<br>|EntityType name of relation target<br>|

#### Response Sample

```JSON
{
  "d": {
    "results": {
      "__metadata": {
        "uri": "https://{UnitFQDN}/{CellName}/{BoxName}/{CollectionName}/$metadata/AssociationEnd(Name='{AssociationEndName}',_EntityType.Name='{EntityTypeName}')",
        "etag": "W/\"1-1487652733383\"",
        "type": "ODataSvcSchema.AssociationEnd"
      },
      "Name": "{AssociationEndName}",
      "Multiplicity": "{Multiplicity}",
      "_EntityType.Name": "{EntityTypeName}",
      "__published": "/Date(1487652733383)/",
      "__updated": "/Date(1487652733383)/"
    }
  }
}
```

### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/\$metadata/AssociationEnd" -X POST -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{ "Name": "{AssociationEndName}", "Multiplicity": "{Multiplicity}", "_EntityType.Name": "{EntityTypeName}"}'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED
