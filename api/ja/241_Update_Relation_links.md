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
* リクエストボディはJSON形式のみ受け付ける
* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはJSON形式とする
* $formatクエリオプションは無視される


### リクエスト
#### リクエストURL
##### Correlating with Box
```
/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/$links/_Box('{BoxName}')
```
##### Correlating with ExtCell
```
/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/$links/_ExtCell('{ExtCellURL}')
```
または、
```
/{CellName}/__ctl/Relation(Name='{RelationName}')/$links/_ExtCell('{ExtCellURL}')
```
または、
```
/{CellName}/__ctl/Relation('{RelationName}')/$links/_ExtCell('{ExtCellURL}')
```
##### Linking with ExtRole
```
/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/$links/_ExtRole(ExtRole='{ExtRoleURL}',_Relation.Name='{RelationName}',_Relation._Box.Name='{BoxName}')
```
または、
```
/{CellName}/__ctl/Relation(Name='{RelationName}')/$links/_ExtRole(ExtRole='{ExtRoleURL}',_Relation.Name='{RelationName}',_Relation._Box.Name='{BoxName}')
```
または、
```
/{CellName}/__ctl/Relation('{RelationName}')/$links/_ExtRole(ExtRole='{ExtRoleURL}',_Relation.Name='{RelationName}',_Relation._Box.Name='{BoxName}')
```
または、
```
/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/$links/_ExtRole(ExtRole='{ExtRoleURL}',_Relation.Name='{RelationName}')
```
または、
```
/{CellName}/__ctl/Relation(Name='{RelationName}')/$links/_ExtRole(ExtRole='{ExtRoleURL}',_Relation.Name='{RelationName}')
```
または、
```
/{CellName}/__ctl/Relation('{RelationName}')/$links/_ExtRole(ExtRole='{ExtRoleURL}',_Relation.Name='{RelationName}')
```
または、
```
/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/$links/_ExtRole(ExtRole='{ExtRoleURL}')
```
または、
```
/{CellName}/__ctl/Relation(Name='{RelationName}')/$links/_ExtRole(ExtRole='{ExtRoleURL}')
```
または、
```
/{CellName}/__ctl/Relation('{RelationName}')/$links/_ExtRole(ExtRole='{ExtRoleURL}')
```
または、
```
/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/$links/_ExtRole('{ExtRoleURL}')
```
または、
```
/{CellName}/__ctl/Relation('{RelationName}')/$links/_ExtRole('{ExtRoleURL}')
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

|ヘッダ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|メソッドオーバーライド機能|任意|×|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用されます。|
|X-Override|ヘッダオーバライド機能|${上書きするヘッダ名}:${値}|×|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定します。|
|X-Personium-RequestKey|イベントログに出力するRequestKeyフィールドの値|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字|×|指定がない場合、PCS-${UNIX時間}を設定する|
|Authorization|OAuth2.0形式で、認証情報を指定する|Bearer {AccessToken}|×|※認証トークンは認証トークン取得APIで取得したトークン|
|Content-Type|リクエストボディの形式を指定する|application/json|×|省略時は[application/json]として扱う|
|Accept|レスポンスボディの形式を指定する|application/json|×|省略時は[application/json]として扱う|
|If-Match|対象ETag値を指定する|ETag値|×|省略時は[*]として扱う|
#### リクエストボディ
##### Format
JSON

|項目名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|uri|紐付けるODataリソースのURI|桁数：1&#65374;1024<br>URIの形式に従う<br>scheme：http / https / urn|○||

#### リクエストサンプル
```JSON
{"uri":"https://{UnitFQDN}/{CellName}/__ctl/Box('{BoxName}')"}
```

### レスポンス
#### ステータスコード
204

#### レスポンスヘッダ

|ヘッダ名|概要|備考|
|:--|:--|:--|
|X-Personium-Version|APIの実行バージョン|リクエストが処理されたAPIバージョン|
|Access-Control-Allow-Origin|クロスドメイン通信許可ヘッダ|返却値は"*"固定|
|DataServiceVersion|ODataのバージョン||
|ETag|リソースのバージョン情報||
#### レスポンスボディ
なし

#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

#### レスポンスサンプル
なし


### cURLサンプル

```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/$links/_Box('{BoxName}')" -X PUT -i -H 'If-Match:*' -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"uri":"https://{UnitFQDN}/{CellName}/__ctl/Box('update_{BoxName}')"}'
```

###### Copyright 2017 FUJITSU LIMITED
