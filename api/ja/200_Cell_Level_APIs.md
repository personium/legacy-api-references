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
|**認証**|[OAuth2_認可エンドポイント](292_OAuth2_Authorization_Endpoint.html)<br>[OAuth2Token_エンドポイント認証](293_OAuth2_Token_Endpoint.html)<br>ULUUT昇格設定<br>[パスワード変更](294_Password_Change.html)|<br>|<br>|<br>|<br>|
|**Role**|<br>|[登録](201_Create_Role.html)|[取得](203_Search_Role.html)<br>[一覧取得](202_Retrieve_Role.html)|[更新](204_Update_Role.html)|[削除](205_Delete_Role.html)|
|<br>|_$links|[登録](206_Create_Role_links.html)|[一覧取得](207_List_Role_links.html)|更新|[削除](209_Delete_Role_links.html)|
|<br>|_NavProp経由|[登録](210_Register_Role_Using_NavProp.html)|[取得](211_List_Using_Role_NavProp.html)|<br>|<br>|
|**Account**|<br>|[登録](212_Create_Account.html)|[取得](213_Retrieve_Account.html)<br>[一覧取得](214_Search_Account.html)|[更新](215_Update_Account.html)|[削除](216_Delete_Account.html)|
|<br>|_$links|[登録](217_Register_Account_links.html)|[一覧取得](218_Acquire_Account_$links_List.html)|更新|[削除](220_Delete_Account_links.html)|
|<br>|_NavProp経由|[登録](221_Register_Account_Navigation_Property.html)|[取得](222_Acquire_Account_Navigation_Property.html)|<br>|<br>|
|**Box**|<br>|[作成](256_Create_Box.html)|[取得](258_Retrieve_Box.html)<br>[一覧取得](257_Search_Box.html)<br>[URL取得](304_Get_Box_URL.html)|[更新](259_Update_Box.html)|[削除](260_Delete_Box.html)|
|<br>|_$links|[登録](261_Register_Box_links.html)|[一覧取得](262_List_Box_links.html)|更新|[削除](264_Delete_Box_links.html)|
|<br>|_NavProp経由|[登録](265_Register_Using_Box_NavProp.html)|[取得](266_List_Box_NavProp.html)|<br>|<br>|
|**ExtCell**|<br>|[登録](223_Create_External_Cell.html)|[取得](225_Get_External_Cell.html)<br>[一覧取得](224_List_External_Cell.html)|[更新](226_Update_External_Cell.html)|[削除](227_Delete_External_Cell.html)|
|<br>|_$links|[登録](228_Register_External_Cell_links.html)|[一覧取得](229_List_External_Cell_links.html)|更新|[削除](231_Delete_External_Cell_links.html)|
|<br>|_NavProp経由|[登録](232_Register_External_Cell_Using_NavProp.html)|[一覧取得](233_List_External_Cell_NavProp.html)|<br>|<br>|
|**Relation**|<br>|[登録](234_Create_Relation.html)|[取得](236_Retrieve_Relation.html)<br>[一覧取得](235_List_Relation.html)|[更新](237_Update_Relation.html)|[削除](238_Delete_Relation.html)|
|<br>|_$links|[登録](239_Register_Relation_links.html)|[一覧取得](240_List_Relation_links.html)|更新|[削除](242_Delete_Relation_links.html)|
|<br>|_NavProp経由|[登録](243_Register_Using_Relation_NavProp.html)|[取得](244_List_Using_Relation_NavProp.html)|<br>|<br>|
|**ExtRole**|<br>|[登録](245_Create_External_Role.html)|[取得](247_Get_External_Role.html)<br>[一覧取得](246_List_External_Role.html)|[更新](248_Update_External_Role.html)|[削除](249_Delete_External_Role.html)|
|<br>|_$links|[登録](250_Register_External_Role_links.html)|[一覧取得](251_Retrieve_External_Role_links.html)|更新|[削除](253_Delete_External_Role_links.html)|
|<br>|_NavProp経由|[登録](254_Register_Using_Role_NavProp.html)|[取得](255_List_External_Role_NavProp.html)|<br>|<br>|
|**認証**<br>(/\__token, /\__authz)|[OAuth2_認可エンドポイント](292_OAuth2_Authorization_Endpoint.html)<br>[OAuth2Token_エンドポイント認証](293_OAuth2_Token_Endpoint.html)|<br>|<br>|<br>|<br>|
|**アクセス制御**|<br>|[制限設定](289_Cell_ACL.html)|[プロパティ取得](290_Cell_Get_Property.html)|[プロパティ変更](291_Cell_Change_Property.html)|<br>|
|[イベント](277_Event_Summary.html)|<br>|[イベント受付](278_Event_Reception.html)|[ログファイル取得](285_Retrieve_Log_File.html)<br>[ログファイル一覧取得](284_Retrieve_Log_File_list.html)<br>[ログファイル情報取得](283_Log_File_Information_Acquisition.html)|<br>|[ログファイル削除](286_Delete_Log_File.html)|
|<br>|イベントログ設定|<br>|取得|更新|<br>|
|<br>|<br>|パス接続<br>リスナー登録|リスナー取得|<br>|リスナー削除|
|**メッセージ制御**|[送信](271_Send_Message.html)|<br>|[取得](272_Retrieve_Sent_Message.html)<br>[一覧取得](273_List_Sent_Messages.html)|<br>|[削除](274_Delete_Sent_Message.html)|
|<br>|**受信**|<br>|[取得](269_Get_Received_Message.html)<br>[一覧取得](268_List_Received_Messages.html)|[状態変更](267_Received_Message_Approval.html)|[削除](270_Delete_an_Incoming_Message.html)|
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
