# リクエストURLの変数一覧
### 概要
APIリファレンスのリクエストURLで使われている変数についての説明

### 変数一覧

|変数名|概要|備考|
|:--|:--|:--|
|{UnitFQDN}|Personiumが動作しているサーバのFQDN<br>UnitとはPersoniumサーバ内で、複数のCellから構成されるデータ領域を指す||
|{CellName}|Cell名<br>Cellとはデータ主体ごとのData Stroreを指す||
|{BoxName}|Box名<br>Boxとはアプリケーションに用いるデータを格納する領域を指す||
|{SchemaURL}|SchemaのURL<br>SchemaとはPersonium内に格納されたSchemaを指す||
|{RoleName}|Role名<br>RoleとはすべてのCellに対して定義される「役割」を指す||
|{AccountName}|Account名<br>Accountとは特定のCellに属するユーザを指す||
|{ExtCellURL}|ExtCellのURL<br>ExtCell（外部Cell）とはあるCellの外にある他のCellを指す||
|{RelationName}|Relation名<br>RelationとはCellとその外部Cellとの関係を示す定義体を指す||
|{ExtRoleURL}|ExtRoleのURL<br>ExtRole（外部Role）とは特定の関係にある外部Cellにて、特定の役割（Role）を付与された利用者主体を指す||
|{LogName}|ログファイル名||
|{MessageID}|MessageのID<br>MessageとはCell間で送受信可能なメッセージを指す||
|{CollectionName}|Collection名<br>CollectionとはCellにあるBoxの中に格納されたデータ集合を指す||
|{ResourcePath}|リソースへのパス<br>Box配下のCollectionとファイルが対象となる||
|{OdataCollectionName}|OdataCollection名<br>OdataCollectionとはユーザーがODataを操作するための特別なコレクションを指す||
|{EntityTypeName}|EntityType名<br>EntityTypeとはデータの構造をEntityDataModel(EDM)であらわすための定義体を指す|Entityの上位概念|
|{AssociationEndName}|AssociationEnd名<br>AssociationEndとはAssociationを構成するエンドポイントとなっているEntityTypeを指す||
|{ComplexTypeName}|ComplexType名<br>ComplexTypeとは下位属性を伴った属性を持つPropertyを指す|ComplexTypePropertyの上位概念|
|{PropertyName}|Property名<br>各EntityTypeの列頭の値を指し、RDBにおけるテーブルの項目名に相当する||
|{ComplexTypePropertyName}|ComplexTypeProperty名<br>ComplexTypePropertyとはComplexTypeの下位属性の名称を指す|ComplexTypeの下位属性|
|{EntityName}|Entity名<br>Entityとはデータの記録構造を指し、RDBにおけるテーブル1行分に相当する|EntityTypeの下位属性|
|{EntityID}|EntityのID||
|{NavigationPropertyName}|NavigationProperty名<br>NavigationPropertyとはEntityDataModelやODataのデータ構造において、Associationの一方のEndから別のEndへのナビゲーションを表すPropertyを指す||
|{ServiceSourceName}|ServiceSource名<br>ServiceとはServiceCollectionに登録されたユーザ定義のサーバサイドロジックを指す||
|{ServiceName}|Service名<br>Serviceを実行する際に用いられる|ServiceSource名から拡張子を除いたもの|

###### Copyright 2017 FUJITSU LIMITED
