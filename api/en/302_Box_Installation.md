# Install Box

### Overview

Install the Box in the specified path using the bar file.For the bar file format, see "[ bar file ](301_Bar_File.html)".<br>
Since this API employs the asynchronous communication method, this API immediately restores after accepting Box installation.<br>
Therefore, to check the Box installation status, use the following API.

* [Box metadata acquisition](303_Progress_of_Bar_File_Installation.html) If Box installation terminates abnormally, you can refer to the cause of the error by checking the Box installation status with this API. Below is a method of calling from acceptance at the client to completion of processing.

```
Example call of Box installation (when polling on client is set to 30 seconds)
 1. Box installation reception
    -- MKCOL /{cell}/{box}
 2. Box Installation status check
    -- GET /{cell}/{box}  -> Returned "during processing".
    -- 30 seconds polling
 3. GET /{cell}/{box}  -> Returned by "processing completion".
 *It loops the above process 2 and polls until the process is completed.
```

### Required Privileges

box-install

### Restrictions

#### Box installation target Box restriction

* If you already have a Box with the same name, you can not install Box.
* If a Box with the same scheme URL already exists, you can not install Box.
* You can not install Box in the main box.
* barFile:The schema field of "/bar/00_meta/00_manifest.json" can not be null.

#### bar file limit

* The file size of the Box installable bar file is limited to the following.<br><br>
    When exceeding the upper limit value, you can not install Box.

    * File size of bar file:100MB
    * Before compression file size of entry in bar file:10MB

* Each entry is stored in the bar file in the order defined in the bar file.

#### Box installation log detailed limit

* The details of log of Box installation is output to EventBus of Cell to which Box installation target Box belongs. following *1

    * Therefore, when referring to log details, refer to using the log file acquisition API.
    * In order to use the log file acquisition API, the authority of "log-read" is necessary.
    * Because event logs other than Box installation also mix, it is necessary to filter by the "RequestKey" field value.
    * The "action" field value obtains the "Requestkey" field value of "MKCOL" and filters it.

#### Other restrictions

* During the Box installation process, you can not operate data on subordinate resources including Box to be installed.
* Do not roll back if an error occurs during Box installation.

##### \*1 Box installation log detailed format

| State<br>                                | "action"<br>        | "object"<br>               | "result"<br>                                   |
|:-- |:-- |:-- |:-- |
| Box installation reception<br>           | "MKCOL"<br>         | BoxURL<br>                 | Response Code                                  |
| Box installation process in progress<br> | Processing code<br> | Entry path in bar file<br> | A message corresponding to the processing code |
| Box Installation Complete<br>            | Processing code<br> | BoxURL<br>                 | A message corresponding to the processing code |

##### Processing code

| Processing code<br> | Message<br>                      | Description<br>                                                  |
|:-- |:-- |:-- |
| PL-BI-0000<br>      | bar install completed<br>        | Box installation complete (normal termination)<br>               |
| PL-BI-0001<br>      | bar install failed ({cause})<br> | Box installation complete (abnormal termination)<br>             |
| PL-BI-1000<br>      | bar install started<br>          | Box installation start<br>                                       |
| PL-BI-1001<br>      | install started<br>              | Bar file entry start installation<br>                            |
| PL-BI-1003<br>      | install completed<br>            | Bar file entry installation completed (normal termination)<br>   |
| PL-BI-1004<br>      | install failed ({cause})<br>     | Bar file entry installation completed (abnormal termination)<br> |
| PL-BI-1005<br>      | Unknown Error ({cause})<br>      | Internal error<br>                                               |

<br>

### Request

#### Request URL

```
/{CellName}/{BoxName}
```

#### Request Method

MKCOL

#### Request Query

| Query Name<br>    | Overview<br>                    | Effective Value<br>                                                                | Required<br> | Notes<br>                                                                                                                |
|:-- |:-- |:-- |:-- |:-- |
| p_cookie_peer<br> | Cookie Authentication Value<br> | The cookie authentication value returned from the server during authentication<br> | No<br>       | Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br> |

#### Request Header

| Header Name<br>            | Overview<br>                                                     | Effective Value<br>                                                                                        | Required<br> | Notes<br>                                                                                                         |
|:-- |:-- |:-- |:-- |:-- |
| X-HTTP-Method-Override<br> | Method override function<br>                                     | User-defined<br>                                                                                           | No<br>       | If you specify this value when requesting with the POST method, the specified value will be used as a method.<br> |
| X-Override<br>             | Header override function<br>                                     | ${OverwrittenHeaderName}:${Value} override} $: $ {value}<br>                                               | No<br>       | Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.<br>       |
| X-Personium-RequestKey<br> | RequestKey field value output in the event log<br>               | Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br> | No<br>       | Supported in V 1.1.7 and later<br>                                                                                |
| Authorization<br>          | Specifies authentication information in the OAuth 2.0 format<br> | Bearer {AccessToken}<br>                                                                                   | No<br>       | * Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br>                |
| Content-Type<br>           | Specifies the request body format<br>                            | application/zip<br>                                                                                        | Yes<br>      | <br>                                                                                                              |
| Content-Length<br>         | Specify the size of the request body<br>                         | Half-width number<br>                                                                                      | No<br>       | <br>                                                                                                              |

#### Request Body

| Overview<br>                                                      | Effective Value<br>                             | Required<br> | Notes<br>                     |
|:-- |:-- |:-- |:-- |
| Specify the bar file to install as binary in the request body<br> | Format specified in the Content-Type header<br> | Yes<br>      | bar file: Zip file format<br> |

For the file structure of the bar file, see the [ bar file ](301_Bar_File.html).

#### Request Sample

None

<br>

### Response

#### Response Code

| Code<br> | Message<br>  | Overview<br>                   | Notes<br> |
|:-- |:-- |:-- |:-- |
| 202<br>  | Accepted<br> | Process acceptance success<br> | <br>      |

#### Response Header

| Header Name<br>                 | Overview<br>                                     | Notes<br>                                          |
|:-- |:-- |:-- |
| Location<br>                    | URL for Box metadata acquisition API<br>         | <br>                                               |
| Access-Control-Allow-Origin<br> | Cross domain communication permission header<br> | Return value fixed to "*"<br>                      |
| X-Personium-Version<br>         | API version that the request is processed<br>    | Version of the API used to process the request<br> |

Location sample

```
Location:https://{UnitFQDN}/{CellName}/{BoxName}
```

For details of URL for [Box metadata acquisition API](303_Progress_of_Bar_File_Installation.html), see Box metadata acquisition.

#### Response Body

None

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

```
Location: https://{UnitFQDN}/{CellName}/{BoxName}
```

For details of URL for [Box metadata acquisition API](303_Progress_of_Bar_File_Installation.html), see Box metadata acquisition.<br><br>

### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}" -X MKCOL -i -H 'Content-type: application/zip' -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' --data-binary @{FileName}
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED