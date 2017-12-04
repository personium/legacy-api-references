# Cell Recursive Delete

### Overview

* This API deletes all the relevant data under the specified Cell.
* This API deletes the following data:
    * Box
    * Account
    * Role
    * Relation
    * ExtCell
    * ExtRole
    * ReceivedMessage
    * SentMessage
    * Collection
    * AssociationEnd
    * EntityType
    * ComplexType
    * Property
    * ComplexTypeProperty
    * $links
    * User data

### Required Privileges

Only unit user

### Restrictions

None<br><br>

### Request

#### Request URL

```
/{CellName}
```

#### Request Method

DELETE

#### Request Query

None

#### Request Header

##### Common Request Header

|Header Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|Method override function<br>|User-defined<br>|No<br>|Specifying this value in a request with the POST method indicates that the specified value is used as the method<br>|
|X-Override<br>|Header override function<br>|${OverwrittenHeaderName}:${Value}<br>|No<br>|The normal HTTP header value is overwritten. Specify multiple X-Override headers for the overwriting of multiple headers<br>|
|X-Personium-RequestKey<br>|RequestKey field value output in the event log<br>|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br>|No<br>|PCS-${UNIXtime} by default<br>Supported in V 1.1.7 and later<br>|

##### Recursively Delete Request Header

|Header Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|Authorization<br>|Specifies authentication information in the OAuth 2.0 format<br>|Bearer {AccessToken}<br>|Yes<br>|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br>|
|X-Personium-Recursive<br>|Specifies a bulk delete<br>|String<br>|Yes<br>|True:This API implements the bulk delete<br>When there is no specified header or False: returns the error response code 412<br>|

#### Request Body

None

#### Request Sample

None

<br>

### Response

#### Response Code

204

#### Response Header

|Header Name<br>|Overview<br>|Notes<br>|
|:--|:--|:--|
|Access-Control-Allow-Origin<br>|Cross domain communication permission header<br>|Return value fixed to "*"<br>|
|X-Personium-Version<br>|API version that the request is processed<br>|Version of the API used to process the request<br>|

#### Response Body

None

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

None<br><br>

### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/" -X DELETE -i -H 'f-Match: *' -H 'X-Personium-Recursive: true' -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED
