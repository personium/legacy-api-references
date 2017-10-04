# Entity_NavProp経由登録
### 概要
ユーザデータのEntityをNavigation Property経由で作成します。
### 必要な権限
write
### 制限事項
* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
* リクエストボディはJSON形式のみ受け付ける
* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはJSON形式とする
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
/{CellName}/{BoxName}/{ODataCollecitonName}/{EntityTypeName}('{EntityID}')/{NavigationPropertyName}
```
|パス<br>|概要<br>|
|:--|:--|
|{CellName}<br>|セル名<br>|
|{BoxName}<br>|ボックス名<br>|
|{ODataCollecitonName}<br>|コレクション名<br>|
|{EntityTypeName}<br>|EntityType名<br>|
|{EntityID}<br>|EntityのID<br>|
|{NavigationPropertyName}<br>|NavigationProperty名<br>|
指定できるNavigationProperty名は、EntitySetと以下の関連を持つものに限る。  

|From<br>|To<br>|
|:--|:--|
|0 .. 1<br>|1<br>|
|0 .. 1<br>|*<br>|
|1<br>|1<br>|
|1<br>|*<br>|
|*<br>|*<br>|
#### メソッド
POST
<br>
#### リクエストクエリ
##### 共通リクエストクエリ
|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|p_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|
#### リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {AccessToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>テスト未実施<br>|
|Accept<br>|レスポンスボディの形式を指定する<br>|application / JSON<br>|×<br>|省略時は[application/json]として扱う<br>未対応<br>|
|Content-Type<br>|リクエストボディの形式を指定する<br>|application / JSON<br>|×<br>|省略時は[application/json]として扱う<br>未対応<br>|
#### リクエストボディ
|項目名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|__id<br>|EntityのID<br>|桁数：1&#65374;200<br>文字種:半角英数字と-(半角ハイフン)と_(半角アンダーバー)<br>|×<br>|指定しない場合ユニークなIDが割り当てられます<br>有効値チェック未実装<br>|
* \__id以外に動的なユーザデータを登録することが可能
	- 文字列のみ設定可能
	- 数値、真偽値、nullを指定した場合は、文字列として扱われる
	- Hash値の指定は不可
	- 最大で400個のユーザデータを指定可能
	- \_published、\_updatedは、予約語であるためリクエストボディの指定は不可

#### リクエストサンプル
```JSON
{"__id": "100-1_20101108-111352093","animalId": "100-1","name": "episode","startedAt": "2010-11-08","episodeType": "care","endedAt": "","outcome": "治療中"}
```

<br>
### レスポンス
#### ステータスコード
201
#### レスポンスヘッダ
|項目名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|Content-Type<br>|返却されるデータの形式<br>| <br>
|Location<br>|作成したEntityのリソースURL<br>|正常にEntityが作成できた場合のみ返却する<br>|
|DataServiceVersion<br>|ODataのバージョン情報<br>|正常にEntityが作成できた場合のみ返却する<br>|
|ETag<br>|リソースのバージョン情報<br>|正常にEntityが作成できた場合のみ返却する<br>|
#### レスポンスボディ
レスポンスはJSONオブジェクトで、オブジェクト（サブオブジェクト）に定義されるキー(名前)と型、並びに値の対応は以下のとおり

|オブジェクト<br>|名前（キー）<br>|型<br>|値<br>|
|:--|:--|:--|:--|
|ルート<br>|d<br>|object<br>|オブジェクト{1}<br>|
|{1}<br>|results<br>|array<br>|オブジェクト{2}の配列<br>|
|{2}<br>|__metadata<br>|object<br>|オブジェクト{3}<br>|
|{3}<br>|uri<br>|string<br>|作成したリソースへのURL<br>|
|{3}<br>|etag<br>|string<br>|Etag値<br>|
|{3}<br>|type<br>|string<br>|EntityType名<br>|
|{2}<br>|__id<br>|string<br>|EntityのID(__id)<br>|
|{2}<br>|__published<br>|string<br>|作成日(UNIX時間)<br>|
|{2}<br>|__updated<br>|string<br>|更新日(UNIX時間)<br>|
|{2}<br>|{NP名}<br>|string<br>|オブジェクト{4}<br>Linkが結ばれている場合のみ返却される。{NP名}:NavigationPropert名<br>|
|{4}<br>|__deferred<br>|object<br>|オブジェクト{5}<br>|
|{5}<br>|uri<br>|string<br>|関係を結んでいるリソースのuri<br>テスト未実施<br>|
|{1}<br>|__count<br>|string<br>|$inlinecountクエリでの取得結果件数<br>|

上記以外にスキーマ設定した項目、または登録時に指定した動的な項目を返却
#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

#### レスポンスサンプル
```JSON
{
  "d": {
    "results": {
      "__metadata": {
        "uri": "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/{EntityTypeName}('100-1_20101108-111352093')",
        "etag": "W/\"1-1487929403469\"",
        "type": "UserData.{EntityTypeName}"
      },
      "__id": "100-1_20101108-111352093",
      "__published": "/Date(1487929403469)/",
      "__updated": "/Date(1487929403469)/",
      "PetName": null,
      "{NavigationPropertyName}": {
        "__deferred": {
          "uri": "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/{EntityTypeName}('100-1_20101108-111352093')/{NavigationPropertyName}"
        }
      }
    }
  }
}
```

<br>
### cURLサンプル

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/{EntityTypeName}('{EntityID}')/{NavigationPropertyName}" -X POST -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"__id": "100-1_20101108-111352093"}'

```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
