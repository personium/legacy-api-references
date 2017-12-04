# Cellスナップショットファイル設定取得
### 概要
Cellスナップショットファイルプロパティを取得する。

### 必要な権限
root

### 制限事項
* レスポンスボディで返却するプロパティを指定する機能（現状allpropとなる）

<br>
### リクエスト
#### リクエストURL
```
/{CellName}/__snapshot
```
または、
```
/{CellName}/__snapshot/{FileName}
```

#### メソッド
PROPFIND

#### リクエストクエリ
|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|p_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|

#### リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用される<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定する<br>|
|X-Personium-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {AccessToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
|Depth<br>|取得するリソースの階層<br>|0:対象のリソース自身 <br>1:対象のリソースとそれの直下のリソース<br>|○<br>|<br>|

#### リクエストボディ
名前空間

|URI<br>|概要<br>|参考prefix<br>|
|:--|:--|:--|
|DAV:<br>|WebDAVの名前空間<br>|D:<br>|

※ 参考prefixは以下表の可読性を高めるためのもので、このprefix文字列の使用を保証するものでも要求するものでもありません。

XMLの構造

ボディはXMLで、以下のスキーマに従っています。

|ノード名<br>|名前空間<br>|ノードタイプ<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|:--|
|propfind<br>|D:<br>|要素<br>|propfindのルート要素を表し、allpropが子となる  <br>| <br>
|allprop  <br>|D:<br>|要素<br>|全プロパティを取得設定を表す  <br>|allprop・・・すべてのプロパティを取得する<br>リクエストボディが空の場合も、allpropとして扱う<br>allprop以外の要素は未対応<br>|

DTD表記
```dtd
<!ELEMENT propfind (allprop) >
<!ELEMENT allprop ENPTY >
```

#### リクエストサンプル
```xml
<?xml version="1.0" encoding="utf-8"?>
<D:propfind xmlns:D="DAV:">
  <D:allprop/>
</D:propfind>
```
<br>
### レスポンス
#### ステータスコード
|コード<br>|メッセージ<br>|概要<br>|
|:--|:--|:--|
|207<br>|Multi-Status<br>|成功<br>|

#### レスポンスヘッダ
|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|Content-Type<br>|返却されるデータの形式<br>|<br>|
|Access-Control-Allow-Origin<br>|クロスドメイン通信許可ヘッダ<br>|返却値は"*"固定<br>|
|X-Personium-Version<br>|APIの実行バージョン<br>|リクエストが処理されたAPIバージョン<br>|

#### レスポンスボディ
名前空間

|URI<br>|概要<br>|参考prefix<br>|
|:--|:--|:--|
|DAV:<br>|WebDAVの名前空間<br>|D:<br>|
|urn:x-personium:xmlns<br>|Personiumの名前空間<br>|p:<br>|

※ 参考prefixは以下表の可読性を高めるためのもので、このprefix文字列の使用を保証するものでも要求するものでもありません。


XMLの構造

ボディはXMLで、以下のスキーマに従っています。

|ノード名<br>|名前空間<br>|ノードタイプ<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|:--|
|multistatus<br>|D:<br>|要素<br>|multistatusのルートを表し、1つ以上複数のresponseが子となる<br>| <br>|
|response<br>|D:<br>|要素<br>|リソース取得のレスポンスを表し、hrefとpropstatが子となる<br>| <br>|
|href<br>|D:<br>|要素<br>|リソースのurl<br>| <br>|
|propstat<br>|D:<br>|要素<br>|リソースのプロパティ情報を表し、statusとpropが子となる<br>| <br>|
|status<br>|D:<br>|要素<br>|リソース取得のレスポンスコードを表す<br>| <br>|
|prop<br>|D:<br>|要素<br>|プロパティ詳細情報を表し、creationdateとresourcetypeとaclとproppatch設定値が子となる<br>| <br>|
|creationdate<br>|D:<br>|要素<br>|リソース作成時刻<br>| <br>|
|getcontentlength<br>|D:<br>|要素<br>|リソースのサイズ<br>|リソースがファイルの場合のみ<br>|
|getcontenttype<br>|D:<br>|要素<br>|リソースのcontenttype<br>|リソースがファイルの場合のみ<br>|
|getlastmodified<br>|D:<br>|要素<br>|リソース更新時刻<br>| <br>|
|resourcetype<br>|D:<br>|要素<br>|リソースのタイプを表す。<br>collectionが子となるか、子は空となる<br>| <br>|
|collection<br>|D:<br>|要素<br>|リソースのタイプがコレクションであることを表す<br>|リソースがWebDAVの場合、この要素のみが表示される<br>|
|acl<br>|D:<br>|要素<br>|リソースに設定されているACL設定<br>|ACL要素以下の内容については、[Cell Level アクセス制御設定API](289_Cell_ACL.html)を参照<br>|
|base<br>|xml:<br>|属性<br>|ACLのPrivilegeのBaseURL<br>|CellへのPROPFINDの場合、デフォルトボックス（"__"）のリソースURL<br>|

DTD表記

名前空間　D:
```dtd
<!ELEMENT multistatus (response*)>
<!ELEMENT response (href, propstat)>
<!ELEMENT href (#PCDATA)>
<!ELEMENT propstat (status, prop)>
<!ELEMENT status (#PCDATA)>
<!ELEMENT prop (creationdate, resourcetype, ANY)>
<!ELEMENT creationdate (#PCDATA)>
<!ELEMENT getcontentlength (#PCDATA)>
<!ELEMENT getcontenttype (#PCDATA)>
<!ELEMENT getlastmodified (#PCDATA)>
<!ELEMENT resourcetype ((collection or EMPTY))>
<!ELEMENT collection EMPTY>
<!ELEMENT acl (ace*)>
```

#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

#### レスポンスサンプル
```xml
<multistatus xmlns="DAV:">
    <response>
        <href>https://{UnitFQDN}/{CellName}/__snapshot/CellExport_2017_01.zip</href>
        <propstat>
            <prop>
                <creationdate>2017-02-15T01:52:34.635+0000</creationdate>
                <getcontentlength>2000000</getcontentlength>
                <getcontenttype>application/zip</getcontenttype>
                <getlastmodified>Wed, 15 Feb 2017 01:52:34 GMT</getlastmodified>
                <resourcetype/>
                <acl xmlns:p="urn:x-personium:xmlns"/>
            </prop>
            <status>HTTP/1.1 200 OK</status>
        </propstat>
    </response>
</multistatus>
```
<br>
### cURLサンプル
```sh
curl "https://{UnitFQDN}/{CellName}/__snapshot/CellExport_2017_01.zip" -X PROPFIND -i  -H 'Depth:0' -H 'Authorization: Bearer {AccessToken}' -d '<?xml version="1.0" encoding="utf-8"?><D:propfind xmlns:D="DAV:"><D:allprop/></D:propfind>'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
