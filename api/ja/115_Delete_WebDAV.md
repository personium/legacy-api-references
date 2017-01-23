# ファイル削除
### 概要
存在するWebDav情報を削除する

### 必要な権限
write

### 制限事項
共通制限
* なし

WebDAV制限
* 未稿

<br>
### リクエスト
#### リクエストURL

|パス<br>|概要<br>|備考<br>|
|:--|:--|:--|
|Cell_name<br>|セル名<br>| <br>
|Box_name<br>|ボックス名<br>| <br>
|resource_path<br>|リソースへのパス<br>|有効値 桁数:1&#65374;128<br>使用可能文字種<br>半角英数字、半角ピリオド(.)、半角アンダーバー(_)、半角ハイフン(-)<br>|
#### メソッド
DELETE

#### リクエストクエリ
共通リクエストクエリ

|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|dc_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|

#### リクエストヘッダ
##### 共通リクエストヘッダ

|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用される<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定する<br>|
|X-Dc-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {TokenValue}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
|If-Match<br>|対象ETag値を指定する<br>|ETag値<br>|×<br>|省略時は[*]として扱う<br>|
#### リクエストボディ
なし

#### リクエストサンプル
なし

<br>
### レスポンス
#### ステータスコード

|コード<br>|メッセージ<br>|概要<br>|
|:--|:--|:--|
|204<br>|No Content<br>|更新成功時<br>|
#### レスポンスヘッダ

|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|Content-Type<br>|application / json<br>|削除に失敗した場合のみ返却する<br>|
#### レスポンスボディ
なし

#### エラーメッセージ一覧
[エラーメッセージ一覧](198_Error_Messages.html)を参照

|コード<br>|メッセージ<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|
|401<br>|Unauthorized<br>|認証トークンが無効<br>| <br>|
|403<br>|Forbidden<br>|アクセス権限が不足している場合<br>| <br>|
|404<br>|Not Found<br>|存在しないリソースを指定<br>| <br>|
|405<br>|Method Not Allowed<br>|許可していないリクエストメソッドを指定<br>| <br>|
|409<br>|Conflict<br>|紐付く別のリソースが存在しているため削除できない場合<br>| <br>|
|412<br>|Precondition Failed<br>|If-Matchの指定誤り<br>| <br>|
|503<br>|Too many concurrent requests.<br>|更新系の処理が競合している場合<br>| <br>|

#### レスポンスサンプル
なし

<br>
### CURLサンプル
#### CURLコマンド(UNIX)
```sh
curl 'https://fqdn/cell_name/box_name/test.txt' -X DELETE -v -k -H 'Authorization:Bearer auth_token'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
