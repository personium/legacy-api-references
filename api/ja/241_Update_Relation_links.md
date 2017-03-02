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
/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/$links/_Box('{BoxName}')
```
##### Correlating with ExtCell
```
/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/$links/_ExtCell('{extcell_url}')
```
または、
```
/{CellName}/__ctl/Relation(Name='{RelationName}')/$links/_ExtCell('{extcell_url}')
```
または、
```
/{CellName}/__ctl/Relation('{RelationName}')/$links/_ExtCell('{extcell_url}')
```
##### Linking with ExtRole
```
/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/$links/_ExtRole(ExtRole='{extrole_url}',_Relation.Name='{RelationName}',_Relation._Box.Name='{BoxName}')
```
または、
```
/{CellName}/__ctl/Relation(Name='{RelationName}')/$links/_ExtRole(ExtRole='{extrole_url}',_Relation.Name='{RelationName}',_Relation._Box.Name='{BoxName}')
```
または、
```
/{CellName}/__ctl/Relation('{RelationName}')/$links/_ExtRole(ExtRole='{extrole_url}',_Relation.Name='{RelationName}',_Relation._Box.Name='{BoxName}')
```
または、
```
/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/$links/_ExtRole(ExtRole='{extrole_url}',_Relation.Name='{RelationName}')
```
または、
```
/{CellName}/__ctl/Relation(Name='{RelationName}')/$links/_ExtRole(ExtRole='{extrole_url}',_Relation.Name='{RelationName}')
```
または、
```
/{CellName}/__ctl/Relation('{RelationName}')/$links/_ExtRole(ExtRole='{extrole_url}',_Relation.Name='{RelationName}')
```
または、
```
/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/$links/_ExtRole(ExtRole='{extrole_url}')
```
または、
```
/{CellName}/__ctl/Relation(Name='{RelationName}')/$links/_ExtRole(ExtRole='{extrole_url}')
```
または、
```
/{CellName}/__ctl/Relation('{RelationName}')/$links/_ExtRole(ExtRole='{extrole_url}')
```
または、
```
/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/$links/_ExtRole('{extrole_url}')
```
または、
```
/{CellName}/__ctl/Relation('{RelationName}')/$links/_ExtRole('{extrole_url}')
```
##### Correlating with the Role
```
/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/$links/_Role(Name='{RoleName}',_Box.Name='{BoxName}')
```
または、
```
/{CellName}/__ctl/Relation(Name='{RelationName}')/$links/_Role(Name='{RoleName}',_Box.Name='{BoxName}')
```
または、
```
/{CellName}/__ctl/Relation('{RelationName}')/$links/_Role(Name='{RoleName}',_Box.Name='{BoxName}')
```
または、
```
/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/$links/_Role(Name='{RoleName}')
```
または、
```
/{CellName}/__ctl/Relation(Name='{RelationName}')/$links/_Role(Name='{RoleName}')
```
または、
```
/{CellName}/__ctl/Relation('{RelationName}')/$links/_Role(Name='{RoleName}')
```
または、
```
/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/$links/_Role('{RoleName}')
```
または、
```
/{CellName}/__ctl/Relation(Name='{RelationName}')/$links/_Role('{RoleName}')
```
または、
```
/{CellName}/__ctl/Relation('{RelationName}')/$links/_Role('{RoleName}')
```
※ \_Box.Name, \_Relation.Name, \_Relation.\_Box.Nameパラメタを省略した場合は、nullが指定されたものとする  
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
|X-Personium-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {UnitUserToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
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
{"uri":"https://{UnitFQDN}/{CellName}/__ctl/Box('{BoxName}')"}
```
<br>
### レスポンス
#### ステータスコード
204

#### レスポンスヘッダ

|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|X-Personium-Version<br>|APIの実行バージョン<br>|リクエストが処理されたAPIバージョン<br>|
|Access-Control-Allow-Origin<br>|クロスドメイン通信許可ヘッダ<br>|返却値は"*"固定<br>|
|DataServiceVersion<br>|ODataのバージョン<br>|&#160;<br>|
|ETag<br>|リソースのバージョン情報<br>|&#160;<br>|
#### レスポンスボディ
なし

#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

#### レスポンスサンプル
なし

<br>
### CURLサンプル

```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/$links/_Box('{BoxName}')" -X PUT -i -H 'If-Match:*' -H 'Authorization: Bearer {UnitUserToken}' -H 'Accept: application/json' -d '{"uri":"https://{UnitFQDN}/{CellName}/__ctl/Box('update_{BoxName}')"}'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
