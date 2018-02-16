# OAuth 2.0 トークンエンドポイント
## 概要

セルの様々なAPIにアクセスするためのアクセストークンを発行するAPIのエンドポイントです。 OAuth 2.0仕様に準拠しており、様々な方法でユーザやアプリを認証してアクセストークンとリフレッシュトークンを発行します。

### ユーザ認証のバリエーション

#### パスワードによるアカウント所有者認証

* OAuth 2.0 の ROPC (Resource Owner Password Credential) フローです。
* 登録したアカウントの名前とパスワードを送信することでアカウントの所有者を認証してトークンを発行します。
* 総当たり攻撃に対する防御策として、認証に失敗後1秒間はそのアカウントについてはパスワードが正当であっても認証エラーとなります。
* OAuth 2.0の仕様に記述されているとおり、このフローを一般のPersoniumアプリは使うべきではありません。

#### トランスセルアクセストークンによる他セルユーザの認証

* OAuth 2.0 拡張の SAML2 assertion フローに対応する認証方法です。
* このセル宛のトランスセルトークンを送信することで、このセルで有効なトークンを取得することができます。


#### リフレッシュトークン送信によるユーザ認証状態継続

* リフレッシュトークンを送信することでそのリフレッシュトークン発行時にうけたユーザ認証状態を引き継いだトークンを取得できます。
* この際client_id, client_secret送信によるアプリの再認証が必要となります。
* Personiumではトークンリフレッシュ時に別アプリのclient_id, client_secretを送信することで、アプリの切り替えが可能です。

### アプリ認証のための情報送信

* PersoniumではOAuthのclient認証の枠組を使ってアプリの認証を行います。
* Personiumではclient_idとして、アプリのスキーマURIを用います。多くの場合スキーマURIはアプリセルのURLです。
* Personiumではclient_secretとしてアプリ認証トークンを用います。

### 発行トークン指定のバリエーション

#### セルローカルトークン

p_target というパラメタの指定がないときはこのセルで有効なトークンを発行します。

#### トランスセルトークン

p_target というパラメタで他のセルのURLを指定することで、他のセルにアクセスするためのトランスセルアクセストークンを発行します。


### その他のバリエーション

#### クッキーの発行

* p_cookie というパラメタをtrue値で指定することでこのセルでのみ有効なクッキーを発行します。
* このクッキーは単体ではアクセストークンの代わりになりませんが、同時に発行されるcookie_peerという文字列と共に用いることでアクセストークン同等の効果を持ちます。
* このセル上のリソースをGETするさいにURLにcookie_peerというクエリパラメタでこの値をつけてアクセスすることで、同時に送られるクッキーと対応が取れているときのみ、アクセストークン送信と同様の扱います。
* これにより例えばHTMLの img video audio等のタグで非公開のメディアを許可されたユーザにのみ表示することなどが可能です。
* 発行されるクッキーはこのセルでのみ有効なものであるため、他セルアクセスのためのトークン取得用パラメタであるp_target と併用することはできません。


## リクエスト
### リクエストURL
```
{CellName}/__token
```
### メソッド
POST

### リクエストクエリ

|クエリ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|p_cookie_peer|クッキー認証値|認証時にサーバから返却されたクッキー認証値|×|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する|

### リクエストヘッダ

|項目名|概要|書式|必須|有効値|
|:--|:--|:--|:--|:--|
|Authorization|OAuth2.0形式で、認証情報を指定する|Basic {String}|×|{{アプリのスキーマURI}:{アプリ認証トークン}}をBase64Encodeした値を指定した場合、スキーマ認証になる<br>上記設定時、リクエストボディにclient_idとclient_secretの設定がある場合、Authorizationヘッダの設定が優先される|
### リクエストボディ
#### パスワード認証

|項目名|概要|書式|必須|有効値|
|:--|:--|:--|:--|:--|
|grant_type|認可タイプ|String|○|password<br>urn&#58;x-personium:oidc:google<br>urn&#58;ietf:params:oauth:grant-type:saml2-bearer<br>refresh_token|
|username|ユーザ名|String|(grant_type=passwordの場合必須)|登録済のユーザ名|
|password|パスワード|String|(grant_type=passwordの場合必須)|登録済のパスワード|
|assertion|トランスセルアクセストークン|String||有効なトランスセルアクセストークン|
|refresh_token|リフレッシュトークン名|String|○|有効リフレッシュトークン|
|id_token|トークンID|JSON Web Token|○(grant_type=urn&#58;x-personium:oidc:googleの場合)|JWT Formed ID Token|
|p_target|トランスセルトークンターゲット|String|×|払い出されるトークンを使う先（セルURL）<br>指定した場合トランスセルトークン認証になる|
|p_target|トランスセルトークンターゲット|String|×|払い出されるトークンを使う先（セルURL）<br>指定した場合トランスセルトークンになる|
|p_target|トランスセルトークンターゲット|String|×|払い出されるトークンを使う先（セルURL） 指定した場合はトランスセルトークン認証になる|
|client_id|アプリセルURL|String|×|スキーマ認証元のアプリセルURL<br>client_secretとともに指定した場合スキーマ認証になる<br>同時にAuthorizationヘッダにもスキーマ認証設定がされている場合、Authorizationヘッダの設定が優先される|
|client_secret|アプリセルから払い出されたトークン|String|×|スキーマ認証元から払い出されたトークンを値に設定する<br>client_idとともに指定した場合スキーマ認証になる<br>同時にAuthorizationヘッダにもスキーマ認証設定がされている場合、Authorizationヘッダの設定が優先される|
|p_owner|ULUUT昇格実行クエリ|String|×|trueのみ有効|
|p_cookie|認証クッキー発行オプション<br>指定された場合は認証クッキーを発行する<br>p_targetが指定された場合は、本パラメタの指定は無視する|String|×|trueのみ有効|


### リクエストサンプル
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
grant_type=password&username=username&password=pass&client_id=https://{UnitFQDN}/app{CellName}
/&client_secret=
WjzDmvJSLvM9qVuJL1xxP6hSxt64HijoIea0P5R2CVloXJ2HEvEILl7UOtEtjSDdjlvyx9wrosPBhDRU97Qnn6EQIQ3MwaqtI
x7HjuX36_ZBC6qxcgscCDmdtGb4nHgo
```
OIDC(Open ID Connect(Google))認証
```
grant_type=urn:x-personium:oidc:google&id_token=IDTOKEN
```

トランスセルアクセストークンの受け入れ
```
grant_type=urn:ietf:params:oauth:grant-type:saml2-bearer&assertion={token}
```

リフレッシュトークンの送信
```
grant_type=refresh_token&refresh_token={token}
```


## レスポンス
### ステータスコード
200

### レスポンスヘッダ

|項目名|概要|備考|
|:--|:--|:--|
|Content-Type|application/json||
|Set-Cookie|クッキー認証情報（p_cookie）|クッキー発行オプション（p_cookie）をリクエスト時に設定した場合のみ|
### レスポンスボディ

|項目名|概要|備考|
|:--|:--|:--|
|access_token|アクセストークン||
|refresh_token_expires_in|リフレッシュトークンの有効期限|24時間（86400秒）<br>※p_ownerをリクエスト時に設定した場合、返却されない|
|refresh_token|リフレッシュトークン|※p_ownerをリクエスト時に設定した場合、返却されない|
|token_type|Bearer||
|expires_in|アクセストークンの有効期限|1時間（3600秒）|
|p_cookie_peer|クッキー認証値|クッキー認証時に指定する認証値<br>※クッキー発行オプション（p_cookie）をリクエスト時に設定した場合のみ返却する|

### レスポンスサンプル
```JSON
{
  "access_token": "AA~osIZ4CZ8cZmxf5NidEueHej_6Lj-ww0c_kJZd4HbHBqFyZ0OZBrS29miYr9Jh19b0o39c
TJdH2Va3xSMMbu6Eg",
  "refresh_token_expires_in": 86400,
  "refresh_token": "RA~uELMJkVpzTtsl1ueh2KlrT9UiOx85-dmg7nGX01YaogoQ86qgfv2VMUxQXSP95uNY9Mu
WxZe0AQFtEnFYyWMoQ",
  "token_type": "Bearer",
  "expires_in": 3600
}
```
### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.md)を参照

## cURLサンプル
#### アカウント所有者認証
```sh
curl "https://{UnitFQDN}/{CellName}/__token" -X POST -i -d \
'grant_type=password&username={username}&password={password}'
```
#### 他セルユーザ認証
```sh
curl "https://{UnitFQDN}/{CellName}/__token" -X POST -i -d \
'grant_type=urn:ietf:params:oauth:grant-type:saml2-bearer&assertion={token}'
```
#### トークンリフレッシュ
```sh
curl "https://{UnitFQDN}/{CellName}/__token" -X POST -i -d \
'grant_type=refresh_token&refresh_token={refresh_token}'
```
#### アカウント所有者 + アプリ認証
```sh
curl "https://{UnitFQDN}/{CellName}/__token" -X POST -i -d \
'grant_type=password&username={user_name}&password={pass}&client_id=\
https://{UnitFQDN}/app{CellName}/&client_secret={token_from_app_cell}'
```
#### 他セルユーザ認証 による他セル向けトランスセルアクセストークン発行
```sh
curl "https://{UnitFQDN}/{CellName}/__token" -X POST -i -d \
'grant_type=urn:ietf:params:oauth:grant-type:saml2-bearer&assertion=\
{SAML_token}&p_target=https://{UnitFQDN}/{CellName}/'
```

