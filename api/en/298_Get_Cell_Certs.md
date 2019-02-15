# Retrieve Cell Publickkey info

## Overview

OpenID ConnectのID Tokenの検証に利用可能な公開鍵情報を取得する。
公開鍵情報には、以下の情報が含まれる。  

* Key type
* Signature algorithm
* Key ID
* Modulus (RSA)
* Exponent (RSA)

### Required Privileges

None


## Request

### Request URL

```
{CellURL}/__certs
```

### Request Method

GET

### Request Query

None

### Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|Accept|Specifies the response body format|application/json|No||


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
|Root|keys|array|key info array||
|keys|kty|string|"RSA"||
|keys|alg|string|"RS256"||
|keys|kid|string|key id|It can be used for identification when there are multiple key info.|
|keys|n|string|RSA Modulus||
|keys|e|string|RSA Exponent||

### Error Messages

Refer to [Error Message List](004_Error_Messages.md)

### Response Sample
```JSON
{
  "keys": [
    {
      "kty": "RSA",
      "alg": "RS256",
      "kid": "fdef9343-68e7-4104-b817-81fe933ae2a0",
      "n": "k52ZUTZpBFU6vH2QK70ahb_FUvcSson5-SGqHmy5_ZliGLiCPoEd_FkQ1jksVzhcVPluD8PouNyukShCMrAszg9kYqJn5dZLUC6nQJFuYHgaoZOYxA1CMC-McG-HXifRlSf9jb4XQG_sU4VQlgaALEof1nH0b3ZkEwJ-HnjXChJvTfVQfYuvcRr2wIqS9AtylCU8N07X8ZN2n-CvIoFjF1RxZefBUxXlhioilE0S17dVn8gwGTSfa-hWq-Pot299K3QvKXuQmnh4gu8UQg7v5aMzNzxjJGeFuv8ui6hZ0oVZCinzTDyFXWkUVo_iXtyCEYcAxwGz-sCAhz0pHI6ttQ",
      "e": "AQAB"
    }
  ]
}
```

## cURL Command
```sh
curl "https://cell1.unit1.example/__certs" -X GET -i -H 'Accept: application/json'
```
