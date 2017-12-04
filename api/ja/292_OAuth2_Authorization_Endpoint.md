# OAuth2.0 認可エンドポイント(\__authz)
### 概要
OAuth2の認可エンドポイントAPI  
このAPIは、JSアプリケーション・ネイティブアプリでPersoniumを利用する場合のOAuth2の認可エンドポイントである。
### 必要な権限
なし
### 前提条件
このAPIを実行するためには、アプリセルURLをスキーマに持つBoxを事前に作成しておく必要がある。
### 制限事項
リクエストクエリ、リクエストボディの「p_target」パラメータの指定は未サポート
* p_targetを指定した場合、レスポンスヘッダの「Location」の値が4,096文字を超えるため、nginxでエラーとなる。
* Internet Explorerでは、URLの最大長が2,048文字に制限されているため、正しくリダイレクトできない。


### リクエスト
#### リクエストURL
```
{CellName}/__authz
```
#### メソッド
GET : 認証フォーム リクエスト
POST : 認証フォーム リクエスト、トークン認証  
#### リクエストクエリ
|項目名|概要|書式|必須|有効値|
|:--|:--|:--|:--|:--|
|response_type|応答タイプ|String|○|トークン|
|client_id|アプリセル URL|String|○|スキーマ認証元のアプリセルURL|
|redirect_uri|クライアントのリダイレクトエンドポイントURL|String|○|アプリセルのデフォルトBOX配下に登録されたリダイレクトスクリプトのURL<br>application/x-www-form-urlencodedでフォーマットされたクエリパラメータを含める事ができる<br>フラグメントを含める事はできない<br>有効桁長:512byte|
|state|リクエストとコールバックの間で状態を維持するために使用するランダムな値|String|×|ランダムな値<br>有効桁長:512byte|
|p_target|トランスセルトークンターゲット|String|×|払い出されるトークンを使うセルURL<br>指定した場合トランスセルトークンを払い出す|
|p_owner|ULUUT昇格実行クエリ|String|×|trueのみ有効<br>※このクエリを設定した場合、認証情報はCookieで返却されない|
|assertion|アクセストークン|String|×|有効なトランスセルトークン<br>引数なしの場合トークン認証にはならない|
|username|ユーザ名|String|×|登録済のユーザ名|
|password|パスワード|String|×|登録済のパスワード|
#### リクエストヘッダ
なし
#### リクエストボディ
リクエストクエリと同じ
#### リクエストサンプル
なし


### レスポンス
#### Forms Authentication Request
##### ステータスコード
200
##### レスポンスヘッダ
|ヘッダ名|概要|備考|
|:--|:--|:--|
|Content-Type|Content-Type of Resource||
##### レスポンスボディ
以下のHTMLフォームを返却する。
![レスポンスボディ](image/OAuth2ResponseBody.png "レスポンスボディ")

##### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

##### レスポンスサンプル
なし
#### Request Token Authentication
##### ステータスコード
302  
ブラウザはredirect_uriにリダイレクトされる。redirect_uriに、「URLパラメータ」で示すフラグメントが格納される。
```
{redirect_uri}#access_token={access_token}&token_type=Bearer&expires_in={expires_in}&state={state}
```
|項目名|概要|備考|
|:--|:--|:--|
|redirect_uri|クライアントのリダイレクトエンドポイントURL|リクエストの「redirect_uri」の値|
|access_token|認証・認可要求フォームで取得したアクセストークン|セルローカルトークンもしくは、トランスセルトークンを返却する|
|token_type|Bearer||
|expires_in|上記「access_token」の有効期限|1時間（3600秒）|
|state|リクエスト時に設定したstateの値|リクエストとコールバックの間で状態を維持するために使用するランダムな値|
##### エラーメッセージ一覧
|項目名|概要|備考|
|:--|:--|:--|
|redirect_uri|Redirect URL|リクエストの「redirect_uri」で指定された、<br>クライアントのリダイレクトスプリクトのURL<br>ただし、以下のエラー内容の場合はこの値は「セルのURL + __html/error」に設定される<br>「redirect_uriがURL形式ではない」「client_idとredirect_uriのセルが異なる」|
|error|エラー内容を示すコード|「error」を参照|
|error_description|エラーの追加情報|例外メッセージなどを設定する|
|error_uri|エラーの追加情報のWebページのURI|空文字を返す<br>※今後のエンハンスに備えて設定|
|state|リクエスト時に設定したstateの値||
|code|Personiumのエラーコード||
|invalid_request|リクエストで必須パラメータが指定されていない<br>リクエストパラメータの形式が不正<br>アカウントロック中||
|unauthorized_client|クライアントが認可されていない<br>ユーザによってキャンセルボタンが押下された||
|access_denied|client_idとredirect_uriのセルが異なる<br>トランスセルトークン認証に失敗した場合||
|unsupported_response_type|response_typeの値が不正||
|server_error|サーバエラー||
|Please, input user ID and password.|「username」もしくは「password」が未入力||
|User ID or password is incorrect.|パスワード認証に失敗した場合||
|Since the Expiration Date of the authentication passed,<br>You must be authorized again.|Cookie認証に失敗した場合||
##### Parameter Check Error
ブラウザはredirect_uriにリダイレクトされる。  
「redirect_uriがURL形式ではない」「client_idとredirect_uriのセルが異なる」「認可処理失敗」
```
{redirect_uri}?code={code}
```

上記以外
```
{redirect_uri}#error={error}&error_description={error_description}&state={state}&code={code}
```


### cURLサンプル
#### GET
```sh
curl "https://{UnitFQDN}/{CellName}/__authz?response_type=token&redirect_uri=https://{UnitFQDN}/{AppliCellName}/__/redirect.html&client_id=https://{UnitFQDN}/{AppliCellName}" -X GET -i
```
#### POST
```sh
curl "https://{UnitFQDN}/{CellName}/__authz" -X POST -i -d 'response_type=token&client_id=https://{UnitFQDN}/{AppliCellName}&redirect_uri=https://{UnitFQDN}/{AppliCellName}/__/redirect.html&state=0000000111&username={AccountUserName}&password={AccountUserPass}'
```

###### Copyright 2017 FUJITSU LIMITED
