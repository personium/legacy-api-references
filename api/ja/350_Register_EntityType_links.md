# EntityType_$links登録
### 概要
EntityTypeの$links情報を登録する
### 必要な権限
alter-Schema
### 制限事項
なし
<br>
### リクエスト
#### リクエストURL
```
EntityType
/{CellName}/{BoxName}/{OdataCollecitonPath}/$metadata/EntityType(Name='{EntityTypeName}')/$links/_AssociationEnd
AssociationEnd
/{CellName}/{BoxName}/{OdataCollecitonPath}/$metadata/AssociationEnd(Name='{AssociationEndName}', _EntityType.Name='{EntityTypeName}')/$links/_AssociationEnd
```
#### メソッド
POST
#### リクエストクエリ
特になし
#### リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {AccessToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
|Accept<br>|レスポンスボディの形式を指定する<br>|application / JSON<br>|×<br>|省略時は[application/json]として扱う<br>|
#### リクエストボディ
|項目名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|uri<br>|linkするAssociationEndのuri<br>|存在するAssociationEnd<br>|○<br>| <br>|
#### リクエストサンプル
```JSON
{"uri": "https://{UnitFQDN}/{CellName}/{BoxName}/{OdataCollecitonPath}/$metadata/AssociationEnd(Name='{AssociationEndName}',_EntityType.Name=null)"}
```

<br>
### レスポンス
#### ステータスコード
204

#### レスポンスヘッダ
|項目名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|DataServiceVersion<br>|ODataProtocolのバージョン情報<br>|正常にAssociationEndが作成できた場合のみ返却する<br>動作確認済み<br>|
#### レスポンスボディ
特になし
#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

#### レスポンスサンプル
なし

<br>
### cURLサンプル

EntityType
```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{OdataCollecitonPath}/$metadata/EntityType(Name='{EntityTypeName}')/$links/_AssociationEnd" -X POST -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -H 'Accept:application/json'
-d '{"uri": "https://{UnitFQDN}/{CellName}/{BoxName}/{OdataCollecitonPath}/$metadata/AssociationEnd(Name='{AssociationEndName}_link',_EntityType.Name=null)"}'

```

AssociationEnd
```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{OdataCollecitonPath}/$metadata/AssociationEnd(Name='{AssociationEndName}2',_EntityType.Name=Entity)/$links/_AssociationEnd" -X POST -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -H 'Accept:application/json' -d '{"uri": "https://{UnitFQDN}/{CellName}/{BoxName}/{OdataCollecitonPath}/$metadata/AssociationEnd(Name='{AssociationEndName}_link',_EntityType.Name=Entity2)"}'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
