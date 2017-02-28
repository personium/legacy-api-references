# プロパティ変更
### 概要
プロパティを変更する

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

|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|p_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|
#### リクエストヘッダ

|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用されます。<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定します。<br>|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {UnitUserToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
|Content-Type<br>|コンテンツ形式を指定する<br>|application / xml<br>|×<br>|<br>|
|Accept<br>|レスポンスで受け入れ可能なメディアタイプを指定する<br>|application / xml<br>|×<br>|<br>|
#### リクエストボディ

|項目名<br>|Namespace<br>|概要<br>|必須<br>|有効値<br>|備考<br>|
|:--|:--|:--|:--|:--|:--|
|DAV:<br>|<br>|XML名前空間設定<br>|○<br>|"DAV:"<br>|<br>|
|urn: x-personium: xmlns<br>|<br>|XML名前空間設定<br>|○<br>|"Urn: x-personium: xmlns"<br>|<br>|
|http://www.w3.com/standards/z39.50/<br>|<br>|XML名前空間設定<br>|○<br>|"Http://www.w3.com/standards/z39.50/"<br>|<br>
|propertyupdate<br>|DAV:<br>|propertyupdate（アクセス制御リスト）のルート<br>|○<br>|<ELEMENT propertyupdate! (Set | remove)><br>|<br>|
|set<br>|DAV:<br>|プロパティ設定<br>|×<br>|<! ELEMENT set (prop *)><br>|<br>|
|remove<br>|DAV:<br>|プロパティ削除<br>|×<br>|<! ELEMENT set (prop *)><br>|<br>|
|prop<br>|DAV:<br>|プロパティ削除値<br>|×<br>|<! ELEMENT prop ANY><br>|ANYに指定したXMLがタグをキーとして削除を行う<br>|
|prop<br>|DAV:<br>|プロパティ設定値<br>|×<br>|<! ELEMENT prop ANY><br>|ANYに指定したXMLタグがキーとなる<br>|
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
|207<br>|MULTI_STATUS<br>|成功<br>|

#### レスポンスヘッダ
なし

#### レスポンスボディ

|項目名<br>|Namespace<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|
|multistatus<br>|DAV:<br>|レスポンスボディのルート<br>|<! ELEMENT multistatus (response *)><br>|
|response<br>|DAV:<br>|responseのルート<br>|<! ELEMENT response (href, propstat)><br>|
|href<br>|DAV:<br>|PROPPATCHを実行したリソースのURL<br>|PROPPATCHを実行したリソースのURL<br>|
|propstat<br>|DAV:<br>|プロパティ設定結果<br>|<! ELEMENT propstat (prop, status)><br>|
|prop<br>|DAV:<br>|プロパティ設定内容<br>|リソース設定の結果を以下のように表示する<br>設定成功：設定したキーと値<br>削除成功：削除したキー<br>|
|status<br>|DAV:<br>|プロパティ設定ステータスコード<br>|設定成功の場合200(OK)が返る<br>|
#### エラーメッセージ一覧
[エラーメッセージ一覧](200_Error_Messages.html)を参照

|コード<br>|メッセージ<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|
|400<br>|Bad Request<br>|リクエストボディの形式が不正<br>リクエストヘッダの形式が不正<br>|<br>|
|401<br>|Unauthorized<br>|認証トークンが無効<br>|<br>|
|403<br>|Forbidden<br>|アクセス権限が不足している場合<br>|<br>|
|404<br>|Not Found<br>|存在しないリソースを指定<br>|<br>|
|405<br>|Method Not Allowed<br>|許可していないリクエストメソッドを指定<br>|<br>|
|412<br>|Precondition Failed<br>|存在しないバージョンを指定<br>|<br>

#### レスポンスサンプル
```xml
<multistatus xmlns="DAV:">
    <response>
        <href>https://{UnitFQDN}/{CellName}/</href>
        <propstat>
            <prop>
                <Z:Author xmlns:p="urn:x-personium:xmlns" xmlns:D="DAV:" xmlns:Z="http://www.w3.com/standards/z39.50/">${author1}</Z:Author>
                <p:hoge xmlns:p="urn:x-personium:xmlns" xmlns:D="DAV:" xmlns:Z="http://www.w3.com/standards/z39.50/">${hoge}</p:hoge>
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
curl "https://{UnitFQDN}/{CellName}" -X PROPPATCH -i -H 'Authorization: Bearer {UnitUserToken}' -H 'Accept: application/json' -d '<?xml version="1.0" encoding="utf-8" ?>
<D:propertyupdate xmlns:D="DAV:" xmlns:p="urn:x-personium:xmlns" xmlns:Z="http://www.w3.com/standards/z39.50/"><D:set><D:prop><Z:Author>${author1}</Z:Author>
<p:hoge>${hoge}</p:hoge></D:prop></D:set><D:remove><D:prop><Z:Author/><p:hoge/></D:prop></D:remove></D:propertyupdate>'
```
<br>
###### Copyright 2017    FUJITSU LIMITED
