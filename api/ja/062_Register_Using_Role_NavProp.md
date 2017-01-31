﻿﻿# ExtRole_NavProp経由登録
### 概要
Cell制御オブジェクトNavigation Property経由で登録し、同時に$linksを登録する。
### 必要な権限
write
### 制限事項
* リクエストヘッダのAcceptは無視される
* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
* リクエストボディはjson形式のみ受け付ける
* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはjson形式とする
* $formatクエリオプションは無視される

<br>
### リクエスト
#### リクエストURL
##### RoleへのnavigationProperty
```
/{CellName}/__ctl/ExtRole(ExtRole='{extRoleName}',_Relation.Name='{RelationName}',
_Relation._Box.Name='{BoxName}')/_Role
```
##### RelationへのnavigationProperty
```
/{CellName}/__ctl/ExtRole(ExtRole='{extRoleName}',_Relation.Name='{RelationName}',
_Relation._Box.Name='{BoxName}')/_Relation
```
※ \_Relation.\_Box.Nameパラメタを省略した場合は、nullが指定されたものとする
※ ExtRoleのNavProp経由登録APIではRoleの登録のみ有効
#### メソッド
POST
#### リクエストクエリ
|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|dc_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|
#### リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用されます。<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定します。<br>|
|X-Dc-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {UnitUserToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
|Content-Type<br>|リクエストボディの形式を指定する<br>|application / json<br>|×<br>|省略時は[application/json]として扱う<br>|
|Accept<br>|レスポンスボディの形式を指定する<br>|application / json<br>|×<br>|省略時は[application/json]として扱う<br>|
#### リクエストボディ
##### ExtRoleを登録する場合
###### Format
JSON

|項目名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|ExtRole<br>|外部ロール名(URL)<br>|桁数：1&#65374;1024<br>URIの形式に従う<br>scheme：http / https / urn<br>Boxに紐付いている場合:/{アプリセル名}/&#95;&#95;role/&#95;&#95;/{Role名}<br>※ただし、BoxにSchema情報が登録されていない場合、Boxに紐付いていないとみなされる<br>Boxに紐付いていない場合:/{Cell名]/&#95;&#95;role/&#95;&#95;/{Role名}<br>|○<br>|<br>|
|_Relation.Name<br>|関係対象のRelation名<br>|桁数：1&#65374;128<br>文字種:半角英数字と-(半角ハイフン)と_(半角アンダーバー)<br>と+(プラス)と:(コロン)<br>ただし、先頭文字に_(半角アンダーバー):(コロン)は指定不可<br>Relation登録APIにて登録済みのRelation<br>null<br>|○<br>|<br>|
|_Relation._Box.Name<br>|関係対象のRelationが紐づくBox名<br>|桁数：1&#65374;128<br>文字種:半角英数字と-(半角ハイフン)と_(半角アンダーバー)<br>Relation登録APIにて登録済みのRelationが紐づくBox名<br>null<br>|×<br>|<br>|
#### リクエストサンプル
```json
{
  "ExtRole": "https://{UnitFQDN}/{CellName}/__role/__/roletest",
  "_Relation.Name": "{RelationName}",
  "_Relation._Box.Name": "{BoxName}"
}
```

<br>
### レスポンス
#### ステータスコード
201
#### レスポンスヘッダ
|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|X-Dc-Version<br>|APIの実行バージョン<br>|リクエストが処理されたAPIバージョン<br>|
|Access-Control-Allow-Origin<br>|クロスドメイン通信許可ヘッダ<br>|返却値は"*"固定<br>|
|Content-Type<br>|返却されるデータの形式<br>|<br>|
|Location<br>|作成したリソースへのURL<br>|<br>|
|ETag<br>|リソースのバージョン情報<br>|<br>|
|DataServiceVersion<br>|ODataのバージョン<br>|<br>|
#### レスポンスボディ
|オブジェクト<br>|項目名<br>|Data Type<br>|備考<br>|
|:--|:--|:--|:--|
|ルート<br>|d<br>|object<br>|オブジェクト{1}<br>|
|{1}<br>|__count<br>|string<br>|$inlinecountクエリでの取得結果件数<br>|
|{1}<br>|results<br>|array<br>|オブジェクト{2}の配列<br>|
|{2}<br>|__published<br>|string<br>|作成日(UNIX時間)<br>|
|{2}<br>|__updated<br>|string<br>|更新日(UNIX時間)<br>|
|{2}<br>|__metadata<br>|object<br>|オブジェクト{3}<br>|
|{3}<br>|etag<br>|string<br>|Etag値<br>|
|{3}<br>|uri<br>|string<br>|作成したリソースへのURL<br>|
##### ExtRoleを登録する場合
|オブジェクト<br>|項目名<br>|Data Type<br>|備考<br>|
|:--|:--|:--|:--|
|{3}<br>|type<br>|string<br>|CellCtl.ExtRole<br>|
|{2}<br>|ExtRole<br>|string<br>|外部ロールURL<br>|
|{2}<br>|_Relation.Name<br>|string<br>|Relation名<br>|
|{2}<br>|_Relation._Box.Name<br>|string<br>|Relationに紐付くBox名<br>|
#### エラーメッセージ一覧
[エラーメッセージ一覧](200_Error_Messages.html)を参照

#### レスポンスサンプル
```json
{
  "d": {
    "results": {
      "__published": "/Date(1371010428917)/",
      "_Relation._Box.Name": null,
      "__updated": "/Date(1371010428917)/",
      "_Relation.Name": "{RelationName}",
      "__metadata": {
        "etag": "W/\"1-1371010428917\"",
        "type": "CellCtl.ExtRole",
        "uri": "https://{UnitFQDN}/{CellName}/__ctl/ExtRole(ExtRole='https://{UnitFQDN}/{CellName}/__role/__/RoleName',_Relation.Name='{RelationName}',_Relation._Box.Name=null)"
      },
      "ExtRole": "https://{UnitFQDN}/{CellName}/__role/__/RoleName"
    }
  }
}
```

<br>
### CURLサンプル
#### CURLコマンド(UNIX)
##### ExtRoleを登録する場合
```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/Relation('{RelationName}')/_ExtRole" -X POST -i -H 'Authorization: Bearer {UnitUserToken}' -H 'Accept: application/json' -d '{"ExtRole":"relation","_Relation.Name":"{RelationName}","_Relation._Box.Name":"box"}'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
