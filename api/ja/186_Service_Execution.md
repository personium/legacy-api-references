# サービス実行
### 概要
登録されたサービスを実行する
### 必要な権限
exec
### 制限事項
* 共通制限
	* なし
* WebDAV制限
	* 未稿

<br>
### リクエスト
#### リクエストURL
```
/{CellName}/{BoxName}/{ResourcePath}/{SrcName}
```
|パス<br>|概要<br>|備考<br>|
|:--|:--|:--|
|{CellName}<br>|セル名<br>| <br>|
|{BoxName}<br>|ボックス名<br>| <br>|
|{ResourcePath}<br>|リソースへのパス<br>|有効値 桁数:1&#65374;128<br>使用可能文字種<br>半角英数字、半角ピリオド(.)、半角アンダーバー(_)、半角ハイフン(-)<br>|
|{SrcName}<br>|サービスソースの名前を指定<br>|有効値(制限) 桁数:1&#65374;128<br>使用可能文字種<br>半角英数字、半角ピリオド(.)、半角アンダーバー(_)、半角ハイフン(-)<br>|
#### メソッド
GET / POST / PUT / DELETE
#### リクエストクエリ
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
#### リクエストボディ
なし
#### リクエストサンプル
なし

<br>
### レスポンス
#### ステータスコード
スクリプトが実行された場合はスクリプトのレスポンスコードが返却される
#### レスポンスヘッダ
|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|Content-Type<br>|エントリーの削除に失敗した場合のみ返却する<br>|application / json<br>|
#### レスポンスボディ
スクリプトが実行された場合はスクリプトのレスポンスボディが返却される
#### エラーメッセージ一覧
[エラーメッセージ一覧](199_Error_Messages.html)を参照
##### WebDAV共通
|コード<br>|メッセージ<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|
|400<br>|Bad Request<br>|リクエストボディの形式が不正 <br>リクエストヘッダの形式が不正<br>| <br>|
|401<br>|Unauthorized<br>|認証トークンが無効<br>| <br>|
|403<br>|Forbidden<br>|アクセス権限が不足している場合<br>| <br>|
|404<br>|Not Found<br>|存在しないリソースを指定<br>| <br>|
|405<br>|Method Not Allowed<br>|許可していないリクエストメソッドを指定<br>| <br>|
|412<br>|Precondition Failed<br>|存在しないバージョンを指定<br>| <br>|
##### サービス実行固有
|コード<br>|メッセージ<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|
|404<br>|404 Not Found (User Script)<br>|ユーザスプリクトが存在しない<br>| <br>|
#### レスポンスサンプル
なし

<br>
### CURLサンプル
#### CURLコマンド(UNIX)
```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/svccol/hello" -X GET -i -H 'Authorization: Bearer {UnitUserToken}' -H 'Accept: application/json'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
