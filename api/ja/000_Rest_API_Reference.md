# Personium REST API リファレンス  
REST API リファレンスへようこそ。  
REST API リファレンスでは、Personiumが提供するすべてのREST APIに関する技術的な詳細仕様を記述しています。

リクエストURLの変数については[リクエストURLの変数一覧](005_Variable_of_Request_URL.md)を参照してください。
### Unit Level API
Unit Level API は、Cell群をホストするユニットに属するAPI(Cellの生成や作成したCell群の管理)です。  
これらAPIは原則としてCellから発行されたアクセストークンではアクセスできません。  

Resource Path
````
https://{UnitFQDN}/
````
||作成・登録|取得|更新|削除|
|:--|:--|:--|:--|:--|
|**Cell**|[作成](100_Create_Cell.html)|[一覧取得](101_List_Cell.html)<br>[取得](102_Get_Cell.html)|[更新](103_Update_Cell.html)|[削除](104_Delete_Cell.html)<br>[再帰的削除](105_Cell_Recursive_Delete.html)|

### Cell Level API
Cell Level API は、
* Cellにアクセスする人やアプリケーションを認証する機能
* Cellに対するアクセス制御を設定する機能
* Cell間の関係を構築する機能
* Boxを生成・管理する機能
* Cell間のメッセージを送受信する機能
* イベント処理の機能
* Cellをエクスポート/インポートする機能

から構成されます。これら機能群の多くはRESTベースでリレーショナルデータ操作を行う標準であるODataプロトコルに準拠する制御オブジェクトで操作可能です。

Resource Path
```
https://{UnitFQDN}/{CellName}
```

##### Cellにアクセスする人やアプリケーションを認証する機能
||作成・登録|取得|更新|削除|その他|
|:--|:--|:--|:--|:--|:--|
|**Account**|[登録](212_Create_Account.html)|[一覧取得](214_Search_Account.html)<br>[取得](213_Retrieve_Account.html)|[更新](215_Update_Account.html)<br>[パスワード変更](294_Password_Change.html)|[削除](216_Delete_Account.html)||
|&nbsp;&nbsp;_$links|[登録](217_Register_Account_links.html)|[一覧取得](218_Acquire_Account_links_List.html)|更新|[削除](220_Delete_Account_links.html)||
|&nbsp;&nbsp;_NavProp経由|[登録](221_Register_Account_Navigation_Property.html)|[取得](222_Acquire_Account_Navigation_Property.html)||||
|**認証**<br>(\_\_token, \_\_authz)|||||[OAuth2.0 認可エンドポイント](292_OAuth2_Authorization_Endpoint.html)<br>[OAuth2.0 トークンエンドポイント](293_OAuth2_Token_Endpoint.html)|

##### Cellに対するアクセス制御を設定する機能
||作成・登録|取得|更新|削除|
|:--|:--|:--|:--|:--|
|**Role**|[登録](201_Create_Role.html)|[一覧取得](202_Retrieve_Role.html)<br>[取得](203_Search_Role.html)|[更新](204_Update_Role.html)|[削除](205_Delete_Role.html)|
|&nbsp;&nbsp;_$links|[登録](206_Create_Role_links.html)|[一覧取得](207_List_Role_links.html)|更新|[削除](209_Delete_Role_links.html)|
|&nbsp;&nbsp;_NavProp経由|[登録](210_Register_Role_Using_NavProp.html)|[取得](211_List_Using_Role_NavProp.html)|||
|**アクセス制御**|[制限設定](289_Cell_ACL.html)|[プロパティ取得](290_Cell_Get_Property.html)|[プロパティ変更](291_Cell_Change_Property.html)||

##### Cell間の関係を構築する機能
||作成・登録|取得|更新|削除|
|:--|:--|:--|:--|:--|
|**ExtCell**|[登録](223_Create_External_Cell.html)|[一覧取得](224_List_External_Cell.html)<br>[取得](225_Get_External_Cell.html)|[更新](226_Update_External_Cell.html)|[削除](227_Delete_External_Cell.html)|
|&nbsp;&nbsp;_$links|[登録](228_Register_External_Cell_links.html)|[一覧取得](229_List_External_Cell_links.html)|更新|[削除](231_Delete_External_Cell_links.html)|
|&nbsp;&nbsp;_NavProp経由|[登録](232_Register_External_Cell_Using_NavProp.html)|[一覧取得](233_List_External_Cell_NavProp.html)|||
|**Relation**|[登録](234_Create_Relation.html)|[一覧取得](235_List_Relation.html)<br>[取得](236_Retrieve_Relation.html)|[更新](237_Update_Relation.html)|[削除](238_Delete_Relation.html)|
|&nbsp;&nbsp;_$links|[登録](239_Register_Relation_links.html)|[一覧取得](240_List_Relation_links.html)|更新|[削除](242_Delete_Relation_links.html)|
|&nbsp;&nbsp;_NavProp経由|[登録](243_Register_Using_Relation_NavProp.html)|[取得](244_List_Using_Relation_NavProp.html)|||
|**ExtRole**|[登録](245_Create_External_Role.html)|[取得](247_Get_External_Role.html)<br>[一覧取得](246_List_External_Role.html)|[更新](248_Update_External_Role.html)|[削除](249_Delete_External_Role.html)|
|&nbsp;&nbsp;_$links|[登録](250_Register_External_Role_links.html)|[一覧取得](251_Retrieve_External_Role_links.html)|更新|[削除](253_Delete_External_Role_links.html)|
|&nbsp;&nbsp;_NavProp経由|[登録](254_Register_Using_Role_NavProp.html)|[取得](255_List_External_Role_NavProp.html)|||

##### Boxを生成・管理する機能
||作成・登録|取得|更新|削除|
|:--|:--|:--|:--|:--|
|**Box**|[作成](256_Create_Box.html)|[取得](258_Retrieve_Box.html)<br>[一覧取得](257_Search_Box.html)<br>[URL取得](304_Get_Box_URL.html)|[更新](259_Update_Box.html)|[削除](260_Delete_Box.html)|
|&nbsp;&nbsp;_$links|[登録](261_Register_Box_links.html)|[一覧取得](262_List_Box_links.html)|更新|[削除](264_Delete_Box_links.html)|
|&nbsp;&nbsp;_NavProp経由|[登録](265_Register_Using_Box_NavProp.html)|[取得](266_List_Box_NavProp.html)|||
|&nbsp;&nbsp;インストール|[Boxインストール](302_Box_Installation.html)|[Boxメタデータ取得](303_Progress_of_Bar_File_Installation.html)|||

##### Cell間のメッセージを送受信する機能
||作成・登録|取得|更新|削除|
|:--|:--|:--|:--|:--|
|**メッセージ制御**<br>[送信](271_Send_Message.html)||[取得](272_Retrieve_Sent_Message.html)<br>[一覧取得](273_List_Sent_Messages.html)||[削除](274_Delete_Sent_Message.html)|
|**メッセージ制御**<br>受信||[取得](269_Get_Received_Message.html)<br>[一覧取得](268_List_Received_Messages.html)|[状態変更](267_Received_Message_Approval.html)|[削除](270_Delete_an_Incoming_Message.html)|

##### イベント処理の機能
||作成・登録|取得|更新|削除|
|:--|:--|:--|:--|:--|
|[イベント](277_Event_Summary.html)|[イベント受付](278_Event_Reception.html)|[ログファイル取得](285_Retrieve_Log_File.html)<br>[ログファイル一覧取得](284_Retrieve_Log_File_list.html)<br>[ログファイル情報取得](283_Log_File_Information_Acquisition.html)||[ログファイル削除](286_Delete_Log_File.html)|

##### Cellをエクスポート/インポートする機能
エクスポート実行でCellのスナップショットファイルが作成されます。<br>インポート実行でスナップショットファイルの内容をCellに取り込みます。<br>スナップショットファイルはWebDAVのインターフェースで操作可能です。

||作成・登録|取得|更新|削除|
|:--|:--|:--|:--|:--|
|**エクスポート**|[実行](501_Export_Cell.html)|[状態取得](502_Progress_of_Export_Cell.html)|||
|**インポート**|[実行](507_Import_Cell.html)|[状態取得](508_Progress_of_Import_Cell.html)|||
|**スナップショット**|[登録更新](503_Register_and_Update_Snapshot_Cell.html)|[取得](504_Get_Snapshot_Cell.html)<br>[設定取得](505_Get_Property_Snapshot_Cell.html)||[削除](506_Delete_Snapshot_Cell.html)|


### Box Level API
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

##### WebDAV
||作成・登録|取得|更新|削除|その他|
|:--|:--|:--|:--|:--|:--|
|コレクション|[作成](306_Create_Collection.html)|[設定取得](305_Get_Property.html)|[設定変更](308_Change_Property.html)<br>[移動名称変更](309_Update_Move_Collection.html)|[削除](310_Delete_Collection.html)||
|ファイル|[登録更新](312_Register_and_Update_WebDAV.html)|[取得](311_Get_WebDav.html)<br>[設定取得](307_Get_Property.html)|[設定変更](313_Change_Property.html)|[削除](314_Delete_WebDAV.html)||
|アクセス制御|||||[設定](315_Configure_Access_Control.html)|

※ すべてのファイルやコレクション（特殊コレクションを含む）に対してACL設定(アクセス制御設定)が可能です。  
※ ACL設定 は PROPFINDメソッドで取得できます。

##### OData
||作成・登録|取得|更新|削除|その他|
|:--|:--|:--|:--|:--|:--|
|**EntityType**|[登録](345_Create_EntityType.html)|[取得](347_Get_EntityType.html)<br>[一覧取得](346_List_EntityType.html)|[更新](348_Update_EntityType.html)|[削除](349_Delete_EntityType.html)||
|&nbsp;&nbsp;_$links|登録|一覧取得|更新|削除||
|&nbsp;&nbsp;_NavProp経由||一覧取得||||
|**AssociationEnd**|[登録](318_Register_AssociationEnd.html)|[取得](320_Get_AssociationEnd.html)<br>[一覧取得](319_List_AssociationEnd.html)|[更新](321_Update_AssociationEnd.html)|[削除](322_Delete_AssociationEnd.html)||
|&nbsp;&nbsp;_$links|[登録](323_Register_AssociationEnd_links.html)|[一覧取得](324_List_AssociationEnd_links.html)||[削除](325_Delete_AssociationEnd_links.html)||
|&nbsp;&nbsp;_NavProp経由||一覧取得||||
|**ComplexType**|[登録](327_Register_ComplexType.html)|[取得](329_Get_ComplexType.html)<br>[一覧取得](328_List_ComplexType.html)|更新|[削除](331_Delete_ComplexType.html)||
|&nbsp;&nbsp;_$links|登録|一覧取得|更新|削除||
|**Property**|[登録](355_Register_Property.html)|[取得](357_Get_Property.html)<br>[一覧取得](356_List_Property.html)|更新|[削除](359_Delete_Property.html)||
|&nbsp;&nbsp;_$links|登録|一覧取得|更新|削除||
|**ComplexTypeProperty**|[登録](336_Register_ComplexTypeProperty.html)|[取得](338_Get_ComplexTypeProperty.html)<br>[一覧取得](337_List_ComplexTypeProperty.html)|[更新](339_Update_ComplexTypeProperty.html)|[削除](340_Delete_ComplexTypeProperty.html)||
|&nbsp;&nbsp;_$links|登録|一覧取得|更新|削除||
|**Entity**|[作成](364_Create_Entity.html)|[取得](366_Get_Entity.html)<br>[一覧取得](365_List_Entity.html)|[更新](367_Update_Entity.html)<br>[部分更新](369_Partial_Update_Entity.html)|[削除](370_Delete_Entity.html)|[一括操作](368_Entity_Bulk_Operations.html)|
|&nbsp;&nbsp;_$links|[登録](373_Register_User_Data_links.html)|[一覧取得](374_User_Data_List_links.html)|更新|[削除](376_Delete_User_Data_links.html)||
|&nbsp;&nbsp;_NavProp経由|[登録](377_Register_using_NavProp.html)|[一覧取得](378_List_using_NavProp.html)||||


##### サーバスクリプト(Engine Service Collection)
PersoniumアプリケーションやCell利用者が作成したサーバサイドロジックを登録しこれを走行させることができます。  
はじめに、ユーザロジックをファイルとして登録し、サービスコレクションの設定を行ってパスとの関連付けを行うことで、  
コレクション配下の任意のパスからのリクエストに対してユーザーロジックを走行させることができます。

||作成・登録|取得|更新|削除|その他|
|:--|:--|:--|:--|:--|:--|
|サービスコレクションソース|[作成](381_Create_Service_Collection_Source.html)|[取得](382_List_Service_Collection_Source.html)|[設定適用](380_Configure_Service_Collection.html)|[削除](383_Delete_Service_Collection_Source.html)|[サービス実行](384_Service_Execution.html)|

##### サービスドキュメント取得/スキーマ取得
||取得|
|:--|:--|
|サービスドキュメント|[取得](317_Document_Acquisition_Service.html)|
|スキーマ|[取得](316_User_Defined_Data_Schema.html)|


##### OData取得共通クエリ
|クエリ|一件取得使用可|一覧取得使用可|
|:--|:--:|:--:|
|[$formatクエリ](404_Format_Query.html)|○|○|
|[$expandクエリ](405_Expand_Query.html)|○|○|
|[$selectクエリ](406_Select_Query.html)|○|○|
|[$orderbyクエリ](400_Orderby_Query.html)|○|○|
|[$topクエリ](401_Top_Query.html)|○|○|
|[$skipクエリ](402_Skip_Query.html)|○|○|
|[$filterクエリ](403_Filter_Query.html)|○|○|
|[$inlinecount](407_Inlinecount_Query.html)|○|○|
|[全文検索(q)クエリ](408_Full_Text_Search_Query.html)|○|○|

### 共通
#### [エラーメッセージ一覧](004_Error_Messages.html)
#### [PersoniumのHTTP実装に関する制限事項](003_Common_Limitations_on_HTTP_Implementation.html)
#### [CORS対応](002_CORS_Support.html)
#### [クロスドメインポリシーファイル取得](001_Cross_Domain_Policy_File.html)

###### Copyright 2017 FUJITSU LIMITED
