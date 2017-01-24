# サービスドキュメント取得
### 概要
ユーザーデータのスキーマ定義を行うためのサービスドキュメントを取得する
### 必要な権限
read
### 制限事項
なし

<br>
### リクエスト
#### リクエストURL
```
/{Cell_name}/{Box_name}/{odata_colleciton_path}/$metadata
```
#### メソッド
GET
#### リクエストクエリ
##### 共通リクエストクエリ
|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|dc_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|
#### リクエストヘッダ
##### 共通リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用される<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定する<br>|
|X-Dc-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
##### 個別リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Accept<br>|返却されるデータの形式<br>|application / atomsvc &#8203;&#8203;+ xml<br>|○<br>|指定がない場合、ユーザデータスキーマのスキーマ情報取得となる<br>|
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
|X-Dc-Version<br>|APIの実行バージョン<br>|リクエストが処理されたAPIバージョン<br>|
|Access-Control-Allow-Origin<br>|クロスドメイン通信許可ヘッダ<br>|返却値は"*"固定<br>|
|Content-Type<br>|返却されるデータの形式<br>| <br>|
|DataServiceVersion<br>|ODataのバージョン情報<br>| <br>|
#### レスポンスボディ
固定で以下を返却する
```xml
<service xmlns='http://www.w3.org/2007/app' xml:base='https://fqdn/cell_name/box_name/odata_colleciton_path/$metadata'
xmlns:atom='http://www.w3.org/2005/Atom' xmlns:app='http://www.w3.org/2007/app'>
  <workspace>
    <atom:title>
      Default
    </atom:title>
    <collection href='ComplexType'>
      <atom:title>
        ComplexType
      </atom:title>
    </collection>
    <collection href='ComplexTypeProperty'>
      <atom:title>
        ComplexTypeProperty
      </atom:title>
    </collection>
    <collection href='AssociationEnd'>
      <atom:title>
        AssociationEnd
      </atom:title>
    </collection>
    <collection href='EntityType'>
      <atom:title>
        EntityType
      </atom:title>
    </collection>
    <collection href='Property'>
      <atom:title>
        Property
      </atom:title>
    </collection>
  </workspace>
</service>
```
#### エラーメッセージ一覧
[エラーメッセージ一覧](198_Error_Messages.html)を参照
#### レスポンスサンプル
なし

<br>
### CURLサンプル
#### CURLコマンド(UNIX)
```sh
curl "https://fqdn/cell_name/box_name/odata_colleciton_path/$metadata' -X GET -i -H 'Authorization: Bearer auth_token' -H 'Accept: application/atomsvc+xml'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
