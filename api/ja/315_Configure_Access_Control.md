# Box Level アクセス制御設定
### 概要
Box Level のアクセス制御機能を提供する

### 必要な権限
write-acl

### 制限事項
* ACL設定を行うと、既存のACL設定を上書きされる形で更新されます
* ACL設定を打ち消す機能（deny）
* ACLで設定出来るprivilegeの一覧取得

<br>
### リクエスト
#### リクエストURL
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
ACL

#### リクエストクエリ
なし

#### リクエストヘッダ

|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用される<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定する<br>|
|X-Personium-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {AccessToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|

#### リクエストボディ
名前空間

|URI<br>|概要<br>|参考prefix<br>|
|:--|:--|:--|
|DAV:<br>|WebDAVの名前空間<br>|D:<br>|
|urn:x-personium:xmlns<br>|Personiumの名前空間<br>|p:<br>|

※ 参考prefixは以下表の可読性を高めるためのもので、このprefix文字列の使用を保証するものでも要求するものでもありません。


XMLの構造  
ボディはXMLで、以下のスキーマに従っています。  
privilegeタグ配下の権限設定の内容については、acl_model（[アクセス制御モデル](../../user_guide/002_Access_Control.html)）を参照。

|ノード名<br>|名前空間<br>|ノードタイプ<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|:--|
|acl<br>|D:<br>|要素<br>|ACL（アクセス制御リスト）のルートを表し、1つ以上複数のaceが子となる<br>| <br>|
|base<br>|xml:<br>|属性<br>|hrefタグ内に記述するURLの基底を表し、任意の値を属性値とする。この属性は任意。<br>| <br>|
|ace<br>|D:<br>|要素<br>|ACE（アクセス制御エレメント）を表し、principalとgrantが一対で子となる<br>|「invert」「deny」「protected」「inherited」はV1.1系未対応<br>|
|principal<br>|D:<br>|要素<br>|権限設定対象を表し、hrefまたはallが子となる<br>| <br>|
|grant<br>|D:<br>|要素<br>|権限付与設定を表し、1つ以上複数のprivilegeが子となる<br>| <br>|
|href<br>|D:<br>|要素<br>|権限設定対象ロール表し、ロールリソースURLを入力するテキストノード<br>|権限設定対象ロールのリソースURLを指定する acl要素内のxml:base属性の設定によって、URLを短縮する事が出来る<br>|
|all<br>|D:<br>|要素<br>|全アクセス主体権限設定<br>| <br>|
|privilege<br>|D:<br>|要素<br>|権限設定を表し、以下の要素のいづれか一つが子となる<br>| <br>|
|read<br>|D:<br>|要素<br>|参照権限<br>| <br>|
|write<br>|D:<br>|要素<br>|編集権限<br>| <br>|
|read-properties<br>|D:<br>|要素<br>|プロパティ参照権限<br>| <br>|
|write-properties<br>|D:<br>|要素<br>|プロパティ編集権限<br>| <br>|
|read-acl<br>|D:<br>|要素<br>|ACL設定参照権限<br>| <br>|
|write-acl<br>|D:<br>|要素<br>|ACL設定編集権限<br>| <br>|
|bind<br>|D:<br>|要素<br>|未稿<br>|V1.1系、V1.2系未対応<br>|
|unbind<br>|D:<br>|要素<br>|未稿<br>|V1.1系、V1.2系未対応<br>|
|exec<br>|D:<br>|要素<br>|サービス実行権限<br>| <br>|



DTD表記

名前空間 D:
```dtd
<!ELEMENT acl (ace*) >
<!ELEMENT ace ((principal or invert), (grant or deny), protected?,inherited?)>
<!ELEMENT principal (href or all)>
<!ELEMENT principal (privilege*)>
<!ELEMENT href (#PCDATA)>
<!ELEMENT all EMPTY>
<!ELEMENT privilege (all or read or write or read-properties or write-properties or read-acl or write-acl or exec or bind or unbind)>
<!ELEMENT read EMPTY>
<!ELEMENT write EMPTY>
<!ELEMENT read-properties EMPTY>
<!ELEMENT write-properties EMPTY>
<!ELEMENT read-acl EMPTY>
<!ELEMENT write-acl EMPTY>
<!ELEMENT bind EMPTY>
<!ELEMENT unbind EMPTY>
<!ELEMENT exec EMPTY>
```

名前空間 p:
```dtd
<!ATTLIST acl requireSchemaAuthz (none or public or confidential) #IMPLIED>
<!ELEMENT exec EMPTY>   
```

名前空間 xml:
```dtd
<!ATTLIST acl base CDATA #IMPLIED>
```

#### リクエストサンプル
```xml
<?xml version="1.0" encoding="utf-8" ?>
<D:acl xmlns:D="DAV:" xmlns:p="urn:x-personium:xmlns"
       xml:base="https://{UnitFQDN}/{CellName}/__role/{BoxName}/"
       p:requireSchemaAuthz="public">
    <D:ace>
        <D:principal>
            <D:all/>
        </D:principal>
        <D:grant>
            <D:privilege><D:read/></D:privilege>
        </D:grant>
    </D:ace>
    <D:ace>
        <D:principal>
            <D:href>role</D:href>
        </D:principal>
        <D:grant>
            <D:privilege><D:read/></D:privilege>
            <D:privilege><D:write/></D:privilege>
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

|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|Content-Type<br>|返却されるデータの形式<br>|更新・作成時に失敗した場合のみ返却する<br>|
#### レスポンスボディ
なし

#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

#### レスポンスサンプル
なし

<br>
### CURLサンプル

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{CollectionName}" -X ACL -i
-H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d
'<?xml version="1.0" encoding="utf-8" ?>
 <D:acl xmlns:D="DAV:" xml:base="https://{UnitFQDN}/{CellName}/__role/{BoxName}/"　xmlns:p="urn:x-personium:xmlns" p:requireSchemaAuthz="none">
  <D:ace>
   <D:principal>
    <D:href>doctor</D:href>
   </D:principal>
   <D:grant>
    <D:privilege>
     <D:read/>
    </D:privilege>
    <D:privilege>
     <D:write/>
    </D:privilege>
   </D:grant>
  </D:ace>
 </D:acl>'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
