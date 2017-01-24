# Relation_$links更新
### 概要
Relationに紐付いたODataリソースを更新する<br>以下のODataリソースを指定できる
* Box
* ExtCell
* ExtRole
* Role

### 必要な権限
なし

### 制限事項
* リクエストヘッダのAcceptは無視される
* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
* リクエストボディはjson形式のみ受け付ける
* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはjson形式とする
* $formatクエリオプションは無視される

<br>
### リクエスト
#### リクエストURL
##### Correlating with Box
```
/{Cell_name}/__ctl/Relation(Name='{relation_name}',_Box.Name='{box_name}')/$links/_Box('{box_name}')
```
##### Correlating with ExtCell
```
/{Cell_name}/__ctl/Relation(Name='{relation_name}',_Box.Name='{box_name}')/$links/_ExtCell('{extcell_url}')
または、
/{Cell_name}/__ctl/Relation(Name='{relation_name}')/$links/_ExtCell('{extcell_url}')
または、
/{Cell_name}/__ctl/Relation('{relation_name}')/$links/_ExtCell('{extcell_url}')
```
##### Linking with ExtRole
```
/{Cell_name}/__ctl/Relation(Name='{relation_name}',_Box.Name='{box_name}')/$links/_ExtRole(ExtRole='{extrole_url}',_Relation.Name='{relation_name}',_Relation._Box.Name='{box_name}')
または、
/{Cell_name}/__ctl/Relation(Name='{relation_name}')/$links/_ExtRole(ExtRole='{extrole_url}',_Relation.Name='{relation_name}',_Relation._Box.Name='{box_name}')
または、
/{Cell_name}/__ctl/Relation('{relation_name}')/$links/_ExtRole(ExtRole='{extrole_url}',_Relation.Name='{relation_name}',_Relation._Box.Name='{box_name}')
または、
/{Cell_name}/__ctl/Relation(Name='{relation_name}',_Box.Name='{box_name}')/$links/_ExtRole(ExtRole='{extrole_url}',_Relation.Name='{relation_name}')
または、
/{Cell_name}/__ctl/Relation(Name='{relation_name}')/$links/_ExtRole(ExtRole='{extrole_url}',_Relation.Name='{relation_name}')
または、
/{Cell_name}/__ctl/Relation('{relation_name}')/$links/_ExtRole(ExtRole='{extrole_url}',_Relation.Name='{relation_name}')
または、
/{Cell_name}/__ctl/Relation(Name='{relation_name}',_Box.Name='{box_name}')/$links/_ExtRole(ExtRole='{extrole_url}')
または、
/{Cell_name}/__ctl/Relation(Name='{relation_name}')/$links/_ExtRole(ExtRole='{extrole_url}')
または、
/{Cell_name}/__ctl/Relation('{relation_name}')/$links/_ExtRole(ExtRole='{extrole_url}')
または、
/{Cell_name}/__ctl/Relation(Name='{relation_name}',_Box.Name='{box_name}')/$links/_ExtRole('{extrole_url}')
または、
/{Cell_name}/__ctl/Relation('{relation_name}')/$links/_ExtRole('{extrole_url}')
```
##### Correlating with the Role
```
/{Cell_name}/__ctl/Relation(Name='{relation_name}',_Box.Name='{box_name}')/$links/_Role(Name='{role_name}',_Box.Name='{box_name}')
または、
/{Cell_name}/__ctl/Relation(Name='{relation_name}')/$links/_Role(Name='{role_name}',_Box.Name='{box_name}')
または、
/{Cell_name}/__ctl/Relation('{relation_name}')/$links/_Role(Name='{role_name}',_Box.Name='{box_name}')
または、
/{Cell_name}/__ctl/Relation(Name='{relation_name}',_Box.Name='{box_name}')/$links/_Role(Name='{role_name}')
または、
/{Cell_name}/__ctl/Relation(Name='{relation_name}')/$links/_Role(Name='{role_name}')
または、
/{Cell_name}/__ctl/Relation('{relation_name}')/$links/_Role(Name='{role_name}')
または、
/{Cell_name}/__ctl/Relation(Name='{relation_name}',_Box.Name='{box_name}')/$links/_Role('{role_name}')
または、
/{Cell_name}/__ctl/Relation(Name='{relation_name}')/$links/_Role('{role_name}')
または、
/{Cell_name}/__ctl/Relation('{relation_name}')/$links/_Role('{role_name}')
```
※ _Box.Name, _Relation.Name, _Relation._Box.Nameパラメタを省略した場合は、nullが指定されたものとする
※ ExtCellのキーはURLエンコードした文字列を指定する。

#### メソッド
PUT

#### リクエストクエリ
なし

#### リクエストヘッダ

|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用されます。<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定します。<br>|
|X-Dc-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {TokenValue}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
|Content-Type<br>|リクエストボディの形式を指定する<br>|application / json<br>|×<br>|省略時は[application/json]として扱う<br>|
|Accept<br>|レスポンスボディの形式を指定する<br>|application / json<br>|×<br>|省略時は[application/json]として扱う<br>|
|If-Match<br>|対象ETag値を指定する<br>|ETag値<br>|×<br>|省略時は[*]として扱う<br>|
#### リクエストボディ
##### Format
JSON

|項目名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|uri<br>|紐付けるODataリソースのURI<br>|桁数：1&#65374;1024<br>URIの形式に従う<br>scheme：http / https / urn<br>|○<br>|&#160;<br>|

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
|DataServiceVersion<br>|ODataのバージョン<br>|&#160;<br>|
|ETag<br>|リソースのバージョン情報<br>|&#160;<br>|
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
curl "https://fqdn/Cell_name/__ctl/Relation(Name='relation_name',_Box.Name='box_name')/$links/_Box('box_name')" -X PUT -H 'If-Match:*' -H 'Authorization:Bearer auth_token' -H 'Accept: application/json'
-d '{"uri":"https://fqdn/Cell_name/__ctl/Box('update_box_name')"}' ```
###### Copyright 2017    FUJITSU LIMITED
