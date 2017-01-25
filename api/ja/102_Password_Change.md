# パスワード変更(\__mypassword)
### 概要
Accountのパスワードに関する操作を行うAPI
##### 自アカウントのパスワード変更
自アカウントのパスワード変更を行う。
※ Account更新APIでもAccountのパスワード変更を行うことができるが、これはセルレベルACLのauth権限が必要で管理目的で利用する。

### 必要な権限
セルレベルACLのauth権限
### 制限事項
なし

<br>
### リクエスト
#### リクエストURL
```
{CellName}/__mypassword
```
#### メソッド
PUT
#### リクエストクエリ
クエリは無視する
#### リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {UnitUserToken}<br>|○<br>|認証トークンは認証トークン取得APIで取得したセルローカルトークン<br>|
|X-Dc-Credential<br>|変更後パスワード<br>|文字列<br>|○<br>|文字数：6&#65374;32文字<br>文字種:半角英数字と-(半角ハイフン)と_(半角アンダーバー)<br>|
#### リクエストボディ
なし

#### リクエストサンプル
なし

<br>
### レスポンス
#### ステータスコード
204
#### レスポンスヘッダ
なし
#### レスポンスボディ
なし
#### エラーメッセージ一覧
[エラーメッセージ一覧](198_Error_Messages.html)を参照

|コード<br>|メッセージ<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|
|400<br>|Bad Request<br>|変更後パスワードが無効<br>リクエストヘッダの形式が不正<br>|<br>|
|401<br>|Unauthorized<br>|認証トークンが無効<br>トークンの有効期限切れ<br>|<br>|
|403<br>|Forbidden<br>|このセルに属するアカウントの認証トークンではない<br>|<br>|
|404<br>|Not Found<br>|存在しないリソースを指定<br>|<br>|
|405<br>|Method Not Allowed<br>|許可していないリクエストメソッドを指定<br>|<br>|
#### レスポンスサンプル
なし

<br>
### CURLサンプル
```sh
curl "https://{UnitFQDN}/{CellName}/__mypassword' -X PUT -i -H 'X-Dc-Credential: change_password' -H 'Authorization: Bearer {UnitUserToken}' -H 'Accept: application/json'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
