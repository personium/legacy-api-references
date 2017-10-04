# メッセージ状態変更(未読,既読,承認)
### 概要
* メッセージを承認する
	* 承認するメッセージのTypeがmessageの場合  
	メッセージの状態を既読/未読に変更する
	* 承認するメッセージのTypeがreq.relation.build/req.role.grantの場合  
	承認するメッセージのRequestRelation, RequestRelationTargetの値に従い、関係を登録し、メッセージの状態を承認/拒否に変更する
	* 承認するメッセージのTypeがreq.relation.break/req.role.revokeの場合  
	承認するメッセージのRequestRelation, RequestRelationTargetの値に従い、関係を削除し、メッセージの状態を承認/拒否に変更する

### 必要な権限
message
social（関係登録/削除承認のみ必要）
### 制限事項
* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
* リクエストボディはJSON形式のみ受け付ける
* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはJSON形式とする
* $formatクエリオプションにatom または xmlを指定した場合、エラーとはならないが、レスポンスボディのデータの保証はない

<br>
### リクエスト
#### リクエストURL
```
/{CellName}/__message/received/{MessageID}
```
#### メソッド
POST
#### リクエストクエリ
|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|p_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|
#### リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用されます。<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定します。<br>|
|X-Personium-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {AccessToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
|Content-Type<br>|リクエストボディの形式を指定する<br>|application / JSON<br>|×<br>|省略時は[application/json]として扱う<br>|
|Accept<br>|レスポンスボディの形式を指定する<br>|application / JSON<br>|×<br>|省略時は[application/json]として扱う<br>|
#### リクエストボディ
JSON

|項目名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Command<br>|メッセージコマンド<br>|Typeがmessageの場合<br>　read: 既読<br>　unread: 未読<br>Typeがreq.relation.build/req.relation.break/req.role.grant/req.role.revokeの場合<br>　approved: 承認<br>　rejected: 拒否<br>　ただし既にapproved、rejectedに変更済みの場合は承認不可<br>|○<br>|<br>|
#### リクエストサンプル
```JSON
{"Command": "approved"}
```

<br>
### レスポンス
#### ステータスコード
204
#### レスポンスヘッダ
|項目名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|ETag<br>|リソースのバージョン情報<br>|<br>|
|DataServiceVersion<br>|ODataのバージョン<br>|<br>|
|Access-Control-Allow-Origin<br>|クロスドメイン通信許可ヘッダ<br>|返却値は"*"固定<br>|
|X-Personium-Version<br>|APIの実行バージョン<br>|リクエストが処理されたAPIバージョン<br>|

#### レスポンスボディ
なし
#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

#### レスポンスサンプル
なし

<br>
### cURLサンプル

```sh
curl "https://{UnitFQDN}/{CellName}/__message/received/{MessageID}" -X POST -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"Command": "approved"}'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
