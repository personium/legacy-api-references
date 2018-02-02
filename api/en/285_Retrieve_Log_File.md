# Log File Acquire

## Overview

acquire event log  
If you exceed the maximum number of generations to hold rotate when the log file, the log file of the oldest is deleted.

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

|Action|Archived log file|Description|Notes|
|:--|:--|:--|:--|
|First Rotation|archive/<br>default.log.1402910774659|<br>Newly rotated file|<br>2014-06-16 18:26:14 +0900|
|2nd Rotation|archive/<br>default.log.1402910774659<br>default.log.1403910784659|<br>the preceding roteted file<br>Newly rotated file|<br>2014-06-16 18:26:14 +0900<br>2014-06-28 08:13:04 +0900|
|3rd Rotation|archive/<br>default.log.1402910774659<br>default.log.1403910784659<br>default.log.1403910784659|<br>the file before preceding file<br>the preceding roteted file<br>Newly rotated file|<br>2014-06-16 18:26:14 +0900<br>2014-06-28 08:13:04 +0900<br>2014-07-09 21:59:44 +0900|


## Request

### Request URL

#### Recent log file

```
/{CellName}/__log/current/{LogName}
```

#### Log file that is rotated

```
/{CellName}/__log/archive/{LogName}
```

\*{LogName} specifies the file name returned by the log file information acquisition API.

### Request Method

GET

### Request Query

|Query Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|p_cookie_peer|Cookie Authentication Value|The cookie authentication value returned from the server during authentication|No|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used|

### Request Header

#### Common Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|Method override function|User-defined|No|Specifying this value in a request with the POST method indicates that the specified value is used as the method|
|X-Override|Header override function|${OverwrittenHeaderName}:${Value}|No|The normal HTTP header value is overwritten. Specify multiple X-Override headers for the overwriting of multiple headers|
|X-Personium-RequestKey|RequestKey field value output in the event log|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters|No|PCS-${UNIXtime} by default|
|Authorization|Specifies authentication information in the OAuth 2.0 format|Bearer {AccessToken}|No|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API|

### Request Body

None


## Response

### Response Code

200

### Response Header

|Header Name|Overview|Notes|
|:--|:--|:--|
|Content-Type|MimeType depending on Resource|"text/csv" or "application/zip"|

### Response Body

If there is no log at current log acquisition time, return an empty response body.  
There is a possibility that the rotate size is about 5MB larger than it configured  
Output form is following

```
{dateTime},[{level}],{RequestKey},{external},{schema},{subject},{type},{object},{info}
```

|Item Name|Overview|Notes|
|:--|:--|:--|
|dateTime|Log write date and time (ISO8601 UTC format)|YYYY-MM-DDTHH:MM:SS.sssZ|
|level|log level INFO,WARN,ERROR|String|
|RequestKey|Value specified on X-Dc-RequestKey header<br>when the X-Dc-RequestKey header is not specified, PCS-$ {UNIX time}|String|
|external|External events:true<br>Internal events:false|String|
|schema|Schema of box of accepted URL|URL format|
|subject|Principal events|URL format|
|type|External events: Type defined in the event reception<br>Internal events: Type defined for each event|String|
|object|External events: Object defined in the event reception<br>Internal events:Requested resource path|String|
|info|External events: Info defined in the event reception<br>Internal events:HTTP status code|String|

### Error Messages

Refer to [Error Message List](004_Error_Messages.md)

### Response Sample

External Events

```
2013-02-04T00:50:12.761Z,[INFO ],Req_animal-access_1001,client,https://{UnitFQDN}/{CellName}/,
https://{UnitFQDN}/servicemanager/#admin,authSchema,/{CellName}/{BoxName}/service_name/token_keeper,
[XXXX2033] Success schema authorization. cellUrl=https://{UnitFQDN}/keeper-d4a57bb26eae481486b07d06487051d1/
```

Internal Event

```
2013-04-18T14:52:39.778Z,[INFO ],"PCS-1364350331902","false","https://{UnitFQDN}/appCell/",
"https://{UnitFQDN}/appCell/#staff","cellctl.Role.list","https://{UnitFQDN}//homeClinic/__ctl/Role","200"
```


## cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/__log/current/default.log" -X GET -i -H \
'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```

