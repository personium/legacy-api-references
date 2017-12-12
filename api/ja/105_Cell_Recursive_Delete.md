# Cell再帰的削除
### 概要
* 指定されたセル配下の関連データを全て削除する
* 削除対象のデータは下記API
	- Box
	- Account
	- Role
	- Relation
	- ExtCell
	- ExtRole
	- ReceivedMessage
	- SentMessage
	- Collection
	- AssociationEnd
	- EntityType
	- ComplexType
	- Property
	- ComplexTypeProperty
	- $links
	- ユーザデータ

### 必要な権限
ユニットユーザーのみ可能

### 制限事項
なし

### リクエスト
#### リクエストURL
```
/{CellName}
```
#### メソッド
DELETE

#### リクエストクエリ
なし

#### リクエストヘッダ
##### 共通リクエストヘッダ

|ヘッダ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|メソッドオーバーライド機能|任意|×|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用される|
|X-Override|ヘッダオーバライド機能|${上書きするヘッダ名}:${値}|×|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定する|
|X-Personium-RequestKey|イベントログに出力するRequestKeyフィールドの値|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字|×|指定がない場合、PCS-${UNIX時間}を設定する|
##### 一括削除リクエストヘッダ

|ヘッダ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|Authorization|OAuth2.0形式で、認証情報を指定する|Bearer {AccessToken}|○|※認証トークンは認証トークン取得APIで取得したトークン|
|X-Personium-Recursive|一括削除の指定|文字列|○|trueの場合は一括削除APIを実施<br>falseの場合およびヘッダの指定がなかった場合はエラーレスポンスコード412を返却|
#### リクエストボディ
なし

#### リクエストサンプル
なし


### レスポンス
#### ステータスコード
204

#### レスポンスヘッダ
|ヘッダ名|概要|備考|
|:--|:--|:--|
|Access-Control-Allow-Origin|クロスドメイン通信許可ヘッダ|返却値は"*"固定|
|X-Personium-Version|APIの実行バージョン|リクエストが処理されたAPIバージョン|
#### レスポンスボディ
なし

#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

#### レスポンスサンプル
なし

### cURLサンプル

```sh
curl "https://{UnitFQDN}/{CellName}/" -X DELETE -i -H 'f-Match: *' -H 'X-Personium-Recursive: true' -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```

###### Copyright 2017 FUJITSU LIMITED
