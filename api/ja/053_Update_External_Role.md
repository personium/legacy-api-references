# ExtRole更新
### 概要
ExtRoleを登録する
### 制限事項
* 共通制限
	* なし
* OData 制限
	* リクエストヘッダのAcceptは無視される
	* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
	* リクエストボディはjson形式のみ受け付ける
	* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはjson形式とする
	* $formatクエリオプションは無視される

### 必要な権限
auth

<br>
### リクエスト
#### リクエストURL
```
/{Cell_name}/__ctl/ExtRole(ExtRole='{extrole_name}',_Relation.Name='{relation_name}',_Relation._Box.Name='{box_name}')
または、
/{Cell_name}/__ctl/ExtRole(ExtRole='{extrole_name}',_Relation.Name='{relation_name}')
```
※ _Relation._Box.Nameパラメタを省略した場合は、nullが指定されたものとする
#### メソッド
PUT
#### リクエストクエリ
|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|dc_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|
#### リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用されます。<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定します。<br>|
|X-Dc-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br><br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {TokenValue}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
|Content-Type<br>|リクエストボディの形式を指定する<br>|application / json<br>|×<br>|省略時は[application/json]として扱う<br>|
|Accept<br>|レスポンスボディの形式を指定する<br>|application / json<br>|×<br>|省略時は[application/json]として扱う<br>|
|If-Match<br>|対象ETag値を指定する<br>|ETag値<br>|×<br>|省略時は[*]として扱う<br>|
#### リクエストボディ
|項目名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|ExtRole<br>|外部ロール名(URL)<br>|桁数：1&#65374;1024<br>URIの形式に従う<br>scheme：http / https / urn<br>Boxに紐付いている場合:/{アプリセル名}/&#95;&#95;role/&#95;&#95;/{Role名}<br>※ただし、BoxにSchema情報が登録されていない場合、Boxに紐付いていないとみなされる<br>Boxに紐付いていない場合:/{Cell名]/&#95;&#95;role/&#95;&#95;/{Role名}<br>|○<br>|<br>|
|_Relation.Name<br>|関係対象のRelation名<br>|桁数：1&#65374;128<br>文字種:半角英数字と-(半角ハイフン)と_(半角アンダーバー)<br>と+(プラス)と:(コロン)<br>ただし、先頭文字に_(半角アンダーバー):(コロン)は指定不可<br>Relation登録APIにて登録済みのRelation<br>null<br>|○<br>|<br>|
|_Relation._Box.Name<br>|関係対象のRelationが紐づくBox名<br>|桁数：1&#65374;128<br>文字種:半角英数字と-(半角ハイフン)と_(半角アンダーバー)<br>Relation登録APIにて登録済みのRelationが紐づくBox名<br>null<br>|×<br>|<br>|
#### リクエストサンプル
```json
{
  "ExtRole": "https://fqdn/cell_name/__role/__/roletest",
  "_Relation.Name": "relation_name",
  "_Relation._Box.Name": "box_name"  
}
```

<br>
### レスポンス
#### ステータスコード
204
#### レスポンスボディ
なし
#### エラーメッセージ一覧
[エラーメッセージ一覧](198_Error_Messages.html)を参照

<br>
### CURLサンプル
#### CURLコマンド(UNIX)
```sh
curl "https://fqdn/cell_name/__ctl/ExtRole(ExtRole='https%3A%2F%2Ffqdn%2Fcellname%2F__role%2F__%2Froletest',_Relation.Name='relation_name',
_Relation._Box.Name=null)" -X PUT -i -H 'If-Match: *' -H 'Authorization: Bearer auth_token' -H 'Accept: application/json' -d '{ "ExtRole": "https://fqdn/cell_name/__role/__/rolename", "_Relation.Name":"relation_name",
"_Relation._Box.Name": null }'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
