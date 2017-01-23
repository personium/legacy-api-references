# ログファイル削除
### Overview
ログファイルを削除する。（最新のログファイルは削除できない。 ）
ローテートされたログファイルの保持世代数は最大12世代である。
ログファイルのローテート時に最大保持世代数を超えた場合は、最古のログファイルが削除される。

### 必要な権限
log

### 制限事項
* 内部イベントのログ出力、ログの出力設定およびその参照は未サポート
* ログファイル名："default.log"（固定）
* ローテートのデフォルトサイズ設定:：50MB
* ログの出力レベル："info"（固定）（INFO, WARN, ERRORすべて出力）

<br>
### リクエスト
#### リクエストURL
###### ローテートされたログファイル
```
/{cell_name}/__log/archive/{log_name}
```
※{log_name}は、ログファイル情報取得API で返却されたファイル名を指定する。

#### メソッド
DELETE

#### リクエストクエリ

|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|dc_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|

#### リクエストヘッダ

|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用されます。<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定します。<br>|
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
|204<br>|No Content<br>|削除成功時<br>|
#### レスポンスヘッダ

|項目名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|Content-Type<br>|application/json<br>|削除に失敗した場合のみ返却する<br>|
#### レスポンスボディ
なし

#### エラーメッセージ一覧
[エラーメッセージ一覧](198_Error_Messages.html)を参照

#### レスポンスサンプル
なし

<br>
### CURLサンプル
#### CURLコマンド(UNIX)
```sh
curl 'https://fqdn/cell_name/__log/archive/default.log.1' -X DELETE -v -k \
-H 'Authorization:Bearer auth_token'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
