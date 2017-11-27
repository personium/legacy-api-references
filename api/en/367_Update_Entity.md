# Entity Update

### Overview

Update Entity of user data.

### Required Privileges

write

### Restrictions

* Always handles Content-Type in the request header as application/json
* Only accepts the request body in the JSON format
* Only application/json is supported for Content-Type in the request header and the JSON format for the response body
* Response body data is not ensured if atom or xml is specified in the $format query option, although it does not result in an error
* User data restrictions

    * Property scope of Edm.DateTime type is not properly checked
    * Array of Edm.DateTime type is not supported
    * If SYSUTCDATETIME () is specified as the property of Edm.DateTime type, the set system time may be different
    * When setting in request body and setting with DefaultValue (__published, __ updated is the latter timing)
    * For EntityType, you can create up to 400 DynamicProperty / DeclaredProperty / ComplexTypeProperty

<br>

### Request

#### Request URL

```
/{CellName}/{BoxName}/{ODataCollecitonName}/{EntityTypeName}({EntityID})}
```

|Path<br>|Overview<br>|
|:--|:--|
|{CellName}<br>|Cell Name<br>|
|{BoxName}<br>|Box Name<br>|
|{ODataCollecitonName}<br>|Collection Name<br>|
|{EntityTypeName}<br>|EntityType name<br>|
|{EntityID}<br>|ID of Entity to update<br>|

#### Request Method

PUT

#### Request Query

##### Common Request Query

|Query Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|p_cookie_peer<br>|Cookie Authentication Value<br>|The cookie authentication value returned from the server during authentication<br>|No<br>|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br>|

##### OData Common Request Query

None

#### Request Header

##### Common Request Header

|Header Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|Method override function<br>|User-defined<br>|No<br>|Specifying this value in a request with the POST method indicates that the specified value is used as the method<br>|
|X-Override<br>|Header override function<br>|${OverwrittenHeaderName}:${Value}<br>|No<br>|The normal HTTP header value is overwritten. Specify multiple X-Override headers for the overwriting of multiple headers<br>|
|X-Personium-RequestKey<br>|RequestKey field value output in the event log<br>|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br>|No<br>|Supported in V 1.1.7 and later<br>|

##### OData Common Request Header

|Header Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|Authorization<br>|Specifies authentication information in the OAuth 2.0 format<br>|Bearer {AccessToken}<br>|No<br>|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br>|

##### OData Update Request Header

|Header Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|Contents-Type<br>|Specifies authentication information in the OAuth 2.0 format<br>|application/json<br>|No<br>|When omitted, treat it as [application/json] <br>|
|Accept<br>|Specifies the response body format<br>|application/json<br>|No<br>|When omitted, treat it as [application/json] <br>|
|If-Match<br>|Specifies the target ETag value<br>|ETag value<br>|No<br>|[*] by default<br>|

#### Request Body

##### Property

Set up schema-defined properties and dynamic (schema undefined) properties, up to 400 properties in total<br>
Contains the number of properties defined by ComplexType in the above

##### Schema defined properties

|Item Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|Property associated with EntityType<br>|User defined item <br>|Based on DefaultValue of default value Property<br>|Based on Property Nullable<br>|<br>|

##### Valid value of value of schema-defined property

|Data type<br>|Effective Value<br>|
|:--|:--|
|String<br>|Number of digits: 0-51200 byte<br>Character type: When a control code is used as a value of a character string, return it in an escaped state at the time of acquisition<br>When "\" is used, it must be specified with "\\"<br>When an integer value, a decimal value, a boolean value, or a date type value is set in a property of a character string type, it is converted into a character string type and registered<br>|
|Integer value<br>|-2147483648 - 2147483647<br>|
|Decimal point<br>|Number of digits in integer part: 1-5 digits<br>Number of digits in decimal part: 1-5 digits<br>|
|Boolean value<br>|true / false / null(treat null as false)<br>|
|Date<br>|It is specified as a character string in the format of Date ([time of long type])<br>The valid value of [time of long type] is -6847804800000(1753-01-01T00:00:00.000Z)-253402300799999(9999-12-31T23:59:59.999Z)<br>In addition, you can specify the following as reserved words<br>SYSUTCDATETIME (): server time<br>|

##### Dynamic (schema undefined) property

Set up schema-defined properties and dynamic (schema-undefined) properties, up to 400 properties in total<br>
Contains the number of properties defined by ComplexType in the above

##### Valid value of dynamic property's key

|Data type<br>|Effective Value<br>|
|:--|:--|
|String<br>|Number of digits: 1-128 :<br>Character type: Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>_However, - (hyphen) and _ (underscore)_  can not be specified as the first character <br>_published, _updated is a reserved word, so it is not possible to specify the request body<br>|

##### Valid value of value of dynamic property

Same as valid value of value of schema-defined property<br>
Array, associative array can not be specified

#### Request Sample

```JSON
{"animalId": "100-2","name": "episode2","startedAt": "2016-02-21","episodeType": "care2","endedAt": "","outcome": "Treated"}
```

<br>

### Response

#### Response Code

204

#### Response Header

|Header Name<br>|Overview<br>|Notes<br>|
|:--|:--|:--|
|ETag<br>|Resource version information<br>|<br>|
|DataServiceVersion<br>|OData version<br>|<br>|

#### Response Body

None

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

None

<br>

### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/{EntityTypeName}('{EntityID}')" -X PUT -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"animalId": "100-2","name": "episode2","startedAt":"2016-02-21","episodeType": "care2","endedAt": "","outcome": "Treated"}'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED