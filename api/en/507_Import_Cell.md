# Cell Import

### Overview

Import cell data from Cell snapshot file.<br>The Cell snapshot file uses a special area (Cell snapshot area) in PersoniumUnit.<br>If processing fails, change the status of Cell to " import failed ".<br>When processing is successful, if the status of Cell is " import failed ", it is changed to " normal ".<br>When the status of Cell is " import failed ", the target Cell does not accept anything other than API. See [ Acquire Properties ](290_Cell_Get_Property.html) for details.<br>Since this API employs the asynchronous communication method, it immediately returns after accepting the API.<br>To check the status of Cell Import, use [ Cell import status acquisition ](508_Progress_of_Import_Cell.html).<br>An example of calling from acceptance at the client to completion of processing is shown below.

```
Call Import example of Cell Import (When polling on client is set to 10 seconds)
 1. Cell import reception
    -- POST /{CellName}/__import
 2. Cell import status check
    -- GET / {CellName} / __ import -> "Processing"being returned.
    -- Polling for 10 seconds
 3. Cell Import Complete
    -- GET /{CellName}/__import -> "Acceptable"being returned.
    * The process in the above 2 loops and polls until completion of processing.
```

### Required Privileges

root

### Restrictions

* Always handles Content-Type in the request header as application/json
* Only accepts the request body in the json format
* Even if processing fails in the middle, rollback is not performed<br>* It is recommended that you export the Cell once before importing the Cell.

<br>

### Request

#### Request URL

```
/{CellName}/__import
```

#### Request Method

POST

#### Request Query

|Query Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|p_cookie_peer<br>|Cookie Authentication Value<br>|The cookie authentication value returned from the server during authentication<br>|No<br>|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br>|

#### Request Header

|Header Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|Method override function<br>|User-defined<br>|No<br>|Specifying this value in a request with the POST method indicates that the specified value is used as the method<br>|
|X-Override<br>|Header override function<br>|${OverwrittenHeaderName}:${Value}<br>|No<br>|The normal HTTP header value is overwritten. Specify multiple X-Override headers for the overwriting of multiple headers<br>|
|X-Personium-RequestKey<br>|RequestKey field value output in the event log<br>|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br>|No<br>|PCS-${UNIXtime} by default<br>Supported in V 1.1.7 and later<br>|
|Authorization<br>|Specifies authentication information in the OAuth 2.0 format<br>|Bearer {AccessToken}<br>|No<br>|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br>|
|Content-Type<br>|Specifies the request body format<br>|application / json<br>|No<br>|[application/json] by default<br>|

#### Request Body

##### Format

JSON

|Item Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|Name<br>|Snapshot file name (excluding extension)<br>|Number of digits: 1-192<br>Character type: Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>|Yes<br>|<br>|

#### Request Sample

```json
{"Name":"CellExport_2017_01"}
```

<br>

### Response

#### Response Code

|Code|Message|Overview|
|:--|:--|:--|
|202|Accepted|Processing reception|

#### Response Header

|Header Name<br>|Overview<br>|Notes<br>|
|:--|:--|:--|
|Content-Type<br>|Format of data to be returned<br>|Return only if creation fails<br>|
|Location<br>|URL for obtaining cell import status<br>|<br>|
|Access-Control-Allow-Origin<br>|Cross domain communication permission header<br>|Return value fixed to "*"<br>|
|X-Personium-Version<br>|API version that the request is processed<br>|Version of the API used to process the request<br>|

#### Response Body

Return error message only if creation fails

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

None

<br>

### cURL Sample

```sh
curl "https://{UnitFQDN}/{CellName}/__import" -X POST -i -H 'Authorization: Bearer {AccessToken}' -d '{"Name":"CellExport_2017_01"}'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED