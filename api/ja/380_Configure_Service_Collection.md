# サービスコレクションソース設定適用
### 概要
サービスコレクションソースの設定を適用する
### 制限事項
未稿
### 必要な権限
write-properties

<br>
### リクエスト
#### リクエストURL
```
/{CellName}/{BoxName}/{CollectionName}
```
|パス<br>|概要<br>|備考<br>|
|:--|:--|:--|
|{CellName}<br>|セル名<br>| <br>|
|{BoxName}<br>|ボックス名<br>| <br>|
|{CollectionName}<br>|サービスコレクション名<br>|有効値 桁数:1&#65374;128<br>使用可能文字種<br>半角英数字、半角ピリオド(.)、半角アンダーバー(_)、半角ハイフン(-)<br>|
#### メソッド
PROPPATCH
#### リクエストクエリ
##### 共通リクエストクエリ
|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|p_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|
#### リクエストヘッダ
##### 共通リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用されます。<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定します。<br>|
|X-Personium-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
##### サービスコレクション設定固有リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {AccessToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
|Content-Type<br>|コンテンツ形式を指定する<br>|application/xml<br>|×<br>| <br>
|Accept<br>|レスポンスで受け入れ可能なメディアタイプを指定する  <br>|application/xml<br>|×<br>| <br>|
#### リクエストボディ
名前空間

|URI<br>|概要<br>|参考prefix<br>|
|:--|:--|:--|
|DAV:<br>|WebDAVの名前空間<br>|D:<br>|
|urn:x-Personium:xmlns<br>|Personium APIの名前空間<br>|p:<br>|
|http://www.w3.com/standards/z39.50/<br>|proppatchの名前空間<br>|Z:<br>|
※ 参考prefixは以下表の可読性を高めるためのもので、このprefix文字列の使用を保証するものでも要求するものでもありません。


XMLの構造  
ボディはXMLで、以下のスキーマに従っています。

|ノード名<br>|名前空間<br>|ノードタイプ<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|:--|
|propertyupdate<br>|D:<br>|要素<br>|propertyupdateのルートを表し、setとremoveが子となる<br>| <br>|
|set<br>|D:<br>|要素<br>|プロパティ設定を表し、1つ以上複数のpropが子となる<br>| <br>|
|remove<br>|D:<br>|要素<br>|プロパティ削除設定を表し、1つ以上複数のpropが子となる<br>| <br>|
|prop<br>|D:<br>|要素<br>|プロパティ値を表し、1つ以上複数の任意の要素が子となる<br>|set時：子のノード名がキーとなる<br>remove時：子のノードを名キーとして削除を行う<br>|
DTD表記
```dtd
<!ELEMENT propertyupdate (set, remove)
<!ELEMENT set (prop*) >
<!ELEMENT remove (prop*) >
<!ELEMENT prop ANY>
```
#### サービスコレクション設定固有定義
|ノード名<br>|名前空間<br>|ノードタイプ<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|:--|
|service  <br>|p:<br>|要素  <br>|サービス設定を表し、1つ以上複数のpath要素を子とする   <br>| <br>|
|language  <br>|p:<br>|属性  <br>|サービスソース言語設定を表し、&quot;JavaScript&quot;を固定で属性値とする   <br>| <br>|
|subject  <br>|p:<br>|属性  <br>|サービスサブジェクト設定を表し、属するセルに登録済みのAccount名を属性値とする   <br>|ロジック内のPersonium APIを設定したAccountに紐付くRole権限で実行する<br>|
|path  <br>|p:<br>|要素<br>|サービスコレクション設定を表す。   <br>| <br>|
|name  <br>|p:<br>|属性<br>|サービス呼び出し名を表し、任意の文字を属性値とする  <br>|この設定値がサービス実行時のリクエストURLの"__src/"直下パス名になります。<br>|
|src<br>|p:<br>|属性<br>|サービスソースファイル名を表し、__src配下に配備されているファイル名を属性値とする  <br>| <br>|
DTD表記
```dtd
<!ELEMENT service (path*)>
<!ATTLIST service language CDATA "JavaScript">
<!ATTLIST service subject CDATA #IMPLIED>
<!ELEMENT path EMPTY>
<!ATTLIST path name CDATA #REQUIRED>
<!ATTLIST path src CDATA #REQUIRED>
```
#### リクエストサンプル
```xml
<D:propertyupdate xmlns:D="DAV:"
    xmlns:p="urn:x-Personium:xmlns"
    xmlns:Z="http://www.w3.com/standards/z39.50/">
    <D:set>
        <D:prop>
          <p:service language="JavaScript">
            <p:path name="${name}" src="${src}"/>
          </p:service>
        </D:prop>
    </D:set>
</D:propertyupdate>
```

<br>
### レスポンス
#### ステータスコード
207
#### レスポンスヘッダ
なし
#### レスポンスボディ
名前空間

|URI<br>|概要<br>|参考prefix<br>|
|:--|:--|:--|
|DAV:<br>|WebDAVの名前空間<br>|D:<br>|
※ 参考prefixは以下表の可読性を高めるためのもので、このprefix文字列の使用を保証するものでも要求するものでもありません。


XMLの構造
ボディはXMLで、以下のスキーマに従っています。

|ノード名<br>|名前空間<br>|ノードタイプ<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|:--|
|multistatus  <br>|D:<br>|要素<br>|multistatusのルートを表し、1つ以上複数のresponseが子となる  <br>| <br>|
|response<br>|D:<br>|要素<br>|multistatusの内容を表し、hrefとpropstatが子となる<br>| <br>|
|href<br>|D:<br>|要素<br>|PROPPATCHを実行したリソースのURL   <br>| <br>|
|propstat  <br>|D:<br>|要素<br>|プロパティ設定結果を表し、propとstatusが子となる  <br>| <br>|
|prop  <br>|D:<br>|要素<br>|プロパティ設定内容を表す<br>|リソース設定の結果を以下のように表示する<br>設定成功：設定したキーと値<br>削除成功：削除したキー<br>|
|status<br>|D:<br>|要素<br>|プロパティ設定ステータスコード<br>|設定成功の場合200(OK)が返る  <br>|
DTD表記
```dtd
<!ELEMENT multistatus (response*)>
<!ELEMENT response (href, propstat)>
<!ELEMENT href (#PCDATA)>
<!ELEMENT propstat (prop, status)>
<!ELEMENT prop ANY>
<!ELEMENT status (#PCDATA)>
```
#### レスポンスサンプル
```xml
<multistatus xmlns="DAV:">
    <response>
        <href>https://{UnitFQDN}/{CellName}/{BoxName}/{CollectionName}</href>
        <propstat>
            <prop>
                <p:service language="JavaScript" xmlns:p="urn:x-Personium:xmlns" xmlns:D="DAV:" xmlns:Z="http://www.w3.com/standards/z39.50/">
                    <p:path name="sample" src="sample.js"/>
                </p:service>
            </prop>
            <status>HTTP/1.1 200 OK</status>
        </propstat>
    </response>
</multistatus>
```
#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

### cURLサンプル

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{CollectionName}" -X PROPPATCH -i -H "Authorization:Bearer {AccessToken}" -H "Accept:application/json" -d "<?xml version=\"1.0\" encoding=\"utf-8\" ?><D:propertyupdate xmlns:D=\"DAV:\" xmlns:p=\"urn:x-Personium:xmlns\" xmlns:Z=\"http://www.w3.com/standards/z39.50/\"><D:set><D:prop><p:service language=\"JavaScript\"><p:path name=\"sample\" src=\"sample.js\"/></p:service></D:prop></D:set></D:propertyupdate>"
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
