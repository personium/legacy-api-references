# Entity取得
### 概要
ユーザデータのEntityを一件取得します。
### 必要な権限
read
### 制限事項
* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
* リクエストボディはjson形式のみ受け付ける
* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはjson形式とする
* $formatクエリオプションにatom または xmlを指定した場合、エラーとはならないが、レスポンスボディのデータの保証はない
  - ユーザデータ制限事項
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
|{EntityID}<br>|取得するEntityのID<br>|
#### メソッド
GET
#### リクエストクエリ
以下のクエリパラメタが利用可能です。

|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|p_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効クッキーの認証情報を利用する場合に指定する<br>|

[$select クエリ](194_$Select_Query.html)

[$expand クエリ](193_$Expand_Query.html)

[$format クエリ](192_$Format_Query.html)

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
##### OData取得リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Accept  <br>|レスポンスボディの形式を指定する  <br>|application/json<br>|×<br>|省略時は[application/json]として扱う  <br>|
|If-None-Match<br>|ETagの値を指定し、変更がない場合は304、変更されている場合は最新リソースを返却する  <br>| <br>|×<br>|ETagに一致しないEntityを取得する場合に指定<br>未対応<br>|
#### リクエストボディ
なし
#### リクエストサンプル
なし

<br>
### レスポンス
#### ステータスコード
200

#### レスポンスヘッダ
|項目名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|Content-Type<br>|返却されるデータの形式<br>|<br>|
|DataServiceVersion<br>|ODataのバージョン情報<br>|正常にEntityが取得できた場合のみ返却する<br>|
#### レスポンスボディ
レスポンスはJSONオブジェクトで、オブジェクト（サブオブジェクト）に定義されるキー(名前)と型、並びに値の対応は以下のとおり

|オブジェクト<br>|名前（キー）<br>|型<br>|値<br>|
|:--|:--|:--|:--|
|ルート<br>|d<br>|object<br>|オブジェクト{1}<br>|
|{1}<br>|results<br>|array<br>|オブジェクト{2}の配列<br>|
|{2}<br>|__metadata<br>|object<br>|オブジェクト{3}<br>|
|{3}<br>|uri<br>|string<br>|作成したリソースへのURL<br>|
|{3}<br>|etag<br>|string<br>|Etag値<br>|
|{3}<br>|type<br>|string<br>|UserData.{EntityTypeName}<br>|
|{2}<br>|__id<br>|string<br>|EntityのID(__id)<br>|
|{2}<br>|__published<br>|string<br>|作成日(UNIX時間)<br>|
|{2}<br>|__updated<br>|string<br>|更新日(UNIX時間)<br>|
|{2}<br>|_{NP名}<br>|string<br>|オブジェクト{4}<br>Linkが結ばれている場合のみ返却される。{NP名}:NavigationPropert名<br>|
|{4}<br>|__deferred<br>|object<br>|オブジェクト{5}<br>|
|{5}<br>|uri<br>|string<br>|関係を結んでいるリソースのuri<br>テスト未実施<br>|
|{1}<br>|__count<br>|string<br>|$inlinecountクエリでの取得結果件数<br>|

上記以外にスキーマ設定した項目、または登録時に指定した動的な項目を返却
##### 数値の扱い
###### 小数値（Edm.Single型）
* JSON形式でUserODataを取得する場合の取り扱いは以下の通り
	* 10.0等の小数部が0となる値は、整数値として返却する

###### 数値（Edm.Double型）
※personium.ioでのDouble型の扱いは、JavaのDoubleの仕様に従います
* JSON形式でUserODataを取得する場合の取り扱いは以下の通り
	* 10.0等の小数部が0となる値は、整数値として返却する
* 返却される値について
	* 登録時の入力値が倍精度以上の精度を持った数である場合、倍精度に丸められてデータ登録を行う
		* 内部的には浮動小数点数として管理されるが、出力時には情報落ちの起こらない範囲で固定小数点数表現に変換して出力する  
		出力された固定小数点数を入力に用いた場合、その入力数ともとの数との同一性は保証される

#### エラーメッセージ一覧
[エラーメッセージ一覧](200_Error_Messages.html)を参照

|コード<br>|概要<br>|備考<br>|
|:--|:--|:--|
|304<br>|Not Modified<br>再取得が必要ない場合<br>| <br>|
|400<br>|リクエストクエリの指定誤り<br>リクエストヘッダの指定誤り<br>| <br>|
|401<br>|認証トークンが無効<br>| <br>|
|403<br>|アクセス権限が不足している場合<br>| <br>|
|404<br>|存在しないCellを指定した場合<br>存在しないBoxを指定した場合<br>存在しないODataCollectionを指定した場合<br>存在しないEntitySetを指定した場合<br>存在しないEntityを指定した場合<br>クエリで指定した範囲に取得するEntityが存在しない場合<br>| <br>|
|405<br>|許可していないリクエストメソッドを指定<br>| <br>|
|412<br>|If-None-Matchの指定誤り<br>| <br>|

#### レスポンスサンプル
```json
{
  "d": {
    "results": {
      "__metadata": {
        "uri": "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/{EntityTypeName}('{EntityID}')",
        "etag": "W/\"1-1487662179733\"",
        "type": "UserData.{EntityTypeName}"
      },
      "__id": "{EntityID}",
      "__published": "/Date(1487662179733)/",
      "__updated": "/Date(1487662179733)/",
      "PetName": null,
      "animalId": "100-1",
      "endedAt": "",
      "episodeType": "care",
      "name": "episode",
      "outcome": "治療中",
      "startedAt": "2010-11-08"
    }
  }
}
```

<br>
### CURLサンプル

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/{EntityTypeName}('{EntityID}')" -X GET -i -H 'Authorization: Bearer {UnitUserToken}' -H 'Accept: application/json'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
