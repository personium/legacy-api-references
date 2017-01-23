# スキーマ取得 ($metadata)
### 概要
スキーマ情報を取得する
### 必要な権限
read
### 制限事項
* レスポンスボディの正常系はXML形式とし、レスポンスヘッダのContent-Typeはapplication/xmlとする
* レスポンスボディの異常系はjson形式とし、レスポンスヘッダのContent-Typeはapplication/jsonとする
* $formatがatomsvcまたはAcceptヘッダがapplication/atomsvc+xmlの場合、SchemaのAtom ServiceDocumentを返却する
* $formatがatomsvcでなく、Acceptヘッダがapplication/atomsvc+xmlでない場合は、ユーザデータのEDMXを返却する
* ComplexTypeおよび、Documentationタグは対応していないため返却しない

<br>
### リクエスト
#### リクエストURL
```
/{Cell_name}/{Box_name}/{odataname}/$metadata
```
#### メソッド
GET
#### リクエストクエリ
###### 共通クエリ
|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|dc_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|
###### 個別クエリ
$formatにatomsvcを指定した場合、SchemaのAtom ServiceDocumentを返却する
その他は無視する
#### リクエストヘッダ
##### 共通リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用される<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定する<br>|
|X-Dc-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
##### 個別リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Accept<br>|返却されるデータの形式<br>|application/atomsvc+xml<br>|×<br>|指定がない場合、ユーザデータスキーマのスキーマ情報取得となる<br>|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {認証トークン}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
#### リクエストボディ
なし
#### リクエストサンプル
なし

<br>
### レスポンス
#### ステータスコード
200
#### レスポンスヘッダ
##### 共通レスポンスヘッダ
|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|X-Dc-Version<br>|APIの実行バージョン<br>|指定がない場合、最新のAPIバージョンが指定される<br>|
|Access-Control-Allow-Origin<br>|クロスドメイン通信許可ヘッダ<br>|返却値は"*"固定<br>|
##### スキーマ取得固有レスポンスヘッダ
|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|Content-Type<br>|返却されるデータの形式<br>|<br>|
|DataServiceVersion<br>|ODataのバージョン<br>|<br>|
#### レスポンスボディ
##### ユーザデータの場合
|URI<br>|概要<br>|参考prefix<br>|
|:--|:--|:--|
|http://schemas.microsoft.com/ado/2007/06/edmx<br>|edmxの名前空間<br>|edmx:<br>|
|http://schemas.microsoft.com/ado/2007/08/dataservices<br>|WCF Data Servicesの名前空間<br>|d:<br>|
|http://schemas.microsoft.com/ado/2007/08/dataservices/metadata<br>|WCF Data Services metadataの名前空間<br>|m:<br>|
|urn:x-dc1:xmlns<br>|personium.ioの名前空間 <br>|dc:<br>|
|http://schemas.microsoft.com/ado/2006/04/edm<br>|schemeの名前空間<br>|-<br>|
※ 参考prefixは以下表の可読性を高めるためのもので、このprefix文字列の使用を保証するものでも要求するものでもありません。
##### XMLの構造
ボディはXML(edmx)で、以下のスキーマに従っています。

|ノード名<br>|名前空間<br>|ノードタイプ<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Edmx <br>|edmx:<br>|要素<br>|EDMXドキュメントのルートを表し、子に必ず１つのDataServices要素を持つ<br>|<br>|
|Version<br>|edmx:<br>|属性<br>|EDMXドキュメントのバージョンを表す。属性値は必ず'1.0'となる<br>|<br>|
|DataServices<br>|edmx:<br>|要素<br>|データサービスを表し、子に複数のSchema要素を持つ<br>|<br>|
|DataServiceVersion<br>|m:<br>|属性<br>|データサービスのバージョンを表す。属性値は必ず'1.0'となる<br>|<br>|
|Schema<br>|-<br>|要素<br>|CSDLドキュメントの最上位要素を表し、子にAssociation・ComplexType・<br>EntityType・EntityContainer・Documentationの要素を持つ<br>|<br>|
|Namespace<br>|-<br>|属性<br>|スキーマの名前を表すし、必須な属性。<br>|値として、System、Transient、または Edm を使用することはできない<br>|
|ComplexType<br>|-<br>|要素<br>|複合型を表し、子にDocumentation・Propertyの要素を持つ<br>|<br>|
|Name<br>|-<br>|属性<br>|複合のスキーマ名を表し、必須な属性<br>|ODataコレクション内にComplexTypeが存在する場合のみ必須<br>|
|Abstract<br>|-<br>|属性<br>|プロパティの形式のデータを表す<br>|Abstractが存在する場合のみ表示<br>|
|Documentation<br>|-<br>|要素<br>|複合型のドキュメントを表す<br>|存在する場合のみ表示<br>|
|EntityType<br>|-<br>|要素<br>|エンティティ型を表し、子にDocumentation・HasStream・BaseType・<br>Key・Property・NavigationPropertyの要素を持つ <br>|<br>|
|HasStream<br>|-<br>|属性<br>|エンティティがメディア リソース ストリームに関連付けられているかを表す<br>|<br>|
|BaseType<br>|-<br>|属性<br>|エンティティ型の基本型を表す<br>|<br>|
|Documentation<br>|-<br>|要素<br>|エンティティ型のドキュメントを表す<br>|<br>|
|Key<br>|-<br>|要素<br>|エンティティのキーを表し、必ず１個以上のPropertyRefが子となる<br>|<br>|
|PropertyRef<br>|-<br>|要素<br>|キーの参照プロパティを表す<br>|<br>|
|Name<br>|-<br>|属性<br>|参照されているプロパティの名前を表し、必須の属性となる<br>|<br>|
|NavigationProperty<br>|-<br>|要素<br>|ナビゲーションプロパティを表す<br>|<br>|
|Name<br>|-<br>|属性<br>|ナビゲーションプロパティの名前を表し、必須の属性となる<br>|<br>|
|Documentation<br>|-<br>|要素<br>|ナビゲーションプロパティのドキュメントを表<br>|<br>|
|Relationship<br>|-<br>|属性<br>|モデルのスコープ内にあるアソシエーションの名前を表し、必須の属性となる<br>|<br>|
|FromRole<br>|-<br>|属性<br>|参照元ロールを表す<br>|<br>|
|ToRole<br>|-<br>|属性<br>|参照先ロールを表す<br>|<br>|
|Association<br>|-<br>|要素<br>|アソシエーションを表し、子にDocumentation・Endの要素を持つ<br>|<br>|
|Name<br>|-<br>|属性<br>|アソシエーション名を表し、必須な属性<br>|<br>|
|Documentation<br>|-<br>|要素<br>|アソシエーションのドキュメントを表す<br>|存在する場合のみ表示<br>|
|End<br>|-<br>|要素<br>|アソシエーションエンドの名前を表し、必須な属性<br>|<br>|
|Role<br>|-<br>|属性<br>|アソシエーションエンドの名前を表し、必須な属性<br>|<br>|
|Type<br>|-<br>|属性<br>|アソシエーションエンドの型を表し、必須な属性<br>|<br>|
|Multiplicity<br>|-<br>|属性<br>|アソシエーションエンドの多重度を表し、必須な属性<br>|<br>|
|EntityContainer<br>|-<br>|要素<br>| エンティティコンテナを表し、子にDocumentation・EntitySet・<br>FunctionImport・AssociationSetの要素を持つ<br>|<br>|
|Name<br>|-<br>|属性<br>|エンティティコンテナ名を表し、必須な属性<br>|<br>|
|IsDefaultEntityContainer<br>|m:<br>|属性<br>|デフォルトエンティティコンテナフラグを表す<br>|<br>|
|Documentation<br>|-<br>|要素<br>|エンティティコンテナのドキュメントのドキュメントを表す<br>|<br>|
|EntitySet<br>|-<br>|要素<br>|エンティティセットを表し、子に Documentationの要素を持つ<br>|<br>|
|Documentation<br>|-<br>|要素<br>|エンティティコンテナのドキュメントのドキュメントを表す<br>|<br>|
|Name<br>|-<br>|属性<br>|エンティティセットを表し、子に Documentationの要素を持つ<br>|<br>|
|EntityType<br>|-<br>|属性<br>|エンティティセットのエンティティタイプを表す<br>|<br>|
|Name<br>|-<br>|属性<br>|エンティティセット名を表し、必須な属性<br>|<br>|
|FunctionImport<br>|-<br>|要素<br>|関数インポート（モデル宣言関数）を表す<br>|<br>|
|Name<br>|-<br>|属性<br>|関数インポート名を表し、必須な属性<br>|<br>|
|EntitySet<br>|-<br>|属性<br>|関数インポートのエンティティセットを表し、子に Documentationの要素を持つ<br>|存在するエンティティセット分 繰り返す<br>|
|ReturnType<br>|-<br>|属性<br>|戻り値の型を表す<br>|<br>|
|HttpMethod<br>|m:<br>|属性<br>|メソッドを表し、必須な属性<br>|FunctionImportが存在する場合<br>|
|Documentation<br>|-<br>|要素<br>|関数インポートのドキュメントを表す <br>|<br>|
|Parameter<br>|-<br>|要素<br>|関数インポートの引数を表す<br>|<br>|
|Name<br>|-<br>|属性<br>|関数インポートの引数名を表し、必須な項目<br>|FunctionImport、Parameterが存在する場合<br>|
|Type<br>|-<br>|属性<br>|関数インポートの引数の型を表し、必須な項目<br>|FunctionImport、Parameterが存在する場合<br>|
|Mode<br>|-<br>|属性<br>|関数インポートの引数のモードを表し、有効値：'In'、'Out'、または 'InOut'<br>|<br>|
|Documentation<br>|-<br>|要素<br>|関数インポートの引数のドキュメントを表す<br>|存在する場合のみ表示<br>|
|AssociationSet<br>|-<br>|要素<br>|アソシエーションセットを表し、子にDocumentationと２つのEndを持つ<br>|<br>|
|Name<br>|-<br>|属性<br>|アソシエーションセット名を表し、必須な属性<br>|<br>|
|Association<br>|-<br>|属性<br>|アソシエーションセットのアソシエーションを表し、必須な属性<br>|<br>|
|Documentation<br>|-<br>|要素<br>|アソシエーションセットのドキュメントを表す<br>|存在する場合のみ表示<br>|
|End<br>|-<br>|要素<br>|アソシエーションセット ENDを表し、必須な要素<br>|<br>|
|Role<br>|-<br>|属性<br>|ロールを表し、必須な属性<br>|<br>|
|EntitySet<br>|-<br>|属性<br>|エンティティセットを表し、必須な属性<br>|<br>|
##### プロパティ
|ノード名<br>|名前空間<br>|ノードタイプ<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Property<br>|-<br>|要素<br>|エンティティ型のPropertyを表す<br>|<br>|
| Name<br>|-<br>|属性<br>|プロパティの名前を表す<br>|<br>|
|Type<br>|-<br>|属性<br>|プロパティ値の型を表す<br>|<br>|
|Nullable<br>|-<br>|属性<br>|null 値の割り当ての可否を表す<br>|<br>|
|MaxLength<br>|-<br>|属性<br>|プロパティの許容最大長を表す<br>|<br>|
|DefaultValue<br>|-<br>|属性<br>|プロパティの既定値を表す<br>|<br>|
|Precision<br>|-<br>|属性<br>|プロパティの有効桁数を表す<br>|<br>|
|Scale<br>|-<br>|属性<br>|プロパティの小数点以下桁数を表す<br>|<br>|
|CollectionKind<br>|-<br>|属性<br>|プロパティの配列種別を表す<br>|コレクションが配列の場合ture,それ以外はnone<br>|
|Format<br>|dc:<br>|属性<br>|プロパティの文字フォーマットを表す<br>|<br>|
|IsDeclared<br>|dc:<br>|属性<br>|静的プロパティか否かを表す<br>|動的プロパティの場合falseで表示,静的プロパティの場合は表示されない<br>|
##### ドキュメンテーション
未対応

|ノード名<br>|名前空間<br>|ノードタイプ<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Summary<br>|-<br>|要素<br>|ドキュメントのサマリを表す<br>|<br>|
|LongDescription<br>|-<br>|要素<br>|ドキュメントの詳細を表す<br>|<br>|
##### DTD表記
名前空間:edmx:
```dtd
<!ELEMENT Edmx (DataServices)>
<!ATTLIST Edmx Version CDDATA "1.0">
<!ELEMENT DataServices (Schema*)>
```
名前空間: m:
```dtd
<!ATTLIST DataServices DataServiceVersion CDDATA "1.0">
<!ATTLIST FunctionImport HttpMethod CDDATA #IMPLIED>
<!ATTLIST EntityContainer IsDefaultEntityContainer CDDATA "true">
```
名前空間:http://schemas.microsoft.com/ado/2006/04/edm
```dtd
<!ELEMENT Schema (Association*,ComplexType*,EntityType*,EntityContainer*)>
<!ATTLIST Schema Namespace CDDATA #REQUIRED>
<!ELEMENT ComplexType (Documentation?,Property*)>
<!ATTLIST ComplexType Name CDDATA #IMPLIED
                      Abstract CDDATA #IMPLIED>
<!ELEMENT Documentation (Summary?,LongDescription?)>
<!ELEMENT Summary (#PCDATA)>
<!ELEMENT LongDescription (#PCDATA)>
<!ELEMENT Property EMPTY>
<!ATTLIST Property Name CDDATA #REQUIRED
                   Type CDDATA #REQUIRED
                   Nullable CDDATA (true|false) #REQUIRED
                   MaxLength CDDATA #IMPLIED
                   DefaultValue CDDATA #IMPLIED
                   Precision CDDATA #IMPLIED
                   Scale CDDATA #IMPLIED
                   CollectionKind (true|none) #IMPLIED>
<!ELEMENT EntityType (Documentation?,Key・Property*・NavigationProperty*)>
<!ATTLIST EntityType OpenType (true|false) #REQUIRED
                     HasStream CDDATA #IMPLIED
                     BaseType CDDATA #IMPLIED>
<!ELEMENT Key (PropertyRef*)>
<!ELEMENT PropertyRef ENPTY>
<!ATTLIST PropertyRef Name CDDATA #REQUIRED>
<!ELEMENT NavigationProperty (Documentation?)>
<!ATTLIST PropertyRef Name CDDATA #REQUIRED
                      Relationship CDDATA #REQUIRED
                      FromRole CDDATA #REQUIRED
                      ToRole  CDDATA #REQUIRED>
<!ELEMENT Association (Documentation?|End+)>
<!ATTLIST Association Name CDDATA #REQUIRED>
<!ELEMENT End ENPTY>
<!ATTLIST End Role CDDATA #REQUIRED
          Type CDDATA #REQUIRED
          Multiplicity ("1","0..1","*") #REQUIRED>
<!ELEMENT EntityContainer (Documentation?|EntitySet*|FunctionImport*|AssociationSet*)>
<!ATTLIST FunctionImport Name CDDATA #REQUIRED
                         EntitySet CDDATA #IMPLIED
                         ReturnType CDDATA #IMPLIED>
<!ELEMENT Parameter (Documentation?)>
<!ATTLIST Parameter Name CDDATA #REQUIRED
                    Type CDDATA #REQUIRED
                    Mode  ("In","Out","InOut") #IMPLIED>
<!ELEMENT AssociationSet (Documentation?|End+)>
<!ATTLIST AssociationSet Name CDDATA #REQUIRED
                         Association CDDATA #REQUIRED>
<!ELEMENT End (Documentation?)>
<!ATTLIST End Role CDDATA #IMPLIED
              EntitySet CDDATA #REQUIRED>
```
名前空間:dc:
```dtd
<!ATTLIST Property Format CDDATA #IMPLIED>
```
#### エラーメッセージ一覧
|コード<br>|概要<br>|
|:--|:--|
|401<br>|認証トークンが無効の場合<br>|
|403<br>|アクセス権限が不足している場合<br>|
|404<br>|存在しないCellを指定した場合<br>存在しないBoxを指定した場合<br>存在しないODataCollection を指定した場合 <br>|
|405<br>|許可していないリクエストメソッドを指定した場合<br>|
[エラーメッセージ一覧](198_Error_Messages.html)を参照
#### レスポンスサンプル
##### SchemaのAtom ServiceDocumentの場合
以下を固定で返却する。
```xml
<service xmlns='http://www.w3.org/2007/app' xml:base='https://fqdn/cell_name/box_name/col/$metadata?$format=atomsvc/'
 xmlns:atom='http://www.w3.org/2005/Atom' xmlns:app='http://www.w3.org/2007/app'>
  <workspace>
    <atom:title>
      Default
    </atom:title>
    <collection href='ComplexType'>
      <atom:title>
        ComplexType
      </atom:title>
    </collection>
    <collection href='ComplexTypeProperty'>
      <atom:title>
        ComplexTypeProperty
      </atom:title>
    </collection>
    <collection href='AssociationEnd'>
      <atom:title>
        AssociationEnd
      </atom:title>
    </collection>
    <collection href='EntityType'>
      <atom:title>
        EntityType
      </atom:title>
    </collection>
    <collection href='Property'>
      <atom:title>
        Property
      </atom:title>
    </collection>
  </workspace>
</service>
```
##### ユーザデータの場合
```xml
<edmx:Edmx Version='1.0' xmlns:edmx='http://schemas.microsoft.com/ado/2007/06/edmx' xmlns:d='http://schemas.microsoft.com/ado/2007/08/dataservices' xmlns:m='http://schemas.microsoft.com/ado/2007/08/dataservices/metadata' xmlns:dc='urn:x-dc1:xmlns'>
  <edmx:DataServices m:DataServiceVersion='1.0'>
    <Schema xmlns='http://schemas.microsoft.com/ado/2006/04/edm' Namespace='UserData'>
      <EntityType Name='Sales' OpenType='true'>
        <Key>
          <PropertyRef Name='__id'/>
        </Key>
        <Property Name='__id' Type='Edm.String' Nullable='false' DefaultValue='UUID()' dc:Format='regEx(&amp;apos;^[a-zA-Z0-9][a-zA-Z0-9-_:]{0,199}$&amp;apos;)'/>
        <Property Name='__published' Type='Edm.DateTime' Nullable='false' DefaultValue='SYSUTCDATETIME()' Precision='3'/>
        <Property Name='__updated' Type='Edm.DateTime' Nullable='false' DefaultValue='SYSUTCDATETIME()' Precision='3'/>
        <NavigationProperty Name='_SalesDetail' Relationship='UserData.Sales-SalesDetail-assoc' FromRole='Sales:sales2salesDetail' ToRole='SalesDetail:salesDetail2sales'/>
      </EntityType>
      <EntityType Name='SalesDetail' OpenType='true'>
        <Key>
          <PropertyRef Name='__id'/>
        </Key>
        <Property Name='__id' Type='Edm.String' Nullable='false' DefaultValue='UUID()' dc:Format='regEx(&amp;apos;^[a-zA-Z0-9][a-zA-Z0-9-_:]{0,199}$&amp;apos;)'/>
        <Property Name='__published' Type='Edm.DateTime' Nullable='false' DefaultValue='SYSUTCDATETIME()' Precision='3'/>
        <Property Name='__updated' Type='Edm.DateTime' Nullable='false' DefaultValue='SYSUTCDATETIME()' Precision='3'/>
        <NavigationProperty Name='_Sales' Relationship='UserData.Sales-SalesDetail-assoc' FromRole='SalesDetail:salesDetail2sales' ToRole='Sales:sales2salesDetail'/>
      </EntityType>
      <Association Name='Sales-SalesDetail-assoc'>
        <End Role='Sales:sales2salesDetail' Type='UserData.Sales' Multiplicity='1'/>
        <End Role='SalesDetail:salesDetail2sales' Type='UserData.SalesDetail' Multiplicity='*'/>
      </Association>
      <EntityContainer Name='UserData' m:IsDefaultEntityContainer='true'>
        <EntitySet Name='SalesDetail' EntityType='UserData.SalesDetail'/>
        <AssociationSet Name='Sales-SalesDetail-assoc' Association='UserData.Sales-SalesDetail-assoc'>
          <End Role='Sales:sales2salesDetail' EntitySet='Sales'/>
          <End Role='SalesDetail:salesDetail2sales' EntitySet='SalesDetail'/>
        </AssociationSet>
      </EntityContainer>
    </Schema>
  </edmx:DataServices>
</edmx:Edmx>
```

<br>
### CURLサンプル
#### CURLコマンド(UNIX)
```sh
curl 'https://fqdn/cell_name/box_name/odata_colleciton_path/$metadata' -X GET -H 'Authorization:Bearer auth_token' -H 'Accept:application/xml' -k
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
