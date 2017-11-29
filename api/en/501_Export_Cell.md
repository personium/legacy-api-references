# Export Cell

### Overview

This API export all data in Cell as Cell snapshot file.<br>The snapshot file is created in a special area (Cell snapshot area) in PersoniumUnit.<br>When exported successfully, created the file which suffix is ".zip" and when fail created the file which suffix is ".error".(In the ".error" file, detail error message is described)<br>The Cell snapshot area is excluded from the Cell export.<br>Since this API employs the asynchronous communication method, it immediately returns after accepting the API.<br>To confirm the Cell export executing status, use [Get Cell Export status](502_Progress_of_Export_Cell.html),[Get Cell Snapshot property](505_Get_Property_Export_Cell.html).<br>An example of calling from acceptance at the client to completion of processing is shown below.

```
Call export call example (with 10 seconds polling on the client)
 1. Cell export reception
    -- POST /{CellName}/__export
 2. Cell export status check
    -- GET /{CellName}/__export -> return "in progress"
    -- wait 10 seconds
 3. Cell export finished
    -- GET /{CellName}/__export -> return "acceptable"
 4. confirm Cell export finished successfully
    -- PROPFIND /{cell}/__snapshot -> when the file suffix is ".zip" exported successfully, ".error" error occured.
 * The process in the above 2 loops and polls until completion of processing.
 * When you wish to acquire details at abnormal termination, refer to the contents of & quot; .error & quot; file.
```

### Required Privileges

root

### Restrictions

* Always handles Content-Type in the request header as application/json
* Accept only JSON format for request body

<br>

### Error file

#### Overview

If the Cell export fails, a file with the extension ". Error" is generated in the Cell snapshot area. (The file name is the name specified by the body)<br>The error content is described in JSON format in the quot; .error & quot; file.<br>The format of the & quot;. error & quot; file is shown below.

|Object<br>|Key<br>|Type<br>|Value<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|Root<br>|code<br>|string<br>|<br>|<br>|
|Root<br>|message<br>|object<br>|<br>|<br>|
|message<br>|lang<br>|string<br>|<br>|<br>|
|message<br>|value<br>|string<br>|<br>|<br>|

#### Sample

```
{
  "code":"PR503-SV-0001",
  "message":
  {
    "lang":"en",
    "value":"Too many concurrent requests."
  }
}
```

<br>

### Request

#### Request URL

```
/{CellName}/__export
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
|X-HTTP-Method-Override<br>|Method override function<br>|User-defined<br>|No<br>|When this value is specified at the time of request in the POST method, the specified value is used as a method.<br>|
|X-Override<br>|Header override function<br>|${OverwrittenHeaderName}:${Value}<br>|No<br>|Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.<br>|
|X-Personium-RequestKey<br>|RequestKey field value output in the event log<br>|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br>|No<br>|PCS-${UNIXtime} by default<br>Supported in V 1.1.7 and later<br>|
|Authorization<br>|Specifies authentication information in the OAuth 2.0 format<br>|Bearer {AccessToken}<br>|No<br>|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br>|
|Content-Type<br>|Specifies the request body format<br>|application / json<br>|No<br>|[application/json] by default<br>|

#### Request Body

##### Format

JSON

|Item Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|Name<br>|Snapshot file name (excluding extension)<br>|Number of digits: 1-192<br>Character type: Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>|No<br>|Default [{CellName}_yyyyMMdd_HHmmss]<br>|

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
|Location<br>|URL for retrieving Cell export status<br>|<br>|
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
curl "https://{UnitFQDN}/{CellName}/__export" -X POST -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"Name":"CellExport_2017_01"}'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED