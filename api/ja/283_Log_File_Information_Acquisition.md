# ログファイル情報取得
### 概要
イベントログのログファイル情報を取得する。  
ローテートされたログファイルの保持世代数は最大12世代である。  
ログファイルのローテート時に最大保持世代数を超えた場合は、最古のログファイルが削除される。

### 必要な権限
log-read

### 制限事項
* 内部イベントのログ出力、ログの出力設定およびその参照は未サポート
* ログファイル名："default.log"（固定）
* ローテートのデフォルトサイズ設定:：50MB
* ログの出力レベル："info"（固定）（INFO, WARN, ERRORすべて出力）
* ローテート時のファイル名は、 default.log.{timestamp} とする。 {timestamp}は、ローテートされたときの時刻で採番される。

<br>
### リクエスト
#### リクエストURL
##### 最新のログファイル一覧取得
```
/{CellName}/__log/current
```

##### 最新のログファイル情報取得
```
/{CellName}/__log/current/default.log
```

##### ローテートされたログファイル一覧取得
```
/{CellName}/__log/archive
```

##### ローテートされたログファイル情報取得
```
/{CellName}/__log/archive/default.log.{timestamp}
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
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用されます。<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定します。<br>|
|X-Personium-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {UnitUserToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
|Depth<br>|取得するリソースの階層<br>|0:対象のリソース自身<br>1:対象のリソースとそれの直下のリソース<br>|○<br>|<br>|
#### リクエストボディ
なし

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
|urn:x-personium:xmlns<br>|Personiumの名前空間<br>|p:<br>|

※ 参考prefixは以下表の可読性を高めるためのもので、このprefix文字列の使用を保証するものでも要求するものでもありません。

##### XMLの構造
ボディはXMLで、以下のスキーマに従っています。

|ノード名<br>|Namespace<br>|ノードタイプ<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|:--|
|multistatus<br>|D:<br>|要素<br>|multistatusのルートを表し、1つ以上複数のresponseが子となる<br>|<br>|
|response<br>|D:<br>|要素<br>|リソース取得のレスポンスを表し、hrefとpropstatが子となる<br>|<br>|
|href<br>|D:<br>|要素<br>|リソースのurl<br>|<br>|
|propstat<br>|D:<br>|要素<br>|リソースのプロパティ情報を表し、statusとpropが子となる<br>|<br>|
|prop<br>|D:<br>|要素<br>|プロパティ詳細情報を表し、creationdateとresourcetypeとaclとproppatch設定値が子となる<br>|<br>|
|creationdate<br>|D:<br>|要素<br>|リソース作成時刻<br>|<br>|
|getcontentlength<br>|D:<br>|要素<br>|リソースのサイズ<br>|リソースがファイルの場合のみ<br>|
|getcontenttype<br>|p:<br>|要素<br>|リソースのcontenttype<br>|リソースがファイルの場合のみ<br>|
|getlastmodified<br>|p:<br>|要素<br>|リソース更新時刻<br>|<br>|
|resourcetype<br>|p:<br>|要素<br>|リソースのタイプを表す。<br>collectionと、odataかserviceのいづれかが子となるか、子は空となる<br>|<br>|
|collection<br>|p:<br>|要素<br>|リソースのタイプがコレクションであることを表す<br>|リソースがWebDAVの場合、この要素のみが表示される<br>|
|odata<br>|p:<br>|要素<br>|リソースのタイプがODataコレクションであることを表す<br>|ODataコレクションの場合表示<br>|
|service<br>|p:<br>|要素<br>|リソースのタイプがサービスコレクションであることを表す<br>|Serviceコレクションの場合表示<br>|
|acl<br>|p:<br>|要素<br>|リソースに設定されているACL設定<br>|ACL設定を取得するためには、対象リソースに対するacl-read権限が必要 ACL要素以下の内容については、[Cell Level アクセス制御設定API](289_Cell_ACL.html)を参照<br>|
|base<br>|p:<br>|要素<br>|ACLのPrivilegeのBaseURL<br>|CellへのPROPFINDの場合、デフォルトボックス（"__"）のリソースURL<br>|
|status<br>|D:<br>|要素<br>|リソース取得のレスポンスコードを表す<br>|<br>|

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

名前空間:p:
```dtd
<!ELEMENT odata EMPTY>
<!ELEMENT service EMPTY>
```

名前空間：xml:
```dtd
<!ATTLIST acl base CDATA #IMPLIED>
```

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
```xml
<multistatus xmlns="DAV:">
    <response>
        <href>https://{UnitFQDN}/{CellName}/__log/archive</href>
        <propstat>
            <prop>
                <creationdate>2017-02-03T01:27:31.093+0000</creationdate>
                <getlastmodified>Fri, 03 Feb 2017 01:27:31 GMT</getlastmodified>
                <resourcetype>
                    <collection/>
                </resourcetype>
            </prop>
            <status>HTTP/1.1 200 OK</status>
        </propstat>
    </response>
</multistatus>
```
<br>
### CURLサンプル
```sh
curl "https://{UnitFQDN}/{CellName}/__log/archive" -X PROPFIND -i -H 'Depth:1' -H 'Authorization: Bearer {UnitUserToken}' -H 'Accept: application/json' -d '<?xml version="1.0" encoding="utf-8"?><D:propfind xmlns:D="DAV:"><D:allprop/></D:propfind>'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
