# Accept Event

## Overview

This API accepts the requested event

### Required Privileges

event

## Request

### Request URL

```
/{CellName}/__event
```

### Request Method

POST

### Request Query

|Query Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|p_cookie_peer|Cookie Authentication Value|The cookie authentication value returned from the server during authentication|No|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used|

### Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|Method override function|User-defined|No|If you specify this value when requesting with the POST method, the specified value will be used as a method.|
|X-Override|Header override function|${OverwrittenHeaderName}:${Value}|No|Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.|
|X-Personium-RequestKey|RequestKey field value output in the event log|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters|No|PCS-${UNIXtime} by default|

### Request Body

JSON

|Item Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|Type|Type of event|String<br>Number of digits: 0&#45;51200 byte|Yes||
|Object|Object of the eventbr|String<br>Number of digits: 0&#45;51200 byte|Yes||
|Info|Information of the event|String<br>Number of digits: 0&#45;51200 byte|Yes||

### Request Sample

```JSON
{
  "Type":"authSchema",
  "Object":"/{CellName}/{BoxName}/service_name/token_keeper",
  "Info":"[XXXX2033] Success schema authorization. cellUrl=https://{UnitFQDN}/keeper-d4a57bb26eae481486b07d06487051d1/"
}
```


## Response

### Response Code

|Code|Message|Overview|
|:--|:--|:--|
|200|OK|Accepted success|

### Response Header

|Header Name|Overview|Notes|
|:--|:--|:--|
|Access-Control-Allow-Origin|Cross domain communication permission header|Return value fixed to "*"|
|X-Personium-Version|API version that the request is processed|Version of the API used to process the request|

### Response Body

None

### Error Messages

Refer to [Error Message List](004_Error_Messages.md)

## cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/__event" -X POST -i -H 'Authorization: Bearer {AccessToken}' -H \
'Accept: application/json' -d '{"level":"INFO", "action":"authSchema", \
"object":"/{CellName}/{BoxName}/service_name/token_keeper", "result":\
"[XXXX2033] Success schema authorization. cellUrl=https://{UnitFQDN}/keeper-d4a57bb26eae481486b07d06487051d1/"}'
```


