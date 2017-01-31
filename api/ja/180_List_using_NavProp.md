# ユーザデータ_NavProp経由一覧取得
### 概要
ユーザデータのEntity一覧をNavigation Property経由で取得します。
### 必要な権限
read
### 制限事項
なし

<br>
### リクエスト
#### リクエストURL
```
/{CellName}/{BoxName}/{OdataCollecitonPath}/{EntitySet}({KeyPredicate})/{{NavigationProperty}}
```
|パス<br>|概要<br>|
|:--|:--|
|{CellName}<br>|セル名<br>|
|{BoxName}<br>|ボックス名<br>|
|{OdataCollecitonPath}<br>|コレクション名<br>|
|{EntitySet}<br>|EntitySet名<br>|
|KeyPredicate<br>|EntityのID<br>|
|{NavigationProperty}<br>|NavigationProperty名<br>|
指定できるNavigationProperty名は、EntitySetと以下の関連を持つものに限る。

|From Role<br>|To Role<br>|
|:--|:--|
|0 .. 1<br>|0 .. 1<br>|
|0 .. 1<br>|1<br>|
|0 .. 1<br>|*<br>|
|1<br>|0 .. 1<br>|
|1<br>|1<br>|
|1<br>|*<br>|
|*<br>|0 .. 1<br>|
|*<br>|1<br>|
|*<br>|*<br>|
#### メソッド
GET
#### リクエストクエリ
以下のクエリパラメタが利用可能です。

|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|dc_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|

[$select クエリ](193_$Select_Query.html)

[$expand クエリ](192_$Expand_Query.html)

[$format クエリ](191_$Format_Query.html)

[$filter クエリ](190_$Filter_Query.html)

[$inlinecount クエリ](194_$Inlinecount_Query.html)

[$orderby クエリ](187_$Orderby_Query.html)

[$top クエリ](188_$Top_Query.html)

[$skip クエリ](189_$Skip_Query.html)

[全文検索(q)クエリ](195_Full_Text_Search_Query.html)

#### リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {UnitUserToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
|Accept<br>|レスポンスボディの形式を指定する<br>|application / json<br>|×<br>|省略時は[application/json]として扱う<br>|
#### リクエストボディ
特になし
#### リクエストサンプル
なし

<br>
### レスポンス
#### ステータスコード
|コード<br>|概要<br>|備考<br>|
|:--|:--|:--|
|200<br>|成功<br>| <br>|
|400<br>|リクエストクエリの指定誤り<br>リクエストヘッダの指定誤り<br>|未対応<br>|
|401<br>|認証トークンが無効<br>|テスト未実施<br>|
|403<br>|アクセス権限が不足している場合<br>| <br>|
|404<br>|存在しないCellを指定した場合<br>存在しないBoxを指定した場合<br>存在しないODataCollectionを指定した場合<br>存在しないソースEntitySetを指定した場合<br>ソースEntitySetと関連の張られていないNavigationProperty名を指定した場合<br>| <br>|
|405<br>|許可していないリクエストメソッドを指定<br>| <br>|
#### レスポンスヘッダ
|項目名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|DataServiceVersion<br>|ODataのバージョン情報<br>|正常にEntityが作成できた場合のみ返却する<br>|
|Content-Type<br>|返却されるデータの形式<br>|未対応<br>|
#### レスポンスボディ
レスポンスはJSONオブジェクトで、オブジェクト（サブオブジェクト）に定義されるキー(名前)と型、並びに値の対応は以下のとおり

|オブジェクト<br>|名前（キー）<br>|型<br>|値<br>|
|:--|:--|:--|:--|
|ルート<br>|d<br>|object<br>|オブジェクト{1}<br>|
|{1}<br>|__count<br>|string<br>|$inlinecountクエリでの取得結果件数<br>|
|{1}<br>|results<br>|array<br>|オブジェクト{2}の配列<br>|
|{2}<br>|__published<br>|string<br>|作成日(UNIX時間)<br>|
|{2}<br>|__updated<br>|string<br>|更新日(UNIX時間)<br>|
|{2}<br>|__metadata<br>|object<br>|オブジェクト{3}<br>|
|{3}<br>|etag<br>|string<br>|Etag値<br>|
|{3}<br>|type<br>|string<br>|EntityType名<br>|
|{3}<br>|uri<br>|string<br>|作成したリソースへのURL<br>|
|{2}<br>|__id<br>|string<br>|EntityのID(__id)<br>|
|{2}<br>|_{NP名}<br>|string<br>|オブジェクト{4}<br>Linkが結ばれている場合のみ返却される。{NP名}:NavigationPropert名<br>|
|{4}<br>|__deferred<br>|object<br>|オブジェクト{5}<br>|
|{5}<br>|uri<br>|string<br>|関係を結んでいるリソースのuri<br>|
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
[エラーメッセージ一覧](199_Error_Messages.html)を参照
#### レスポンスサンプル
```json
{       "d":
       {
           "results":
           [
               {
                   "__metadata":
                   {
                       "uri": "http://localhost:8080/dc1-core/test_cell1/{BoxName}1/entyty_set/Sales('test')",
                       "type": "PF-9dqR7RLalymQqQc4wtw.Sales"
                   },
                   "__id": "test",
                   "__published": "/Date(1342572786509)/",
                   "__updated": "/Date(1342572786509)/",
                   "_Product":
                   {
                       "__deferred":
                       {
                           "uri": "http://localhost:8080/dc1-core/test_cell1/{BoxName}1/entyty_set/Sales('test')/_Product"
                       }
                   },
                   "_SalesDetail":
                   {
                       "__deferred":
                       {
                           "uri": "http://localhost:8080/dc1-core/test_cell1/{BoxName}1/entyty_set/Sales('test')/_SalesDetail"
                       }
                   },
                   "_Supplier":
                   {
                       "__deferred":
                       {
                           "uri": "http://localhost:8080/dc1-core/test_cell1/{BoxName}1/entyty_set/Sales('test')/_Supplier"
                       }
                   }
               }
           ],
           "__count": "1"
       }
    }
```

<br>
### CURLサンプル
#### CURLコマンド(UNIX)
```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{OdataCollecitonPath}/parent('100-1_20101108-111352092')/_child" -X GET -i -H 'Authorization: Bearer {UnitUserToken}' -H 'Accept: application/json'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
