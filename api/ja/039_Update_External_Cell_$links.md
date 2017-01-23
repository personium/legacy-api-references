# Extcell_$links更新
### 概要
Boxに紐付いたODataリソースを一覧取得する<br>以下のODataリソースを指定することができる

* Box
* ExtCell
* ExtRole
* Role

### 必要な権限
なし

### OData 制限
* リクエストヘッダのAcceptは無視される
* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
* リクエストボディはjson形式のみ受け付ける
* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはjson形式とする
* $formatクエリオプションは無視される

<br>
### リクエスト
#### リクエストURL
##### ExtCellとの$links
```
/{Cell_name}/__ctl/Relation(Name='{relation_name}',_Box.Name='{box_name}')/$links/_ExtCell('{extcell_url}')
または、
/{Cell_name}/__ctl/Relation(Name='{relation_name}')/$links/_ExtCell('{extcell_url}')
または、
/{Cell_name}/__ctl/Relation('{relation_name}')/$links/_ExtCell('{extcell_url}')
```
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
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値} &#160;override} $: $ {value}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定します。<br>|
|X-Dc-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {TokenValue}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
|Contents-Type<br>|リクエストボディの形式を指定する<br>|application/json<br>|×<br>|省略時は[application/json]として扱う<br>|
|Accept<br>|レスポンスボディの形式を指定する<br>|application / json<br>|×<br>|省略時は[application/json]として扱う<br>|
|If-Match<br>|対象ETag値を指定する<br>|ETag値<br>|×<br>|省略時は[*]として扱う<br>|
#### リクエストボディ
#### 形式
JSON

#### 説明

|項目名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Url<br>|CellへのURL<br>|桁数：1&#65374;1024<br>URIの形式に従う<br>scheme：http, https<br>トレイリングスラッシュ(URL終端の/)必須<br>|○<br>|&#160;<br>|
#### リクエストサンプル
```json
{"uri":"https://fqdn/Cell_name/__ctl/Box('box_name')"}    
```
<br>
### レスポンス
#### ステータスコード
204

#### レスポンスヘッダ

|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|X-Dc-Version<br>|APIの実行バージョン<br>|リクエストが処理されたAPIバージョン<br>|
|Access-Control-Allow-Origin<br>|クロスドメイン通信許可ヘッダ<br>|返却値は"*"固定<br>|
|Content-Type<br>|返却されるデータの形式<br>|&#160;<br>|
|DataServiceVersion<br>|ODataのバージョン<br>|&#160;<br>|
#### ODataレスポンスヘッダ

|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|DataServiceVersion<br>|ODataのバージョン &#160;<br>|&#160;<br>|
|ETag<br>|リソースのバージョン情報 &#160;<br>|&#160;<br>|
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
curl 'https://fqdn/Cell_name/__ctl/Relation(Name=\'relation_name\',_Box.Name=\'box_name\')/$links/_Box(\'box_name\')' -X PUT -v -k \
-d '{"uri":"https://fqdn/Cell_name/__ctl/Box('update_box_name')"}' \
-H 'Authorization:Bearer auth_token' -H 'If-Match:*'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
