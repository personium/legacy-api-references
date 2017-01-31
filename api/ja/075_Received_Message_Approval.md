# メッセージ状態変更(未読,既読,承認)
### 概要
* メッセージを承認する
	* 承認するメッセージのTypeがmessageの場合
	メッセージの状態を既読/未読に変更する
	* 承認するメッセージのTypeがreq.relation.buildの場合
	承認するメッセージのRequestRelation, RequestRelationTargetの値に従い、関係を登録し、メッセージの状態を承認/拒否に変更する
	* 承認するメッセージのTypeがreq.relation.breakの場合
	承認するメッセージのRequestRelation, RequestRelationTargetの値に従い、関係を削除し、メッセージの状態を承認/拒否に変更する

### 必要な権限
message
social（関係登録/削除承認のみ必要）
### 制限事項
* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
* リクエストボディはjson形式のみ受け付ける
* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはjson形式とする
* $formatクエリオプションにatom または xmlを指定した場合、エラーとはならないが、レスポンスボディのデータの保証はない

<br>
### リクエスト
#### リクエストURL
```
/{CellName}/__message/received/{uuid}
```
#### メソッド
POST
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
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {UnitUserToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
|Content-Type<br>|リクエストボディの形式を指定する<br>|application / json<br>|×<br>|省略時は[application/json]として扱う<br>|
|Accept<br>|レスポンスボディの形式を指定する<br>|application / json<br>|×<br>|省略時は[application/json]として扱う<br>|
#### リクエストボディ
##### Format
JSON

|項目名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Command<br>|メッセージコマンド<br>|Typeがmessageの場合<br>　read: 既読<br>　unread: 未読<br>Typeがreq.relation.build/req.relation.breakの場合<br>　approved: 承認<br>　rejected: 拒否<br>　ただし既にapproved、rejectedに変更済みの場合は承認不可<br>|○<br>|<br>|
#### リクエストサンプル
```json
{"Command": "approved"}
```

<br>
### レスポンス
#### ステータスコード
204
#### レスポンスヘッダ
|項目名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|X-Dc-Version<br>|APIの実行バージョン<br>|リクエストが処理されたAPIバージョン<br>|
|Access-Control-Allow-Origin<br>|クロスドメイン通信許可ヘッダ<br>|返却値は"*"固定<br>|
|Content-Type<br>|返却されるデータの形式<br>|<br>|
|Location<br>|作成したリソースへのURL<br>|<br>|
|ETag<br>|リソースのバージョン情報<br>|<br>|
|DataServiceVersion<br>|ODataのバージョン<br>|<br>|
#### レスポンスボディ
なし
#### エラーメッセージ一覧
##### レスポンスフォーマット
|Object<br>|Name(Key)<br>|型<br>|値<br>|
|:--|:--|:--|:--|
|root<br>|code<br>|string<br>|エラーコード<br>|
|root<br>|message<br>|object<br>|オブジェクト{1}<br>|
|{1}<br>|lang<br>|string<br>|言語<br>固定値:en<br>|
|{1}<br>|value<br>|string<br>|×<br>|
[エラーメッセージ一覧](200_Error_Messages.html)を参照

#### レスポンスサンプル
なし

<br>
### CURLサンプル
#### CURLコマンド(UNIX)
```sh
curl "https://{UnitFQDN}/{CellName}/__message/received/{uuid}" -X POST -i -H 'Authorization: Bearer {UnitUserToken}' -H 'Accept: application/json' -d '{"Command": "approved"}' 
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
