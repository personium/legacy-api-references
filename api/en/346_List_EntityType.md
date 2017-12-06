# Get EntityType list

### Overview

Get a list of existing EntityType information

### Required Privileges

read

### Restrictions

* Always handles Content-Type in the request header as application/json
* Only accepts the request body in the JSON format
* Only application/json is supported for Content-Type in the request header and the JSON format for the response body
* Response body data is not ensured if atom or xml is specified in the $format query option, although it does not result in an error


### Request

#### Request URL

```
/{CellName}/{BoxName}/{ODataCollecitonName}/$metadata/EntityType
```

#### Request Method

GET

#### Request Query

The following query parameters are available

|Query Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|p_cookie_peer|Cookie Authentication Value|The cookie authentication value returned from the server during authentication|No|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used|

[$select  Query](406_Select_Query.html)

[$expand  Query](405_Expand_Query.html)

[$format  Query](404_Format_Query.html)

[$filter  Query](403_Filter_Query.html)

[$inlinecount  Query](407_Inlinecount_Query.html)

[$orderby  Query](400_Orderby_Query.html)

[$top  Query](401_Top_Query.html)

[$skip  Query](402_Skip_Query.html)

[Full-text Search (q) Query](408_Full_Text_Search_Query.html)

#### Request Header

##### Common Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|Method override function|User-defined|No|Specifying this value in a request with the POST method indicates that the specified value is used as the method|
|X-Override|Header override function|${OverwrittenHeaderName}:${Value}|No|The normal HTTP header value is overwritten. Specify multiple X-Override headers for the overwriting of multiple headers|
|X-Personium-RequestKey|RequestKey field value output in the event log|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters|No|Supported in V 1.1.7 and later|

##### OData Common Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|Authorization|Specifies authentication information in the OAuth 2.0 format|Bearer {AccessToken}|No|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API|

##### OData Acquire List Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|Accept|Specifies the response body format|application/json|No|[application/json] by default|

#### Request Sample

None

#### Request Sample

None


### Response

#### Response Code

200

#### Response Header

None

#### Response Body

##### Common

The response is a JSON object, the correspondence between the key (name) and type defined in the object (subobject) and the value are as follows

|Object|Name(Key)|Type|Value|
|:--|:--|:--|:--|
|Root|d|object|Object{1}|
|{1}|results|array|Array object {2}|
|{2}|__metadata|object|Object{3}|
|{3}|uri|string|URL to the resource that was created|
|{3}|etag|string|Etag value|
|{2}|__published|string|Creation date (UNIX time)|
|{2}|__updated|string|Update date (UNIX time)|
|{1}|__count|string|Get number of results in $inlinecount query|

##### EntityType specific response body

|Object|Name(Key)|Type|Value|
|:--|:--|:--|:--|
|{3}|type|string|ODataSvcSchema.EntityType|
|{2}|Name|string|EntityType name|

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

```JSON
{
  "d": {
    "results": [
      {
        "__metadata": {
          "uri": "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/$metadata/EntityType('{EntityTypeName}')",
          "etag": "W/\"1-1487586234224\"",
          "type": "ODataSvcSchema.EntityType"
        },
        "Name": "{EntityTypeName}",
        "__published": "/Date(1487586234224)/",
        "__updated": "/Date(1487586234224)/",
        "_AssociationEnd": {
          "__deferred": {
            "uri": "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/$metadata/EntityType('{EntityTypeName}')/_AssociationEnd"
          }
        },
        "_Property": {
          "__deferred": {
            "uri": "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/$metadata/EntityType('{EntityTypeName}')/_Property"
          }
        }
      },
      {
        "__metadata": {
          "uri": "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/$metadata/EntityType('{EntityTypeName}')",
          "etag": "W/\"1-1487589344011\"",
          "type": "ODataSvcSchema.EntityType"
        },
        "Name": "{EntityTypeName}",
        "__published": "/Date(1487589344011)/",
        "__updated": "/Date(1487589344011)/",
        "_AssociationEnd": {
          "__deferred": {
            "uri": "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/$metadata/EntityType('{EntityTypeName}')/_AssociationEnd"
          }
        },
        "_Property": {
          "__deferred": {
            "uri": "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/$metadata/EntityType('{EntityTypeName}')/_Property"
          }
        }
      }
    ]
  }
}
```


### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{OdataCollecitonName}/\$metadata/EntityType" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```


###### Copyright 2017 FUJITSU LIMITED