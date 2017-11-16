# Accept Event

### Overview

This API accepts the requested event

### Required Privileges

event

### Supported Versions

1.1.7

### Restrictions

None

#### Common Event API

* Log output of the internal event is not supported
* Set the log file name as "default.log "
*  Rotate will be according to following default settings

    * Rotate number: 12 generations
    * Set rotate size: 50MB
    * Compression method after rotation: zip<br>Configure the log output level to "info" (output for all INFO, WARN, ERROR).

* Log output configuration reference is not supported

<br>

### Request

#### Request URL

```
/{CellName}/__event
```

#### Request Method

POST

#### Request Query

| Query Name<br>    | Overview<br>                    | Effective Value<br>                                                                | Required<br> | Notes<br>                                                                                                                |
|:-- |:-- |:-- |:-- |:-- |
| p_cookie_peer<br> | Cookie Authentication Value<br> | The cookie authentication value returned from the server during authentication<br> | No<br>       | Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br> |

#### Request Header

| Header Name<br>            | Overview<br>                                       | Effective Value<br>                                                                                        | Required<br> | Notes<br>                                                                                                         |
|:-- |:-- |:-- |:-- |:-- |
| X-HTTP-Method-Override<br> | Method override function<br>                       | User-defined<br>                                                                                           | No<br>       | If you specify this value when requesting with the POST method, the specified value will be used as a method.<br> |
| X-Override<br>             | Header override function<br>                       | ${OverwrittenHeaderName}:${Value}<br>                                                                      | No<br>       | Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.<br>       |
| X-Personium-RequestKey<br> | RequestKey field value output in the event log<br> | Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br> | No<br>       | PCS-${UNIXtime} by default<br>Supported in V 1.1.7 and later<br>                                                  |

#### Request Body

JSON

| Item Name<br> | Overview<br>            | Effective Value<br>                          | Required<br> | Notes<br>                                                                                   |
|:-- |:-- |:-- |:-- |:-- |
| level<br>     | Log output level<br>    | "INFO"/"WARN"/"ERROR"<br>                    | Yes<br>      | In case lower case "info"/"warn"/"error" is specified, it is treated as upper case letters. |
| action<br>    | Action of the event<br> | String<br>Number of digits: 0-51200 byte<br> | Yes<br>      | <br>                                                                                        |
| object<br>    | Object of the eventbr   | String<br>Number of digits: 0-51200 byte<br> | Yes<br>      | <br>                                                                                        |
| result<br>    | Result of the eventbr   | String<br>Number of digits: 0-51200 byte<br> | Yes<br>      | <br>                                                                                        |

#### Request Sample

```JSON
{
  "level":"INFO",
  "action":"authSchema",
  "object":"/{CellName}/{BoxName}/service_name/token_keeper",
  "result":"[XXXX2033] Success schema authorization. cellUrl=https://{UnitFQDN}/keeper-d4a57bb26eae481486b07d06487051d1/"
}
```

<br>

### Response

#### Response Code

| Code<br> | Message<br> | Overview<br>         |
|:-- |:-- |:-- |
| 200<br>  | OK<br>      | Accepted success<br> |

#### Response Header

| Header Name<br>                 | Overview<br>                                     | Notes<br>                                          |
|:-- |:-- |:-- |
| Access-Control-Allow-Origin<br> | Cross domain communication permission header<br> | Return value fixed to "*"<br>                      |
| X-Personium-Version<br>         | API version that the request is processed<br>    | Version of the API used to process the request<br> |

#### Response Body

None

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/__event" -X POST -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"level":"INFO", "action":"authSchema", "object":"/{CellName}/{BoxName}/service_name/token_keeper", "result":"[XXXX2033] Success schema authorization. cellUrl=https://{UnitFQDN}/keeper-d4a57bb26eae481486b07d06487051d1/"}'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED