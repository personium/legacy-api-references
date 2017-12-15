# OAuth2.0 トークンエンドポイント(\__token)
### 概要
認証の方式は以下の3種類


パスワード認証
* アカウント認証によってトークン発行元のセルでのみ有効なセルローカルトークンを取得する。
* 認証に失敗した場合は、1秒間アカウントがロックされる
  * ロック中に該当アカウントに対する認証リクエストを実行した場合
      * ユーザ名、パスワードの正当性にかかわらず認証エラーが返却される
      * 再認証リクエスト時間からさらに1秒間アカウントがロックが延長される

トークン認証
* トランスセルトークンを使って発行元のセルでのみ有効なセルローカルトークンを取得する。

リフレッシュトークン認証
* セルローカルトークンをリフレッシュして再度セルローカルトークンを取得する。


### 必要な権限
なし

### 制限事項
なし


### リクエスト
#### リクエストURL
```
{CellName}/__token
```
#### メソッド
POST

#### リクエストクエリ

|クエリ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|p_cookie_peer|クッキー認証値|認証時にサーバから返却されたクッキー認証値|×|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する|
#### リクエストヘッダ

|項目名|概要|書式|必須|有効値|
|:--|:--|:--|:--|:--|
|Authorization|OAuth2.0形式で、認証情報を指定する|Basic {String}|×|{{スキーマ認証元のアプリセルURL}:{スキーマ認証元から払い出されたトークン}}をBase64Encodeした値を指定した場合、スキーマ認証になる<br>上記設定時、リクエストボディにclient_idとclient_secretの設定がある場合、Authorizationヘッダの設定が優先される|
#### リクエストボディ
##### パスワード認証

|項目名|概要|書式|必須|有効値|
|:--|:--|:--|:--|:--|
|grant_type|認証タイプ|String|○|password<br>urn:x-personium:oidc:google|
|username|ユーザ名|String|○(grant_type=passwordの場合)|登録済のユーザ名|
|password|パスワード|String|○(grant_type=passwordの場合)|登録済のパスワード|
|id_token|トークンID|JSON Web Token|○(grant_type=urn:x-personium:oidc:googleの場合)|JWT Formed ID Token|
|p_target|トランスセルトークンターゲット|String|×|払い出されるトークンを使う先（セルURL）<br>指定した場合トランスセルトークン認証になる|
|client_id|アプリセルURL|String|×|スキーマ認証元のアプリセルURL<br>client_secretとともに指定した場合スキーマ認証になる<br>同時にAuthorizationヘッダにもスキーマ認証設定がされている場合、Authorizationヘッダの設定が優先される|
|client_secret|アプリセルから払い出されたトークン|String|×|スキーマ認証元から払い出されたトークンを値に設定する<br>client_idとともに指定した場合スキーマ認証になる<br>同時にAuthorizationヘッダにもスキーマ認証設定がされている場合、Authorizationヘッダの設定が優先される|
|p_owner|ULUUT昇格実行クエリ|String|×|trueのみ有効|
|p_cookie|認証クッキー発行オプション<br>指定された場合は認証クッキーを発行する<br>p_targetが指定された場合は、本パラメタの指定は無視する|String|×|trueのみ有効|
#### リクエストサンプル
パスワード認証
```
grant_type=password&username=username&password=pass
```

パスワード認証 （クッキー発行）
```
grant_type=password&username=username&password=pass&p_cookie=true
```

パスワード認証によるトランスセルトークン発行
```
grant_type=password&username=username&password=pass&p_target=https://{UnitFQDN}/{CellName}/
```

スキーマ付きパスワード認証
```
grant_type=password&username=username&password=pass&client_id=https://{UnitFQDN}/app{CellName}/&client_secret=
WjzDmvJSLvM9qVuJL1xxP6hSxt64HijoIea0P5R2CVloXJ2HEvEILl7UOtEtjSDdjlvyx9wrosPBhDRU97Qnn6EQIQ3MwaqtIx7HjuX36_ZBC6qxcgscCDmdtGb4nHgo
```
OIDC(Open ID Connect(Google))認証
```
grant_type=urn:x-personium:oidc:google&id_token=IDTOKEN
```

##### トークン認証

|項目名|概要|書式|必須|有効値|
|:--|:--|:--|:--|:--|
|grant_type|認証タイプ|String|○|urn: ietf: params: oauth: grant-type: saml2-bearer|
|assertion|アクセストークン|String|○|有効なトークン|
|p_target|トランスセルトークンターゲット|String|×|払い出されるトークンを使う先（セルURL）<br>指定した場合トランスセルトークンになる|
|client_id|アプリセルURL|String|×|スキーマ認証元のアプリセルURL<br>client_secretとともに指定した場合スキーマ認証になる<br>同時にAuthorizationヘッダにもスキーマ認証設定がされている場合、Authorizationヘッダの設定が優先される|
|client_secret|アプリセルから払い出されたトークン|String|×|スキーマ認証元から払い出されたトークンを値に設定する<br>client_idとともに指定した場合スキーマ認証になる<br>同時にAuthorizationヘッダにもスキーマ認証設定がされている場合、Authorizationヘッダの設定が優先される|
|p_cookie|認証クッキー発行オプション<br>指定された場合は認証クッキーを発行する<br>p_targetが指定された場合は、本パラメタの指定は無視する|String|×|trueのみ有効|

```
grant_type=urn:ietf:params:oauth:grant-type:saml2-bearer&assertion={token}
```

##### リフレッシュトークン認証

|項目名|概要|書式|必須|有効値|
|:--|:--|:--|:--|:--|
|grant_type|認証タイプ|String|○|リフレッシュトークン|
|refresh_token|リフレッシュトークン名|String|○|有効リフレッシュトークン|
|p_target|トランスセルトークンターゲット|String|×|払い出されるトークンを使う先（セルURL） 指定した場合はトランスセルトークン認証になる|
|client_id|アプリセルURL|String|×|スキーマ認証元のアプリセルURL<br>client_secretとともに指定した場合スキーマ認証になる<br>同時にAuthorizationヘッダにもスキーマ認証設定がされている場合、Authorizationヘッダの設定が優先される|
|client_secret|アプリセルから払い出されたトークン|String|×|スキーマ認証元から払い出されたトークンを値に設定する<br>client_idとともに指定した場合スキーマ認証になる<br>同時にAuthorizationヘッダにもスキーマ認証設定がされている場合、Authorizationヘッダの設定が優先される|
|p_owner|ULUUT昇格実行クエリ|String|×|trueのみ有効|
|p_cookie|認証クッキー発行オプション<br>指定された場合は認証クッキーを発行する<br>p_targetが指定された場合は、本パラメタの指定は無視する|String|×|trueのみ有効|

```
grant_type=refresh_token&refresh_token={token}
```


### レスポンス
#### ステータスコード
200

#### レスポンスヘッダ

|項目名|概要|備考|
|:--|:--|:--|
|Content-Type|application/json||
|Set-Cookie|クッキー認証情報（p_cookie）|クッキー発行オプション（p_cookie）をリクエスト時に設定した場合のみ|
#### レスポンスボディ

|項目名|概要|備考|
|:--|:--|:--|
|access_token|アクセストークン||
|refresh_token_expires_in|リフレッシュトークンの有効期限|24時間（86400秒）<br>※p_ownerをリクエスト時に設定した場合、返却されない|
|refresh_token|リフレッシュトークン|※p_ownerをリクエスト時に設定した場合、返却されない|
|token_type|Bearer||
|expires_in|アクセストークンの有効期限|1時間（3600秒）|
|p_cookie_peer|クッキー認証値|クッキー認証時に指定する認証値<br>※クッキー発行オプション（p_cookie）をリクエスト時に設定した場合のみ返却する|
#### レスポンスサンプル
```JSON
{
  "access_token": "AA~osIZ4CZ8cZmxf5NidEueHej_6Lj-ww0c_kJZd4HbHBqFyZ0OZBrS29miYr9Jh19b0o39cTJdH2Va3xSMMbu6Eg",
  "refresh_token_expires_in": 86400,
  "refresh_token": "RA~uELMJkVpzTtsl1ueh2KlrT9UiOx85-dmg7nGX01YaogoQ86qgfv2VMUxQXSP95uNY9MuWxZe0AQFtEnFYyWMoQ",
  "token_type": "Bearer",
  "expires_in": 3600
}
```
#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.md)を参照

### cURLサンプル
##### パスワード認証
```sh
curl "https://{UnitFQDN}/{CellName}/__token" -X POST -i -d 'grant_type=password&username={username}&password={password}'
```
##### トークン認証
```sh
curl "https://{UnitFQDN}/{CellName}/__token" -X POST -i -d 'grant_type=urn:ietf:params:oauth:grant-type:saml2-bearer&assertion={token}'
```
##### リフレッシュトークン認証
```sh
curl "https://{UnitFQDN}/{CellName}/__token" -X POST -i -d 'grant_type=refresh_token&refresh_token={refresh_token}'
```
##### パスワード認証 + スキーマ認証
```sh
curl "https://{UnitFQDN}/{CellName}/__token" -X POST -i -d 'grant_type=password&username={user_name}&password={pass}&client_id=https://{UnitFQDN}/app{CellName}/&client_secret={token_from_app_cell}'
```
##### トークン認証 + トランスセルトークン認証
```sh
curl "https://{UnitFQDN}/{CellName}/__token" -X POST -i -d 'grant_type=urn:ietf:params:oauth:grant-type:saml2-bearer&assertion={SAML_token}&p_target=https://{UnitFQDN}/{CellName}/'
```

###### Copyright 2017 FUJITSU LIMITED
