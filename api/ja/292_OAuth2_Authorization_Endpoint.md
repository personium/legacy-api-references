# OAuth2.0 認可エンドポイント(\__authz)
## 概要
OAuth2の認可エンドポイントAPI  
このAPIは、JSアプリケーション・ネイティブアプリでPersoniumを利用する場合のOAuth2の認可エンドポイントである。

### 制限事項
scope=openidは、以下のresponse_typeのみ指定可能  

* response_type=id_token
* response_type=code

## リクエスト
### リクエストURL
```
{CellName}/__authz
```

### メソッド
GET : 認証フォームリクエスト
POST : 認可処理リクエスト（トークン認証、コード認証、id_token認証）

### リクエストクエリ
|項目名|概要|書式|必須|有効値|
|:--|:--|:--|:--|:--|
|response_type|応答タイプ|String|○|token, code, id_token(scope=openid必須)|
|client_id|アプリセル URL|String|○|スキーマ認証元のアプリセルURL|
|redirect_uri|クライアントのリダイレクトエンドポイントURL|String|○|アプリセルのデフォルトBOX配下に登録されたリダイレクトスクリプトのURL<br>application/x-www-form-urlencodedでフォーマットされたクエリパラメータを含める事ができる<br>フラグメントを含める事はできない<br>有効桁長:512byte|
|state|リクエストとコールバックの間で状態を維持するために使用するランダムな値|String|×|ランダムな値<br>有効桁長:512byte|
|scope|要求するアクセス範囲|String|×|Personiumでは"openid"を指定可能|
|username|ユーザ名|String|×|登録済のユーザ名|
|password|パスワード|String|×|登録済のパスワード|
|expires_in|アクセストークンの有効期限（秒）|Int<br>1～3600|×|発行されるアクセストークンの有効期限を指定<br>デフォルトは3600（1時間）<br>※response_typeがtoken以外の場合は、本パラメタの指定は無視する|
|cancel_flg|キャンセルフラグ|Boolean|×|認可処理のユーザキャンセルフラグ<br>trueが設定された場合、ユーザによりキャンセルされたものとする|
|password_change_required|パスワード変更必須フラグ|Boolean|×|認証したアカウントがパスワード変更必須であったかを示すフラグ<br>※認可リクエストのレスポンスで返却されたpassword_change_required|
|access_token|アクセストークン|String|×|認証したアカウントのアクセストークンを指定<br>※認可リクエストのレスポンスで返却されたaccess_token|

### リクエストヘッダ
なし

### リクエストボディ
リクエストクエリと同じ


## レスポンス（認証フォームリクエスト）
認証フォームまたはパスワード変更フォームを表示する。
ただし、リクエストパラメータが不正な場合はフォームを表示せずリダイレクトエンドポイントにリダイレクトされる。

認証フォームとパスワード変更フォームは、システムのデフォルトまたは指定したhtmlを使用することができる。  
htmlを指定する場合、[Unitの設定](../../server-operator/unit_config_list.md)または[対象Cellのプロパティ設定](./291_Cell_Change_Property.md)が必要。2つを同時に設定した場合、対象Cellのプロパティ設定が優先される。  

Unitの設定  
```
io.personium.core.cell.authorizationhtmlurl.default={htmlが取得可能なURL}
io.personium.core.cell.authorizationpasswordchangehtmlurl.default={htmlが取得可能なURL}
```

対象Cellのプロパティ設定  
```xml
<p:authorizationhtmlurl>{htmlが取得可能なURL}</p:authorizationhtmlurl>
<p:authorizationpasswordchangehtmlurl>{htmlが取得可能なURL}</p:authorizationpasswordchangehtmlurl>
```
URLに指定可能なスキームは"http","https","personium-localunit","personium-localcell"。

### 認証フォーム表示、パスワード変更フォーム表示
#### ステータスコード
200  
#### レスポンスヘッダ
|ヘッダ名|概要|備考|
|:--|:--|:--|
|Content-Type|text/html; charset=UTF-8||
#### レスポンスボディ
認証フォームまたはパスワード変更フォーム(HTML)を返却する。<br>
リクエスト時に設定したpassword_change_requiredがtrueの場合、パスワード変更フォームを返却する。<br>
それ以外の場合、認証フォームを返却する。


### パラメータチェックエラー（client_id、redirect_uri）
「client_id、redirect_uriが未指定」「client_id、redirect_uriがURL形式ではない」「client_idとredirect_uriのセルが異なる」の場合
#### ステータスコード
303  
システムのエラーページにリダイレクトされる。
```
{error_page_uri}?code={code}
```

#### URLパラメータ
|項目名|概要|備考|
|:--|:--|:--|
|error_page_uri|Redirect URL|システムのエラーページ「セルのURL + __html/error」|
|code|[Personiumのメッセージコード](004_Error_Messages.md)||

### パラメータチェックエラー（上記以外）
client_id、redirect_uri、username、password以外のリクエストパラメータで、必須項目が未設定もしくは設定値が不正な形式の場合<br>
または、cancel_flgにtrueが設定されている（ユーザによりキャンセルされた）場合
#### ステータスコード
303  
URLパラメータで示すフラグメントまたはクエリが格納される。
（response_type=codeの場合はクエリが格納され、それ以外の場合はフラグメントが格納される）
```
{redirect_uri}#error={error}&error_description={error_description}&state={state}&code={code}
```
#### URLパラメータ
|項目名|概要|備考|
|:--|:--|:--|
|redirect_uri|Redirect URL|リクエストの「redirect_uri」で指定された、<br>クライアントのリダイレクトスプリクトのURL|
|error|エラー内容を示すコード|「error」を参照|
|error_description|エラーの追加情報|例外メッセージなどを設定する|
|error_uri|エラーの追加情報のWebページのURI|空文字を返す<br>※今後のエンハンスに備えて設定|
|state|リクエスト時に設定したstateの値||
|code|[Personiumのメッセージコード](004_Error_Messages.md)||
##### error
|コード値|概要|備考|
|:--|:--|:--|
|invalid_request|リクエストで必須パラメータが指定されていない<br>リクエストパラメータの形式が不正<br>認証失敗||
|unauthorized_client|クライアントが認可されていない<br>ユーザによってキャンセルされた||
|access_denied|client_idとredirect_uriのセルが異なる<br>トランスセルトークン認証に失敗した場合||
|unsupported_response_type|response_typeの値が不正||
|server_error|サーバエラー||

## レスポンス（認可処理リクエスト）
認可処理を行う。処理結果によってリダイレクト先やURLパラメータが異なる。  
処理結果のパターンは以下の通り。
- 認可処理成功（トークン認証）
- 認可処理成功（コード認証）
- 認可処理成功（id_token認証）
- 認証失敗
- パラメータチェックエラー（client_id、redirect_uri）
- パラメータチェックエラー（上記以外）

### 認可処理成功（トークン認証）
認可処理に成功 かつ リクエストのresponse_typeにtokenを指定した場合
#### ステータスコード
303  
ブラウザはクライアントのリダイレクトエンドポイントURL（リクエストの「redirect_uri」の値）にリダイレクトされる。<br>
redirect_uriに、「URLパラメータ」で示すフラグメントが格納される。
```
{redirect_uri}#access_token={access_token}&token_type=Bearer&expires_in={expires_in}&state={state}&last_authenticated={last_authenticated}&failed_count={failed_count}
```
#### URLパラメータ
|項目名|概要|備考|
|:--|:--|:--|
|redirect_uri|クライアントのリダイレクトエンドポイントURL|リクエストの「redirect_uri」の値|
|access_token|取得されたアクセストークン|セルローカルトークンもしくは、トランスセルトークンを返却する|
|token_type|Bearer||
|expires_in|アクセストークンの有効期限（秒）|リクエスト時に設定した有効期限<br>デフォルトは3600（1時間）|
|state|リクエスト時に設定したstateの値|リクエストとコールバックの間で状態を維持するために使用するランダムな値|
|last_authenticated|前回認証日時|前回の認証日時（long型のUNIX時間）<br>初回認証時はnull<br>※パスワード認証の場合のみ返却する|
|failed_count|認証失敗回数|前回認証時からのパスワード認証に連続で失敗した回数<br>※パスワード認証の場合のみ返却する|
|box_not_installed|Box未インストールフラグ|アプリセルURLをスキーマに持つBoxが作成されていない場合のみtrueを返却する|

### 認可処理成功（コード認証）
認可処理に成功 かつ リクエストのresponse_typeにcodeを指定した場合
#### ステータスコード
303  
ブラウザはクライアントのリダイレクトエンドポイント（リクエストの「redirect_uri」の値）にリダイレクトされる。<br>
redirect_uriに、「URLパラメータ」で示すクエリが格納される。
```
{redirect_uri}?code={code}&state={state}&last_authenticated={last_authenticated}&failed_count={failed_count}
```
#### URLパラメータ
|項目名|概要|備考|
|:--|:--|:--|
|redirect_uri|クライアントのリダイレクトエンドポイントURL|リクエストの「redirect_uri」の値|
|code|取得されたCode|grant_type:authorization_codeで認可可能なCode|
|state|リクエスト時に設定したstateの値|リクエストとコールバックの間で状態を維持するために使用するランダムな値|
|last_authenticated|前回認証日時|前回の認証日時（long型のUNIX時間）<br>初回認証時はnull<br>※パスワード認証の場合のみ返却する|
|failed_count|認証失敗回数|前回認証時からのパスワード認証に連続で失敗した回数<br>※パスワード認証の場合のみ返却する|
|box_not_installed|Box未インストールフラグ|アプリセルURLをスキーマに持つBoxが作成されていない場合のみtrueを返却する|

### 認可処理成功（id_token認証）
認可処理に成功 かつ リクエストのresponse_typeにid_tokenを指定した場合
#### ステータスコード
303  
ブラウザはクライアントのリダイレクトエンドポイントURL（リクエストの「redirect_uri」の値）にリダイレクトされる。<br>
redirect_uriに、「URLパラメータ」で示すフラグメントが格納される。
```
{redirect_uri}#id_token={id_token}&state={state}&last_authenticated={last_authenticated}&failed_count={failed_count}
```

#### URLパラメータ
|項目名|概要|備考|
|:--|:--|:--|
|redirect_uri|クライアントのリダイレクトエンドポイントURL|リクエストの「redirect_uri」の値|
|id_token|取得されたid_token|OpenID Connectで利用可能なid_token|
|state|リクエスト時に設定したstateの値|リクエストとコールバックの間で状態を維持するために使用するランダムな値|
|last_authenticated|前回認証日時|前回の認証日時（long型のUNIX時間）<br>初回認証時はnull<br>※パスワード認証の場合のみ返却する|
|failed_count|認証失敗回数|前回認証時からのパスワード認証に連続で失敗した回数<br>※パスワード認証の場合のみ返却する|
|box_not_installed|Box未インストールフラグ|アプリセルURLをスキーマに持つBoxが作成されていない場合のみtrueを返却する|

### 認証失敗
認可処理中で行っている認証に失敗した場合（パスワード不一致、アカウントロック中など）
#### ステータスコード
303  
ブラウザは認可エンドポイント（セルのURL + \_\_authz）に再度リダイレクトされる。<br>
リダイレクトURLには下記パラメータのクエリが格納される。
```
{authorization_endpoint_url}?response_type={response_type}&redirect_uri={redirect_uri}&client_id={client_id}&state={state}&scope={scope}&response_type={response_type}&expires_in={expires_in}&error={error}&error_description={error_description}&error_uri={error_uri}&code={code}&password_change_required={password_change_required}&access_token={access_token}
```
#### URLパラメータ
|項目名|概要|備考|
|:--|:--|:--|
|authorization_endpoint_url|認可エンドポイントのURL（セルのURL + __authz）||
|response_type|リクエスト時に設定したresponse_typeの値||
|client_id|リクエスト時に設定したclient_idの値||
|redirect_uri|リクエスト時に設定したredirect_uriの値||
|state|リクエスト時に設定したstateの値||
|scope|リクエスト時に設定したscopeの値||
|response_type|リクエスト時に設定したresponse_typeの値||
|expires_in|リクエスト時に設定したexpires_inの値||
|error|エラー内容を示すコード|「error」を参照|
|error_description|エラーの追加情報|例外メッセージなどを設定する|
|error_uri|エラーの追加情報のWebページのURI|空文字を返す<br>※今後のエンハンスに備えて設定|
|code|[Personiumのメッセージコード](004_Error_Messages.md)||
|password_change_required|認証したアカウントがパスワード変更必須であったかを示すフラグ|認証できるがパスワード変更が必須の場合にのみtrueを返却する|
|access_token|認証したアカウントのアクセストークン|password_change_requiredがtrueの場合のみ返却する<br>パスワード変更のみ可能なアクセストークンを返却|

### パラメータチェックエラー（client_id、redirect_uri）
「client_id、redirect_uriが未指定」「client_id、redirect_uriがURL形式ではない」「client_idとredirect_uriのセルが異なる」の場合<br>
「レスポンス（認証フォームリクエスト）」の「パラメータチェックエラー（client_id、redirect_uri）」と同様。

### パラメータチェックエラー（上記以外）
client_id、redirect_uri、username、password以外のリクエストパラメータで、必須項目が未設定もしくは設定値が不正な形式の場合<br>
「レスポンス（認証フォームリクエスト）」の「パラメータチェックエラー（上記以外）」と同様。

## cURLサンプル
### GET（認証フォーム表示）
```sh
curl "https://cell1.unit1.example/__authz?response_type=token&\
redirect_uri=https://app-cell1.unit1.example/__/redirect.md&\
client_id=https://app-cell1.unit1.example" -X GET -i
```
### POST（認可処理リクエスト）
```sh
curl "https://cell1.unit1.example/__authz" -X POST -i \
-d 'response_type=token&client_id=https://app-cell1.unit1.example/&\
redirect_uri=https://app-cell1.unit1.example/__/redirect.md&\
state=0000000111&username=account1&password=pass'
```
