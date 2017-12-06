# Create Cell

### Overview

This API creates a new Cell

### Required Privileges

Only unit users permitted

### Restrictions

* General Restrictions
    * None

* OData Restrictions
    * Always handles Content-Type in the request header as application/json
    * Only accepts the request body in the JSON format
    * Only application/json is supported for Content-Type in the request header and the JSON format for the response body
    * Response body data is not ensured if atom or xml is specified in the $format query option, although it does not result in an error


### Request

#### Request URL

```
/__ctl/Cell
```

#### Request Method

POST

#### Request Query

None

#### Request Header

##### Common Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|Method override function|User-defined|No|Specifying this value in a request with the POST method indicates that the specified value is used as the method|
|X-Override|Header override function|${OverwrittenHeaderName}:${Value}|No|The normal HTTP header value is overwritten. Specify multiple X-Override headers for the overwriting of multiple headers|
|X-Personium-RequestKey|RequestKey field value output in the event log|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters|No|PCS-${UNIXtime} by default<br>Supported in V 1.1.7 and later|

##### OData Common Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|Authorization|Specifies authentication information in the OAuth 2.0 format|Bearer {AccessToken}|No|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API|

##### OData Create Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|Content-Type|Specifies the request body format|application/json|No|[application/json] by default|
|Accept|Specifies the response body format|application/json|No|[application/json] by default|

#### Request Body

##### Format

JSON

|Item Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|Name|Cell name|Number of digits: 1 - 128<br>Character type: Single-byte alphanumeric characters, hyphens ("-"), and underscores ("\_")<br>However, the string cannot start with a underscore ("\_") or colon (:)|Yes||

#### Request Sample

```JSON
{"Name":"{CellName}"}
```


### Response

#### Response Code

201

#### Response Header

|Header Name|Overview|Notes|
|:--|:--|:--|
|Content-Type|Format of data to be returned||
|Location|URL to the resource that was created||
|DataServiceVersion|OData version||
|ETag|Resource version information||
|Access-Control-Allow-Origin|Cross domain communication permission header|Return value fixed to "*"|
|X-Personium-Version|Personium API version|Version of the API used to process the request|

#### Response Body

##### Common

The response is a JSON object defined in an object (subobject)

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

##### Individual response body

|Object|Name(Key)|Type|Value|
|:--|:--|:--|:--|
|{3}|type|string|UnitCtl.Cell|
|{2}|Name|string|Cell name|

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

```JSON
{
  "d": {
    "results": {
      "__metadata": {
        "uri": "https://{UnitFQDN}/__ctl/Cell(Name='{CellName}')",
        "etag": "W/\"1-1486427790039\"",
        "type": "UnitCtl.Cell"
      },
      "Name": "{CellName}",
      "__published": "/Date(1486427790039)/",
      "__updated": "/Date(1486427790039)/"
    }
  }
}
```


### cURL Command

```sh
curl "https://{UnitFQDN}/__ctl/Cell" -X POST -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"Name":"{CellName}"}'
```


###### Copyright 2017 FUJITSU LIMITED
