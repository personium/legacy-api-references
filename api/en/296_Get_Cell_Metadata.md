# Get Cell Metadata

## Overview

Get the metadata of Cell. The metadata includes the following information.

* Cell name
* Cell URL

### Required Privileges

None


## Request

### Request URL

```
{CellURL}
```

### Request Method

GET

### Request Query

None

### Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|Accept|Specifies the response body format|application/json|Yes||


## Response

### Response Code

200

### Response Header

|Header Name|Overview|Notes|
|:--|:--|:--|
|Content-Type|Format of data to be returned||
|Access-Control-Allow-Origin|Cross domain communication permission header|Return value fixed to "*"|
|X-Personium-Version|Personium API version|Version of the API used to process the request|

### Response Body

Response is JSON format, defined in object (subobject).  
The correspondence between key (name) and type, and value are as follows.  

|Object|Name(Key)|Type|Value|Notes|
|:--|:--|:--|:--|:--|
|Root|cell|object|Object (cell format)||
|cell|name|string|The name of the Cell||
|cell|url|string|The URL of the Cell||

### Error Messages

Refer to [Error Message List](004_Error_Messages.md)

### Response Sample

```JSON
{
  "cell": {
      "name": "cell1",
      "url": "https://example.com/cell1/"
  }
}
```

## cURL Command

```sh
curl "{CellURL}" -X GET -i -H 'Accept: application/json'
```
