# Extcell_$links削除
## 概要
ExtCellとの$links情報を削除する

### 制限事項
* リクエストヘッダのAcceptは無視される
* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
* リクエストボディはJSON形式のみ受け付ける
* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはJSON形式とする
* $formatクエリオプションは無視される


## リクエスト
### リクエストURL
#### Correlating with the role
```
/{CellName}/__ctl/ExtCell(Url='{ExtCellURL}')/$links/_Role(Name='{RoleName}',_Box.Name='{BoxName}')
```
または、
```
/{CellName}/__ctl/ExtCell(Url='{ExtCellURL}')/$links/_Role(Name='{RoleName}')
```
または、
```
/{CellName}/__ctl/ExtCell(Url='{ExtCellURL}')/$links/_Role('{RoleName}')
```
または、
```
/{CellName}/__ctl/ExtCell('{ExtCellURL}')/$links/_Role(Name='{RoleName}',_Box.Name='{BoxName}')
```
または、
```
/{CellName}/__ctl/ExtCell('{ExtCellURL}')/$links/_Role(Name='{RoleName}')
```
または、
```
/{CellName}/__ctl/ExtCell('{ExtCellURL}')/$links/_Role('{RoleName}')
```
#### Correlating with the relation
```
/{CellName}/__ctl/ExtCell(Url='{ExtCellURL}')/$links/_Relation(Name='{RelationName}'
,_Box.Name='{BoxName}')
```
または、
```
/{CellName}/__ctl/ExtCell(Url='{ExtCellURL}')/$links/_Relation(Name='{RelationName}')
```
または、
```
/{CellName}/__ctl/ExtCell(Url='{ExtCellURL}')/$links/_Relation('{RelationName}')
```
または、
```
/{CellName}/__ctl/ExtCell('{ExtCellURL}')/$links/_Relation(Name='{RelationName}'
,_Box.Name='{BoxName}')
```
または、
```
/{CellName}/__ctl/ExtCell('{ExtCellURL}')/$links/_Relation(Name='{RelationName}')
```
または、
```
/{CellName}/__ctl/ExtCell('{ExtCellURL}')/$links/_Relation('{RelationName}')
```
※ \_Box.Nameパラメタを省略した場合は、nullが指定されたものとする

### メソッド
DELETE
### リクエストクエリ
|クエリ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|p_cookie_peer|クッキー認証値|認証時にサーバから返却されたクッキー認証値|×|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する|
### リクエストヘッダ
|ヘッダ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|メソッドオーバーライド機能|任意|×|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用されます。|
|X-Override|ヘッダオーバライド機能|${上書きするヘッダ名}:${値} &#160;override} $: $ {value}|×|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定します。|
|X-Personium-RequestKey|イベントログに出力するRequestKeyフィールドの値|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字|×|指定がない場合、PCS-${UNIX時間}を設定する|
|Authorization|OAuth2.0形式で、認証情報を指定する|Bearer {AccessToken}|×|※認証トークンは認証トークン取得APIで取得したトークン|
|If-Match|対象ETag値を指定する|ETag値|×|省略時は[*]として扱う|
### リクエストボディ
なし

## レスポンス
### ステータスコード
204
### レスポンスヘッダ
|ヘッダ名|概要|備考|
|:--|:--|:--|
|DataServiceVersion|ODataのバージョン||
|Access-Control-Allow-Origin|クロスドメイン通信許可ヘッダ|返却値は"*"固定|
|X-Personium-Version|APIの実行バージョン|リクエストが処理されたAPIバージョン|
### レスポンスボディ
なし
### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.md)を参照

## cURLサンプル

```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/ExtCell('https%3A%2F%2F{UnitFQDN}%2F{ExtCellName}%2F')\
/\$links/_Relation(Name='{RelationName}',_Box.Name='{BoxName}')" -X DELETE -i -H \
'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```

