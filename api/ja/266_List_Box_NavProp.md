# Box_NavProp経由取得
### 概要
セル制御オブジェクトをNavigation Property経由で取得する
### 必要な権限
* Roleを取得する場合<br>auth-read
* Relationを取得する場合<br>social-read

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
/{CellName}/__ctl/Box(Name='{BoxName}',Schema='schema_url')/_Role
```
または、
```
/{CellName}/__ctl/Box(Name='{BoxName}')/_Role
```
または、
```
/{CellName}/__ctl/Box('{BoxName}')/_Role
```
##### RelationへのnavigationProperty
```
/{CellName}/__ctl/Box(Name='{BoxName}',Schema='schema_url')/_Relation
```
または、
```
/{CellName}/__ctl/Box(Name='{BoxName}')/_Relation
```
または、
```
/{CellName}/__ctl/Box('{BoxName}')/_Relation
```
※ Schemaパラメタを省略した場合は、nullが指定されたものとする
#### メソッド
GET
#### リクエストクエリ
以下のクエリパラメタが利用可能です。

|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|p_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|

[$select クエリ](406_Select_Query.html)

[$expand クエリ](405_Expand_Query.html)

[$format クエリ](404_Format_Query.html)

[$filter クエリ](403_Filter_Query.html)

[$inlinecount クエリ](407_Inlinecount_Query.html)

[$orderby クエリ](400_Orderby_Query.html)

[$top クエリ](401_Top_Query.html)

[$skip クエリ](402_Skip_Query.html)

[全文検索(q)クエリ](408_Full_Text_Search_Query.html)

#### リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用されます。<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}override} $: $ {value}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定します。<br>|
|X-Personium-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {UnitUserToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
|Accept<br>|レスポンスボディの形式を指定する<br>|application / json<br>|×<br>|省略時は[application/json]として扱う<br>|
#### リクエストボディ
なし
#### リクエストサンプル
なし

<br>
### レスポンス
#### ステータスコード
200
#### レスポンスヘッダ
|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|Content-Type<br>|返却されるデータの形式<br>|<br>|
|DataServiceVersion<br>|ODataのバージョン<br>|<br>|
|Access-Control-Allow-Origin<br>|クロスドメイン通信許可ヘッダ<br>|返却値は"*"固定<br>|
|X-Personium-Version<br>|APIの実行バージョン<br>|リクエストが処理されたAPIバージョン<br>|
#### レスポンスボディ
|オブジェクト<br>|項目名<br>|Data Type<br>|備考<br>|
|:--|:--|:--|:--|
|ルート<br>|d<br>|object<br>|オブジェクト{1}<br>|
|{1}<br>|results<br>|array<br>|オブジェクト{2}の配列<br>|
|{2}<br>|__metadata<br>|object<br>|オブジェクト{3}<br>|
|{3}<br>|uri<br>|string<br>|作成したリソースへのURL<br>|
|{3}<br>|etag<br>|string<br>|Etag値<br>|
|{2}<br>|__published<br>|string<br>|作成日(UNIX時間)<br>|
|{2}<br>|__updated<br>|string<br>|更新日(UNIX時間)<br>|
|{1}<br>|__count<br>|string<br>|$inlinecountクエリでの取得結果件数<br>|

##### Relationを取得した場合
|オブジェクト<br>|項目名<br>|Data Type<br>|備考<br>|
|:--|:--|:--|:--|
|{3}<br>|type<br>|string<br>|CellCtl.Relation<br>|
|{2}<br>|Name<br>|string<br>|Relation名<br>|
|{2}<br>|_Box.Name<br>|string<br>|関係対象のBox名<br>|
#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

##### レスポンスサンプル
```json
{
  "d": {
    "results": [
      {
        "__metadata": {
          "uri": "https://{UnitFQDN}/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')",
          "etag": "W/\"1-1486700131198\"",
          "type": "CellCtl.Relation"
        },
        "Name": "{RelationName}",
        "_Box.Name": "{BoxName}",
        "__published": "/Date(1486700131198)/",
        "__updated": "/Date(1486700131198)/",
        "_Box": {
          "__deferred": {
            "uri": "https://{UnitFQDN}/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/_Box"
          }
        },
        "_ExtCell": {
          "__deferred": {
            "uri": "https://{UnitFQDN}/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/_ExtCell"
          }
        },
        "_ExtRole": {
          "__deferred": {
            "uri": "https://{UnitFQDN}/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/_ExtRole"
          }
        },
        "_Role": {
          "__deferred": {
            "uri": "https://{UnitFQDN}/{CellName}/__ctl/Relation(Name='{RelationName}',_Box.Name='{BoxName}')/_Role"
          }
        }
      }
    ]
  }
}
```

<br>
### CURLサンプル
```sh
curl
"https://{UnitFQDN}/{CellName}/__ctl/Box('{BoxName}')/_Role" -X POST -i  -H 'Authorization: Bearer {UnitUserToken}' -H 'Accept: application/json' -d '{"Name":"{RoleName}"}'
```
##### Relationを取得する場合
```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/Box('{BoxName}')/_Relation" -X GET -i -H 'Authorization: Bearer {UnitUserToken}' -H 'Accept: application/json'
```

<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
