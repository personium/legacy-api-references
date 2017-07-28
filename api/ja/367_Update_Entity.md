# Entity更新
### 概要
ユーザデータのEntityを更新します。
### 必要な権限
write
### 制限事項
* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
* リクエストボディはjson形式のみ受け付ける
* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはjson形式とする
* $formatクエリオプションにatom または xmlを指定した場合、エラーとはならないが、レスポンスボディのデータの保証はない
* ユーザデータ制限事項
	- Edm.DateTime型のプロパティの有効範囲のチェックが適切に行われない
	- Edm.DateTime型の配列は未サポート
	- Edm.DateTime型のプロパティにSYSUTCDATETIME()を指定した場合、設定されるシステム時間が異なる場合がある
      - リクエストボディでの設定時とDefaultValueでの設定時（\__published、\__updatedは後者のタイミング）
	- 1つのEntityTypeに対して作成出来るのは、DynamicProperty・DeclaredProperty・ComplexTypeProperty合わせて400個まで

<br>
### リクエスト
#### リクエストURL
```
/{CellName}/{BoxName}/{ODataCollecitonName}/{EntityTypeName}({EntityID})}
```
|パス<br>|概要<br>|
|:--|:--|
|{CellName}<br>|セル名<br>|
|{BoxName}<br>|ボックス名<br>|
|{ODataCollecitonName}<br>|コレクション名<br>|
|{EntityTypeName}<br>|EntityType名<br>|
|{EntityID}<br>|更新するEntityのID<br>|
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
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {AccessToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
##### OData更新リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Contents-Type<br>|OAuth2.0形式で、認証情報を指定する<br>|application/json<br>|×<br>|省略時は[application/json]として扱う  <br>|
|Accept<br>|レスポンスボディの形式を指定する<br>|application/json<br>|×<br>|省略時は[application/json]として扱う  <br>|
|If-Match<br>|対象ETag値を指定する<br>|ETag値<br>|×<br>|省略時は[*]として扱う<br>|
#### リクエストボディ
##### プロパティ
スキーマ定義済みのプロパティと動的（スキーマ未定義）プロパティ、合わせて最大で400個のプロパティを設定可能  
上記にはComplexTypeで定義されているプロパティ数を含む

##### スキーマ定義済みのプロパティ
|項目名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|EntityTypeに紐づくProperty<br>|ユーザ定義項目  <br>|デフォルト値 PropertyのDefaultValueに基づく<br>|PropertyのNullableに基づく<br>| <br>|
##### スキーマ定義済みプロパティのvalueの有効値  
|データ型<br>|有効値<br>|
|:--|:--|
|文字列<br>|桁数：0&#65374;51200 byte<br>文字種: 文字列の値に制御コードを使用した場合、取得時にエスケープした状態で返却する<br>「\」を使用する場合、「\\\」で指定する必要がある<br>文字列型のプロパティに整数値、小数値、真偽値、日付型の値を設定した場合、文字列型に変換して登録される<br>|
|整数値<br>|-2147483648 &#65374; 2147483647<br>|
|小数値<br>|整数部分の桁数：1&#65374;5桁<br>小数部分の桁数：1&#65374;5桁<br>|
|真偽値<br>|true / false / null(nullを指定した場合はfalseとして扱う)<br>|
|日付<br>|/Date(【long型の時刻】)/の形式で文字列で指定する<br>【long型の時刻】の有効値は、-6847804800000(1753-01-01T00:00:00.000Z)&#65374;253402300799999(9999-12-31T23:59:59.999Z)<br>また、予約語として以下を指定可能<br>SYSUTCDATETIME()：サーバ時間<br>|
##### 動的（スキーマ未定義）プロパティ
スキーマ定義済みのプロパティと動的（スキーマ未定義）プロパティ、合わせて最大で400個のプロパティを設定可能  
上記にはComplexTypeで定義されているプロパティ数を含む
##### 動的プロパティのkeyの有効値
|データ型<br>|有効値<br>|
|:--|:--|
|文字列<br>|桁数：1&#65374;128 :<br>文字種:半角英数字と-(半角ハイフン)と_(半角アンダーバー)<br>_ただし、先頭文字に-(半角ハイフン)と_(半角アンダーバー)は指定不可  <br>_published、_updatedは、予約語であるためリクエストボディの指定は不可<br>|
##### 動的プロパティのvalueの有効値
スキーマ定義済みプロパティのvalueの有効値と同様  
配列、連想配列は指定不可

#### リクエストサンプル
```json
{"animalId": "100-2","name": "episode2","startedAt": "2016-02-21","episodeType": "care2","endedAt": "","outcome": "治療済"}
```

<br>
### レスポンス
#### ステータスコード
204
#### レスポンスヘッダ
|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|ETag<br>|リソースのバージョン情報<br>| <br>|
|DataServiceVersion<br>|ODataのバージョン<br>| <br>|
#### レスポンスボディ
なし
#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

|コード<br>|概要<br>|備考<br>|
|:--|:--|:--|
|400<br>|リクエストクエリの指定誤り<br>リクエストヘッダの指定誤り<br>リクエストボディが有効値でない場合<br>リクエストボディに最大個数より多くユーザデータを指定した場合<br>リクエストボディの__idにnullを指定した場合<br>|<br>|
|401<br>|認証トークンが無効<br>|<br>|
|403<br>|アクセス権限が不足している場合<br>|<br>|
|404<br>|存在しないCellを指定した場合<br>存在しないBoxを指定した場合<br>存在しないODataCollectionを指定した場合<br>|<br>|
|405<br>|許可していないリクエストメソッドを指定<br>|<br>|
|409<br>|既に同一のIDが作成されている場合<br>|<br>|
|412<br>|If-Matchの指定誤り<br>|<br>|

#### レスポンスサンプル
なし

<br>
### CURLサンプル

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/{EntityTypeName}('{EntityID}')" -X PUT -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"animalId": "100-2","name": "episode2","startedAt":"2016-02-21","episodeType": "care2","endedAt": "","outcome": "治療済"}'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
