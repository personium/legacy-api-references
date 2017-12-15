# Account Update

### Overview

Update existing Account information

### Required Privileges

auth

### Restrictions

* Always handles Content-Type in the request header as application/json
* Only accepts the request body in the JSON format
* Only application/json is supported for Content-Type in the request header and the JSON format for the response body
* Response body data is not ensured if atom or xml is specified in the $format query option, although it does not result in an error


### Request

#### Request URL

```
/{CellName}/__ctl/Account(Name='{AccountName}')
```

or

```
/{CellName}/__ctl/Account('{AccountName}')
```

#### Request Method

PUT

#### Request Query

|Query Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|p_cookie_peer|Cookie Authentication Value|The cookie authentication value returned from the server during authentication|No|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used|

#### Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|Method override function|User-defined|No|If you specify this value when requesting with the POST method, the specified value will be used as a method.|
|X-Override|Header override function|${OverwrittenHeaderName}:${Value}|No|Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.|
|X-Personium-RequestKey|RequestKey field value output in the event log|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters|No|PCS-${UNIXtime} by default|
|Authorization|Specifies authentication information in the OAuth 2.0 format|Bearer {AccessToken}|No|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API|
|Content-Type|Specifies the request body format|application/json|No|[application/json] by default|
|Accept|Specifies the response body format|application/json|No|[application/json] by default|
|If-Match|Specifies the target ETag value|ETag value|Yes||
|X-Personium-Credential|Password|String|No|Number of character:6 - 92<br>Character type: Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")|

#### Request Body

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|Name|Account Name|Number of digits: 1 - 128<br>Character type: Half size alphanumeric characters and following half-width symbol (-_!$*=^`{&#124;}~.@) <br>However, the first character can not be specified as the first character|Yes||
|Type|Account Type|basic(ID/PW authentication)<br>oidc:google(Google OpenID Connect authentication)<br>or divide upper case by space character<br>Description: If omitted, it is updated with basic|No|default: basic|
|LastAuthenticated|last authentication date|It is specified as a character string in the format of Date ([time of long type])<br>The valid value of [time of long type] is -6847804800000(1753-01-01T00:00:00.000Z)-253402300799999(9999-12-31T23:59:59.999Z)<br>Description: Updated with null if omitted|No|default: null|

#### Request Sample

Account name update

```JSON
{
  "Name": "{AccountName}"
}
```

Account name and Account type update

```JSON
{
  "Name": "{AccountName}","Type":"oidc:google"
}
```


### Response

#### Response Code

204

#### Response Header

None

#### Response Body

None

#### Error Messages

Refer to [Error Message List](004_Error_Messages.md)

#### Response Sample

None

### cURL Command

Account name update

```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/Account('{AccountName}')" -X PUT -i -H 'If-Match: *' -H 'X-Personium-Credential:password' -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"Name":"{AccountName}"}'
```

Account name and Account type update

```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/Account('{AccountName}')" -X PUT -i -H 'If-Match: *' -H 'X-Personium-Credential:password' -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"Name":"{AccountName}","Type":"oidc:google"}'
```


###### Copyright 2017 FUJITSU LIMITED
