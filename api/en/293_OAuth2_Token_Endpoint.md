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

<br>

### Request

#### Request URL

```
{CellName}/__token
```

#### Request Method

POST

#### Request Query

|Query Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|p_cookie_peer<br>|Cookie Authentication Value<br>|The cookie authentication value returned from the server during authentication<br>|No<br>|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br>|

#### Request Header

|Item Name<br>|Overview<br>|Format<br>|Required<br>|Effective Value<br>|
|:--|:--|:--|:--|:--|
|Authorization<br>|Specifies authentication information in the OAuth 2.0 format<br>|Basic {String}<br>|No<br>|If you specify Base64 Encode value for {{Schema Authenticator's source URL}: {Token paid out from the schema authentication source}}, it becomes schema authentication<br>At the above setting, if there is a setting of client_id and client_secret in the request body, the setting of the authorization header takes precedence<br>|

#### Request Body

##### Password authentication

|Item Name<br>|Overview<br>|Format<br>|Required<br>|Effective Value<br>|
|:--|:--|:--|:--|:--|
|grant_type<br>|Authentication type<br>|String<br>|Yes<br>|password<br>urn:x-personium:oidc:google<br>|
|username<br>|User name<br>|String<br>|Yes(When grant_type = password)<br>|Registered user name<br>|
|password<br>|Password<br>|String<br>|Yes(When grant_type = password)<br>|Registered password<br>|
|id_token<br>|Token ID<br>|JSON Web Token<br>|Yes(grant_type=urn:x-personium:oidc:For google)<br>|JWT Formed ID Token<br>|
|p_target<br>|Transcell token target<br>|String<br>|No<br>|Where to use the token to be paid (cell URL)<br>If specified, it becomes transcellation token authentication<br>|
|client_id<br>|App cell URL<br>|String<br>|No<br>|Schema Authenticator's source App cell URL<br>When specified with client_secret, it becomes schema authentication<br>At the same time, if the Authorization header also has schema authentication settings, the setting of the Authorization header takes precedence<br>|
|client_secret<br>|Token paid out from the application cell<br>|String<br>|No<br>|Set the token that was paid out from the schema authentication source to the value<br>When specified with client_id, it becomes schema authentication<br>At the same time, if the Authorization header also has schema authentication settings, the setting of the Authorization header takes precedence<br>|
|p_owner<br>|ULUUT promotion execution Query<br>|String<br>|No<br>|Valid only for true<br>|
|p_cookie<br>|Authentication cookie issuance option<br>If specified, issue an authentication cookie<br>When p_target is specified, specification of this parameter is ignored<br>|String<br>|No<br>|Valid only for true<br>|

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

|Item Name<br>|Overview<br>|Format<br>|Required<br>|Effective Value<br>|
|:--|:--|:--|:--|:--|
|grant_type<br>|Authentication type<br>|String<br>|Yes<br>|urn: ietf: params: oauth: grant-type: saml2-bearer<br>|
|assertion<br>|Access token<br>|String<br>|Yes<br>|A valid token<br>|
|p_target<br>|Transcell token target<br>|String<br>|No<br>|Where to use the token to be paid (cell URL)<br>When specified, it becomes a transcell token<br>|
|client_id<br>|App cell URL<br>|String<br>|No<br>|Schema Authenticator's source App cell URL<br>When specified with client_secret, it becomes schema authentication<br>At the same time, if the Authorization header also has schema authentication settings, the setting of the Authorization header takes precedence<br>|
|client_secret<br>|Token paid out from the application cell<br>|String<br>|No<br>|Set the token that was paid out from the schema authentication source to the value<br>When specified with client_id, it becomes schema authentication<br>At the same time, if the Authorization header also has schema authentication settings, the setting of the Authorization header takes precedence<br>|
|p_cookie<br>|Authentication cookie issuance option<br>If specified, issue an authentication cookie<br>When p_target is specified, specification of this parameter is ignored<br>|String<br>|No<br>|Valid only for true<br>|

```
grant_type=urn:ietf:params:oauth:grant-type:saml2-bearer&assertion={token}
```

##### Refresh token authentication

|Item Name<br>|Overview<br>|Format<br>|Required<br>|Effective Value<br>|
|:--|:--|:--|:--|:--|
|grant_type<br>|Authentication type<br>|String<br>|Yes<br>|Refresh token<br>|
|refresh_token<br>|Refresh token name<br>|String<br>|Yes<br>|Effective refresh token<br>|
|p_target<br>|Transcell token target<br>|String<br>|No<br>|Where to use the token to be paid (cell URL) If specified, it becomes transcell token authentication<br>|
|client_id<br>|App cell URL<br>|String<br>|No<br>|Schema Authenticator's source App cell URL<br>When specified with client_secret, it becomes schema authentication<br>At the same time, if the Authorization header also has schema authentication settings, the setting of the Authorization header takes precedence<br>|
|client_secret<br>|Token paid out from the application cell<br>|String<br>|No<br>|Set the token that was paid out from the schema authentication source to the value<br>When specified with client_id, it becomes schema authentication<br>At the same time, if the Authorization header also has schema authentication settings, the setting of the Authorization header takes precedence<br>|
|p_owner<br>|ULUUT promotion execution Query<br>|String<br>|No<br>|Valid only for true<br>|
|p_cookie<br>|Authentication cookie issuance option<br>If specified, issue an authentication cookie<br>When p_target is specified, specification of this parameter is ignored<br>|String<br>|No<br>|Valid only for true<br>|

```
grant_type=refresh_token&refresh_token={token}
```

<br>

### Response

#### Response Code

200

#### Response Header

|Item Name<br>|Overview<br>|Notes<br>|
|:--|:--|:--|
|Content-Type<br>|application/json<br>|<br>|
|Set-Cookie<br>|Cookie authentication information (p_cookie)<br>|Only when setting cookie issue option (p_cookie) at request<br>|

#### Response Body

|Item Name<br>|Overview<br>|Notes<br>|
|:--|:--|:--|
|access_token<br>|Access token<br>|<br>|
|refresh_token_expires_in<br>|Refresh token expiration date<br>|24 hours (86400 seconds)<br>*If p_owner is set at the time of request, it will not be returned<br>|
|refresh_token<br>|Refresh token<br>|*If p_owner is set at the time of request, it will not be returned<br>|
|token_type<br>|Bearer<br>|<br>|
|expires_in<br>|Access token expiration date<br>|1 hour (3600 seconds)<br>|
|p_cookie_peer<br>|Cookie Authentication Value<br>|Authentication value specified at the time of cookie authentication<br>*Return only when the cookie issue option (p_cookie) is set at the time of request<br>|

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

Refer to [Error Message List](004_Error_Messages.html)

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

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED
