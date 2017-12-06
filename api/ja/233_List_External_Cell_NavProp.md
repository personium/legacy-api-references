# ExtCell_NavProp経由一覧取得
### 概要
セル制御オブジェクトをNavigation Property経由で取得する

### 必要な権限
* 取得するセル制御オブジェクトと経由するセル制御オブジェクト双方の参照権限
* 例） Role経由でRelationを取得する場合<br>auth-readとsocial-read

### 制限事項
* リクエストヘッダのAcceptは無視される
* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
* リクエストボディはJSON形式のみ受け付ける
* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはJSON形式とする
* $formatクエリオプションは無視される


### リクエスト
#### リクエストURL
##### RoleへのnavigationProperty
```
/{CellName}/__ctl/ExtCell(Url='{ExtCellURL}')/_Role
```
または、
```
/{CellName}/__ctl/ExtCell('{ExtCellURL}')/_Role
```
##### RelationへのnavigationProperty
```
/{CellName}/__ctl/ExtCell(Url='{ExtCellURL}')/_Relation
```
または、
```
/{CellName}/__ctl/ExtCell('{ExtCellURL}')/_Relation
```
※{ExtCellURL}についてはURLエンコードが必要

#### メソッド
GET

#### リクエストクエリ
以下のクエリパラメタが利用可能です。

|クエリ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|p_cookie_peer|クッキー認証値|認証時にサーバから返却されたクッキー認証値|×|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する|

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

|ヘッダ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|メソッドオーバーライド機能|任意|×|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用されます。|
|X-Override|ヘッダオーバライド機能|${上書きするヘッダ名}:${値}|×|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定します。|
|X-Personium-RequestKey|イベントログに出力するRequestKeyフィールドの値|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字|×|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応|
|Authorization|OAuth2.0形式で、認証情報を指定する|Bearer {AccessToken}|×|※認証トークンは認証トークン取得APIで取得したトークン|
|Accept|レスポンスボディの形式を指定する|application/json|×|省略時は[application/json]として扱う|
#### リクエストボディ
なし

#### リクエストサンプル
なし


### レスポンス
#### ステータスコード
200

#### レスポンスヘッダ

|ヘッダ名|概要|備考|
|:--|:--|:--|
|Content-Type|返却されるデータの形式||
|DataServiceVersion|ODataのバージョン||
|Access-Control-Allow-Origin|クロスドメイン通信許可ヘッダ|返却値は"*"固定|
|X-Personium-Version|APIの実行バージョン|リクエストが処理されたAPIバージョン|
#### レスポンスボディ

|オブジェクト|Item Name|Data Type|Remarks|
|:--|:--|:--|:--|
|ルート|d|object|オブジェクト{1}|
|{1}|results|array|オブジェクト{2}の配列|
|{2}|__metadata|object|オブジェクト{3}|
|{3}|uri|string|作成したリソースへのURL|
|{3}|etag|string|Etag値|
|{2}|__published|string|作成日(UNIX時間)|
|{2}|__updated|string|更新日(UNIX時間)|
|{1}|__count|string|$inlinecountクエリでの取得結果件数|

##### ExtCellを取得した場合
##### ExtCell固有レスポンスボディ

|オブジェクト|Item Name|Data Type|Remarks|
|:--|:--|:--|:--|
|{3}|type|string|CellCtl.ExtRole|
|{2}|Name|string|関係対象のRole名|
|{2}|_Box.Name|string|関係対象のBox名|
#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

#### レスポンスサンプル
```JSON
{
  "d": {
    "results": [
      {
        "__metadata": {
          "uri": "https://{UnitFQDN}/{CellName}/__ctl/Role(Name='{RoleName}',_Box.Name='{BoxName}')",
          "etag": "W/\"1-1487320623218\"",
          "type": "CellCtl.Role"
        },
        "Name": "{RoleName}",
        "_Box.Name": "{BoxName}",
        "__published": "/Date(1487320623218)/",
        "__updated": "/Date(1487320623218)/",
        "_Box": {
          "__deferred": {
            "uri": "https://{UnitFQDN}/{CellName}/__ctl/Role(Name='{RoleName}',_Box.Name='{BoxName}')/_Box"
          }
        },
        "_Account": {
          "__deferred": {
            "uri": "https://{UnitFQDN}/{CellName}/__ctl/Role(Name='{RoleName}',_Box.Name='{BoxName}')/_Account"
          }
        },
        "_ExtCell": {
          "__deferred": {
            "uri": "https://{UnitFQDN}/{CellName}/__ctl/Role(Name='{RoleName}',_Box.Name='{BoxName}')/_ExtCell"
          }
        },
        "_ExtRole": {
          "__deferred": {
            "uri": "https://{UnitFQDN}/{CellName}/__ctl/Role(Name='{RoleName}',_Box.Name='{BoxName}')/_ExtRole"
          }
        },
        "_Relation": {
          "__deferred": {
            "uri": "https://{UnitFQDN}/{CellName}/__ctl/Role(Name='{RoleName}',_Box.Name='{BoxName}')/_Relation"
          }
        }
      }
    ]
  }
}
```

### cURLサンプル
##### RoleのnavigationProperty経由一覧
```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/ExtCell('https%3A%2F%2F{UnitFQDN}%2F{ExtCellName}%2F')/_Role" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```

###### Copyright 2017 FUJITSU LIMITED
