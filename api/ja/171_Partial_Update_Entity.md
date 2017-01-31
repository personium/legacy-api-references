﻿﻿# Entity部分更新
### 概要
ユーザデータのEntityを部分更新します
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
/{CellName}/{BoxName}/{OdataCollecitonPath}/{EntitySet}({KeyPredicate})
```
|パス<br>|概要<br>|
|:--|:--|
|{CellName}<br>|セル名<br>|
|{BoxName}<br>|ボックス名<br>|
|{OdataCollecitonPath}<br>|コレクション名<br>|
|{EntitySet}<br>|EntitySet名<br>|
|KeyPredicate<br>|更新するEntityのID<br>|
#### メソッド
MERGE
#### リクエストクエリ
##### 共通リクエストクエリ
|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|dc_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|
#### リクエストヘッダ
##### 共通リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-Dc-Version<br>|APIの実行バージョン<br>|有効なバージョン<br>|×<br>|指定がない場合、最新のAPIバージョンが指定される<br>|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用される<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定する<br>|
|X-Dc-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
##### OData共通リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {UnitUserToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
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

|項目名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|__id<br>|EntityのID<br>|桁数：1&#65374;200<br>文字種:半角英数字と-(半角ハイフン)と_(半角アンダーバー)と:(半角コロン)<br>,ただし、先頭文字に-(半角ハイフン)と_(半角アンダーバー)と:(半角コロン)は指定不可<br>|×<br>|指定しない場合ユニークなIDが割り当てられます<br>有効値のチェック未対応<br>|
###### スキーマ定義済みのプロパティ
|項目名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|EntityTypeに紐づくProperty<br>|ユーザ定義項目<br>|デフォルト値 PropertyのDefaultValueに基づく<br>|PropertyのNullableに基づく<br>|<br>|
<!--###### スキーマ定義済みプロパティのvalueの有効値-->
###### 動的（スキーマ未定義）プロパティ
|データ型<br>|有効値<br>|
|:--|:--|
|文字列<br>|桁数：0&#65374;51200 byte<br>文字種: 文字列の値に制御コードを使用した場合、取得時にエスケープした状態で返却する<br>「\」を使用する場合、「\\\」で指定する必要がある<br>文字列型のプロパティに整数値、小数値、真偽値、日付型の値を設定した場合、文字列型に変換して登録される<br>|
|整数値<br>|-2147483648 &#65374; 2147483647<br>|
|小数値<br>|整数部分の桁数：1&#65374;5桁<br>小数部分の桁数：1&#65374;5桁<br>|
|真偽値<br>|true / false / null(nullを指定した場合はfalseとして扱う)<br>|
|日付<br>|/Date(【long型の時刻】)/の形式で文字列で指定する<br>【long型の時刻】の有効値は、-6847804800000(1753-01-01T00:00:00.000Z)&#65374;253402300799999(9999-12-31T23:59:59.999Z)<br>また、予約語として以下を指定可能<br>SYSUTCDATETIME()：サーバ時間<br>|
スキーマ定義を行わなくても動的にプロパティを設定することが可能
登録可能なデータ型は「文字列」「数値」「真偽値」「null」のみ設定可能
###### 動的プロパティのkeyの有効値
|データ型<br>|有効値<br>|
|:--|:--|
|文字列<br>|桁数：1&#65374;128 :<br>文字種:半角英数字と-(半角ハイフン)と_(半角アンダーバー)<br>_ただし、先頭文字に-(半角ハイフン)と_(半角アンダーバー)は指定不可  <br>_published、_updatedは、予約語であるためリクエストボディの指定は不可<br>|
###### 動的プロパティのvalueの有効値
スキーマ定義済みプロパティのvalueの有効値と同様
配列、連想配列は指定不可
* リクエストボディに__idを指定した場合、無視される
	- 存在しないプロパティを指定した場合、そのプロパティは追加プロパティとして更新される
	- 指定されていない項目の更新は行わない(元の値を維持する)

※下記は未実装で現状指定しない項目はDefaultValueの値となる
* ComplexType内の項目を指定した場合、指定した項目のみ更新される
	- 指定されていない項目の更新は行わない(元の値を維持する)

#### リクエストサンプル
なし

<br>
### レスポンス
#### ステータスコード
204
#### レスポンスヘッダ
##### ODataレスポンスヘッダ
|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|DataServiceVersion<br>|ODataのバージョン<br>| <br>|
|ETag<br>|リソースのバージョン情報<br>| <br>|
#### レスポンスボディ
なし
#### エラーメッセージ一覧
[エラーメッセージ一覧](200_Error_Messages.html)を参照
#### レスポンスサンプル
なし

<br>
### CURLサンプル
#### CURLコマンド(UNIX)
```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{OdataCollecitonPath}/parent('100-1_20101108-111352093')" -X MERGE -i -H 'If-Match:*' -H 'Authorization: Bearer {UnitUserToken}' -H 'Accept: application/json' -d '{"name": "episode","outcome": "治療後"}'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
