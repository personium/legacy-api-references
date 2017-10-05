# リクエストURLの変数一覧
### 概要
APIリファレンスのリクエストURLで使われている変数についての説明

### 変数一覧

|変数名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|{UnitFQDN}<br>|Personiumが動作しているサーバのFQDN<br>UnitとはPersoniumサーバ内で、複数のCellから構成されるデータ領域を指す<br>|<br>|
|{CellName}<br>|Cell名<br>Cellとはデータ主体ごとのData Stroreを指す<br>|<br>|
|{BoxName}<br>|Box名<br>Boxとはアプリケーションに用いるデータを格納する領域を指す<br>|<br>|
|{SchemaURL}<br>|SchemaのURL<br>SchemaとはPersonium内に格納されたSchemaを指す|<br>|
|{RoleName}<br>|Role名<br>RoleとはすべてのCellに対して定義される「役割」を指す<br>|<br>|
|{AccountName}<br>|Account名<br>Accountとは特定のCellに属するユーザを指す<br>|<br>|
|{ExtCellURL}<br>|ExtCellのURL<br>ExtCell（外部Cell）とはあるCellの外にある他のCellを指す<br>|<br>|
|{RelationName}<br>|Relation名<br>RelationとはCellとその外部Cellとの関係を示す定義体を指す<br>|<br>|
|{ExtRoleURL}<br>|ExtRoleのURL<br>ExtRole（外部Role）とは特定の関係にある外部Cellにて、特定の役割（Role）を付与された利用者主体を指す<br>|<br>|
|{LogName}<br>|ログファイル名<br>|<br>|
|{MessageID}<br>|MessageのID<br>MessageとはCell間で送受信可能なメッセージを指す<br>|<br>|
|{CollectionName}<br>|Collection名<br>CollectionとはCellにあるBoxの中に格納されたデータ集合を指す<br>|<br>|
|{ResourcePath}<br>|リソースへのパス<br>Box配下のCollectionとファイルが対象となる<br>|<br>|
|{OdataCollectionName}<br>|OdataCollection名<br>OdataCollectionとはユーザーがODataを操作するための特別なコレクションを指す<br>|<br>|
|{EntityTypeName}<br>|EntityType名<br>EntityTypeとはデータの構造をEntityDataModel(EDM)であらわすための定義体を指す<br>|Entityの上位概念<br>|
|{AssociationEndName}<br>|AssociationEnd名<br>AssociationEndとはAssociationを構成するエンドポイントとなっているEntityTypeを指す<br>|<br>|
|{ComplexTypeName}<br>|ComplexType名<br>ComplexTypeとは下位属性を伴った属性を持つPropertyを指す<br>|ComplexTypePropertyの上位概念<br>|
|{PropertyName}<br>|Property名<br>各EntityTypeの列頭の値を指し、RDBにおけるテーブルの項目名に相当する<br>|<br>|
|{ComplexTypePropertyName}<br>|ComplexTypeProperty名<br>ComplexTypePropertyとはComplexTypeの下位属性の名称を指す<br>|ComplexTypeの下位属性<br>|
|{EntityName}<br>|Entity名<br>Entityとはデータの記録構造を指し、RDBにおけるテーブル1行分に相当する<br>|EntityTypeの下位属性<br>|
|{EntityID}<br>|EntityのID<br>|<br>|
|{NavigationPropertyName}<br>|NavigationProperty名<br>NavigationPropertyとはEntityDataModelやODataのデータ構造において、Associationの一方のEndから別のEndへのナビゲーションを表すPropertyを指す<br>|<br>|
|{ServiceSourceName}<br>|ServiceSource名<br>ServiceとはServiceCollectionに登録されたユーザ定義のサーバサイドロジックを指す<br>|<br>|
|{ServiceName}<br>|Service名<br>Serviceを実行する際に用いられる<br>|ServiceSource名から拡張子を除いたもの<br>|
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
