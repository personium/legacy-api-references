# ComplexTypeProperty登録
### 概要
ユーザーデータに指定するComplexTypePropertyを定義する

### 制限事項
* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
* リクエストボディはjson形式のみ受け付ける
* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはjson形式とする
* $formatクエリオプションにatom または xmlを指定した場合、エラーとはならないが、レスポンスボディのデータの保証はない
* 個別の制限
	- 関連対象のComplexTypeを使用したEntityTypeのユーザーデータが存在する場合は、Nullableがtrueの場合のみ登録可能
	- ComplexTypeの階層数が5階層以内となる場合のみ、登録可能
	- Edm.DateTimeのDefaultValueの有効範囲のチェックが適切に行われない
* personium.ioの仕様として定めている制限
	- 1つのEntityTypeに対して作成出来るのは、DynamicProperty・DeclaredProperty・ComplexTypeProperty合わせて400個まで
	- 各階層のComplexTypePropertyのタイプがSimpleTypeの最大登録個数は下記個数まで登録可能
     - 2階層目：50個
     - 3階層目：30個
     - 4階層目：10個
	- 各階層のComplexTypePropertyのタイプがComplexTypeの最大登録個数は下記個数まで登録可能
     - 1階層目：20個
     - 2階層目：5個
     - 3階層目：2個
     - 4階層目：0個

### 必要な権限
alter-schema

<br>
### リクエスト
#### リクエストURL
```
/{CellName}/{BoxName}/{OdataCollecitonPath}/$metadata/ComplexTypeProperty
```
#### メソッド
POST

#### リクエストクエリ

|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|p_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|


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

##### OData登録リクエストヘッダ

|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Content-Type<br>|リクエストボディの形式を指定する<br>|application/json<br>|×<br>|省略時は[application/json]として扱う  <br>|
|Accept  <br>|レスポンスボディの形式を指定する  <br>|application/json<br>|×<br>|省略時は[application/json]として扱う  <br>|
#### リクエストボディ
##### Format
JSON

|項目名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Name<br>|EntityType名<br>|桁数：1&#65374;128<br>文字種:半角英数字と-(半角ハイフン)と_(半角アンダーバー)<br>ただし、先頭文字に-(半角ハイフン)と_(半角アンダーバー)は指定不可<br>null<br>|○<br>| <br>|
|_ComplexType.Name<br>|紐付くComplexType名<br>|桁数：1&#65374;128<br>文字種:半角英数字と-(半角ハイフン)と_(半角アンダーバー)<br>ただし、先頭文字に-(半角ハイフン)と_(半角アンダーバー)は指定不可<br>|○<br>| <br>|
|Type<br>|型定義<br>|Edm.Boolean / Edm.String / Edm.Single / Edm.Int32 / Edm.DateTime / 登録済みComplexType名<br>|○<br>| <br>|
|Nullable<br>|Null値許可<br>|true / false<br>デフォルト値は Null<br>|×<br>| <br>|
|DefaultValue<br>|デフォルト値<br>|下記表を参照<br>デフォルト値は Null<br>|×<br>| <br>|
|CollectionKind<br>|配列種別<br>|None / List<br>デフォルト値は "None"<br>|×<br>| <br>|

##### Valid values &#8203;&#8203;for DefaultValue
DefaultValueの有効値はTypeの値（型定義）によって異なり、以下の定義となる型の異なる項目についても文字列で定義を行う  

|Type Value<br>|有効値<br>|
|:--|:--|
|Edm.String<br>|桁数：0&#65374;51200 byte<br>「\」を使用する場合、「\\」で指定する必要がある<br>|
|Edm.Int32<br>|-2147483648　&#65374;　2147483647<br>|
|Edm.Single<br>|整数部分の桁数：1&#65374;5桁<br>小数部分の桁数：1&#65374;5桁<br>|
|Edm.Boolean<br>|true / false<br>|
|Edm.DateTime<br>|/Date(【long型の時刻】)/の形式で文字列で指定する<br>　【long型の時刻】の有効値は、-6847804800000(1753-01-01T00:00:00.000Z)&#65374;253402300799999(9999-12-31T23:59:59.999Z)<br>また、予約語として以下を指定可能<br>　SYSUTCDATETIME()：サーバ時間<br>|

#### リクエストサンプル
```json
{
   "Name": "PostalCode",
  "_ComplexType.Name": "Address",
  "Type": "Edm.String",
  "Nullable": true,
  "DefaultValue": null,
  "CollectionKind": "None"  
}
```
<br>
### レスポンス
#### ステータスコード
201

#### レスポンスヘッダ
##### 共通レスポンスヘッダ

|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|X-Personium-Version<br>|APIの実行バージョン<br>|リクエストが処理されたAPIバージョン<br>|
|Access-Control-Allow-Origin<br>|クロスドメイン通信許可ヘッダ<br>|返却値は"*"固定<br>|

##### ODataレスポンスヘッダ

|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|Content-Type<br>|返却されるデータの形式<br>| <br>
|Location<br>|作成したリソースへのURL<br>| <br>
|ETag<br>|リソースのバージョン情報<br>| <br>
|DataServiceVersion<br>|ODataのバージョン<br>| <br>

#### レスポンスボディ
##### 共通レスポンスボディ

レスポンスはJSONオブジェクトで、オブジェクト（サブオブジェクト）に定義されるキー(名前)と型、並びに値の対応は以下のとおりです。

|オブジェクト<br>|名前（キー）<br>|型<br>|値<br>|
|:--|:--|:--|:--|
|ルート<br>|d<br>|object<br>|オブジェクト{1}<br>|
|{1}<br>|__count<br>|string<br>|$inlinecountクエリでの取得結果件数<br>|
|{1}<br>|results<br>|array<br>|オブジェクト{2}の配列<br>|
|{2}<br>|__published<br>|string<br>|作成日(UNIX時間)<br>|
|{2}<br>|__updated<br>|string<br>|更新日(UNIX時間)<br>|
|{2}<br>|__metadata<br>|object<br>|オブジェクト{3}<br>|
|{3}<br>|etag<br>|string<br>|Etag値<br>|
|{3}<br>|uri<br>|string<br>|作成したリソースへのURL<br>|

##### Property固有レスポンスボディ

|オブジェクト<br>|名前（キー）<br>|型<br>|値<br>|
|:--|:--|:--|:--|
|{3}<br>|type<br>|string<br>|ODataSvcSchema.AssociationEnd<br>|
|{2}<br>|Name<br>|string<br>|Property名<br>|
|{2}<br>|_EntityType.Name<br>|string<br>|紐付くEntityType名<br>|
|{2}<br>|Type<br>|string<br>|型定義<br>|
|{2}<br>|Nullable<br>|boolean<br>|Null値許可<br>|
|{2}<br>|DefaultValue<br>|string<br>|デフォルト値<br>|
|{2}<br>|CollectionKind<br>|string<br>|配列種別<br>|

#### エラーメッセージ一覧
[エラーメッセージ一覧](200_Error_Messages.html)を参照

|コード<br>|メッセージ<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|
|400<br>|Bad Request<br>|リクエストボディの形式が不正 <br>リクエストヘッダの形式が不正<br>既に別のスキーマ型のIndexが作成されている場合<br>| <br>|
|401<br>|Unauthorized<br>|認証トークンが無効<br>| <br>|
|403<br>|Forbidden<br>|アクセス権限が不足している場合<br>| <br>|
|404<br>|Not Found<br>|存在しないリソースを指定<br>| <br>|
|405<br>|Method Not Allowed<br>|許可していないリクエストメソッドを指定<br>| <br>|
|409<br>|Conflict<br>|ODataコレクションに同じ"Name"と"_ComplexType.Name"のComplexTypePropertyが存在している場合<br>| <br>|
|412<br>|Precondition Failed<br>|存在しないバージョンを指定<br>| <br>|

#### レスポンスサンプル
```json
{
   "d": {
    "results": {
      "_ComplexType": {
        "__deferred": {
          "uri": "https://{UnitFQDN}/{CellName}/{BoxName}/{OdataCollecitonPath}/$metadata/ComplexTypeProperty(Name='PostalCode',_ComplexType.Name='Address')/_ComplexType"
        }
      },
      "Name": "PostalCode",
      "_ComplexType.Name": "Address",
      "Type": "Edm.String",
      "Nullable": true,
      "DefaultValue": null,
      "CollectionKind": "None",
      "__published": "/Date(1349434504818)/",
      "__updated": "/Date(1349434504818)/",
      "__metadata": {
        "etag": "1-1349434504818",
        "type": "ODataSvcSchema.ComplexTypeProperty",
        "uri": "https://{UnitFQDN}/{CellName}/{BoxName}/{OdataCollecitonPath}/$metadata/ComplexTypeProperty(Name='PostalCode',_ComplexType.Name='Address')"
      }
    }
  }
}
```

<br>
### CURLサンプル

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{OdataCollecitonPath}/$metadata/ComplexTypeProperty" -X POST -i  -H 'Authorization: Bearer {UnitUserToken}' -H 'Accept: application/json' -d '{
  "Name": "PostalCode",
  "_ComplexType.Name": "Address",
  "Type": "Edm.String",
  "Nullable": true,
  "DefaultValue": null,
  "CollectionKind": "None"  
}'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
