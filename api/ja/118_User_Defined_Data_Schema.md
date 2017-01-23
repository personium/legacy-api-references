# スキーマ取得（$metadata）
### 概要
ユーザーデータのスキーマ定義を行うためのスキーマ情報を取得する

### 必要な権限
read

### 制限事項
なし

<br>
### リクエスト
#### リクエストURL
```
/{Cell_name}/{Box_name}/{odata_colname}/$metadata
```
#### メソッド
GET

#### リクエストクエリ
共通リクエストクエリ

|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|dc_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|

#### リクエストヘッダ
##### 共通リクエストヘッダ

|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用される<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定する<br>|
|X-Dc-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
#### リクエストボディ
なし

#### リクエストサンプル
なし

<br>
### レスポンス
#### ステータスコード
200

#### レスポンスヘッダ

|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|X-Dc-Version<br>|APIの実行バージョン<br>|リクエストが処理されたAPIバージョン<br>|
|Access-Control-Allow-Origin<br>|クロスドメイン通信許可ヘッダ<br>|返却値は"*"固定<br>|
|Content-Type<br>|返却されるデータの形式<br>| <br>
|DataServiceVersion<br>|ODataのバージョン情報<br>| <br>
#### レスポンスボディ
固定で以下を返却する
```xml
<edmx:Edmx Version='1.0' xmlns:edmx='http://schemas.microsoft.com/ado/2007/06/edmx'
xmlns:d='http://schemas.microsoft.com/ado/2007/08/dataservices' xmlns:m='http://schemas.microsoft.com/ado/2007/08/dataservices/metadata'
xmlns:dc='urn:x-dc1:xmlns'>
  <edmx:DataServices m:DataServiceVersion='1.0'>
    <Schema xmlns='http://schemas.microsoft.com/ado/2006/04/edm' Namespace='ODataSvcSchema'>
      <EntityType Name='EntityType'>
        <Key>
          <PropertyRef Name='Name'/>
        </Key>
        <Property Name='Name' Type='Edm.String' Nullable='false' dc:Format='regEx(&amp;apos;^[a-zA-Z0-9][a-zA-Z0-9-_]{0,127}$&amp;apos;)'/>
        <Property Name='__published' Type='Edm.DateTime' Nullable='false' DefaultValue='SYSUTCDATETIME()' Precision='3'/>
        <Property Name='__updated' Type='Edm.DateTime' Nullable='false' DefaultValue='SYSUTCDATETIME()' Precision='3'/>
        <NavigationProperty Name='_AssociationEnd' Relationship='ODataSvcSchema.EntityType-AssociationEnd-assoc' FromRole='EntityType-AssociationEnd' ToRole='AssociationEnd-EntityType'/>
        <NavigationProperty Name='_Property' Relationship='ODataSvcSchema.EntityType-Property-assoc' FromRole='EntityType-Property' ToRole='Property-EntityType'/>
      </EntityType>
      <EntityType Name='AssociationEnd'>
        <Key>
          <PropertyRef Name='Name'/>
          <PropertyRef Name='_EntityType.Name'/>
        </Key>
        <Property Name='Name' Type='Edm.String' Nullable='false' dc:Format='regEx(&amp;apos;^[a-zA-Z0-9][a-zA-Z0-9-_]{0,127}$&amp;apos;)'/>
        <Property Name='Multiplicity' Type='Edm.String' Nullable='false' dc:Format='regEx(&amp;apos;0\.\.1|1|\*&amp;apos;)'/>
        <Property Name='_EntityType.Name' Type='Edm.String' Nullable='false' dc:Format='regEx(&amp;apos;^[a-zA-Z0-9][a-zA-Z0-9-_]{0,127}$&amp;apos;)'/>
        <Property Name='__published' Type='Edm.DateTime' Nullable='false' DefaultValue='SYSUTCDATETIME()' Precision='3'/>
        <Property Name='__updated' Type='Edm.DateTime' Nullable='false' DefaultValue='SYSUTCDATETIME()' Precision='3'/>
        <NavigationProperty Name='_EntityType' Relationship='ODataSvcSchema.EntityType-AssociationEnd-assoc' FromRole='AssociationEnd-EntityType' ToRole='EntityType-AssociationEnd'/>
        <NavigationProperty Name='_AssociationEnd' Relationship='ODataSvcSchema.AssociationEnd-AssociationEnd-assoc' FromRole='AssociationEnd-AssociationEnd' ToRole='AssociationEnd-AssociationEnd'/>
      </EntityType>
      <EntityType Name='Property'>
        <Key>
          <PropertyRef Name='Name'/>
          <PropertyRef Name='_EntityType.Name'/>
        </Key>
        <Property Name='Name' Type='Edm.String' Nullable='false' dc:Format='regEx(&amp;apos;^[a-zA-Z0-9][a-zA-Z0-9-_]{0,127}$&amp;apos;)'/>
        <Property Name='_EntityType.Name' Type='Edm.String' Nullable='false' dc:Format='regEx(&amp;apos;^[a-zA-Z0-9][a-zA-Z0-9-_]{0,127}$&amp;apos;)'/>
        <Property Name='Type' Type='Edm.String' Nullable='false'/>
        <Property Name='Nullable' Type='Edm.Boolean' Nullable='false' DefaultValue='true'/>
        <Property Name='DefaultValue' Type='Edm.String' Nullable='true'/>
        <Property Name='CollectionKind' Type='Edm.String' Nullable='true' DefaultValue='None'/>
        <Property Name='IsKey' Type='Edm.Boolean' Nullable='false' DefaultValue='false'/>
        <Property Name='UniqueKey' Type='Edm.String' Nullable='true' dc:Format='regEx(&amp;apos;^[a-zA-Z0-9][a-zA-Z0-9-_]{0,127}$&amp;apos;)'/>
        <Property Name='__published' Type='Edm.DateTime' Nullable='false' DefaultValue='SYSUTCDATETIME()' Precision='3'/>
        <Property Name='__updated' Type='Edm.DateTime' Nullable='false' DefaultValue='SYSUTCDATETIME()' Precision='3'/>
        <NavigationProperty Name='_EntityType' Relationship='ODataSvcSchema.EntityType-Property-assoc' FromRole='Property-EntityType' ToRole='EntityType-Property'/>
      </EntityType>
      <EntityType Name='ComplexType'>
        <Key>
          <PropertyRef Name='Name'/>
        </Key>
        <Property Name='Name' Type='Edm.String' Nullable='false' dc:Format='regEx(&amp;apos;^[a-zA-Z0-9][a-zA-Z0-9-_]{0,127}$&amp;apos;)'/>
        <Property Name='__published' Type='Edm.DateTime' Nullable='false' DefaultValue='SYSUTCDATETIME()' Precision='3'/>
        <Property Name='__updated' Type='Edm.DateTime' Nullable='false' DefaultValue='SYSUTCDATETIME()' Precision='3'/>
        <NavigationProperty Name='_ComplexTypeProperty' Relationship='ODataSvcSchema.ComplexType-ComplexTypeProperty-assoc' FromRole='ComplexType-ComplexTypeProperty' ToRole='ComplexTypeProperty-ComplexType'/>
      </EntityType>
      <EntityType Name='ComplexTypeProperty'>
        <Key>
          <PropertyRef Name='Name'/>
          <PropertyRef Name='_ComplexType.Name'/>
        </Key>
        <Property Name='Name' Type='Edm.String' Nullable='false' dc:Format='regEx(&amp;apos;^[a-zA-Z0-9][a-zA-Z0-9-_]{0,127}$&amp;apos;)'/>
        <Property Name='_ComplexType.Name' Type='Edm.String' Nullable='false' dc:Format='regEx(&amp;apos;^[a-zA-Z0-9][a-zA-Z0-9-_]{0,127}$&amp;apos;)'/>
        <Property Name='Type' Type='Edm.String' Nullable='false'/>
        <Property Name='Nullable' Type='Edm.Boolean' Nullable='false' DefaultValue='true'/>
        <Property Name='DefaultValue' Type='Edm.String' Nullable='true'/>
        <Property Name='CollectionKind' Type='Edm.String' Nullable='true' DefaultValue='None'/>
        <Property Name='__published' Type='Edm.DateTime' Nullable='false' DefaultValue='SYSUTCDATETIME()' Precision='3'/>
        <Property Name='__updated' Type='Edm.DateTime' Nullable='false' DefaultValue='SYSUTCDATETIME()' Precision='3'/>
        <NavigationProperty Name='_ComplexType' Relationship='ODataSvcSchema.ComplexType-ComplexTypeProperty-assoc' FromRole='ComplexTypeProperty-ComplexType' ToRole='ComplexType-ComplexTypeProperty'/>
      </EntityType>
      <Association Name='EntityType-AssociationEnd-assoc'>
        <End Role='EntityType-AssociationEnd' Type='ODataSvcSchema.EntityType' Multiplicity='0..1'/>
        <End Role='AssociationEnd-EntityType' Type='ODataSvcSchema.AssociationEnd' Multiplicity='*'/>
      </Association>
      <Association Name='AssociationEnd-AssociationEnd-assoc'>
        <End Role='AssociationEnd-AssociationEnd' Type='ODataSvcSchema.AssociationEnd' Multiplicity='0..1'/>
        <End Role='AssociationEnd-AssociationEnd' Type='ODataSvcSchema.AssociationEnd' Multiplicity='0..1'/>
      </Association>
      <Association Name='EntityType-Property-assoc'>
        <End Role='EntityType-Property' Type='ODataSvcSchema.EntityType' Multiplicity='1'/>
        <End Role='Property-EntityType' Type='ODataSvcSchema.Property' Multiplicity='*'/>
      </Association>
      <Association Name='ComplexType-ComplexTypeProperty-assoc'>
        <End Role='ComplexType-ComplexTypeProperty' Type='ODataSvcSchema.ComplexType' Multiplicity='1'/>
        <End Role='ComplexTypeProperty-ComplexType' Type='ODataSvcSchema.ComplexTypeProperty' Multiplicity='*'/>
      </Association>
      <EntityContainer Name='ODataSvcSchema' m:IsDefaultEntityContainer='true'>
        <EntitySet Name='ComplexType' EntityType='ODataSvcSchema.ComplexType'/>
        <EntitySet Name='ComplexTypeProperty' EntityType='ODataSvcSchema.ComplexTypeProperty'/>
        <EntitySet Name='AssociationEnd' EntityType='ODataSvcSchema.AssociationEnd'/>
        <EntitySet Name='EntityType' EntityType='ODataSvcSchema.EntityType'/>
        <EntitySet Name='Property' EntityType='ODataSvcSchema.Property'/>
        <AssociationSet Name='EntityType-AssociationEnd-assoc' Association='ODataSvcSchema.EntityType-AssociationEnd-assoc'>
          <End Role='EntityType-AssociationEnd' EntitySet='EntityType'/>
          <End Role='AssociationEnd-EntityType' EntitySet='AssociationEnd'/>
        </AssociationSet>
        <AssociationSet Name='AssociationEnd-AssociationEnd-assoc' Association='ODataSvcSchema.AssociationEnd-AssociationEnd-assoc'>
          <End Role='AssociationEnd-AssociationEnd' EntitySet='AssociationEnd'/>
          <End Role='AssociationEnd-AssociationEnd' EntitySet='AssociationEnd'/>
        </AssociationSet>
        <AssociationSet Name='EntityType-Property-assoc' Association='ODataSvcSchema.EntityType-Property-assoc'>
          <End Role='EntityType-Property' EntitySet='EntityType'/>
          <End Role='Property-EntityType' EntitySet='Property'/>
        </AssociationSet>
        <AssociationSet Name='ComplexType-ComplexTypeProperty-assoc' Association='ODataSvcSchema.ComplexType-ComplexTypeProperty-assoc'>
          <End Role='ComplexType-ComplexTypeProperty' EntitySet='ComplexType'/>
          <End Role='ComplexTypeProperty-ComplexType' EntitySet='ComplexTypeProperty'/>
        </AssociationSet>
      </EntityContainer>
    </Schema>
  </edmx:DataServices>
</edmx:Edmx>
```

#### エラーメッセージ一覧
[エラーメッセージ一覧](198_Error_Messages.html)を参照

#### レスポンスサンプル
なし

<br>
### CURLサンプル
#### CURLコマンド(UNIX)
```sh
curl 'https://fqdn/cell_name/box_name/odata_colname/$metadata/$metadata' -X GET
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
