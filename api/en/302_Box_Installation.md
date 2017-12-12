# Install Box

### Overview

Install the Box in the specified path using the bar file.For the bar file format, see "[ bar file ](301_Bar_File.html)".  
Since this API employs the asynchronous communication method, this API immediately restores after accepting Box installation.  
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

* The file size of the Box installable bar file is limited to the following.  
    When exceeding the upper limit value, you can not install Box.
    * File size of bar file: 100MB
    * Before compression file size of entry in bar file: 10MB
* Each entry is stored in the bar file in the order defined in the bar file.

#### Box installation log detailed limit

* The details of log of Box installation is output to EventBus of Cell to which Box installation target Box belongs. following \*1
    * Therefore, when referring to log details, refer to using the log file acquisition API.
    * In order to use the log file acquisition API, the authority of "log-read" is necessary.
    * Because event logs other than Box installation also mix, it is necessary to filter by the "RequestKey" field value.
    * The "action" field value obtains the "Requestkey" field value of "MKCOL" and filters it.

#### Other restrictions

* During the Box installation process, you can not operate data on subordinate resources including Box to be installed.
* Do not roll back if an error occurs during Box installation.

##### \*1 Box installation log detailed format

|State|"action"|"object"|"result"|
|:--|:--|:--|:--|
|Box installation reception|"MKCOL"|BoxURL|Response Code|
|Box installation process in progress|Processing code|Entry path in bar file|A message corresponding to the processing code|
|Box Installation Complete|Processing code|BoxURL|A message corresponding to the processing code|

##### Processing code

|Processing code|Message|Description|
|:--|:--|:--|
|PL-BI-0000|bar install completed|Box installation complete (normal termination)|
|PL-BI-0001|bar install failed ({cause})|Box installation complete (abnormal termination)|
|PL-BI-1000|bar install started|Box installation start|
|PL-BI-1001|install started|Bar file entry start installation|
|PL-BI-1003|install completed|Bar file entry installation completed (normal termination)|
|PL-BI-1004|install failed ({cause})|Bar file entry installation completed (abnormal termination)|
|PL-BI-1005|Unknown Error ({cause})|Internal error|


### Request

#### Request URL

```
/{CellName}/{BoxName}
```

#### Request Method

MKCOL

#### Request Query

|Query Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|p_cookie_peer|Cookie Authentication Value|The cookie authentication value returned from the server during authentication|No|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used|

#### Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|Method override function|User-defined|No|If you specify this value when requesting with the POST method, the specified value will be used as a method.|
|X-Override|Header override function|${OverwrittenHeaderName}:${Value} override} $: $ {value}|No|Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.|
|X-Personium-RequestKey|RequestKey field value output in the event log|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters|No||
|Authorization|Specifies authentication information in the OAuth 2.0 format|Bearer {AccessToken}|No|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API|
|Content-Type|Specifies the request body format|application/zip|Yes||
|Content-Length|Specify the size of the request body|Half-width number|No||

#### Request Body

|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|
|Specify the bar file to install as binary in the request body|Format specified in the Content-Type header|Yes|bar file: Zip file format|

For the file structure of the bar file, see the [ bar file ](301_Bar_File.html).

#### Request Sample

None


### Response

#### Response Code

|Code|Message|Overview|Notes|
|:--|:--|:--|:--|
|202|Accepted|Process acceptance success||

#### Response Header

|Header Name|Overview|Notes|
|:--|:--|:--|
|Location|URL for Box metadata acquisition API||
|Access-Control-Allow-Origin|Cross domain communication permission header|Return value fixed to "*"|
|X-Personium-Version|API version that the request is processed|Version of the API used to process the request|

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

For details of URL for [Box metadata acquisition API](303_Progress_of_Bar_File_Installation.html), see Box metadata acquisition.

### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}" -X MKCOL -i -H 'Content-type: application/zip' -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' --data-binary @{FileName}
```


###### Copyright 2017 FUJITSU LIMITED
