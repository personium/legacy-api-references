# Account取得
### 概要
既存のAccount情報を取得する
### 必要な権限
auth-read
### 制限事項
* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
* リクエストボディはjson形式のみ受け付ける
* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはjson形式とする
* $formatクエリオプションにatom または xmlを指定した場合、エラーとはならないが、レスポンスボディのデータの保証はない

<br>
### リクエスト
#### リクエストURL
```
/{CellName}/__ctl/Account(Name='aoount_name')
または、
/{CellName}/__ctl/Account('{AccountName}')
```
#### メソッド
GET
#### リクエストクエリ
|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|dc_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|

[$select クエリ](192_$select_Query.html)

[$expand クエリ](191_$expand_Query.html)

[$format クエリ](190_$Format_Query.html)

#### リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、<br>指定した値がメソッドとして使用されます。<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合<br>X-Overrideヘッダを複数指定します。<br>|
|X-Dc-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {UnitUserToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
|Accept<br>|レスポンスボディの形式を指定する<br>|application / json<br>|×<br>|省略時は[application/json]として扱う<br>|
|If-None-Match<br>|対象ETag値を指定する<br>|ETag値<br>|○<br>|未対応<br>|
#### リクエストボディ
なし
#### リクエストサンプル
なし

<br>
### レスポンス
#### ステータスコード
200
#### レスポンスヘッダ
なし
#### レスポンスボディ
|オブジェクト<br>|Name(Key)<br>|Type<br>|Value<br>|
|:--|:--|:--|:--|
|Root<br>|d<br>|object<br>|Object {1}<br>|
|{1}<br>|__count<br>|string<br>|Get number of results in $inlinecount query<br>|
|{1}<br>|results<br>|array<br>|Array object {2}<br>|
|{2}<br>|__published<br>|string<br>|Creation date (UNIX time)<br>|
|{2}<br>|__updated<br>|string<br>|Update date (UNIX time)<br>|
|{2}<br>|__metadata<br>|object<br>|Object {3}<br>|
|{2}<br>|Name<br>|string<br>|Account name<br>|
|{2}<br>|Cell<br>|string<br>|null<br>|
|{2}<br>|Type<br>|string<br>|basic<br>|
|{3}<br>|etag<br>|string<br>|ETag value<br>|
|{3}<br>|uri<br>|string<br>|URL to the resource that you createdv<br>|
|{3}<br>|type<br>|string<br>|CellCtl.Account<br>|

#### エラーメッセージ一覧
[エラーメッセージ一覧](198_Error_Messages.html)を参照

#### レスポンスサンプル
```json
{
  "d": {
    "results": {
      "Name": "{AccountName}",
      "__published": "/Date(1349355810698)/",
      "Cell": null,
      "__updated": "/Date(1349355810698)/",
      "Type": "basic",
      "__metadata": {
        "etag": "1-1349355810698",
        "type": "CellCtl.Account",
        "uri": "https://{UnitFQDN}/{CellName}/__ctl/Account('{AccountName}')"
      }
    }
  }
}
```

<br>
### CURLサンプル
#### CURLコマンド(UNIX)
```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/Account('{AccountName}')" -X GET -i -H 'Authorization: Bearer {UnitUserToken}' -H 'Accept: application/json'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
