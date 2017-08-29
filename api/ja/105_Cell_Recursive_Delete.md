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
<br>
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

|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用される<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定する<br>|
|X-Personium-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
##### 一括削除リクエストヘッダ

|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {AccessToken}<br>|○<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
|X-Personium-Recursive<br>|一括削除の指定<br>|文字列<br>|○<br>|trueの場合は一括削除APIを実施<br>falseの場合およびヘッダの指定がなかった場合はエラーレスポンスコード412を返却<br>|
#### リクエストボディ
なし

#### リクエストサンプル
なし

<br>
### レスポンス
#### ステータスコード
204

#### レスポンスヘッダ
|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|Access-Control-Allow-Origin<br>|クロスドメイン通信許可ヘッダ<br>|返却値は"*"固定<br>|
|X-Personium-Version<br>|APIの実行バージョン<br>|リクエストが処理されたAPIバージョン<br>|
#### レスポンスボディ
なし

#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

#### レスポンスサンプル
なし
<br>
### cURLサンプル

```sh
curl "https://{UnitFQDN}/{CellName}/" -X DELETE -i -H 'f-Match: *' -H 'X-Personium-Recursive: true' -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
