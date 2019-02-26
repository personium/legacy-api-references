# OAuth2.0 Authorization Endpoint(\_\_authz)

## Overview

OAuth2 Authorization Endpoint  
This API is the OAuth2 authorization endpoint, for utilizing the Personium on JS application and native application.

### Precondition

In order to execute this API, it is necessary to prepare in advance Box which has the application cell URL in the schema.

### Restrictions

For scope=openid, only the following response_type can be specified.  

* response_type=id_token
* response_type=code


## Request

### Request URL

```
{CellName}/__authz
```

### Request Method

GET : Forms Authentication Request  
POST : Forms Authentication Request, Request Token Authentication, Request Code Authentication

### Request Query

|Item Name|Overview|Format|Required|Effective Value|
|:--|:--|:--|:--|:--|
|response_type|Response Type|String|Yes|token, code, id_token(scope=openid required)|
|client_id|Application Cell URL|String|Yes|Application Cell URL of authentication form request|
|redirect_uri|Client redirect endpoint URL|String|Yes|URL of the redirect script registered under the default BOX of the application cell<br>Query parameters formatted with application/x-www-form-urlencoded can be included<br>It is not possible to include fragments<br>Effective digit length:512byte|
|state|Random value used to maintain state between request and callback|String|No|Random value<br>Effective digit length:512byte|
|scope|Access scope requested|String|No|Personium can specify "openid"|
|username|User name|String|No|Registered user name|
|password|Password|String|No|Registered password|
|expires_in|Access token expiration in (sec)|Int<br>1～3600|No|Specify expiration in of issued access token<br>The default is 3600 (1 hour)<br>* When response_type is other than token, specification of this parameter is ignored|

### Request Header

None

### Request Body

Same as request query


## Response

### Forms Authentication Request

The authentication form can use the system default, or the specified html.  
When specifying html, [Unit setting](../../server-operator/unit_config_list.md) or [Target cell property setting](./291_Cell_Change_Property.md) is required. When both are set, the property setting of the target cell takes precedence.  

Unit setting  
```
io.personium.core.cell.authorizationhtmlurl.default={URL that html can obtain}
```

Target cell property setting  
```xml
<p:authorizationhtmlurl>{URL that html can obtain}</p:authorizationhtmlurl>
```
The schemes that can be specified as URL are "http", "https", "personium-localunit", "personium-localcell".

#### Response Code

200

#### Response Header

|Header Name|Overview|Notes|
|:--|:--|:--|
|Content-Type|text/html; charset=UTF-8||

#### Response Body

Return HTML authentication form.

#### Error Messages

Refer to [Error Message List](004_Error_Messages.md)

#### Response Sample

None

### Request Token Authentication

#### Response Code

303  
The browser is redirected to redirect\_uri. A fragment indicated by "URL parameter" is stored in redirect\_uri.

```
{redirect_uri}#access_token={access_token}&token_type=Bearer&expires_in={expires_in}&state={state}&last_authenticated={last_authenticated}&failed_count={failed_count}
```

#### URL parameter

|Item Name|Overview|Notes|
|:--|:--|:--|
|redirect_uri|Client redirect endpoint URL|The value of "redirect_uri" in the request|
|access_token|Access token acquired in the authentication/authorization request form|Return cell local token or transcell token|
|token_type|Bearer||
|expires_in|Access token expiration in (sec)|Expiration date set at the time of request<br>The default is 3600 (1 hour)|
|state|Value of state set at the time of request|Random value used to maintain state between request and callback|
|last_authenticated|Last authentication date and time|Last authentication date and time（UNIX time of long type）<br>initial authentication is null<br>\*Return only when password authentication|
|failed_count|Number of authentication failures|Number of consecutive failures in password authentication since last authentication<br>\*Return only when password authentication|

#### Error Messages

|Item Name|Overview|Notes|
|:--|:--|:--|
|redirect_uri|Redirect URL|The URL of the client's redirect split<br>specified by the "redirect_uri" of the request<br>However, in the case of the following error contents, this value is set to "cell URL + __html/error"<br>"Redirect_uri is not in URL format" "cell in client_id and redirect_uri is different"|
|error|Code indicating error content|See "error"|
|error_description|Additional information on errors|Set exception message etc|
|error_uri|Web page URI of additional information on error|Return empty string<br>* Set for future enhancemen|
|state|Value of state set at the time of request||
|code|Personium error code||
|invalid_request|A required parameter is not specified in the request<br>Invalid request parameter format<br>Account locked||
|unauthorized_client|The client is not authorized<br>Cancel button pressed by user||
|access_denied|The cells of client_id and redirect_uri are different<br>When transcell token authentication fails||
|unsupported_response_type|Invalid value of response_type||
|server_error|Server Error||
|Please, input user ID and password.|"Username" or "password" has not been entered||
|User ID or password is incorrect.|When password authentication fails||
|Since the Expiration Date of the authentication passed,<br>You must be authorized again.|Cookie authentication failed||

#### Parameter Check Error

The browser is redirected to redirect\_uri.  
"Redirect\_uri is not in URL format" "cell in client\_id and redirect\_uri is different" "Authorization processing failure"

```
{redirect_uri}?code={code}
```

Other than those above

```
{redirect_uri}#error={error}&error_description={error_description}&state={state}&code={code}
```


### Request Code Authentication

#### Response Code

303  
The browser is redirected to redirect\_uri. A fragment indicated by "URL parameter" is stored in redirect\_uri.

```
{redirect_uri}?code={code}&state={state}&last_authenticated={last_authenticated}&failed_count={failed_count}
```

#### URL parameter

|Item Name|Overview|Notes|
|:--|:--|:--|
|redirect_uri|Client redirect endpoint URL|The value of "redirect_uri" in the request|
|code|Code acquired in the authentication/authorization request form|Code that can be authorized with "grant_type: authorization_code"|
|state|Value of state set at the time of request|Random value used to maintain state between request and callback|
|last_authenticated|Last authentication date and time|Last authentication date and time（UNIX time of long type）<br>initial authentication is null<br>\*Return only when password authentication|
|failed_count|Number of authentication failures|Number of consecutive failures in password authentication since last authentication<br>\*Return only when password authentication|

#### Error Messages

|Item Name|Overview|Notes|
|:--|:--|:--|
|redirect_uri|Redirect URL|The URL of the client's redirect split<br>specified by the "redirect_uri" of the request<br>However, in the case of the following error contents, this value is set to "cell URL + __html/error"<br>"Redirect_uri is not in URL format" "cell in client_id and redirect_uri is different"|
|error|Code indicating error content|See "error"|
|error_description|Additional information on errors|Set exception message etc|
|error_uri|Web page URI of additional information on error|Return empty string<br>* Set for future enhancemen|
|state|Value of state set at the time of request||
|code|Personium error code||

#### Parameter Check Error

The browser is redirected to redirect\_uri.  
"Redirect\_uri is not in URL format" "cell in client\_id and redirect\_uri is different" "Authorization processing failure"

```
{redirect_uri}?code={code}
```

Other than those above

```
{redirect_uri}?error={error}&error_description={error_description}&state={state}&code={code}
```


## cURL Command

### GET

```sh
curl "https://cell1.unit1.example/__authz?response_type=token&\
redirect_uri=https://app-cell1.unit1.example/__/redirect.md&\
client_id=https://app-cell1.unit1.example" -X GET -i
```

### POST

```sh
curl "https://cell1.unit1.example/__authz" -X POST -i \
-d 'response_type=token&client_id=https://app-cell1.unit1.example/&\
redirect_uri=https://app-cell1.unit1.example/__/redirect.md&\
state=0000000111&username=account1&password=pass'
```
