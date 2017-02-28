# CellレベルAPI概要
CellレベルAPI は、
* Cellにアクセスする人やアプリケーションを認証するための認証機構
* ソーシャルグラフを構築するための機能
* Boxの生成・管理を行う機能
* メッセージやイベント処理の機能
* これらAPI群に対するアクセス制御を設定する機能

等から構成されます。これら機能群の多くはRESTベースでリレーショナルデータ操作を行う標準であるODataプロトコルで操作可能な制御オブジェクトという形で実装しています。
<br>

|<br>|<br>|作成・登録<br>|取得<br>|更新<br>|削除<br>|
|:--|:--|:--|:--|:--|:--|
|**認証**|[OAuth2_認可エンドポイント](100_OAuth2_Authorization_Endpoint.html)<br>[OAuth2Token_エンドポイント認証](101_OAuth2_Token_Endpoint.html)<br>[ULUUT昇格設定](007_UUT_Elevation_Setting.html)<br>[パスワード変更](102_Password_Change.html)|<br>|<br>|<br>|<br>|
|**ODataスキーマ**|<br>|<br>|EDMX取得 ($metadata)<br>AtomPubサービスドキュメント取得<br>|<br>|<br>|
|**Role**|<br>|[登録](009_Create_Role.html)|[取得](011_Search_Role.html)<br>[一覧取得](010_Retrieve_Role.html)|[更新](012_Update_Role.html)|[削除](013_Delete_Role.html)|
|<br>|_$links|[登録](014_Create_Role_$links.html)|[一覧取得](015_List_Role_$links.html)|更新|[削除](017_Delete_Role_$links.html)|
|<br>|_NavProp経由|[登録](018_Register_Role_Using_NavProp.html)|[取得](019_List_Using_Role_NavProp.html)|<br>|<br>|
|**Account**|<br>|[登録](020_Create_Account.html)|[取得](022_Search_Account.html)<br>[一覧取得](021_Retrieve_Account.html)|[更新](023_Update_Account.html)|[削除](024_Delete_Account.html)|
|<br>|_$links|[登録](025_Register_Account_$links.html)|[一覧取得](026_Acquire_Account_$links_List.html)|更新|[削除](028_Delete_Account_$links.html)|
|<br>|_NavProp経由|[登録](029_Register_Account_Navigation_Property.html)|[取得](030_Acquire_Account_Navigation_Property.html)|<br>|<br>|
|**Box**|<br>|[作成](064_Create_Box.html)|[取得](066_Retrieve_Box.html)<br>[一覧取得](065_Search_Box.html)<br>[URL取得](107_Get_Box_URL.html)|[更新](067_Update_Box.html)|[削除](068_Delete_Box.html)|
|<br>|_$links|[登録](069_Register_Box_$links.html)|[一覧取得](070_List_Box_$links.html)|更新|[削除](072_Delete_Box_$links.html)|
|<br>|_NavProp経由|[登録](073_Register_Using_Box_NavProp.html)|[取得](074_List_Box_NavProp.html)|<br>|<br>|
|**ExtCell**|<br>|[登録](031_Create_External_Cell.html)|[取得](033_Get_External_Cell.html)<br>[一覧取得](032_List_External_Cell.html)|[更新](034_Update_External_Cell.html)|[削除](035_Delete_External_Cell.html)|
|<br>|_$links|[登録](036_Register_External_Cell_$links.html)|[一覧取得](037_List_External_Cell_$links.html)|更新|[削除](039_Delete_External_Cell_$links.html)|
|<br>|_NavProp経由|[登録](040_Register_External_Cell_Using_NavProp.html)|[取得](041_List_External_Cell_NavProp.html)|<br>|<br>|
|**Relation**|<br>|[登録](042_Create_Relation.html)|[取得](044_Retrieve_Relation.html)<br>[一覧取得](043_List_Relation.html)|[更新](045_Update_Relation.html)|[削除](046_Delete_Relation.html)|
|<br>|_$links|[登録](047_Register_Relation_$links.html)|[一覧取得](048_List_Relation_$links.html)|更新|[削除](050_Delete_Relation_$links.html)|
|<br>|_NavProp経由|[登録](051_Register_Using_Relation_NavProp.html)|[取得](052_List_Using_Relation_NavProp.html)|<br>|<br>|
|**ExtRole**|<br>|[登録](053_Create_External_Role.html)|[取得](055_Get_External_Role.html)<br>[一覧取得](054_List_External_Role.html)|[更新](056_Update_External_Role.html)|[削除](057_Delete_External_Role.html)|
|<br>|_$links|[登録](058_Register_External_Role_$links.html)|[一覧取得](059_Retrieve_External_Role_$links.html)|更新|[削除](061_Delete_External_Role_$links.html)|
|<br>|_NavProp経由|[登録](062_Register_Using_Role_NavProp.html)|[取得](063_List_External_Role_NavProp.html)|<br>|<br>|
|**認証**<br>(/\__auth, /\__authz)|[OAuth2_認可エンドポイント](100_OAuth2_Authorization_Endpoint.html)<br>[OAuth2Token_エンドポイント認証](101_OAuth2_Token_Endpoint.html)|<br>|<br>|<br>|<br>|
|**アクセス制御**|<br>|[制限設定](097_Cell_ACL.html)|[プロパティ取得](098_Cell_Get_Property.html)|[プロパティ変更](099_Cell_Change_Property.html)|<br>|
|[イベント](085_Event_Summary.html)|<br>|[イベント受付](086_Event_Reception.html)|[ログファイル取得](093_Retrieve_Log_File.html)<br>[ログファイル一覧取得](092_Retrieve_Log_File_list.html)<br>[ログファイル情報取得](091_Log_File_Information_Acquisition.html)|<br>|[ログファイル削除](094_Delete_Log_File.html)|
|<br>|イベントログ設定|<br>|取得|更新|<br>|
|<br>|<br>|パス接続<br>リスナー登録|リスナー取得|<br>|リスナー削除|
|**メッセージ制御**|[送信](079_Send_Message.html)|<br>|[取得](080_Retrieve_Sent_Message.html)<br>[一覧取得](081_List_Sent_Messages.html)|<br>|[削除](082_Delete_Sent_Message.html)|
|<br>|**受信**|<br>|[取得](077_Get_Received_Message.html)<br>[一覧取得](076_List_Received_Messages.html)|[状態変更](075_Received_Message_Approval.html)|[削除](078_Delete_an_Incoming_Message.html)|
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
