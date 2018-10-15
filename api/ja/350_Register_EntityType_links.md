# EntityTypeと他オブジェクトとのリンク
## 概要
EntityTypeの$links情報を登録する
### 必要な権限
alter-schema

## リクエスト
### リクエストURL
```
EntityType
{CellURL}{BoxName}/{OdataCollecitonPath}/$metadata/EntityType(Name='{EntityTypeName}')
/$links/_AssociationEnd
AssociationEnd
{CellURL}{BoxName}/{OdataCollecitonPath}/$metadata/AssociationEnd(Name='{AssociationEndName}', 
_EntityType.Name='{EntityTypeName}')/$links/_AssociationEnd
```
### メソッド
POST
### リクエストクエリ
特になし
### リクエストヘッダ
|ヘッダ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|Authorization|OAuth2.0形式で、認証情報を指定する|Bearer {AccessToken}|×|※認証トークンは認証トークン取得APIで取得したトークン|
|Accept|レスポンスボディの形式を指定する|application/json|×|省略時は[application/json]として扱う|
### リクエストボディ
|項目名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|uri|linkするAssociationEndのuri|存在するAssociationEnd|○||
### リクエストサンプル
```JSON
{"uri": "{CellURL}{BoxName}/{OdataCollecitonPath}/$metadata
/AssociationEnd(Name='{AssociationEndName}',_EntityType.Name=null)"}
```


## レスポンス
### ステータスコード
204

### レスポンスヘッダ
|項目名|概要|備考|
|:--|:--|:--|
|DataServiceVersion|ODataProtocolのバージョン情報|正常にAssociationEndが作成できた場合のみ返却する<br>動作確認済み|
### レスポンスボディ
特になし
### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.md)を参照


## cURLサンプル

EntityType
```sh
curl "{CellURL}{BoxName}/{OdataCollecitonPath}/$metadata/EntityType(Name='\
{EntityTypeName}')/$links/_AssociationEnd" -X POST -i -H 'Authorization: Bearer {AccessToken}' -H \
'Accept: application/json' -H 'Accept:application/json' -d '{"uri": \
"{CellURL}{BoxName}/{OdataCollecitonPath}/$metadata/AssociationEnd(Name=\
'{AssociationEndName}_link',_EntityType.Name=null)"}'

```

AssociationEnd
```sh
curl "{CellURL}{BoxName}/{OdataCollecitonPath}/$metadata/AssociationEnd(Name=\
'{AssociationEndName}2',_EntityType.Name=Entity)/$links/_AssociationEnd" -X POST -i -H 'Authorization\
: Bearer {AccessToken}' -H 'Accept: application/json' -H 'Accept:application/json' -d '{"uri": \
"{CellURL}{BoxName}/{OdataCollecitonPath}/$metadata/AssociationEnd(Name=\
'{AssociationEndName}_link',_EntityType.Name=Entity2)"}'
```

