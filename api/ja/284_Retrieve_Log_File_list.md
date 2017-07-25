# ログファイル一覧取得
### 概要
指定されたURL配下のログファイルの一覧を取得する
### 必要な権限
log-read
### 制限事項
* コレクションの階層の深さの最大値は50
* 各コレクション配下の子要素数の最大値は1コレクションあたり10,000
	- Box直下のコレクションを1階層目とし、以下2階層目、3階層目…とカウントする。コレクション配下に作成するファイルは、階層としてカウントしない。
* 内部イベントのログ出力は未サポート
* ログの出力設定、および出力設定の参照は未サポート
  - ログファイル名は"default.log"固定とする
  - ローテートは以下のデフォルト設定に従い行う
    - ローテートのサイズ設定:50MB
* ログの出力レベルは"info"固定（INFO, WARN, ERRORすべて出力）とする
* ローテート時のファイル名は、 default.log.{timestamp} とする。 {timestamp}は、ローテートされたときの時刻で採番される。

|アクション<br>|アーカイブされたログファイル<br>|説明<br>|備考<br>|
|:--|:--|:--|:--|
|First Rotation<br>|archive/<br>default.log.1402910774659<br>|<br>新規にローテートされたファイル<br>|<br>2014-06-16 18:26:14 +0900<br>|
|2nd Rotation<br>|archive/<br>default.log.1402910774659<br>default.log.1403910784659<br>|<br>前回ローテートされたファイル<br>新規にローテートされたファイル<br>|<br>2014-06-16 18:26:14 +0900<br>2014-06-28 08:13:04 +0900<br>|
|3rd Rotation<br>|archive/<br>default.log.1402910774659<br>default.log.1403910784659<br>default.log.1403910784659<br>|<br>前々回にローテートされたファイル<br>前回ローテートされたファイル<br>新規にローテートされたファイル<br>|<br>2014-06-16 18:26:14 +0900<br>2014-06-28 08:13:04 +0900<br>2014-07-09 21:59:44 +0900<br>|

<br>
### リクエスト
#### リクエストURL
##### 最新のログファイルの一覧を取得
```
/{CellName}/__log/current
```
##### ローテートされたログファイルの一覧を取得
```
/{CellName}/__log/archive
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
|allprop<br>|D:<br>|要素<br>|全プロパティを取得設定を表す<br>|allprop・・・すべてのプロパティを取得する<br>リクエストボディが空の場合も、allpropとして扱う<br>allprop以外の要素はv1.2系、v1.1系未対応<br>|
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
|207<br>|Multi-Status<br>|取得成功時<br>|
#### レスポンスヘッダ
|項目名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|Content-Type<br>|Resourceのデータ形式に応じたMimeType<br>|"application/xml"<br>|
|Access-Control-Allow-Origin<br>|クロスドメイン通信許可ヘッダ<br>|返却値は"*"固定<br>|
|X-Personium-Version<br>|APIの実行バージョン<br>|有効なバージョン|

#### レスポンスボディ
##### 名前空間
|URI<br>|概要<br>|参考Prefix<br>|
|:--|:--|:--|
|DAV:<br>|WebDAVの名前空間<br>|D:<br>|
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
|status<br>|D:<br>|要素<br>|リソース取得のレスポンスコードを表す<br>|<br>|
|prop<br>|D:<br>|要素<br>|プロパティ詳細情報を表し、creationdateとresourcetypeとaclとproppatch設定値が子となる<br>|<br>|
|creationdate<br>|D:<br>|要素<br>|リソース作成時刻<br>|<br>|
|getcontentlength<br>|D:<br>|要素<br>|リソースのサイズ<br>|リソースがファイルの場合のみ<br>|
|getcontenttype<br>|p:<br>|要素<br>|リソースのcontenttype<br>|リソースがファイルの場合のみ<br>|
|getlastmodified<br>|p:<br>|要素<br>|リソース更新時刻<br>|<br>|
|resourcetype<br>|p:<br>|要素<br>|リソースのタイプを表す。<br>collectionと、odataかserviceのいづれかが子となるか、子は空となる<br>|<br>|
|collection<br>|p:<br>|要素<br>|リソースのタイプがコレクションであることを表す<br>|リソースがWebDAVの場合、この要素のみが表示される<br>|
|odata<br>|p:<br>|要素<br>|リソースのタイプがODataコレクションであることを表す<br>|ODataコレクションの場合表示<br>|
|service<br>|p:<br>|要素<br>|リソースのタイプがサービスコレクションであることを表す<br>|Serviceコレクションの場合表示<br>|
|acl<br>|p:<br>|要素<br>|リソースに設定されているACL設定<br>|ACL設定を取得するためには、対象リソースに対するacl-read権限が必要 ACL要素以下の内容については、[セルレベルアクセス制御設定API](289_Cell_ACL.html)を参照<br>|
|base<br>|p:<br>|要素<br>|ACLのPrivilegeのBaseURL<br>|CellへのPROPFINDの場合、デフォルトボックス（"__"）のリソースURL<br>|
##### DTD表記
##### 名前空間：D:
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
##### 名前空間:p:
```dtd
<!ELEMENT odata EMPTY>
<!ELEMENT service EMPTY>
```
##### 名前空間：xml:
```dtd
<!ATTLIST acl base CDATA #IMPLIED>
```
#### レスポンスサンプル
##### resourcetype要素
```xml
<?xml version="1.0" encoding="utf-8"?>
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
なお、ログファイルのローテート時にファイルのZIP圧縮有無を指定可能とする予定（ログ設定更新API）
この際、ログファイルの圧縮有無によって、href要素のファイル名と、getcontenttype要素のMimeTypeが切り替わる

|ZIP圧縮有無<br>|href要素のファイル名(例)<br>|getcontenttypeの値<br>|備考<br>|
|:--|:--|:--|:--|
|圧縮なし<br>|default.log.1364460341902<br>|text/csv<br>|ローテートなしの場合も同様<br>|
|圧縮あり<br>|default.log.1364460341902.zip<br>|application/zip<br>|<br>|
#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

|コード<br>|メッセージ<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|
|400<br>|Bad Request<br>|リクエストボディの形式が不正<br>リクエストヘッダの形式が不正<br>既に別のスキーマ型のIndexが作成されている場合<br>|<br>|
|401<br>|Unauthorized<br>|認証トークンが無効<br>|<br>|
|403<br>|Forbidden<br>|アクセス権限が不足している場合<br>|<br>|
|404<br>|Not Found<br>|存在しないリソースを指定<br>|<br>|
|405<br>|Method Not Allowed<br>|許可していないリクエストメソッドを指定<br>|<br>|

<br>
### CURLサンプル

```sh
curl "https://{UnitFQDN}/{CellName}/__log/archive" -X PROPFIND -i -H 'Depth:1' -H 'Authorization: Bearer {UnitUserToken}' -H 'Accept: application/json'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
