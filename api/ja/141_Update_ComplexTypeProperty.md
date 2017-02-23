# ComplexTypeProrerty更新
### 概要
既存のComplexTypeProrerty情報を更新する

### 必要な権限
alter-schema

### 制限事項
* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
* リクエストボディはjson形式のみ受け付ける
* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはjson形式とする
* $formatクエリオプションにatom または xmlを指定した場合、エラーとはならないが、レスポンスボディのデータの保証はない

<br>
### リクエスト
#### リクエストURL
```
/{CellName}/{BoxName}/{OdataCollecitonPath}/$metadata/ComplexTypeProrerty('ComplexTypeProrerty')
```
#### メソッド
PUT

#### リクエストクエリ
##### 共通リクエストクエリ

|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|p_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|

##### OData 共通リクエストクエリ
なし

#### リクエストヘッダ
##### 共通リクエストヘッダ

|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用される<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定する<br>|
|X-Personium-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|

##### OData共通リクエストヘッダ

|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {UnitUserToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|

##### OData更新リクエストヘッダ

|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Content-Type<br>|リクエストボディの形式を指定する<br>|application / json<br>|×<br>|省略時は[application/json]として扱う<br>|
|Accept<br>|レスポンスボディの形式を指定する<br>|application / json<br>|×<br>|省略時は[application/json]として扱う<br>|
|If-Match<br>|対象ETag値を指定する<br>|ETag値<br>|×<br>|省略時は[*]として扱う<br>|

#### リクエストボディ
##### Format
JSON

|項目名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Name<br>|ComplexTypeProrerty名<br>|桁数：1&#65374;128<br>文字種:半角英数字と-(半角ハイフン)と_(半角アンダーバー)<br>ただし、先頭文字に-(半角ハイフン)と_(半角アンダーバー)は指定不可<br>null<br>|○<br>| <br>|

#### リクエストサンプル
```json
{"Name":"animal"}   
```
<br>
### レスポンス
#### ステータスコード
204
#### レスポンスヘッダ
なし
#### レスポンスボディ
なし
#### エラーメッセージ一覧
[エラーメッセージ一覧](200_Error_Messages.html)を参照

|コード<br>|メッセージ<br>|概要<br>|
|:--|:--|:--|
|400<br>|Bad Request<br>|リクエストボディの形式が不正 <br>リクエストヘッダの形式が不正<br>|
|401<br>|Unauthorized<br>|認証トークンが無効<br>|
|403<br>|Forbidden<br>|アクセス権限が不足している場合<br>|
|404<br>|Not Found<br>|存在しないリソースを指定<br>|
|405<br>|Method Not Allowed<br>|許可していないリクエストメソッドを指定<br>|
|409<br>|Conflict<br>|ODataコレクションに同じ"Name"のComplexTypeProrertyが存在している場合<br>|
|412<br>|Precondition Failed<br>|存在しないバージョンを指定<br>|

#### レスポンスサンプル
なし

<br>
### CURLサンプル

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{OdataCollecitonPath}/$metadata/ComplexTypeProrerty('animal')" -X PUT -i -H 'If-Match: *' -H 'Authorization: Bearer {UnitUserToken}' -H 'Accept: application/json' -d '{"Name":"ComplexTypeProrerty"}'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
