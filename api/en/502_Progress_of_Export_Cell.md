# Get cell export status

### Overview

Get the status of Cell export. The Cell export state includes the following information.

* Cell export status

    * Cell export acceptance accepted
    * Cell export in progress

* Export start date and time
* Progress rate
* Export file name

### Required Privileges

root

### Restrictions

* None

<br>

### Request

#### Request URL

```
/{CellName}/__export
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
|X-Personium-RequestKey<br>|RequestKey field value output in the event log<br>|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br>|No<br>|Supported in V 1.1.7 and later<br>|
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
|200<br>|OK<br>|On success<br>|Cell Export status refers to response body<br>|

#### Response Header

|Header Name<br>|Overview<br>|Notes<br>|
|:--|:--|:--|
|Access-Control-Allow-Origin<br>|Cross domain communication permission header<br>|Return value fixed to "*"<br>|
|X-Personium-Version<br>|API version that the request is processed<br>|Version of the API used to process the request<br>|
|Content-Type<br>|Format of data to be returned<br>|<br>|

#### Response Body

Response is JSON format and is defined as an object (subobject).<br>
The correspondence between key (name) and type, and value are as follows.

|Object<br>|Key<br>|Type<br>|Value<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|Root<br>|status<br>|string<br>|One of the following strings: <br>"ready"<br>"exportation in progress"<br>|"ready":Cell export acceptance accepted<br>"exportation in progress":Cell export in progress<br>|
|Root<br>|started_at<br>|string<br>|Start time (ISO 8610 UTC format)<br>|Do not output when status is below.<br>"ready"<br>|
|Root<br>|progress<br>|string<br>|Progress rate (for example, "30%")<br>|Do not output when status is below.<br>"ready"<br>|
|Root<br>|exportation_name<br>|string<br>|Export file name (excluding extension)<br>|Do not output when status is below.<br>"ready"<br>|

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

Cell export acceptance accepted (including when cell export is completed)

```json
{
  "status": "ready"
}
```

Cell export processing in progress

```json
{
  "status": "exportation in progress",
  "started_at": "2017-02-13T09:00:00.000Z",
  "progress": "81%",
  "exportation_name": "CellExport_2017_01"
}
```

<br>

### cURL Sample

```sh
curl "https://{UnitFQDN}/{CellName}/__export" -X GET -i -H 'Authorization: Bearer {AccessToken}'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED