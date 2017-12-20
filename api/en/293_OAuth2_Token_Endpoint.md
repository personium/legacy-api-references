# OAuth2.0 Token Endpoint(\_\_token)

### Overview

There are three types of authentication as follows

Password authentication

* Acquires a cell local token that is valid only in the cell of the token issuing source by account authentication.
* Account is locked for 1 second if authentication fails
* When an authentication request for the corresponding account is executed during locking
    * An authentication error is returned irrespective of the validity of the user name and password
    * Account is extended lock for 1 second from re-authentication request time

Token authentication

* Use a transcell token to obtain a cell local token that is valid only in the issuing cell.

Refresh token authentication

* Refresh the cell local token and acquire the cell local token again.

### Required Privileges

None

### Restrictions

None


### Request

#### Request URL

```
{CellName}/__token
```

#### Request Method

POST

#### Request Query

|Query Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|p_cookie_peer|Cookie Authentication Value|The cookie authentication value returned from the server during authentication|No|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used|

#### Request Header

|Item Name|Overview|Format|Required|Effective Value|
|:--|:--|:--|:--|:--|
|Authorization|Specifies authentication information in the OAuth 2.0 format|Basic {String}|No|If you specify Base64 Encode value for {{Schema Authenticator's source URL}: {Token paid out from the schema authentication source}}, it becomes schema authentication<br>At the above setting, if there is a setting of client_id and client_secret in the request body, the setting of the authorization header takes precedence|

#### Request Body

##### Password authentication

|Item Name|Overview|Format|Required|Effective Value|
|:--|:--|:--|:--|:--|
|grant_type|Authentication type|String|Yes|password<br>urn&#58;x-personium:oidc:google|
|username|User name|String|Yes(When grant_type = password)|Registered user name|
|password|Password|String|Yes(When grant_type = password)|Registered password|
|id_token|Token ID|JSON Web Token|Yes(grant_type=urn&#58;x-personium:oidc:For google)|JWT Formed ID Token|
|p_target|Transcell token target|String|No|Where to use the token to be paid (cell URL)<br>If specified, it becomes transcellation token authentication|
|client_id|App cell URL|String|No|Schema Authenticator's source App cell URL<br>When specified with client_secret, it becomes schema authentication<br>At the same time, if the Authorization header also has schema authentication settings, the setting of the Authorization header takes precedence|
|client_secret|Token paid out from the application cell|String|No|Set the token that was paid out from the schema authentication source to the value<br>When specified with client_id, it becomes schema authentication<br>At the same time, if the Authorization header also has schema authentication settings, the setting of the Authorization header takes precedence|
|p_owner|ULUUT promotion execution Query|String|No|Valid only for true|
|p_cookie|Authentication cookie issuance option<br>If specified, issue an authentication cookie<br>When p_target is specified, specification of this parameter is ignored|String|No|Valid only for true|

#### Request Sample

Password authentication

```
grant_type=password&username=username&password=pass
```

Password authentication(Issued cookie)

```
grant_type=password&username=username&password=pass&p_cookie=true
```

Issuing a transcell token by password authentication

```
grant_type=password&username=username&password=pass&p_target=https://{UnitFQDN}/{CellName}/
```

Password authentication with schema

```
grant_type=password&username=username&password=pass&client_id=https://{UnitFQDN}/app{CellName}/&client_secret=
WjzDmvJSLvM9qVuJL1xxP6hSxt64HijoIea0P5R2CVloXJ2HEvEILl7UOtEtjSDdjlvyx9wrosPBhDRU97Qnn6EQIQ3MwaqtIx7HjuX36_ZBC6qxcgscCDmdtGb4nHgo
```

OIDC(Open ID Connect(Google)) authentication

```
grant_type=urn:x-personium:oidc:google&id_token=IDTOKEN
```

##### Token authentication

|Item Name|Overview|Format|Required|Effective Value|
|:--|:--|:--|:--|:--|
|grant_type|Authentication type|String|Yes|urn: ietf: params: oauth: grant-type: saml2-bearer|
|assertion|Access token|String|Yes|A valid token|
|p_target|Transcell token target|String|No|Where to use the token to be paid (cell URL)<br>When specified, it becomes a transcell token|
|client_id|App cell URL|String|No|Schema Authenticator's source App cell URL<br>When specified with client_secret, it becomes schema authentication<br>At the same time, if the Authorization header also has schema authentication settings, the setting of the Authorization header takes precedence|
|client_secret|Token paid out from the application cell|String|No|Set the token that was paid out from the schema authentication source to the value<br>When specified with client_id, it becomes schema authentication<br>At the same time, if the Authorization header also has schema authentication settings, the setting of the Authorization header takes precedence|
|p_cookie|Authentication cookie issuance option<br>If specified, issue an authentication cookie<br>When p_target is specified, specification of this parameter is ignored|String|No|Valid only for true|

```
grant_type=urn:ietf:params:oauth:grant-type:saml2-bearer&assertion={token}
```

##### Refresh token authentication

|Item Name|Overview|Format|Required|Effective Value|
|:--|:--|:--|:--|:--|
|grant_type|Authentication type|String|Yes|Refresh token|
|refresh_token|Refresh token name|String|Yes|Effective refresh token|
|p_target|Transcell token target|String|No|Where to use the token to be paid (cell URL) If specified, it becomes transcell token authentication|
|client_id|App cell URL|String|No|Schema Authenticator's source App cell URL<br>When specified with client_secret, it becomes schema authentication<br>At the same time, if the Authorization header also has schema authentication settings, the setting of the Authorization header takes precedence|
|client_secret|Token paid out from the application cell|String|No|Set the token that was paid out from the schema authentication source to the value<br>When specified with client_id, it becomes schema authentication<br>At the same time, if the Authorization header also has schema authentication settings, the setting of the Authorization header takes precedence|
|p_owner|ULUUT promotion execution Query|String|No|Valid only for true|
|p_cookie|Authentication cookie issuance option<br>If specified, issue an authentication cookie<br>When p_target is specified, specification of this parameter is ignored|String|No|Valid only for true|

```
grant_type=refresh_token&refresh_token={token}
```


### Response

#### Response Code

200

#### Response Header

|Item Name|Overview|Notes|
|:--|:--|:--|
|Content-Type|application/json||
|Set-Cookie|Cookie authentication information (p_cookie)|Only when setting cookie issue option (p_cookie) at request|

#### Response Body

|Item Name|Overview|Notes|
|:--|:--|:--|
|access_token|Access token||
|refresh_token_expires_in|Refresh token expiration date|24 hours (86400 seconds)<br>*If p_owner is set at the time of request, it will not be returned|
|refresh_token|Refresh token|*If p_owner is set at the time of request, it will not be returned|
|token_type|Bearer||
|expires_in|Access token expiration date|1 hour (3600 seconds)|
|p_cookie_peer|Cookie Authentication Value|Authentication value specified at the time of cookie authentication<br>*Return only when the cookie issue option (p_cookie) is set at the time of request|

#### Response Sample

```JSON
{
  "access_token": "AA~osIZ4CZ8cZmxf5NidEueHej_6Lj-ww0c_kJZd4HbHBqFyZ0OZBrS29miYr9Jh19b0o39cTJdH2Va3xSMMbu6Eg",
  "refresh_token_expires_in": 86400,
  "refresh_token": "RA~uELMJkVpzTtsl1ueh2KlrT9UiOx85-dmg7nGX01YaogoQ86qgfv2VMUxQXSP95uNY9MuWxZe0AQFtEnFYyWMoQ",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

#### Error Messages

Refer to [Error Message List](004_Error_Messages.md)

### cURL Command

##### Password authentication

```sh
curl "https://{UnitFQDN}/{CellName}/__token" -X POST -i -d 'grant_type=password&username={username}&password={password}'
```

##### Token authentication

```sh
curl "https://{UnitFQDN}/{CellName}/__token" -X POST -i -d 'grant_type=urn:ietf:params:oauth:grant-type:saml2-bearer&assertion={token}'
```

##### Refresh token authentication

```sh
curl "https://{UnitFQDN}/{CellName}/__token" -X POST -i -d 'grant_type=refresh_token&refresh_token={refresh_token}'
```

##### Password authentication + Schema authentication

```sh
curl "https://{UnitFQDN}/{CellName}/__token" -X POST -i -d 'grant_type=password&username={user_name}&password={pass}&client_id=https://{UnitFQDN}/app{CellName}/&client_secret={token_from_app_cell}'
```

##### Token authentication + Transcel token authentication

```sh
curl "https://{UnitFQDN}/{CellName}/__token" -X POST -i -d 'grant_type=urn:ietf:params:oauth:grant-type:saml2-bearer&assertion={SAML_token}&p_target=https://{UnitFQDN}/{CellName}/'
```


###### Copyright 2017 FUJITSU LIMITED
