# Get cell import status

### Overview

Get Cell Import status. The Cell import state includes the following information.

* Cell import status
    * Cell import acceptance available
    * Cell import processing in progress
    * Cell Import Abnormal termination
* Import start date and time
* Progress rate
* Import file name
* Error message

### Required Privileges

root

### Restrictions

* None

<br>

### Request

#### Request URL

```
/{CellName}/__import
```

#### Request Method

GET

#### Request Query

|Query Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|p_cookie_peer<br>|Cookie Authentication Value<br>|The cookie authentication value returned from the server during authentication<br>|No<br>|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br>|

#### Request Header

|Header Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|X-Override<br>|Header override function<br>|${OverwrittenHeaderName}:${Value} override} $: $ {value}<br>|No<br>|Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.<br>|
|X-Personium-RequestKey<br>|RequestKey field value output in the event log<br>|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br>|No<br>||
|Authorization<br>|Specifies authentication information in the OAuth 2.0 format<br>|Bearer {AccessToken}<br>|No<br>|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br>|

#### Request Body

None

#### Request Sample

None

<br>

### Response

#### Response Code

|Code<br>|Message<br>|Overview<br>|Notes<br>|
|:--|:--|:--|:--|
|200<br>|OK<br>|On success<br>|Cell import status refers to response body<br>|

#### Response Header

|Header Name<br>|Overview<br>|Notes<br>|
|:--|:--|:--|
|Access-Control-Allow-Origin<br>|Cross domain communication permission header<br>|Return value fixed to "*"<br>|
|X-Personium-Version<br>|API version that the request is processed<br>|Version of the API used to process the request<br>|
|Content-Type<br>|Format of data to be returned<br>|<br>|

#### Response Body

Response is JSON format and is defined as an object (subobject).  
The correspondence between key (name) and type, and value are as follows.

|Object<br>|Key<br>|Type<br>|Value<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|Root<br>|status<br>|string<br>|One of the following strings: <br>"ready"<br>"importation in progress"<br>"import failed"<br>|"ready":Cell import acceptance available<br>"importation in progress":Cell import processing in progress<br>"import failed":Cell Import Abnormal termination|
|Root<br>|started_at<br>|string<br>|Start time (ISO 8610 UTC format)<br>|Do not output when status is below.<br>"ready"<br>|
|Root<br>|progress<br>|string<br>|Progress rate (for example, "30%")<br>|Do not output when status is below.<br>"ready"<br>|
|Root<br>|importation_name<br>|string<br>|Import file name (excluding extension)<br>|Do not output when status is below.<br>"ready"<br>|
|Root<br>|message<br>|object<br>|Object (message format)<br>|Output only when status is below.<br>"import failed"<br>See [ Error Messages ](004_Error_Messages.html) for details<br>|

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

Cell Import accepted (Including when importing Cell)

```json
{
  "status": "ready"
}
```

When Cell import processing is in progress

```json
{
  "status": "importation in progress",
  "started_at": "2017-02-13T09:00:00.000Z",
  "progress": "81%",
  "importation_name": "CellExport_2017_01"
}
```

Cell Import Abnormal termination

```json
{
  "status": "import failed",
  "started_at": "2017-02-13T09:00:00.000Z",
  "progress": "81%",
  "importation_name": "CellExport_2017_01",
  "message": {
      "code" : "PR409-OD-0003",
      "message" : {
          "lang" : "en",
          "value" : "The entity already exists."
      }
  }
}
```

<br>

### cURL Sample

```sh
curl "https://{UnitFQDN}/{CellName}/__import" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED
