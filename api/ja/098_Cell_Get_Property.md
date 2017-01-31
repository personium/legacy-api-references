﻿﻿# プロパティ取得
### 概要
セルのプロパティを取得する

### 必要な権限
propfind
* ACLの設定状況を取得する場合は、合わせてacl-readが必要

### 制限事項
V1.0版での制限
* 対象リソースが返却出来るプロパティの一覧を返却する機能
* レスポンスボディで返却するプロパティを指定する機能
	- 現状allpropとなる

<br>
### リクエスト
#### リクエストURL
```
/{CellName}
```

|Path<br>|概要<br>|
|:--|:--|
|{CellName}<br>|セル名<br>|

#### メソッド
PROPFIND

#### リクエストクエリ

|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|dc_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|
#### リクエストヘッダ

|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用されます。<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定します。<br>|
|X-Dc-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {UnitUserToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
|Depth<br>|取得するリソースの階層<br>|0:対象のリソース自身<br>1:対象のリソースとそれの直下のリソース<br>|○<br>|<br>|
#### リクエストボディ
##### 名前空間

|URI<br>|概要<br>|備考()prefix<br>|
|:--|:--|:--|
|DAV:<br>|WebDAVの名前空間<br>|D:<br>|
※ 参考prefixは以下表の可読性を高めるためのもので、このprefix文字列の使用を保証するものでも要求するものでもありません。

##### XMLの構造
ボディはXMLで、以下のスキーマに従っています。

|ノード名<br>|Namespace<br>|ノードタイプ<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|:--|
|propfind<br>|D:<br>|要素<br>|propfindのルート要素を表し、allpropが子となる。<br>|<br>|
|allprop<br>|<br>|要素<br>|全プロパティを取得設定を表す<br>|allprop・・・すべてのプロパティを取得する<br>リクエストボディが空の場合も、allpropとして扱う<br>allprop以外の要素はv1.2系、v1.1系未対応<br>|

##### DTD表記
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

|項目名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|Content-Type<br>|返却されるデータの形式<br>|<br>|
|DataServiceVersion<br>|ODataのバージョン情報<br>|正常にEntityが取得できた場合のみ返却する<br>|
#### レスポンスボディ
名前空間

|項目名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|Content-Type<br>|返却されるデータの形式<br>|D:<br>|
|urn:x-dc1:xmlns<br>|personium.ioの名前空間<br>|dc:<br>|

※ 参考prefixは以下表の可読性を高めるためのもので、このprefix文字列の使用を保証するものでも要求するものでもありません。

##### XMLの構造
ボディはXMLで、以下のスキーマに従っています。

|ノード名<br>|Namespace<br>|ノードタイプ<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|:--|
|multistatus<br>|D:<br>|要素<br>|multistatusのルートを表し、1つ以上複数のresponseが子となる<br>|<br>|
|response<br>|D:<br>|要素<br>|リソース取得のレスポンスを表し、hrefとpropstatが子となる<br>|<br>|
|href<br>|D:<br>|要素<br>|リソースのurl<br>|<br>|
|propstat<br>|D:<br>|要素<br>|リソースのプロパティ情報を表し、statusとpropが子となる<br>|<br>|
|status<br>|D:<br>|要素<br>|リソース取得のレスポンスコードを表す<br>|<br>|
|prop<br>|D:<br>|要素<br>|プロパティ詳細情報を表し、creationdateとresourcetypeとaclとproppatch設定値が子となる<br>|<br>|
|creationdate<br>|D:<br>|要素<br>|リソース作成時刻<br>|<br>|
|getcontentlength<br>|D:<br>|要素<br>|リソースのサイズ<br>|リソースがファイルの場合のみ<br>|
|getcontenttype<br>|dc:<br>|要素<br>|リソースのcontenttype<br>|リソースがファイルの場合のみ<br>|
|getlastmodified<br>|dc:<br>|要素<br>|リソース更新時刻<br>|<br>|
|resourcetype<br>|dc:<br>|要素<br>|リソースのタイプを表す。 collection、odata、serviceのいずれかが子となるか、子は空となる<br>|<br>|
|collection<br>|dc:<br>|要素<br>|リソースのタイプがコレクションであることを表す<br>リソースがWebDAVの場合、この要素のみが表示される<br>|<br|
|odata<br>|dc:<br>|要素<br>|リソースのタイプがODataコレクションであることを表す<br>|ODataコレクションの場合表示<br>|
|service<br>|dc:<br>|要素<br>|リソースのタイプがサービスコレクションであることを表す<br>|Serviceコレクションの場合表示<br>|
|acl<br>|dc:<br>|要素<br>|リソースに設定されているACL設定<br>|ACL設定を取得するためには、対象リソースに対するacl-read権限が必要<br>ACL要素以下の内容については、セルレベルアクセス制御設定APIを参照<br>|
|base<br>|dc:<br>|要素<br>|ACLのPrivilegeのBaseURL<br>|CellへのPROPFINDの場合、デフォルトボックス（"__"）のリソースURL<br>|

##### DTD表記
名前空間：D:
```dtd
<!ELEMENT multistatus (response*)>
<!ELEMENT response (href, propstat)>
<!ELEMENT href (#PCDATA)>
<!ELEMENT propstat (status, prop)>
<!ELEMENT status (#PCDATA)>
<!ELEMENT prop (creationdate, resourcetype, acl, ANY)>
<!ELEMENT creationdate (#PCDATA)>
<!ELEMENT getcontentlength (#PCDATA)>
<!ELEMENT getcontenttype (#PCDATA)>
<!ELEMENT getlastmodified (#PCDATA)>
<!ELEMENT resourcetype ((collection, (odata or service) or EMPTY))>
<!ELEMENT collection EMPTY>
<!ELEMENT acl (ace*)>
```

名前空間:dc:
```dtd
<!ELEMENT odata EMPTY>
<!ELEMENT service EMPTY>
```

名前空間：xml:
```dtd
<!ATTLIST acl base CDATA #IMPLIED>
```

#### エラーメッセージ一覧
[エラーメッセージ一覧](200_Error_Messages.html)を参照

#### レスポンスサンプル
```xml
<?xml version="1.0" encoding="utf-8"?> <multistatus xmlns="DAV:">
    <response>
        <href>https://{UnitFQDN}/{CellName}</href>
        <propstat>
            <prop>
                <creationdate>2012-05-14T15:21:27.140+0900</creationdate>
                <getlastmodified>Mon, 14 May 2012 07:05:04 GMT</getlastmodified>
                <resourcetype>
                    <collection/>
                </resourcetype>
            </prop>
            <status>HTTP/1.1 200 OK</status>
        </propstat>
    </response>
    <response>
        <href>https://{UnitFQDN}/{CellName}</href>
        <propstat>
            <prop>
                <creationdate>2012-05-14T16:05:04.265+0900</creationdate>
                <getcontentlength>0</getcontentlength>
                <getcontenttype>text/html</getcontenttype>
                <getlastmodified>Mon, 14 May 2012 07:05:04 GMT</getlastmodified>
                <resourcetype/>
            </prop>
            <status>HTTP/1.1 200 OK</status>
        </propstat>
    </response>
</multistatus>
```

<br>
### CURLサンプル
```sh
curl "https://{UnitFQDN}/{CellName}" -X PROPFIND -i -H 'Depth:1' -H 'Authorization: Bearer {UnitUserToken}' -H 'Accept: application/json' -d '<?xml version="1.0" encoding="utf-8"?><D:propfind xmlns:D="DAV:"><D:allpop/></D:propfind>'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
