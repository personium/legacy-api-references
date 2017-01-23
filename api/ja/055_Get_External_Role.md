# ExtRole取得
### 概要
既存のExtRole情報を取得する
### 必要な権限
social
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
auth-read

<br>
### リクエスト
#### リクエストURL
```
/{Cell_name}/__ctl/ExtRole
```
#### メソッド
GET
#### リクエストクエリ
|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|dc_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|

[$select クエリ](192_$select_Query.html)

[$expand クエリ](191_$expand_Query.html)

[$format クエリ](190_$Format_Query.html)

#### リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用されます。<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定します。<br>|
|X-Dc-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {TokenValue}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
|Accept<br>|レスポンスボディの形式を指定する<br>|application / json<br>|×<br>|省略時は[application/json]として扱う<br>|

<br>
### レスポンス
#### ステータスコード
200
#### レスポンスボディ
###### 共通レスポンスボディ
レスポンスはJSONオブジェクトで、オブジェクト（サブオブジェクト）に定義されるキー(名前)と型、並びに値の対応は以下のとおりです。

|オブジェクト<br>|項目名<br>|Data Type<br>|備考<br>|
|:--|:--|:--|:--|
|ルート<br>|d<br>|object<br>|オブジェクト{1}<br>|
|{1}<br>|__count<br>|string<br>|$inlinecountクエリでの取得結果件数<br>|
|{1}<br>|results<br>|array<br>|オブジェクト{2}の配列<br>|
|{2}<br>|__published<br>|string<br>|作成日(UNIX時間)<br>|
|{2}<br>|__updated<br>|string<br>|更新日(UNIX時間)<br>|
|{2}<br>|__metadata<br>|object<br>|オブジェクト{3}<br>|
|{3}<br>|etag<br>|string<br>|Etag値<br>|
|{3}<br>|uri<br>|string<br>|作成したリソースへのURL<br>|
#### ExtRole固有レスポンスボディ
|オブジェクト<br>|項目名<br>|Data Type<br>|備考<br>|
|:--|:--|:--|:--|
|{3}<br>|type<br>|string<br>|CellCtl.ExtRole<br>|
|{2}<br>|ExtRole<br>|string<br>|外部ロールURL<br>|
|{2}<br>|_Relation.Name<br>|string<br>|Relation名<br>|
|{2}<br>|_Relation._Box.Name<br>|string<br>|Relationに紐付くBox名<br>|
#### エラーメッセージ一覧
[エラーメッセージ一覧](198_Error_Messages.html)を参照

#### レスポンスサンプル
```json
{
  "d": {
    "results": {
      "__published": "/Date(1371010428917)/",
      "_Relation._Box.Name": null,
      "__updated": "/Date(1371010428917)/",
      "_Relation.Name": "relation_name",
      "__metadata": {
        "etag": "W/\"1-1371010428917\"",
        "type": "CellCtl.ExtRole",
        "uri": "https://fqdn/cell_name/__ctl/ExtRole(ExtRole='https://fqdn/cell_name/__role/__/role_name',_Relation.Name='relation',_Relation._Box.Name=null)"
      },
      "ExtRole": "https://fqdn/cell_name/__role/__/role_name"  
    }
  }
}
```

<br>
### CURLサンプル
#### CURLコマンド(UNIX)
```sh
curl 'https://fqdn/cellname/__ctl/ExtRole(ExtRole=\'https%3A%2F%2Ffqdn%2Fcellname%2F__role%2F__%2Froletest\',_Relation.Name=\'relation\',_Relation._Box.Name=\'box\')' \
-X GET -v -k -H 'Authorization:Bearer auth_token'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
