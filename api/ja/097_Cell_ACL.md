# アクセス制御設定
### 概要
セルレベルのアクセス制御機能を提供する。

### 必要な権限
acl

### 制限事項
ACL設定を行うと、既存のACL設定を上書きされる形で更新されます。
* V1.0版での制限
	- ACL設定を打ち消す機能（deny）
	- ACLで設定出来るprivilegeの一覧取得

<br>
### リクエスト
#### リクエストURL
```
/{Cell_name}
```
#### メソッド
ACL

#### リクエストクエリ
共通リクエストクエリ

|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|dc_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|



#### 共通リクエストヘッダ

|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用されます。<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定します。<br>|
|X-Dc-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
#### リクエストヘッダ

|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {TokenValue}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
#### リクエストボディ
#### 名前空間

|URI<br>|概要<br>|備考 (prefix)<br>|
|:--|:--|:--|
|DAV:<br>|WebDAVの名前空間<br>|D:<br>|
|urn:x-dc1:xmlns<br>|personium.ioの名前空間<br>|dc:<br>|
※ 参考prefixは以下表の可読性を高めるためのもので、このprefix文字列の使用を保証するものでも要求するものでもありません。

#### XMLの構造
ボディはXMLで、以下のスキーマに従っています。<br>
privilegeタグ配下の権限設定の内容については、認証モデルを参照。

|ノード名<br>|Namespace<br>|ノードタイプ<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|:--|
|acl<br>|D:<br>|要素<br>|ACL（アクセス制御リスト）のルートを表し、1つ以上複数のaceが子となる<br>|<br>|
|base<br>|D:<br>|要素<br>|hrefタグ内に記述するURLの基底を表し、任意の値を属性値とする。<br>|<br>|
|ace<br>|D:<br>|要素<br>|ACE（アクセス制御エレメント）を表し、principalとgrantが一対で子となる<br>|「invert」「deny」「protected」「inherited」はV1.1系未対応<br>|
|principal<br>|D:<br>|要素<br>|権限設定対象を表し、hrefまたはallが子となる<br>|<br>|
|grant<br>|D:<br>|要素<br>|権限付与設定を表し、1つ以上複数のprivilegeが子となる<br>|<br>|
|href<br>|D:<br>|要素<br>|権限設定対象ロール表し、ロールリソースURLを入力するテキストノード<br>|権限設定対象ロールのリソースURLを指定する<br>acl要素内のxml:base属性の設定によって、URLを短縮する事が出来る<br>|
|all<br>|D:<br>|要素<br>|全アクセス主体権限設定<br>|全てのロールや認証されていないアクセス主体（Authorizationヘッダなし）に対してのの設定となります<br>|
|privilege<br>|D:<br>|要素<br>|権限設定を表し、以下の要素のいづれか一つが子となる<br>|<br>|
|all<br>|dc:<br>|要素<br>|全権限<br>|<br>
|auth<br>|dc:<br>|要素<br>|認証系管理API編集・参照権限<br>|<br>
|auth-read<br>|dc:<br>|要素<br>|認証系管理API参照権限<br>|<br>
|message<br>|dc:<br>|要素<br>|メッセージ系管理API編集・参照権限<br>|<br>
|message-read<br>|dc:<br>|要素<br>|メッセージ系管理API参照権限<br>|<br>
|event<br>|dc:<br>|要素<br>|イベント系管理API編集・参照権限<br>|<br>
|event-read<br>|dc:<br>|要素<br>|イベント系管理API参照権限<br>|<br>
|log<br>|dc:<br>|要素<br>|イベントバスのログAPI編集・参照権限<br>|<br>
|log-read<br>|dc:<br>|要素<br>|イベントバスのログAPI参照権限<br>|<br>
|social<br>|dc:<br>|要素<br>|関係系管理API編集・参照権限<br>|<br>
|social-read<br>|dc:<br>|要素<br>|関係系管理API参照権限<br>|<br>
|box<br>|dc:<br>|要素<br>|ボックス管理API編集・参照権限<br>|<br>
|box-read<br>|dc:<br>|要素<br>|ボックス管理API参照権限<br>|<br>
|box-install<br>|dc:<br>|要素<br>|Boxインストール実行権限 ※V1.2.3対応<br>|<br>|
|box-export<br>|dc:<br>|要素<br>|Boxエクスポート実行権限<br>|未対応(設定不可)<br>|
|acl<br>|dc:<br>|要素<br>|ACL管理API編集・参照権限<br>|<br>
|acl-read<br>|dc:<br>|要素<br>|ACL管理API参照権限<br>|<br>
|propfind<br>|dc:<br>|要素<br>|プロパティ取得API参照権限<br>|<br>

##### DTD表記
名前空間：D:
```dtd
<!ELEMENT acl (ace*) >
<!ATTLIST acl base CDATA #IMPLIED>
<!ELEMENT ace ((principal or invert), (grant or deny), protected?,inherited?)>
<!ELEMENT principal (href or all)>
<!ELEMENT principal (privilege+)>
<!ELEMENT href (#PCDATA)>
<!ELEMENT all EMPTY>
<!ELEMENT privilege (all or auth or auth-read or message or message-read or event or event-read or social or social-read or box or box-read or acl or acl-read or propfind)>
```


名前空間:xml:
```dtd
<!ATTLIST acl base CDATA #IMPLIED>
```

名前空間：dc:
```dtd
<!ELEMENT all EMPTY>
<!ELEMENT auth EMPTY>
<!ELEMENT auth-read EMPTY>
<!ELEMENT message EMPTY>
<!ELEMENT message-read EMPTY>
<!ELEMENT event EMPTY>
<!ELEMENT event-read EMPTY>
<!ELEMENT log EMPTY>
<!ELEMENT log-read EMPTY>
<!ELEMENT social EMPTY>
<!ELEMENT social-read EMPTY>
<!ELEMENT box EMPTY>
<!ELEMENT box-read EMPTY>
<!ELEMENT box-install EMPTY>
<!ELEMENT box-export EMPTY>
<!ELEMENT acl EMPTY>
<!ELEMENT acl-read EMPTY>
<!ELEMENT propfind EMPTY>
```

#### リクエストサンプル
```xml
<?xml version="1.0" encoding="utf-8" ?>
<D:acl xmlns:D="DAV:" xmlns:dc="urn:x-dc1:xmlns" xml:base="https://example.com/testcell1/__role/box1/">
    <D:ace>
        <D:principal>
            <D:all/>
        </D:principal>
        <D:grant>
           <D:privilege><dc:auth/></D:privilege>
           <D:privilege><dc:box/></D:privilege>
        </D:grant>
    </D:ace>
    <D:ace>
        <D:principal>
            <D:href>role</D:href>
        </D:principal>
        <D:grant>
            <D:privilege><dc:all/></D:privilege>
        </D:grant>
    </D:ace>
</D:acl>        
```
<br>
### レスポンス
#### ステータスコード

|コード<br>|メッセージ<br>|概要<br>|
|:--|:--|:--|
|200<br>|OK<br>|成功<br>|
#### レスポンスヘッダ

|項目名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|Content-Type<br>|返却されるデータの形式<br>|<br>|
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

<br>
### CURLサンプル
```sh
curl 'https://example.com/cell'  -X ACL -v -k \
-H 'Authorization: Bearer auth_token' \
-d '<?xml version="1.0" encoding="utf-8" ?><D:acl xmlns:D="DAV:" xmlns:dc="urn:x-dc1:xmlns" xml:base="http://localhost:8080/dc1-core/acell1/__role/__/"> \
<D:ace><D:principal><D:href>doctor</D:href></D:principal><D:grant><D:privilege><dc:box-read/></D:privilege><D:privilege><dc:auth/></D:privilege></D:grant>
</D:ace></D:acl>'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
