# ExtCell登録
### 概要
新規ExtCellを作成する

### 必要な権限
auth

### 制限事項
* リクエストヘッダのAcceptは無視される
* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
* リクエストボディはjson形式のみ受け付ける
* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはjson形式とする
* $formatクエリオプションは無視される

<br>
### リクエスト
#### リクエストURL
```
/{CellName}/__ctl/ExtCell
```
#### メソッド
POST

#### リクエストクエリ

|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|dc_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|
&#160;

#### リクエストヘッダ

|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用されます。<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定します。<br>|
|X-Dc-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>&#160;<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {UnitUserToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
|Content-Type<br>|リクエストボディの形式を指定する<br>|application / json<br>|×<br>|省略時は[application/json]として扱う<br>|
|Accept<br>|レスポンスボディの形式を指定する<br>|application / json<br>|×<br>|省略時は[application/json]として扱う<br>|
#### リクエストボディ

|項目名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Url<br>|CellへのURL<br>|桁数：1&#65374;1024<br>URIの形式に従う<br>scheme：http, https<br>トレイリングスラッシュ(URL終端の/)必須<br>|○<br>|&#160;<br>|
#### リクエストサンプル
```json
{
  "Url": "https://{UnitFQDN}/{CellName}/"
}
```
<br>
### レスポンス
#### ステータスコード
201

#### レスポンスヘッダ

|項目名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|X-Dc-Version<br>|APIの実行バージョン<br>|リクエストが処理されたAPIバージョン<br>|
|Access-Control-Allow-Origin<br>|クロスドメイン通信許可ヘッダ<br>|返却値は"*"固定<br>|
|Content-Type<br>|返却されるデータの形式<br>|&#160;<br>|
|Location<br>|作成したリソースへのURL<br>|&#160;<br>|
|ETag<br>|リソースのバージョン情報<br>|&#160;<br>|
|DataServiceVersion<br>|ODataのバージョン<br>|&#160;<br>|
#### レスポンスボディ

|オブジェクト<br>|項目名<br>|Data Type<br>|備考<br>|
|:--|:--|:--|:--|
|ルート&#160;<br>|d<br>|object<br>|オブジェクト{1}<br>|
|{1}<br>|__count<br>|string<br>|$inlinecountクエリでの取得結果件数<br>|
|{1}<br>|results<br>|array<br>|オブジェクト{2}の配列<br>|
|{2}<br>|__published<br>|string<br>|作成日(UNIX時間)<br>|
|{2}<br>|__updated<br>|string<br>|更新日(UNIX時間)<br>|
|{2}<br>|__metadata<br>|object<br>|オブジェクト{3}<br>|
|{2}<br>|Url<br>|string<br>|対象CellのURL<br>|
|{3}<br>|etag<br>|string<br>|Etag値<br>|
|{3}<br>|uri<br>|string<br>|作成したリソースへのURL<br>|
|{3}<br>|type<br>|string<br>|CellCtl.ExtCell<br>|
#### エラーメッセージ一覧
[エラーメッセージ一覧](200_Error_Messages.html)を参照
.html)を参照

|コード<br>|メッセージ<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|
|400<br>|Bad Request<br>|リクエストボディの形式が不正&#160;<br>リクエストヘッダの形式が不正<br>|&#160;<br>|
|401<br>|Unauthorized<br>|認証トークンが無効<br>|&#160;<br>|
|403<br>|Forbidden<br>|アクセス権限が不足している場合<br>|&#160;<br>|
|404<br>|Not Found<br>|存在しないリソースを指定<br>|&#160;<br>|
|405<br>|Method Not Allowed<br>|許可していないリクエストメソッドを指定<br>|&#160;<br>|
|409<br>|Conflict<br>|セル内に同じ"Name"のaccountが存在している場合<br>|&#160;<br>
|412<br>|Precondition Failed<br>|存在しないバージョンを指定<br>|&#160;<br>|

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
<br>
### CURLサンプル
#### CURLコマンド(UNIX)
```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/ExtCell" -X POST -i -H 'Authorization: Bearer {UnitUserToken}' -H 'Accept: application/json'  -d '{"Url":"https://{UnitFQDN}/{CellName}/"}'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
