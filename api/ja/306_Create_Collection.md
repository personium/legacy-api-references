# コレクション作成
### 概要
コレクションを作成する
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
/{CellName}/{BoxName}/{CollectionName}
```
|パス<br>|概要<br>|備考<br>|
|:--|:--|:--|
|{CellName}<br>|セル名<br>|<br>
|{BoxName}<br>|ボックス名<br>|<br>
|{CollectionName}<br>|コレクション名<br>|有効値 桁数:1&#65374;128<br>使用可能文字種<br>半角英数字、半角ピリオド(.)、半角アンダーバー(_)、半角ハイフン(-)<br>|
#### メソッド
MKCOL
#### リクエストクエリ
|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|p_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|
#### リクエストヘッダ
##### 共通リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用される<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定する<br>|
|X-Personium-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
##### 個別リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {AccessToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
#### リクエストボディ
##### 名前空間
|URI<br>|概要<br>|参考prefix<br>|
|:--|:--|:--|
|DAV:<br>|WebDAVの名前空間<br>|D:<br>|
|urn:x-personium:xmlns<br>|Personiumの名前空間<br>|p:<br>|
※ 参考prefixは以下表の可読性を高めるためのもので、このprefix文字列の使用を保証するものでも要求するものでもありません。
##### XMLの構造
ボディはXMLで、以下のスキーマに従っています。

|ノード名<br>|名前空間<br>|ノードタイプ<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|:--|
|mkcol <br>|D:<br>|要素<br>|mkcolのルート要素を表し、setが子となる <br>|<br>|
|set<br>|D:<br>|要素<br>|プロパティ設定を表し、propが子となる <br>|<br>|
|prop<br>|D:<br>|要素<br>|プロパティ設定値を表し、resourcetypeが子となる <br>|<br>|
|resourcetype<br>|D:<br>|要素<br>|リソースタイプ設定を表し、collection・odata・serviceのいずれかが子となる<br>|<br>|
|collection<br>|D:<br>|要素<br>|コレクションを表す<br>|collectionノードのみ指定されている場合WebDAVコレクション作成となる<br>|
|odata<br>|p:<br>|要素<br>|ODataコレクションを表す<br>|collectionノードとodataノードが指定されている場合ODataコレクション作成となる<br>|
|service<br>|p:<br>|要素<br>|Serviceコレクションを表す<br>|collectionノードとserviceノードが指定されている場合Serviceコレクション作成となる<br>|
##### DTD表記
##### 名前空間D:
```dtd
<!ELEMENT mkcol (set) >
<!ELEMENT set (prop) >
<!ELEMENT prop (resourcetype) >
<!ELEMENT resourcetype (collection or odata or service) >
<!ELEMENT collection EMPTY>       
```
##### 名前空間 p:
```dtd
<!ELEMENT odata EMPTY>
<!ELEMENT service EMPTY>
```
#### リクエストサンプル
WebDAVコレクション作成
```xml
<?xml version="1.0" encoding="utf-8"?>
<D:mkcol xmlns:D="DAV:" xmlns:p="urn:x-personium:xmlns">
  <D:set>
    <D:prop>
      <D:resourcetype>
        <D:collection/>
      </D:resourcetype>
    </D:prop>
  </D:set>
</D:mkcol>
```
ODataコレクション作成
```xml
<?xml version="1.0" encoding="utf-8"?>
<D:mkcol xmlns:D="DAV:" xmlns:p="urn:x-personium:xmlns">
  <D:set>
    <D:prop>
      <D:resourcetype>
        <D:collection/>
        <p:odata/>
      </D:resourcetype>
    </D:prop>
  </D:set>
</D:mkcol>
```
Serviceコレクション作成
```xml
<?xml version="1.0" encoding="utf-8"?>
<D:mkcol xmlns:D="DAV:" xmlns:p="urn:x-personium:xmlns">
  <D:set>
    <D:prop>
      <D:resourcetype>
        <D:collection/>
        <p:service/>
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
|Access-Control-Allow-Origin<br>|クロスドメイン通信許可ヘッダ<br>|返却値は"*"固定<br>|
|X-Personium-Version<br>|APIの実行バージョン<br>|リクエストが処理されたAPIバージョン<br>|
##### WebDAV共通レスポンスヘッダ
|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|ETag<br>|リソースのバージョン情報<br>|正常にコレクションが作成できた場合のみ返却する<br>|
#### レスポンスボディ
なし
#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

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
WebDAVコレクション作成
```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{CollectionName}" -X MKCOL -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '<?xml version="1.0" encoding="utf-8"?><D:mkcol xmlns:D="DAV:" xmlns:p="urn:x-personium:xmlns"><D:set><D:prop><D:resourcetype><D:collection/></D:resourcetype></D:prop></D:set></D:mkcol>'
```
ODataコレクション作成
```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{CollectionName}" -X MKCOL -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '<?xml version="1.0" encoding="utf-8"?><D:mkcol xmlns:D="DAV:" xmlns:p="urn:x-personium:xmlns"><D:set><D:prop><D:resourcetype><D:collection/><p:odata/></D:resourcetype></D:prop></D:set></D:mkcol>'
```
Serviceコレクション作成
```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{CollectionName}" -X MKCOL -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '<?xml version="1.0" encoding="utf-8"?><D:mkcol xmlns:D="DAV:" xmlns:p="urn:x-personium:xmlns"><D:set><D:prop><D:resourcetype><D:collection/><p:service/></D:resourcetype></D:prop></D:set></D:mkcol>'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
