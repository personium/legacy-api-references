# ExtCell_NavProp経由一覧取得
### 概要
セル制御オブジェクトをNavigation Property経由で取得する

### 必要な権限
* 取得するセル制御オブジェクトと経由するセル制御オブジェクト双方の参照権限
* 例） Role経由でRelationを取得する場合<br>auth-readとsocial-read

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
/{CellName}/__ctl/ExtCell(Url='{extcell_url}')/_Role
```
または、
```
/{CellName}/__ctl/ExtCell('{extcell_url}')/_Role
```
##### RelationへのnavigationProperty
```
/{CellName}/__ctl/ExtCell(Url='{extcell_url}')/_Relation
```
または、
```
/{CellName}/__ctl/ExtCell('{extcell_url}')/_Relation
```
※{extcell_url}についてはURLエンコードが必要

#### メソッド
GET

#### リクエストクエリ
以下のクエリパラメタが利用可能です。

|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|p_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|

[$select クエリ](194_$Select_Query.html)

[$expand クエリ](193_$Expand_Query.html)

[$format クエリ](192_$Format_Query.html)

[$filter クエリ](190_$Filter_Query.html)

[$inlinecount クエリ](195_$Inlinecount_Query.html)

[$orderby クエリ](187_$Orderby_Query.html)

[$top クエリ](188_$Top_Query.html)

[$skip クエリ](189_$Skip_Query.html)

[全文検索(q)クエリ](196_Full_Text_Search_Query.html)

#### リクエストヘッダ

|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用されます。<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定します。<br>|
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
|X-Personium-Version<br>|APIの実行バージョン<br>|リクエストが処理されたAPIバージョン<br>|
|Access-Control-Allow-Origin<br>|クロスドメイン通信許可ヘッダ<br>|返却値は"*"固定<br>|
|Content-Type<br>|返却されるデータの形式<br>|&#160;<br>
|DataServiceVersion<br>|ODataのバージョン<br>|&#160;<br>
#### レスポンスボディ

|オブジェクト<br>|Item Name<br>|Data Type<br>|Remarks<br>|
|:--|:--|:--|:--|
|ルート&#160;&#160;<br>|d<br>|object<br>|オブジェクト{1}<br>|
|{1}<br>|__count<br>|string<br>|$inlinecountクエリでの取得結果件数<br>|
|{1}<br>|results<br>|array<br>|オブジェクト{2}の配列<br>|
|{2}<br>|__published<br>|string<br>|作成日(UNIX時間)<br>|
|{2}<br>|__updated<br>|string<br>|更新日(UNIX時間)<br>|
|{2}<br>|__metadata<br>|object<br>|オブジェクト{3}<br>|
|{3}<br>|etag<br>|string<br>|Etag値<br>|
|{3}<br>|uri<br>|string<br>|作成したリソースへのURL<br>|
##### ExtCellを取得した場合
##### ExtCell固有レスポンスボディ

|オブジェクト<br>|Item Name<br>|Data Type<br>|Remarks<br>|
|:--|:--|:--|:--|
|{3}<br>|type<br>|string<br>|CellCtl.ExtCell &#160;<br>|
|{2}<br>|Url<br>|string<br>|対象CellのURL &#160;<br>|
#### エラーメッセージ一覧
[エラーメッセージ一覧](200_Error_Messages.html)を参照

#### レスポンスサンプル
```json
{
  "d": {
    "results": {
      "__published": "/Date(1349353355940)/",
      "__updated": "/Date(1349353355940)/",
      "Url": "https://{UnitFQDN}/{CellName}/",
      "__metadata": {
        "etag": "1-1349353355940",
        "type": "CellCtl.ExtCell",
        "uri": "https://{UnitFQDN}/{CellName}/__ctl/ExtCell('https://{UnitFQDN}/{CellName2}/')"  
      }
    }
  }
}
```

### CURLサンプル
なし
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
