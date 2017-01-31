# personium.io REST API リファレンス
REST API リファレンスへようこそ。
REST API リファレンスでは、personium.ioが提供するすべてのREST APIに関する技術的な詳細仕様を記述しています。
<br>
### Unit Level API
Unit Level APIは、Cell群をホストするユニットに属するAPI(Cellの生成や作成したCell群の管理)です。
これらAPIは原則としてCellから発行されたアクセストークンではアクセスできません。
（ユニットレベルのトークンへの昇格操作を経て利用可能）

Resource Path
````
https://{UnitFQDN}/
````
|<br>|<br>|作成・登録<br>|取得<br>|更新<br>|削除<br>|
|:--|:--|:--|:--|:--|:--|
|**Cell**|<br>|[作成](001_Create_Cell.html)|[一覧取得](002_List_Cell.html)<br>[取得](003_Get_Cell.html)|[更新](004_Update_Cell.html)|[削除](005_Delete_Cell.html)<br>[再帰的削除](006_Cell_Recursive_Delete.html)|
|**UUT**|[昇格設定](007_UUT_Elevation_Setting.html)|<br>|<br>|<br>|<br>|
<br>
### [Cell Level API](008_Cell_Level_APIs.html)
Resource Path
```
https://{UnitFQDN}/{CellName}
```

##### Cell内部設定API：単体セルモデルにおける設定

|<br>|<br>|作成・登録<br>|取得<br>|更新<br>|削除<br>|
|:--|:--|:--|:--|:--|:--|
|**Role**|<br>|[登録](009_Create_Role.html)|[一覧取得](010_Retrieve_Role.html)<br>[取得](011_Search_Role.html)|[更新](012_Update_Role.html)|[削除](013_Delete_Role.html)|
|<br>|_$links|[登録](014_Create_Role_$links.html)|[一覧取得](015_List_Role_$links.html)|[更新]
(016_Update_Role_$links.html)|[削除](017_Delete_Role_$links.html)|
|<br>|_NavProp経由|[登録](018_Register_Role_Using_NavProp.html)|[取得](019_List_Using_Role_NavProp.html)|<br>|<br>|
|**Account**|<br>|[登録](020_Create_Account.html)|[一覧取得](021_Retrieve_Account.html)<br>[取得]022_Search_Account.html)|[更新](023_Update_Account.html)|[削除](024_Delete_Account.html)|
|<br>|_$links|[登録](025_Register_Account_$links.html)|[一覧取得](026_Acquire_Account_$links_List.html)|[更新](027_Update_Account_$links.html)|[削除](028_Delete_Account_$links.html)|
|<br>|_NavProp経由|[登録](029_Register_Account_Navigation_Property.html)|[取得](030_Acquire_Account_Navigation_Property.html)|<br>|<br>|
|**Box**|<br>|[作成](064_Create_Box.html)|[取得](065_Search_Box.html)<br>[一覧取得](066_Retrieve_Box.html)<br>[URL取得](107_Get_Box_URL.html)|[更新](067_Update_Box.html)|[削除](068_Delete_Box.html)|
|<br>|_$links|[登録](069_Register_Box_$links.html)|[一覧取得](070_List_Box_$links.html)|[
更新](071_Update_Box_$links.html)
|[削除](072_Delete_Box_$links.html)|
|<br>|_NavProp経由|[登録](073_Register_Using_Box_NavProp.html)|[取得](074_List_Box_NavProp.html)|<br>|<br>|
|**認証**|[パスワード変更](102_Password_Change.html)|<br>|<br>|<br>|<br>|


##### ソーシャル連携設定API：複数セルモデルにおける設定

|<br>|<br>|作成・登録<br>|取得<br>|更新<br>|削除<br>|
|:--|:--|:--|:--|:--|:--|
|**ExtCell**|<br>|[登録](031_Create_External_Cell.html)|[一覧取得](032_List_External_Cell.html)<br>[取得](033_Get_External_Cell.html)|[更新](034_Update_External_Cell.html)|[削除](035_Delete_External_Cell.html)|
|<br>|_$links|[登録](036_Register_External_Cell_$links.html)|[一覧取得](037_List_External_Cell_$links.html)|[更新](038_Update_External_Cell_$links.html)|[削除](039_Delete_External_Cell_$links.html)|
|<br>|_NavProp経由|[登録](040_Register_External_Cell_Using_NavProp.html)|[取得](041_List_External_Cell_NavProp.html)|<br>|<br>|
|**Relation**|<br>|[登録](042_Create_Relation.html)|[一覧取得](043_List_Relation.html)<br>[取得](044_Retrieve_Relation.html)|[更新](045_Update_Relation.html)|[削除](046_Delete_Relation.html)|
|<br>|_$links|[登録](047_Register_Relation_$links.html)|[一覧取得](048_List_Relation_$links.html)|[更新](049_Update_Relation_$links.html)|[削除](050_Delete_Relation_$links.html)|
|<br>|_NavProp経由|[登録](051_Register_Using_Relation_NavProp.html)|[取得](052_List_Using_Relation_NavProp.html)|<br>|<br>|
|**ExtRole**|<br>|[登録](053_Create_External_Role.html)|[取得](054_List_External_Role.html)<br>[一覧取得](055_Get_External_Role.html)|[更新](056_Update_External_Role.html)|[削除](057_Delete_External_Role.html)|
|<br>|_$links|[登録](058_Register_External_Role_$links)|[一覧取得](059_Retrieve_External_Role_$links.html)|[更新](060_Update_External_Role_$links.html)|[削除](061_Delete_External_Role_$links.html)|
|<br>|_NavProp経由|[登録](062_Register_Using_Role_NavProp.html)|[取得](063_List_External_Role_NavProp.html)|<br>|<br>|
|**認証**<br>(/\__auth, /\__authz)|[OAuth2_認可エンドポイント](100_OAuth2_Authorization_Endpoint.html)<br>[OAuth2Token_エンドポイント認証](101_OAuth2_Token_Endpoint.html)|<br>|<br>|<br>|
|**アクセス制御**|<br>|[制限設定](097_Cell_ACL.html)|[プロパティ取得](098_Cell_Get_Property.html)|[プロパティ変更](099_Cell_Change_Property.html)|<br>|
|[イベント](088_Event_Summary.html)|<br>|[イベント受付](086_Event_Reception.html)|[ログファイル取得](092_Retrieve_Log_File.html)<br>[ログファイル一覧取得](092_ログファイル一覧取得API.html)<br>[ログファイル情報取得](091_Log_File_Information_Acquisition.html)|<br>|[ログファイル削除](094_Delete_Log_File.html)|
|**メッセージ制御**|[送信](080_Send_Message.html)|<br>|[取得](079_Retrieve_Sent_Message.html)<br>[一覧取得](078_List_Received_Messages.html)|<br>|[削除](082_Delete_Sent_Message.html)|
|<br>|**受信**|<br>|[取得](077_Get_Received_Message.html)<br>[一覧取得](076_List_Received_Messages.html)|[状態変更](075_Received_Message_Approval.html)|[削除](078_Delete_an_Incoming_Message.html)|
<br>
### [Box Level API](103_Box_Level_APIs.html)
Resource Path（※一部例外あり）

```
https://{UnitFQDN}/{CellName}/{BoxName}
https://{UnitFQDN}/{CellName}/{BoxName}/{ResourcePath}
```

##### Box管理
|<br>|<br>|作成・登録<br>|取得<br>|更新<br>|削除<br>|
|:--|:--|:--|:--|:--|:--|
|<br>|<br>|[Boxインストール](105_Box_Installation.html)|[Boxメタデータ取得](106_Progress_of_Bar_File_Installation.html)|<br>|<br>|

##### WebDAV
|<br>|<br>|作成・登録<br>|取得<br>|更新<br>|削除<br>|
|:--|:--|:--|:--|:--|:--|
|コレクション|<br>|[作成](109_Create_Collection.html)|[設定取得](110_Get_Property.html)|[設定変更](111_Change_Property.html)<br>[移動名称変更](501_Update_Move_Collection.html)|[削除](112_Delete_Collection.html)|
|ファイル|<br>|[登録更新](114_Register_and_Update_WebDAV.html)|[取得](113_Get_WebDav.html)<br>[設定取得](110_Get_Property.html)|[設定変更](111_Change_Property.html)|[削除](112_Delete_Collection.html)|
|アクセス制御|[設定](117_Configure_Access_Control.html)|<br>|<br>|<br>|<br>|

##### OData
|<br>|<br>|作成・登録<br>|取得<br>|更新<br>|削除<br>|
|:--|:--|:--|:--|:--|:--|
|**EntityType**|<br>|[登録](147_Create_EntityType.html)|[取得](149_Get_EntityType.html)<br>[一覧取得](148_List_EntityType.html)|[更新](150_Update_EntityType.html)|[削除](151_Delete_EntityType.html)|
|<br>|_$links|[登録](152_Register_EntityType_$links.html)|[一覧取得](153_List_EntityType_$links.html)|[更新](154_Update_EntityType_$links.html)|[削除](155_Delete_EntityType_$links.html)|
|<br>|_NavProp経由|<br>|[一覧取得](155_List_EntityType_Links_Navigation_Property.html)|<br>|<br>|<br>|
|**AssociationEnd**|<br>|[登録](120_Register_AssociationEnd.html)|[取得](122_Get_AssociationEnd.html)<br>[一覧取得](121_List_AssociationEnd.html)|[更新](123_Update_AssociationEnd.html)|[削除](124_Delete_AssociationEnd.html)|
|<br>|_$links|[登録](125_Register_AssociationEnd_$links.html)|[一覧取得](126_List_AssociationEnd_$links.html)|<br>|[削除](127_Delete_AssociationEnd_$links.html)|
|<br>|_NavProp経由|<br>|一覧取得|<br>|<br>|
|**ComplexType**|<br>|[登録](129_Register_ComplexType.html)|[取得](131_Get_ComplexType.html)<br>[一覧取得](130_List_ComplexType.html)|[更新](132_Update ComplexType.html)|[削除](133_Delete_ComplexType.html)|
|<br>|_$links|[登録](134_Register_ComplexType_$links.html)|[一覧取得](135_List_ComplexType_$links.html)|更新|[削除](136_Delete_ComplexType_$links.html)|
|**Property**|<br>|[登録](156_Register_Property.html)|[取得](158_Get_Property.html)<br>[一覧取得](157_List_Property.html)|[更新](159_Update_Property.html)|[削除](160_Delete_Property.html)|
|<br>|_$links|[登録](161_Register_Property_$links.html)|[一覧取得](162_List_Property_$links.html)|[更新](163_Update_List_Property_$links.html)|[削除](164_Delete_Property_$links.html)|
|**ComplexTypeProperty**|<br>|[登録](129_Register_ComplexType.html)|[取得](131_Get_ComplexType.html)<br>[一覧取得](130_List_ComplexType.html)|[更新](132_Update ComplexType.html)|[削除](133_Delete_ComplexType.html)|
|<br>|_$links|登録|一覧取得|更新|削除|
|**Entity**|[一括操作](170_Entity_Bulk_Operations.html)|[作成](166_Create_Entity.html)|[取得](168_Get_Entity.html)<br>[一覧取得](167_List_Entity.html)|[更新](169_Update_Entity.html)<br>[部分更新](171_Partial_Update_Entity.html)|[削除](172_Delete_Entity.html)|
|（ユーザデータ）|_$links|[登録](175_Register_User_Data_$links.html)|[一覧取得](177_User_Data_List_$links.html)|[更新](178_Update_User_Data_$links.html)|[削除](179_Delete_User_Data_$links.html)|
|（ユーザデータ）|_NavProp経由|[登録](180_Register_using_NavProp.html)|[一覧取得](181_List_using_NavProp.html)|<br>|<br>|


##### [サーバスクリプト(Engine Service Collection)](182_Engine_Service_Collection_APIs.html)
|<br>|<br>|作成・登録<br>|取得<br>|更新<br>|削除<br>|
|:--|:--|:--|:--|:--|:--|
|サービスコレクションソース|<br>|[作成](184_Create_Service_Collection_Source.html)|[取得](185_List_Service_Collection_Source.html)|[設定適用](183_Configure_Service_Collection.html)|[削除](186_Delete_Service_Collection_Source.html)|
|<br>|[サービス実行](187_Service_Execution.html)<br>|<br>|<br>|<br>|
<br>
### Cell/Box 共通 API
##### サービスドキュメント取得/スキーマ取得
|<br>|<br>|作成・登録<br>|取得<br>|更新<br>|削除<br>|
|:--|:--|:--|:--|:--|:--|
|サービスドキュメント|<br>|<br>|[取得](119_Document_Acquisition_Service.html)|<br>|<br>|
|スキーマ|<br>|<br>|[取得](118_User_Defined_Data_Schema.html)|<br>|<br>|


##### OData取得共通クエリ
|クエリ|一件取得使用可|一覧取得使用可|備考|
|:--|:--:|:--:|:--|
|[$formatクエリ](192_$Format_Query.html)|○|○|<br>|
|[$expandクエリ](193_$Expand_Query.html)|○|○|<br>|
|[$selectクエリ](194_$Select_Query.html)|○|○|<br>|
|[$orderbyクエリ](187_$Orderby_Query.html)|○|<br>|<br>|
|[$topクエリ](188_$Top_Query.html)|○|<br>|<br>|
|[$skipクエリ](189_$Skip_Query.html)|○|<br>|<br>|
|[$filterクエリ](190_$Filter_Query.html)|○|<br>|<br>|
|[$inlinecount](195_$Inlinecount_Query.html)|○|<br>|<br>|
|[全文検索(q)クエリ](196_Full_Text_Search_Query.html)|○|<br>|<br>|
<br>
### 共通
#### [エラーメッセージ一覧](200_Error_Messages.html)
#### [personiumのHTTP実装に関する制限事項](199_Common_Limitations_on_HTTP_Implementation.html)
#### [CORS対応](198_CORS_Support.html)
#### [クロスドメインポリシーファイル取得](197_Cross_Domain_Policy_File.html)
<br>
##### [用語集](201_Glossary.html)
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
