# コレクション設定変更
### 概要
コレクションの設定を変更する
### 必要な権限
write-properties
### 制限事項
未稿

<br>
### リクエスト
#### リクエストURL
```
/{CellName}
```
または、
```
/{CellName}/{BoxName}
```
または、
```
/{CellName}/{BoxName}/{ResourcePath}
```
|パス<br>|概要<br>|備考<br>|
|:--|:--|:--|
|{CellName}<br>|セル名<br>| <br>
|{BoxName}<br>|ボックス名<br>| <br>
|{ResourcePath}<br>|リソースへのパス<br>|有効値 桁数:1&#65374;128<br>使用可能文字種<br>半角英数字、半角ピリオド(.)、半角アンダーバー(_)、半角ハイフン(-)<br>|
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
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用される<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定する<br>|
|X-Personium-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
##### 個別リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {AccessToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|

|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Content-Type<br>|コンテンツ形式を指定する<br>|application/xml<br>|×<br>| <br>|
|Accept<br>|レスポンスで受け入れ可能なメディアタイプを指定する<br>|application/xml<br>|×<br>| <br>|
#### リクエストボディ
##### 名前空間
|URI<br>|概要<br>|参考prefix<br>|
|:--|:--|:--|
|DAV:<br>|WebDAVの名前空間<br>|D:<br>|
|urn:x-personium:xmlns<br>|Personiumの名前空間<br>|p:<br>|
|http://www.w3.com/standards/z39.50/<br>|proppatchの名前空間<br>|Z:<br>|
※ 参考prefixは以下表の可読性を高めるためのもので、このprefix文字列の使用を保証するものでも要求するものでもありません。
##### XMLの構造
|ノード名<br>|名前空間<br>|ノードタイプ<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|:--|
|propertyupdate<br>|D:<br>|要素<br>|propertyupdateのルートを表し、setとremoveが子となる<br>| <br>|
|set<br>|D:<br>|要素<br>|プロパティ設定を表し、1つ以上複数のpropが子となる<br>| <br>|
|remove<br>|D:<br>|要素<br>|プロパティ削除設定を表し、1つ以上複数のpropが子となる<br>| <br>|
|prop<br>|D:<br>|要素<br>|プロパティ値を表し、1つ以上複数の任意の要素が子となる<br>|set時：子のノード名がキーとなる<br>remove時：子のノードを名キーとして削除を行う<br>|
##### DTD表記
```dtd
<!ELEMENT propertyupdate (set, remove) >
<!ELEMENT set (prop*) >
<!ELEMENT remove (prop*) >
<!ELEMENT prop ANY>
```
#### リクエストサンプル
```xml
<D:propertyupdate xmlns:D="DAV:"
    xmlns:p="urn:x-personium:xmlns"
    xmlns:Z="http://www.w3.com/standards/z39.50/">
    <D:set>
        <D:prop>
            <Z:Author>Author1 update</Z:Author>
            <p:hoge>fuga</p:hoge>
        </D:prop>
    </D:set>
    <D:remove>
        <D:prop>
            <Z:Author/>
            <p:hoge/>
        </D:prop>
    </D:remove>
</D:propertyupdate>
```

<br>
### レスポンス
#### ステータスコード
|コード<br>|メッセージ<br>|概要<br>|
|:--|:--|:--|
|207<br>|Multi-Status<br>|成功<br>|
#### レスポンスヘッダ
未稿
#### レスポンスボディ
##### 名前空間
|URI<br>|概要<br>|参考prefix<br>|
|:--|:--|:--|
|DAV:<br>|WebDAVの名前空間<br>|D:<br>|
※ 参考prefixは以下表の可読性を高めるためのもので、このprefix文字列の使用を保証するものでも要求するものでもありません。
##### XMLの構造
ボディはXMLで、以下のスキーマに従っています。

|ノード名<br>|名前空間<br>|ノードタイプ<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|:--|
|multistatus<br>|D:<br>|要素<br>|multistatusのルートを表し、1つ以上複数のresponseが子となる<br>| <br>|
|response<br>|D:<br>|要素<br>|multistatusの内容を表し、hrefとpropstatが子となる<br>| <br>|
|href<br>|D:<br>|要素<br>|PROPPATCHを実行したリソースのURL<br>| <br>|
|propstat<br>|D:<br>|要素<br>|プロパティ設定結果を表し、propとstatusが子となる<br>| <br>|
|prop<br>|D:<br>|要素<br>|プロパティ設定内容を表す  <br>|リソース設定の結果を以下のように表示する<br>設定成功：設定したキーと値<br>削除成功：削除したキー<br>|
|status<br>|D:<br>|要素<br>|プロパティ設定ステータスコード<br>|設定成功の場合200(OK)が返る<br>|
##### DTD表記
##### 名前空間 D:
```dtd
<!ELEMENT multistatus (response*)>
<!ELEMENT response (href, propstat)>
<!ELEMENT href (#PCDATA)>
<!ELEMENT propstat (prop, status)>
<!ELEMENT prop ANY>
<!ELEMENT status (#PCDATA)   
```
#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

|コード<br>|メッセージ<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|
|400<br>|Bad Request<br>|リクエストボディの形式が不正<br>リクエストヘッダの形式が不正<br>| <br>|
|401<br>|Unauthorized<br>|認証トークンが無効<br>| <br>|
|403<br>|Forbidden<br>|アクセス権限が不足している場合<br>| <br>|
|404<br>|Not Found<br>|存在しないリソースを指定<br>| <br>|
|405<br>|Method Not Allowed<br>|許可していないリクエストメソッドを指定<br>| <br>|
|412<br>|Precondition Failed<br>|存在しないバージョンを指定<br>| <br>|
|503<br>|Too many concurrent requests.<br>|更新系の処理が競合している場合<br>| <br>|
#### レスポンスサンプル
```xml
<multistatus xmlns="DAV:">
    <response>
        <href>https://{CellName}/{BoxName}/{ResourcePath}</href>
        <propstat>
            <prop>
                <Z:Author xmlns:p="urn:x-personium:xmlns" xmlns:D="DAV:" xmlns:Z="http://www.w3.com/standards/z39.50/">author1</Z:Author>
                <p:hoge xmlns:p="urn:x-personium:xmlns" xmlns:D="DAV:" xmlns:Z="http://www.w3.com/standards/z39.50/">foo</p:hoge>
                <Z:Author xmlns:p="urn:x-personium:xmlns" xmlns:D="DAV:" xmlns:Z="http://www.w3.com/standards/z39.50/"/>
                <p:hoge xmlns:p="urn:x-personium:xmlns" xmlns:D="DAV:" xmlns:Z="http://www.w3.com/standards/z39.50/"/>
            </prop>
            <status>HTTP/1.1 200 OK</status>
        </propstat>
    </response>
</multistatus>
```
<br>
### CURLサンプル

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{ResourcePath}' -X PROPPATCH -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '<?xml version="1.0" encoding="utf-8" ?><D:propertyupdate xmlns:D="DAV:" xmlns:p="urn:x-personium:xmlns" xmlns:Z="http://www.w3.com/standards/z39.50/"><D:set><D:prop><Z:Author>${author1}</Z:Author><p:hoge>${hoge}</p:hoge></D:prop></D:set><D:remove><D:prop><Z:Author/><p:hoge/></D:prop></D:remove></D:propertyupdate>'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
