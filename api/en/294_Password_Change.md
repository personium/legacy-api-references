# Change Password(\_\_mypassword)

### Overview

An API that performs operations on the account's password

##### Change password of own account

Change password of your account.<br>
\* The Account update API can also change the password of the Account, but this requires the auth authority of the cell level ACL and uses it for management purposes.<br>
\* Because of changes to the account, CellLocalToken by account authentication is mandatory, not UnitUserToken.

### Required Privileges

Auth authority of cell-level ACL

### Restrictions

None

<br>

### Request

#### Request URL

```
{CellName}/__mypassword
```

#### Request Method

PUT

#### Request Query

None

#### Request Header

|Header Name<br>|Overview<br>|Effective Value<br>|Required<br>|Notes<br>|
|:--|:--|:--|:--|:--|
|Authorization<br>|Specifies authentication information in the OAuth 2.0 format<br>|Bearer {CellLocalToken}<br>|Yes<br>|The authentication token is a cell local token acquired by the authentication token acquisition API<br>|
|X-Personium-Credential<br>|Password after change<br>|String<br>|Yes<br>|Number of character:6 - 92<br>Character type: Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>|

#### Request Body

None

#### Request Sample

None

<br>

### Response

#### Response Code

204

#### Response Header

None

#### Response Body

None

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

None

<br>

### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/__mypassword" -X PUT -i -H 'X-Personium-Credential: change_password' -H 'Authorization: Bearer {CellLocalToken}' -H 'Accept: application/json'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED