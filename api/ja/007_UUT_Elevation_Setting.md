# ULUUT昇格設定
### 概要
UUT（Unit User Token）昇格設定を変更する
### 必要な権限
ユニットユーザのみ可能

### 制限事項
未稿
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
PROPPATCH

#### リクエストクエリ
なし

#### リクエストヘッダ

|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用されます。<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定します。<br>|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {UnitUserToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
|Content-Type<br>|コンテンツ形式を指定する<br>|application / xml<br>|×<br>|&#160;<br>|
|Accept<br>|レスポンスで受け入れ可能なメディアタイプを指定する<br>|application / xml<br>|×<br>|&#160;<br>|
#### リクエストボディ

|項目名<br>|Namespace<br>|概要<br>|必須<br>|有効値<br>|備考<br>|
|:--|:--|:--|:--|:--|:--|
|DAV:<br>|&#160;<br>|XML名前空間設定<br>|○<br>|"DAV:"<br>|&#160;<br>|
|urn: x-dc1: xmlns<br>|&#160;<br>|XML名前空間設定<br>|○<br>|"Urn: x-dc1: xmlns"<br>|&#160;<br>|
|http://www.w3.com/standards/z39.50/<br>|&#160;<br>|XML名前空間設定<br>|○<br>|"http://www.w3.com/standards/z39.50/"<br>|&#160;<br>|
|propertyupdate<br>|DAV:<br>|propertyupdate（アクセス制御リスト）のルート<br>|○<br>|<ELEMENT propertyupdate! (Set &#124; remove)><br>|&#160;<br>|
|set<br>|DAV:<br>|プロパティ設定<br>|×<br>|<! ELEMENT set (prop *)><br>|&#160;<br>|
|remove<br>|DAV:<br>|プロパティ削除<br>|×<br>|<! ELEMENT set (prop *)><br>|&#160;<br>|
|prop<br>|DAV:<br>|プロパティ削除値<br>|×<br>|<! ELEMENT prop ANY><br>|ANYに指定したXMLがタグをキーとして削除を行う<br>|
|prop<br>|DAV:<br>|プロパティ設定値<br>|×<br>|<! ELEMENT prop ANY><br>|ANYに指定したXMLタグがキーとなる<br>|
|ownerRepresentativeAccounts<br>|&#160;<br>|昇格設定<br>|○<br>|<! ELEMENT ownerRepresentativeAccounts (account *)><br>|&#160;<br>|
|account<br>|&#160;<br>|昇格対象アカウント設定<br>|○<br>|<! ELEMENT account ANY><br>|昇格を認めるアカウント名を値として指定する<br>|
#### XMLの構造
###### ボディはXMLで、以下のスキーマに従っています。

|ノード名<br>|Namespace<br>|ノードタイプ<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|:--|
|propertyupdate<br>|D:<br>|要素<br>|propertyupdateのルートを表し、setとremoveが子となる &#160;<br>|&#160;<br>|
|set<br>|D:<br>|要素<br>|プロパティ設定を表し、1つ以上複数のpropが子となる &#160;<br>|&#160;<br>|
|remove<br>|D:<br>|要素<br>|プロパティ削除設定を表し、1つ以上複数のpropが子となる<br>|&#160;<br>|
|prop<br>|D:<br>|要素<br>|プロパティ値を表し、1つ以上複数の任意の要素が子となる<br>|set時：子のノード名がキーとなる<br>remove時：子のノードを名キーとして削除を行う<br>|
#### DTD表記
```dtd
<!ELEMENT propertyupdate (set, remove) >
<!ELEMENT set (prop*) >
<!ELEMENT remove (prop*) >
<!ELEMENT prop ANY>
```
#### ULUUT昇格設定固有要素
###### ボディはXMLで、以下のスキーマに従っています。

|ノード名<br>|Namespace<br>|ノードタイプ<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|:--|
|ownerRepresentativeAccounts &#160;<br>|dc:<br>|要素<br>|昇格設定リストを表し、１つ以上複数のaccount要素をが子となる &#160;&#160;<br>|&#160;<br>|
|account &#160;<br>|dc:<br>|要素<br>|昇格対象アカウント設定を表し、昇格対象となるアカウント名を記述する &#160;&#160;<br>|&#160;<br>|
#### DTD表記
```dtd
<!ELEMENT ownerRepresentativeAccounts (account*)>
<!ELEMENT account (#PCDATA)>
```
#### リクエストサンプル
```xml
<D:propertyupdate xmlns:D="DAV:" xmlns:dc="urn:x-dc1:xmlns">
  <D:set>
    <D:prop>
      <dc:ownerRepresentativeAccounts><dc:account>account1</dc:account><dc:account>account2</dc:account></dc:ownerRepresentativeAccounts>
    </D:prop>
  </D:set>
</D:propertyupdate>
```
<br>
### レスポンス
#### ステータスコード

|コード<br>|メッセージ<br>|概要<br>|
|:--|:--|:--|
|207<br>|MULTI_STATUS<br>|成功<br>|
#### レスポンスヘッダ
未稿

#### レスポンスボディ
名前空間

|URI<br>|概要<br>|備考 (prefix)<br>|
|:--|:--|:--|
|multistatus<br>|WebDAVの名前空間<br>|D:<br>|

#### XMLの構造
###### ボディはXMLで、以下のスキーマに従っています。

|ノード名<br>|Namespace<br>|ノードタイプ<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|:--|
|multistatus &#160;<br>|D:<br>|要素<br>|multistatusのルートを表し、1つ以上複数のresponseが子となる &#160;&#160;<br>|&#160;<br>|
|response &#160;<br>|D:<br>|要素<br>|multistatusの内容を表し、hrefとpropstatが子となる &#160;&#160;<br>|&#160;<br>|
|href<br>|D:<br>|要素<br>|PROPPATCHを実行したリソースのURL<br>|&#160;<br>|
|propstat &#160;<br>|D:<br>|要素<br>|プロパティ設定結果を表し、propとstatusが子となる &#160;<br>|&#160;<br>|
|prop<br>|D:<br>|要素<br>|プロパティ設定内容を表す &#160;<br>|リソース設定の結果を以下のように表示する 設定成功：設定したキーと値 削除成功：削除したキー &#160;<br>|
|status<br>|D:<br>|要素<br>|プロパティ設定ステータスコード<br>|設定成功の場合200(OK)が返る<br>|
#### DTD表記
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
        <href>http://localhost:9998/testcell1/box1/patchcol</href>
        <propstat>
            <prop>
                <Z:Author xmlns:dc="urn:x-dc1:xmlns" xmlns:D="DAV:" xmlns:Z="http://www.w3.com/standards/z39.50/">Author1 update</Z:Author>
                <dc:hoge xmlns:D="DAV:" xmlns:dc="urn:x-dc1:xmlns" xmlns:Z="http://www.w3.com/standards/z39.50/">fuga</dc:hoge>
                <Z:Author xmlns:dc="urn:x-dc1:xmlns" xmlns:D="DAV:" xmlns:Z="http://www.w3.com/standards/z39.50/"/>
                <dc:hoge xmlns:D="DAV:" xmlns:dc="urn:x-dc1:xmlns" xmlns:Z="http://www.w3.com/standards/z39.50/"/>
            </prop>
            <status>HTTP/1.1 200 OK</status>
        </propstat>
    </response>
</multistatus>   
```
#### エラーメッセージ一覧
[エラーメッセージ一覧](200_Error_Messages.html)を参照

|コード<br>|メッセージ<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|
|400<br>|Bad Request<br>|リクエストボディの形式が不正&#160;<br>リクエストヘッダの形式が不正<br>|&#160;<br>|
|401<br>|Unauthorized<br>|認証トークンが無効<br>|&#160;<br>|
|403<br>|Forbidden<br>|アクセス権限が不足している場合<br>|&#160;<br>|
|404<br>|Not Found<br>|存在しないリソースを指定<br>|&#160;<br>|
|405<br>|Method Not Allowed<br>|許可していないリクエストメソッドを指定<br>|&#160;<br>|
|412<br>|Precondition Failed<br>|存在しないバージョンを指定<br>|&#160;<br>
<br>
### CURLサンプル
```sh
curl "https://{UnitFQDN}/cell -X PROPPATCH" -H 'Authorization: Bearer {UnitUserToken}' -d '<?xml version="1.0" encoding="utf-8" ?>
<D:propertyupdate xmlns:D="DAV:" xmlns:dc="urn:x-dc1:xmlns" xmlns:Z="http://www.w3.com/standards/z39.50/">
<D:set><D:prop><dc:requireSchemaAuthz>confidential</dc:requireSchemaAuthz></D:prop></D:set></D:propertyupdate>'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
