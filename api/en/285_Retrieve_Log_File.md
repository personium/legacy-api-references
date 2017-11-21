# Log File Acquire

### Overview

acquire event log<br>
br />
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
/{CellName}/__log/current/{LogName}
```

##### Log file that is rotated

```
/{CellName}/__log/archive/{LogName}
```

\*{LogName} specifies the file name returned by the log file information acquisition API./p>

#### Request Method

GET

#### Request Query

| Query Name<br>    | Overview<br>                    | Effective Value<br>                                                                | Required<br> | Notes<br>                                                                                                                |
|:-- |:-- |:-- |:-- |:-- |
| p_cookie_peer<br> | Cookie Authentication Value<br> | The cookie authentication value returned from the server during authentication<br> | No<br>       | Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br> |

#### Request Header

##### Common Request Header

| Header Name<br>            | Overview<br>                                                     | Effective Value<br>                                                                                        | Required<br> | Notes<br>                                                                                                                    |
|:-- |:-- |:-- |:-- |:-- |
| X-HTTP-Method-Override<br> | Method override function<br>                                     | User-defined<br>                                                                                           | No<br>       | Specifying this value in a request with the POST method indicates that the specified value is used as the method<br>         |
| X-Override<br>             | Header override function<br>                                     | ${OverwrittenHeaderName}:${Value}<br>                                                                      | No<br>       | The normal HTTP header value is overwritten. Specify multiple X-Override headers for the overwriting of multiple headers<br> |
| X-Personium-RequestKey<br> | RequestKey field value output in the event log<br>               | Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br> | No<br>       | PCS-${UNIXtime} by default<br>Supported in V 1.1.7 and later<br>                                                             |
| Authorization<br>          | Specifies authentication information in the OAuth 2.0 format<br> | Bearer {AccessToken}<br>                                                                                   | No<br>       | * Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br>                           |

#### Request Body

None

#### Request Sample

None

<br>

### Response

#### Response Code

200

#### Response Header

| Header Name<br>  | Overview<br>                       | Notes<br>                           |
|:-- |:-- |:-- |
| Content-Type<br> | MimeType depending on Resource<br> | "text/csv" or "application/zip"<br> |

#### Response Body

If there is no log at current log acquisition time, return an empty response body.<br>
There is a possibility that the rotate size is about 5MB larger than it configured<br>
Output form is following

```
{dateTime},[{level}],{RequestKey},{name},{schema},{subject},{action},{object},{result}
```

| Item Name<br>  | Overview<br>                                                                                                          | Notes<br>                    |
|:-- |:-- |:-- |
| dateTime<br>   | Log write date and time (ISO8601 UTC format)<br>                                                                      | YYYY-MM-DDTHH:MM:SS.sssZ<br> |
| level<br>      | log level INFO,WARN,ERROR<br>                                                                                         | String<br>                   |
| RequestKey<br> |  Value specified on X-Dc-RequestKey header<br>when the X-Dc-RequestKey header is not specified, PCS-$ {UNIX time}<br> | String<br>                   |
| name<br>       | External events: client<br>internal event: server<br>                                                                 | String<br>                   |
| schema<br>     | Schema of box of accepted URL<br>                                                                                     | URL format<br>               |
| subject<br>    | Principal events<br>                                                                                                  | URL format<br>               |
| action<br>     | External events: Action defined in the event reception<br>Internal events: HTTP method name<br>                       | String<br>                   |
| object<br>     | External events: Object defined in the event reception<br>Internal events:Requested resource path<br>                 | String<br>                   |
| result<br>     | External events: Result defined in the event reception<br>Internal events:Requested resource path<br>                 | String<br>                   |

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

External Events

```
2013-02-04T00:50:12.761Z,[INFO ],Req_animal-access_1001,client,https://{UnitFQDN}/{CellName}/,https://{UnitFQDN}/servicemanager/#admin,authSchema,/{CellName}/{BoxName}/service_name/token_keeper,[XXXX2033] Success schema authorization. cellUrl=https://{UnitFQDN}/keeper-d4a57bb26eae481486b07d06487051d1/
```

Internal Event

```
2013-04-18T14:52:39.778Z,[ERROR],PCS-1364350331902,server,https://{UnitFQDN}/appCell/,https://{UnitFQDN}/appCell/#staff,POST,/homeClinic/__token,200
```

<br>

### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/__log/current/default.log" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'          
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED