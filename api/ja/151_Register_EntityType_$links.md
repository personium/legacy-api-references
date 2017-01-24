# EntityType_$links登録
### 概要
EntityTypeの$links情報を登録する
### 必要な権限
alter-schema
### 制限事項
なし
<br>
### リクエスト
#### リクエストURL
```
EntityType
/{Cell_name}/{Box_name}/{odata_colleciton_path}/$metadata/EntityType(Name='{entityType_name}')/$links/_AssociationEnd
AssociationEnd
/{Cell_name}/{Box_name}/{odata_colleciton_path}/$metadata/AssociationEnd(Name='{AssociationEnd_name}', _EntityType.Name='{EntityType_name}')/$links/_AssociationEnd
```
#### メソッド
POST
#### リクエストクエリ
特になし
#### リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {TokenValue}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
|Accept<br>|レスポンスボディの形式を指定する<br>|application / json<br>|×<br>|省略時は[application/json]として扱う<br>|
#### リクエストボディ
|項目名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|uri<br>|linkするAssociationEndのuri<br>|存在するAssociationEnd<br>|○<br>| <br>|
#### リクエストサンプル
```json
{"uri": "https://fqdn/cell_name/box_name/odata_colleciton_path/$metadata/AssociationEnd(Name='AssociationEnd_Name',_EntityType.Name=null)"}
```

<br>
### レスポンス
#### ステータスコード
|コード<br>|概要<br>|備考<br>|
|:--|:--|:--|
|204<br>|正常に登録が完了した場合<br>| <br>|
|400<br>|リクエストボディがJSON形式でなかった場合 <br>必須項目が指定されていない場合<br>| <br>|
|401<br>|認証トークンが無効の場合<br>| <br>|
|403<br>|アクセス権限が不足している場合<br>| <br>|
|404<br>|存在しないCellを指定した場合<br>存在しないBoxを指定した場合<br>存在しないODataCollectionを指定した場合<br>存在しないEntityを指定した場合<br>有効値でないリクエストボディの場合<br>| <br>|
|405<br>|許可していないリクエストメソッドを指定した場合<br>| <br>|
|409<br>|既にAssociationEndのリンク情報が登録されている場合<br>| <br>|
#### レスポンスヘッダ
|項目名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|DataServiceVersion<br>|ODataProtocolのバージョン情報<br>|正常にAssociationEndが作成できた場合のみ返却する<br>動作確認済み<br>|
#### レスポンスボディ
特になし
#### エラーメッセージ一覧
なし
#### レスポンスサンプル
なし

<br>
### CURLサンプル
#### CURLコマンド(UNIX)
EntityType
```sh
curl "https://fqdn/cell_name/box_name/odata_colleciton_path/$metadata/EntityType(Name='entitytype_Name')/$links/_AssociationEnd" -X POST -i -H 'Authorization: Bearer auth_token' -H 'Accept: application/json' -H 'Accept:application/json'
-d '{"uri": "https://fqdn/cell_name/box_name/odata_colleciton_path/$metadata/AssociationEnd(Name='AssociationEnd_Name_link',_EntityType.Name=null)"}'

```

AssociationEnd
```sh
curl "https://fqdn/cell_name/box_name/odata_colleciton_path/$metadata/AssociationEnd(Name='AssociationEnd_Name2',_EntityType.Name=Entity)/$links/_AssociationEnd" -X POST -i -H 'Authorization: Bearer auth_token' -H 'Accept: application/json' -H 'Accept:application/json' -d '{"uri": "https://fqdn/cell_name/box_name/odata_colleciton_path/$metadata/AssociationEnd(Name='associationEnd_Name_link',_EntityType.Name=Entity2)"}'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
