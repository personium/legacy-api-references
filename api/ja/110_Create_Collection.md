# コレクション作成
### 概要
WebDAVのコレクションを作成する
### 必要な権限
write
### 制限事項
共通制限
* なし
WebDAV制限
* 未稿

<br>
### リクエスト
#### リクエストURL
```
/{Cell_name}/{Box_name}/{resource_path}
```
|パス<br>|概要<br>|備考<br>|
|:--|:--|:--|
|Cell_name<br>|セル名<br>|<br>
|Box_name<br>|ボックス名<br>|<br>
|resource_path<br>|リソースへのパス<br>|有効値 桁数:1&#65374;128<br>使用可能文字種<br>半角英数字、半角ピリオド(.)、半角アンダーバー(_)、半角ハイフン(-)<br>|
#### メソッド
MKCOL
#### リクエストクエリ
##### 共通リクエストクエリ
|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|dc_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|
##### WebDav 共通リクエストクエリ
なし
#### リクエストヘッダ
##### 共通リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用される<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定する<br>|
|X-Dc-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
###### 個別リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {TokenValue}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
#### リクエストボディ
##### 名前空間
|URI<br>|概要<br>|参考prefix<br>|
|:--|:--|:--|
|DAV:<br>|WebDAVの名前空間<br>|D:<br>|
|urn:x-dc1:xmlns<br>|personium.ioの名前空間<br>|dc:<br>|
※ 参考prefixは以下表の可読性を高めるためのもので、このprefix文字列の使用を保証するものでも要求するものでもありません。
##### XMLの構造
ボディはXMLで、以下のスキーマに従っています。

|ノード名<br>|名前空間<br>|ノードタイプ<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|:--|
|mkcol <br>|D:<br>|要素<br>|mkcolのルート要素を表し、setが子となる <br>|<br>|
|set<br>|D:<br>|要素<br>|プロパティ設定を表し、propが子となる <br>|<br>|
|prop<br>|D:<br>|要素<br>|プロパティ設定値を表し、resourcetypeが子となる <br>|<br>|
|resourcetype<br>|D:<br>|要素<br>|リソースタイプ設定を表し、collection・odata・serviceのいずれかが子となる<br>|<br>|
|collection<br>|D:<br>|要素<br>|WebDAVコレクションを表す<br>|WebDAVコレクション作成時に設定する<br>|
|odata<br>|dc:<br>|要素<br>|ODataコレクションを表す<br>|ODataコレクション作成時に設定する<br>|
|service<br>|dc:<br>|要素<br>|Serviceコレクションを表す<br>|Serviceコレクション作成時に設定する<br>|
##### DTD表記
###### 名前空間D:
```dtd
<!ELEMENT mkcol (set) >
<!ELEMENT set (prop) >
<!ELEMENT prop (resourcetype) >
<!ELEMENT resourcetype (collection or odata or service) >
<!ELEMENT collection EMPTY>       
```
###### 名前空間 dc:
```dtd
<!ELEMENT odata EMPTY>
<!ELEMENT service EMPTY>
```
#### リクエストサンプル
```xml
<?xml version="1.0" encoding="utf-8"?>
<D:mkcol xmlns:D="DAV:" xmlns:dc="urn:x-dc1:xmlns">
  <D:set>
    <D:prop>
      <D:resourcetype>
        <D:collection/>
      </D:resourcetype>
    </D:prop>
  </D:set>
</D:mkcol>
```

<br>
### レスポンス
#### ステータスコード
201
#### レスポンスヘッダ
##### 共通レスポンスヘッダ
|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|X-Dc-Version<br>|APIの実行バージョン<br>|リクエストが処理されたAPIバージョン<br>|
|Access-Control-Allow-Origin<br>|クロスドメイン通信許可ヘッダ<br>|返却値は"*"固定<br>|
##### WebDAV共通レスポンスヘッダ
|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|Content-Type<br>|返却されるデータの形式<br>|テスト未実施<br>|
|Location<br>|作成したコレクションのリソースURL<br>|正常にコレクションが作成できた場合のみ返却する<br>|
|ETag<br>|リソースのバージョン情報<br>|正常にコレクションが作成できた場合のみ返却する<br>|
#### レスポンスボディ
なし
#### エラーメッセージ一覧
[エラーメッセージ一覧](198_Error_Messages.html)を参照

|コード<br>|メッセージ<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|
|400<br>|Bad Request<br>|リクエストボディの形式が不正<br>リクエストヘッダの形式が不正<br>|<br>|
|401<br>|Unauthorized<br>|認証トークンが無効<br>|<br>|
|403<br>|Forbidden<br>|アクセス権限が不足している場合<br>|<br>|
|404<br>|Not Found<br>|存在しないリソースを指定<br>|<br>|
|405<br>|Method Not Allowed<br>|許可していないリクエストメソッドを指定<br>|<br>|
|412<br>|Precondition Failed<br>|存在しないバージョンを指定<br>|<br>|
#### レスポンスサンプル
なし

<br>
### CURLサンプル
#### CURLコマンド(UNIX)
```sh
curl 'https://fqdn/cell_name/box_name/collection_name' -X MKCOL -v \ -H 'Authorization:Bearer auth_token'\
-d '<?xml version="1.0" encoding="utf-8"?><D:mkcol xmlns:D="DAV:" xmlns:dc="urn:x-dc1:xmlns"><D:set><D:prop>
<D:resourcetype><D:collection/></D:resourcetype></D:prop></D:set></D:mkcol>'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
