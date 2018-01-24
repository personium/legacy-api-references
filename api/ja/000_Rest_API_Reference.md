# Personium REST API リファレンス  
REST API リファレンスへようこそ。  
REST API リファレンスでは、Personiumが提供するすべてのREST APIに関する技術的な詳細仕様を記述しています。

リクエストURLの変数については[リクエストURLの変数一覧](005_Variable_of_Request_URL.md)を参照してください。

## Unit Level API
Unit Level API はCell群をホストするユニットに属するAPIで、Cellの生成や作成したCell群の管理を行うものです。  
これらAPIは原則としてCellから発行されたアクセストークンではアクセスできず、ユニットユーザとしてのトークンでのみアクセス可能です。 

Unit Root URL
````
https://{UnitFQDN}/
````

Unit Level API はUnit制御オブジェクトという形で実現されています。
ODataに従ったREST操作でUnit制御オブジェクトを操作してください。

### Cell (ユニット制御オブジェクト)

|種別|操作|
|:--|:--|
|基本操作|[作成](100_Create_Cell.md)&nbsp; &nbsp;[一覧取得](101_List_Cell.md)&nbsp; &nbsp;[取得](102_Get_Cell.md)&nbsp; &nbsp;[更新](103_Update_Cell.md)&nbsp; &nbsp;[削除](104_Delete_Cell.md)&nbsp; &nbsp;[再帰的削除](105_Cell_Recursive_Delete.md)|

## Cell Level API
Cell Level API は、
* Cellにアクセスする人やアプリケーションを認証する機能
* Cellに対するアクセス制御を設定する機能
* Cell間の関係を構築する機能
* Boxを生成・管理する機能
* Cell間のメッセージを送受信する機能
* イベント処理の機能
* Cellをエクスポート/インポートする機能

から構成されます。これら機能群の多くはCell制御オブジェクトという形で実現されています。Cell制御オブジェクトはODataに従ったREST操作で操作可能です。

Cell Root URL
```
https://{UnitFQDN}/{CellName}/
```

### Cellにアクセスする人やアプリケーションを認証する機能
#### 認証

* [OAuth2.0 認可エンドポイント](292_OAuth2_Authorization_Endpoint.md)<br>
* [OAuth2.0 トークンエンドポイント](293_OAuth2_Token_Endpoint.md)<br>
* [パスワード変更](294_Password_Change.md)<br>

#### Account (Cell制御オブジェクト)

|種別|操作|
|:--|:--|
|基本操作|[登録](212_Create_Account.md)&nbsp; [取得](213_Retrieve_Account.md)&nbsp;[一覧取得](214_Search_Account.md) &nbsp;[更新](215_Update_Account.md) &nbsp;[削除](216_Delete_Account.md) |
|他オブジェクトとのリンク|[リンク](217_Register_Account_links.md)&nbsp; &nbsp;  [リンク解除](220_Delete_Account_links.md) &nbsp; &nbsp; [リンク一覧取得](218_Acquire_Account_links_List.md) &nbsp; &nbsp; リンク更新はありません|
|Navigation Property 経由の操作|[登録](221_Register_Account_Navigation_Property.md)&nbsp; &nbsp;[一覧取得](222_Acquire_Account_Navigation_Property.md)|

### Cellに対するアクセス制御を設定する機能

#### Role (Cell制御オブジェクト)

|種別|操作|
|:--|:--|
|基本操作|[登録](201_Create_Role.md)&nbsp;&nbsp; [一覧取得](202_Retrieve_Role.md)&nbsp;&nbsp;[取得](203_Search_Role.md)&nbsp;&nbsp;[更新](204_Update_Role.md)&nbsp;&nbsp;[削除](205_Delete_Role.md)|
|他オブジェクトとのリンク|[リンク](206_Create_Role_links.md)&nbsp;&nbsp;[リンク解除](209_Delete_Role_links.md)&nbsp;&nbsp;[リンク一覧取得](207_List_Role_links.md)&nbsp;&nbsp;リンク更新はありません|
|Navigation Property 経由の操作|[登録](210_Register_Role_Using_NavProp.md)&nbsp; &nbsp;[一覧取得](211_List_Using_Role_NavProp.md)|

#### アクセス制御

* [制限設定](289_Cell_ACL.md)
* [プロパティ取得](290_Cell_Get_Property.md)
* [プロパティ変更](291_Cell_Change_Property.md)

### Cell間の関係を構築する機能

#### ExtCell (Cell制御オブジェクト)

|種別|操作|
|:--|:--|
|基本操作|[登録](223_Create_External_Cell.md)&nbsp;&nbsp; [一覧取得](224_List_External_Cell.md)&nbsp;&nbsp;[取得](225_Get_External_Cell.md)&nbsp;&nbsp;[更新](226_Update_External_Cell.md)&nbsp;&nbsp;[削除](227_Delete_External_Cell.md)|
|他オブジェクトとのリンク|[リンク](228_Register_External_Cell_links.md)&nbsp;&nbsp;[リンク解除](231_Delete_External_Cell_links.md)&nbsp;&nbsp;[リンク一覧取得](229_List_External_Cell_links.md)&nbsp;&nbsp;リンク更新はありません|
|Navigation Property 経由の操作|[登録](232_Register_External_Cell_Using_NavProp.md)&nbsp; &nbsp;[一覧取得](233_List_External_Cell_NavProp.md)|

#### Relation (Cell制御オブジェクト)

|種別|操作|
|:--|:--|
|基本操作|[登録](234_Create_Relation.md)&nbsp; &nbsp; [一覧取得](235_List_Relation.md)&nbsp; &nbsp;[取得](236_Retrieve_Relation.md)&nbsp; &nbsp;[更新](237_Update_Relation.md)&nbsp; &nbsp;[削除](238_Delete_Relation.md)|
|他オブジェクトとのリンク|[リンク](239_Register_Relation_links.md)&nbsp;&nbsp;[リンク解除](242_Delete_Relation_links.md)&nbsp;&nbsp;[リンク一覧取得](240_List_Relation_links.md)&nbsp;&nbsp;リンク更新はありません|
|Navigation Property 経由の操作|[登録](243_Register_Using_Relation_NavProp.md)&nbsp; &nbsp;[一覧取得](244_List_Using_Relation_NavProp.md)|

#### ExtRole (Cell制御オブジェクト)

|種別|操作|
|:--|:--|
|基本操作|[登録](245_Create_External_Role.md)&nbsp;&nbsp; [一覧取得](246_List_External_Role.md)&nbsp;&nbsp;[取得](247_Get_External_Role.md)&nbsp;&nbsp;[更新](248_Update_External_Role.md)&nbsp;&nbsp;[削除](249_Delete_External_Role.md)|
|他オブジェクトとのリンク|[リンク](250_Register_External_Role_links.md)&nbsp; &nbsp;[リンク解除](253_Delete_External_Role_links.md)&nbsp;&nbsp;[リンク一覧取得](251_Retrieve_External_Role_links.md)&nbsp; &nbsp;リンク更新はありません|
|Navigation Property 経由の操作|[登録](254_Register_Using_Role_NavProp.md)&nbsp; &nbsp;[一覧取得](255_List_External_Role_NavProp.md)|


### Boxを生成・管理する機能
||作成・登録|取得|更新|削除|
|:--|:--|:--|:--|:--|
|**Box**|[作成](256_Create_Box.md)|[取得](258_Retrieve_Box.md)<br>[一覧取得](257_Search_Box.md)<br>[URL取得](304_Get_Box_URL.md)|[更新](259_Update_Box.md)|[削除](260_Delete_Box.md)<br>[再帰的削除](295_Box_Recursive_Delete.md)|
|&nbsp;&nbsp;_$links|[登録](261_Register_Box_links.md)|[一覧取得](262_List_Box_links.md)|更新|[削除](264_Delete_Box_links.md)|
|&nbsp;&nbsp;_NavProp経由|[登録](265_Register_Using_Box_NavProp.md)|[取得](266_List_Box_NavProp.md)|||
|&nbsp;&nbsp;インストール|[Boxインストール](302_Box_Installation.md)|[Boxメタデータ取得](303_Progress_of_Bar_File_Installation.md)|||

### Cell間のメッセージを送受信する機能
||作成・登録|取得|更新|削除|
|:--|:--|:--|:--|:--|
|**メッセージ制御**<br>[送信](271_Send_Message.md)||[取得](272_Retrieve_Sent_Message.md)<br>[一覧取得](273_List_Sent_Messages.md)||[削除](274_Delete_Sent_Message.md)|
|**メッセージ制御**<br>受信||[取得](269_Get_Received_Message.md)<br>[一覧取得](268_List_Received_Messages.md)|[状態変更](267_Received_Message_Approval.md)|[削除](270_Delete_an_Incoming_Message.md)|

### イベント制御の機能

* [イベントの概要](277_Event_Summary.md)
* [外部イベント受付](278_Event_Reception.md)

#### イベント制御ルール　（Cell制御オブジェクト）

（未稿）

#### イベントログ操作

* [ログファイル取得](285_Retrieve_Log_File.md) 
* [ログファイル一覧取得](284_Retrieve_Log_File_list.md) 
* [ログファイル情報取得](283_Log_File_Information_Acquisition.md) 
* [ログファイル削除](286_Delete_Log_File.md)

### Cellをエクスポート/インポートする機能
エクスポート実行でCellのスナップショットファイルが作成されます。<br>インポート実行でスナップショットファイルの内容をCellに取り込みます。<br>スナップショットファイルはWebDAVのインターフェースで操作可能です。

||作成・登録|取得|更新|削除|
|:--|:--|:--|:--|:--|
|**エクスポート**|[実行](501_Export_Cell.md)|[状態取得](502_Progress_of_Export_Cell.md)|||
|**インポート**|[実行](507_Import_Cell.md)|[状態取得](508_Progress_of_Import_Cell.md)|||
|**スナップショット**|[登録更新](503_Register_and_Update_Snapshot_Cell.md)|[取得](504_Get_Snapshot_Cell.md)<br>[設定取得](505_Get_Property_Snapshot_Cell.md)||[削除](506_Delete_Snapshot_Cell.md)|


## Box Level API
Box Level API は、アプリケーション等がデータを操作するためのAPIで、WebDAVをベースとしたファイルシステム的な考え方のAPI群です。  
通常のファイルシステムと同様にファイルの配置・取得、フォルダ（collection）の作成・管理、ファイルやフォルダの一覧取得、アクセス制御の設定・参照等が可能です。

また以下の特殊コレクションをサポートしているため、ファイル状のデータのみではなく、様々な形態のデータを扱うことができます。  
これら特殊コレクションはBoxが提供するWebDAV空間上のいかなるパスに作成することもできます。

|特殊コレクション|用途|備考|
|:--|:--|:--|
|OData Service Collection|リレーショナルデータ||
|Engine Service Collection|カスタマイズロジックの走行||
|CALDAV Collection|カレンダーデータ|未実装|
|Link Collection|他のCellや他Boxの特定の領域へのエイリアス|未実装|

Resource Path（※一部例外あり）

```
https://{UnitFQDN}/{CellName}/{BoxName}
https://{UnitFQDN}/{CellName}/{BoxName}/{ResourcePath}
```

### WebDAV
||作成・登録|取得|更新|削除|その他|
|:--|:--|:--|:--|:--|:--|
|コレクション|[作成](306_Create_Collection.md)|[設定取得](305_Get_Property.md)|[設定変更](308_Change_Property.md)<br>[移動名称変更](309_Update_Move_Collection.md)|[削除](310_Delete_Collection.md)||
|ファイル|[登録更新](312_Register_and_Update_WebDAV.md)|[取得](311_Get_WebDav.md)<br>[設定取得](307_Get_Property.md)|[設定変更](313_Change_Property.md)|[削除](314_Delete_WebDAV.md)||
|アクセス制御|||||[設定](315_Configure_Access_Control.md)|

※ すべてのファイルやコレクション（特殊コレクションを含む）に対してACL設定(アクセス制御設定)が可能です。  
※ ACL設定 は PROPFINDメソッドで取得できます。

### OData
||作成・登録|取得|更新|削除|その他|
|:--|:--|:--|:--|:--|:--|
|**EntityType**|[登録](345_Create_EntityType.md)|[取得](347_Get_EntityType.md)<br>[一覧取得](346_List_EntityType.md)|[更新](348_Update_EntityType.md)|[削除](349_Delete_EntityType.md)||
|&nbsp;&nbsp;_$links|登録|一覧取得|更新|削除||
|&nbsp;&nbsp;_NavProp経由||一覧取得||||
|**AssociationEnd**|[登録](318_Register_AssociationEnd.md)|[取得](320_Get_AssociationEnd.md)<br>[一覧取得](319_List_AssociationEnd.md)|[更新](321_Update_AssociationEnd.md)|[削除](322_Delete_AssociationEnd.md)||
|&nbsp;&nbsp;_$links|[登録](323_Register_AssociationEnd_links.md)|[一覧取得](324_List_AssociationEnd_links.md)||[削除](325_Delete_AssociationEnd_links.md)||
|&nbsp;&nbsp;_NavProp経由||一覧取得||||
|**ComplexType**|[登録](327_Register_ComplexType.md)|[取得](329_Get_ComplexType.md)<br>[一覧取得](328_List_ComplexType.md)|更新|[削除](331_Delete_ComplexType.md)||
|&nbsp;&nbsp;_$links|登録|一覧取得|更新|削除||
|**Property**|[登録](355_Register_Property.md)|[取得](357_Get_Property.md)<br>[一覧取得](356_List_Property.md)|更新|[削除](359_Delete_Property.md)||
|&nbsp;&nbsp;_$links|登録|一覧取得|更新|削除||
|**ComplexTypeProperty**|[登録](336_Register_ComplexTypeProperty.md)|[取得](338_Get_ComplexTypeProperty.md)<br>[一覧取得](337_List_ComplexTypeProperty.md)|[更新](339_Update_ComplexTypeProperty.md)|[削除](340_Delete_ComplexTypeProperty.md)||
|&nbsp;&nbsp;_$links|登録|一覧取得|更新|削除||
|**Entity**|[作成](364_Create_Entity.md)|[取得](366_Get_Entity.md)<br>[一覧取得](365_List_Entity.md)|[更新](367_Update_Entity.md)<br>[部分更新](369_Partial_Update_Entity.md)|[削除](370_Delete_Entity.md)|[一括操作](368_Entity_Bulk_Operations.md)|
|&nbsp;&nbsp;_$links|[登録](373_Register_User_Data_links.md)|[一覧取得](374_User_Data_List_links.md)|更新|[削除](376_Delete_User_Data_links.md)||
|&nbsp;&nbsp;_NavProp経由|[登録](377_Register_using_NavProp.md)|[一覧取得](378_List_using_NavProp.md)||||


### サーバスクリプト(Engine Service Collection)
PersoniumアプリケーションやCell利用者が作成したサーバサイドロジックを登録しこれを走行させることができます。  
はじめに、ユーザロジックをファイルとして登録し、サービスコレクションの設定を行ってパスとの関連付けを行うことで、  
コレクション配下の任意のパスからのリクエストに対してユーザーロジックを走行させることができます。

||作成・登録|取得|更新|削除|その他|
|:--|:--|:--|:--|:--|:--|
|サービスコレクションソース|[作成](381_Create_Service_Collection_Source.md)|[取得](382_List_Service_Collection_Source.md)|[設定適用](380_Configure_Service_Collection.md)|[削除](383_Delete_Service_Collection_Source.md)|[サービス実行](384_Service_Execution.md)|

### サービスドキュメント取得/スキーマ取得
||取得|
|:--|:--|
|サービスドキュメント|[取得](317_Document_Acquisition_Service.md)|
|スキーマ|[取得](316_User_Defined_Data_Schema.md)|


### OData取得共通クエリ
|クエリ|一件取得使用可|一覧取得使用可|
|:--|:--|:--|
|[$formatクエリ](404_Format_Query.md)|○|○|
|[$expandクエリ](405_Expand_Query.md)|○|○|
|[$selectクエリ](406_Select_Query.md)|○|○|
|[$orderbyクエリ](400_Orderby_Query.md)|○|○|
|[$topクエリ](401_Top_Query.md)|○|○|
|[$skipクエリ](402_Skip_Query.md)|○|○|
|[$filterクエリ](403_Filter_Query.md)|○|○|
|[$inlinecount](407_Inlinecount_Query.md)|○|○|
|[全文検索(q)クエリ](408_Full_Text_Search_Query.md)|○|○|

## 共通
### [エラーメッセージ一覧](004_Error_Messages.md)
### [PersoniumのHTTP実装に関する制限事項](003_Common_Limitations_on_HTTP_Implementation.md)
### [CORS対応](002_CORS_Support.md)
### [クロスドメインポリシーファイル取得](001_Cross_Domain_Policy_File.md)

