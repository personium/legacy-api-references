# OAuth2.0 Token Endpoint

## Overview

An API endpoint that issues access tokens to access various APIs in the cell. It complies with the OAuth 2.0 specification, it authenticates users and applications in various ways and issues access tokens and refresh tokens.

### Variation of user authentication

#### Account holder authentication with password

* Resource Owner Password Credential (ROPC) flow in OAuth 2.0.
* Based on the registered account name and password, you can authenticate the owner of the account and receive access token issuance.
* As a defense against brute force attack, even if the password is legitimate for that account for one second after failure of authentication, an authentication error will result.
* This flow should be used only by the cell application, which is the premise that cell owners place high levels of trust.
* As described in the OAuth 2.0 specification, general Personium applications should not use this flow.

#### Authentication of other cell user by transcell access token

* The authentication method corresponding to the SAML 2 assertion flow of the OAuth 2.0 extension.
* You can issue an access token by authenticating another cell user by sending a transcell access token addressed to this cell.
* Re-authentication of the application is required here even if application authentication has been undergone when issuing a transcell access token to be transmitted.

#### Continuation of user authentication by refresh token transmission

* By sending a refresh token, it is possible to receive a new access token that inherited the user authentication state received when the refresh token was issued.
* Re-authentication of the application is required here even if application authentication has been undergone when issuing a refresh token to be transmitted.
* In Personium, applications can be switched by sending client_id and client_secret of different applications at token refresh.
* This is for transferring processing from a cell application to a general application, which is a premise that the cell owner places a high level of trust, and should not share or exchange access tokens or refresh tokens among general applications.

### Information transmission for application authentication

* Personium uses OAuth's client authentication framework to authenticate the application.
* Personium uses the schema URI of the application as client_id. In many cases the schema URI is the URL of the applet.
* Personium uses an application authentication token as client_secret.
* Information transmission for application authentication is not mandatory.

### Variation of issue token specification

#### Issuing an access token

If there is no parameter specified as p_target, it will issue a valid access token (cell local access token) in this cell.

#### Issuing a Transcell Access Token

By specifying the URL of another cell by using the parameter called p_target, we issue a Transcell access token for accessing other cells
### Other variations

#### Issuing cookies

* If p_cookie is specified as a true value, cookies that are valid only in this cell are issued.
* This cookie alone does not substitute for the access token, but by using it together with the character string "cookie_peer" issued at the same time, it has the same effect as the access token.
* When you get a resource on this cell, access this URL by adding this value with a query parameter called cookie_peer to the URL so that it handles the same as access token transmission only when correspondence with cookies sent at the same time is obtained.
* With this, it is possible to display private media only to authorized users using tags such as HTML img video audio.
* Cookies that are issued are valid only in this cell and can not be used in combination with p_target, which is a token acquisition parameter for accessing other cells.


## Request

### Request URL

```
{CellName}/__token
```

### Request Method

POST

### Request Query

|Query Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|p_cookie_peer|Cookie Authentication Value|The cookie authentication value returned from the server during authentication|No|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used|

### Request Header

|Item Name|Overview|Format|Required|Effective Value|
|:--|:--|:--|:--|:--|
|Authorization|Specifies authentication information in the OAuth 2.0 format|Basic {String}|No|If you specify Base64 Encode value for {{Schema Authenticator's source URL}: {Token paid out from the schema authentication source}}, it becomes schema authentication<br>At the above setting, if there is a setting of client_id and client_secret in the request body, the setting of the authorization header takes precedence|

### Request Body

|Item Name|Overview|Format|Required|Effective Value|
|:--|:--|:--|:--|:--|
|grant_type|Authentication type|String|Yes|password<br>urn&#58;x-personium:oidc:google<br>urn&#58;ietf:params:oauth:grant-type:saml2-bearer<br>refresh_token|
|username|User name|String|Yes(When grant_type = password)|Registered user name|
|password|Password|String|Yes(When grant_type = password)|Registered password|
|assertion|Transcell token target|String|Yes(When grant_type=urn&#58;ietf:params:oauth:grant-type:saml2-bearer)|A valid transacell access token|
|refresh_token|Refresh token name|String|Yes(When grant_type=refresh_token)|Effective refresh token|
|id_token|Token ID|JSON Web Token|Yes(grant_type=urn&#58;x-personium:oidc:For google)|JWT Formed ID Token|
|p_target|Issue target|String|No|Where to use the token to be paid (cell URL)<br>If specified, it becomes transcellation token authentication|
|client_id|Application schema URI|String|No|In many cases App store URL<br>When specified with client_secret Is issued application-certified token<br>At the same time, if the same information is transmitted in the Authorization header, the setting of the Authorization header takes precedence|
|client_secret|Application authentication token|String|No|An application authentication token issued from an application cell or the like<br>When specified with client_id Issue an application-certified token<br>At the same time, if the same information is transmitted in the Authorization header, the setting of the Authorization header takes precedence|
|p_owner|ULUUT promotion execution Query|String|No|Valid only for true|
|p_cookie|Authentication cookie issuance option<br>If specified, issue an authentication cookie<br>When p_target is specified, specification of this parameter is ignored|String|No|Valid only for true|

### Request Sample

Password authentication

```
grant_type=password&username=username&password=pass
```

Issuing a transcell access token with account owner authentication by password

```
grant_type=password&username=username&password=pass&p_target=https://{UnitFQDN}/{CellName}/
```

Issuing application-authenticated access token by password owner authentication and application authentication token sending

```
grant_type=password&username=username&password=pass&client_id=https://{UnitFQDN}/app{CellName}
/&client_secret=
WjzDmvJSLvM9qVuJL1xxP6hSxt64HijoIea0P5R2CVloXJ2HEvEILl7UOtEtjSDdjlvyx9wrosPBhDRU97Qnn6EQIQ3MwaqtI
x7HjuX36_ZBC6qxcgscCDmdtGb4nHgo

```

Account holder authentication by Google ID issued Open ID Connect Id Token

```
grant_type=urn:x-personium:oidc:google&id_token=IDTOKEN
```

Refreshing tokens with refresh tokens

```
grant_type=refresh_token&refresh_token={token}
```

Cookies are also issued by password owner authentication

```
grant_type=password&username=username&password=pass&p_cookie=true
```

## Response

### Response Code

200

### Response Header

|Item Name|Overview|Notes|
|:--|:--|:--|
|Content-Type|application/json||
|Set-Cookie|Cookie authentication information (p_cookie)|Only when setting cookie issue option (p_cookie) at request|

### Response Body

|Item Name|Overview|Notes|
|:--|:--|:--|
|access_token|Access token||
|refresh_token_expires_in|Refresh token expiration date|24 hours (86400 seconds)<br>*If p_owner is set at the time of request, it will not be returned|
|refresh_token|Refresh token|*If p_owner is set at the time of request, it will not be returned|
|token_type|Bearer||
|expires_in|Access token expiration date|1 hour (3600 seconds)|
|p_cookie_peer|Cookie Authentication Value|Authentication value specified at the time of cookie authentication<br>*Return only when the cookie issue option (p_cookie) is set at the time of request|

### Response Sample

```JSON
{
  "access_token": "AA~osIZ4CZ8cZmxf5NidEueHej_6Lj-ww0c_kJZd4HbHBqFyZ0OZBrS29miYr9Jh19b0o39cTJdH2Va3xSMMbu6Eg",
  "refresh_token_expires_in": 86400,
  "refresh_token": "RA~uELMJkVpzTtsl1ueh2KlrT9UiOx85-dmg7nGX01YaogoQ86qgfv2VMUxQXSP95uNY9MuWxZe0AQFtEnFYyWMoQ",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

### Error Messages

Refer to [Error Message List](004_Error_Messages.md)

## cURL Command

### Account holder authentication

```sh
curl "https://{UnitFQDN}/{CellName}/__token" -X POST -i -d \
'grant_type=password&username={username}&password={password}'
```

### Other cell user authentication

```sh
curl "https://{UnitFQDN}/{CellName}/__token" -X POST -i -d \
'grant_type=urn:ietf:params:oauth:grant-type:saml2-bearer&assertion={token}'
```

### Token Refresh

```sh
curl "https://{UnitFQDN}/{CellName}/__token" -X POST -i -d \
'grant_type=refresh_token&refresh_token={refresh_token}'
```

### Account holder + Application authentication

```sh
curl "https://{UnitFQDN}/{CellName}/__token" -X POST -i -d \
'grant_type=password&username={user_name}&password={pass}&client_id=https://{UnitFQDN}
/app{CellName}/&client_secret={token_from_app_cell}'
```

### Issuing transcell access token for other cell by other cell user authentication

```sh
curl "https://{UnitFQDN}/{CellName}/__token" -X POST -i -d 'grant_type=urn:ietf:params:oauth:grant-type:\
saml2-bearer&assertion={SAML_token}&p_target=https://{UnitFQDN}/{CellName}/'
```

