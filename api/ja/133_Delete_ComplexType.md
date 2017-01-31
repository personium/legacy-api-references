# ComplexType削除
### 概要
既存のComplexType情報を削除する
PropertyまたはComplexTypePropertyのTypeとして使用されている場合は削除できない

### 必要な権限
alter-schema

### 制限事項
* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
* リクエストボディはjson形式のみ受け付ける
* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはjson形式とする
* $formatクエリオプションにatom または xmlを指定した場合、エラーとはならないが、レスポンスボディのデータの保証はない
* 削除対象に関係性設定が存在する場合、削除は実行されません。

<br>
### リクエスト
#### リクエストURL
```
/{CellName}/{BoxName}/{OdataCollecitonPath}/$metadata/ComplexType('{ComplextypeName}')
```
#### メソッド
DELETE

#### リクエストクエリ
##### 共通リクエストクエリ

|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|dc_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|

#### リクエストヘッダ
##### 共通リクエストヘッダ

|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用される<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定する<br>|
|X-Dc-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|

##### OData共通リクエストヘッダ

|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {UnitUserToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|

##### OData削除リクエストヘッダ

|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|If-Match<br>|対象ETag値を指定する<br>|ETag値<br>|×<br>|省略時は[*]として扱う  <br>|
#### リクエストボディ
なし

#### リクエストサンプル
なし

<br>
### レスポンス
#### ステータスコード
204

#### レスポンスヘッダ
なし

#### レスポンスボディ
なし

#### レスポンスサンプル
なし

#### エラーメッセージ一覧
[エラーメッセージ一覧](200_Error_Messages.html)を参照

<br>
### CURLサンプル
#### CURLコマンド(UNIX)
```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{OdataCollecitonPath}/$metadata/complextype('Address')" -X DELETE -i -H 'Authorization: Bearer {UnitUserToken}' -H 'Accept: application/json'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
