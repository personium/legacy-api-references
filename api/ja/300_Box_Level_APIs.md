# BoxレベルAPI概要
Box Level API は、アプリケーション等がデータを操作するためのAPIで、WebDAVをベースとしたファイルシステム的な考え方のAPI群です。  
通常のファイルシステムと同様にファイルの配置・取得、フォルダ（collection）の作成・管理、ファイルやフォルダの一覧取得、アクセス制御の設定・参照等が可能です。

また以下の特殊コレクションをサポートしているため、ファイル状のデータのみではなく、様々な形態のデータを扱うことができます。  
これら特殊コレクションはBoxが提供するWebDAV空間上のいかなるパスに作成することもできます。

|特殊コレクション<br>|用途<br>|備考<br>|
|:--|:--|:--|
|OData Service Collection<br>|リレーショナルデータ<br>|<br>|
|Engine Service Collection<br>|カスタマイズロジックの走行<br>|<br>|
|CALDAV Collection<br>|カレンダーデータ<br>|未実装<br>|
|Link Collection<br>|他のCellや他Boxの特定の領域へのエイリアス<br>|未実装<br>|

##### WebDAV
|<br>|<br>|作成・登録<br>|取得<br>|更新<br>|削除<br>|
|:--|:--|:--|:--|:--|:--|
|コレクション|<br>|[作成](109_Create_Collection.html)|[設定取得](110_Get_Property.html)|[設定変更](109_Create_Collection.html)<br>[移動名称変更](501_Update_Move_Collection.html)|[削除](112_Delete_Collection.html)|
|ファイル|<br>|[登録更新](114_Register_and_Update_WebDAV.html)|[取得](113_Get_WebDav.html)<br>[設定取得](110_Get_Property.html)|[設定変更](111_Change_Property.html)|[削除](116_Delete_WebDAV.html)|
|アクセス制御|[設定](117_Configure_Access_Control.html)|<br>|<br>|<br>|<br>|

※ すべてのファイルやコレクション（特殊コレクションを含む）に対してACL設定(アクセス制御設定)が可能です。  
※ ACL設定 は PROPFINDメソッドで取得できます。

<br>
#### 特別なコレクション: OData Serviceコレクション
##### OData
|<br>|<br>|作成・登録<br>|取得<br>|更新<br>|削除<br>|
|:--|:--|:--|:--|:--|:--|
|**EntityType**|<br>|[登録](147_Create_EntityType.html)|[取得](149_Get_EntityType.html)<br>[一覧取得](148_List_EntityType.html)|[更新](150_Update_EntityType.html)|[削除](151_Delete_EntityType.html)|
|<br>|_$links|[登録](152_Register_EntityType_$links.html)|[一覧取得](153_List_EntityType_$links.html)|[更新](154_Update_EntityType_$links.html)|[削除](155_Delete_EntityType_$links.html)|
|<br>|_NavProp経由|<br>|[一覧取得](155_List_EntityType_Links_Navigation_Property.html)|<br>|<br>|
|**AssociationEnd**|<br>|[登録](120_Register_AssociationEnd.html)|[取得](122_Get_AssociationEnd.html)<br>[一覧取得](121_List_AssociationEnd.html)|[更新](123_Update_AssociationEnd.html)|[削除](124_Delete_AssociationEnd.html)|
|<br>|_$links|[登録](125_Register_AssociationEnd_$links.html)|[一覧取得](126_List_AssociationEnd_$links.html)|<br>|[削除](127_Delete_AssociationEnd_$links.html)|
|<br>|_NavProp経由|<br>|[一覧取得](128_List_AssociationEnd_Navigation_Property.html)|<br>|<br>|
|**ComplexType**|<br>|[登録](129_Register_ComplexType.html)|[取得](131_Get_ComplexType.html)<br>[一覧取得](130_List_ComplexType.html)|[更新](132_Update ComplexType.html)|[削除](133_Delete_ComplexType.html)|
|<br>|_$links|[登録](134_Register_ComplexType_$links.html)|[一覧取得](135_List_ComplexType_$links.html)|更新|[削除](136_Delete_ComplexType_$links.html)|
|**Property**|<br>|[登録](157_Register_Property.html)|[取得](159_Get_Property.html)<br>[一覧取得](158_List_Property.html)|[更新](160_Update_Property.html)|[削除](161_Delete_Property.html)|
|<br>|_$links|[登録](162_Register_Property_$links.html)|[一覧取得](163_List_Property_$links.html)|[更新](164_Update_List_Property_$links.html)|[削除](165_Delete_Property_$links.html)|
|**ComplexTypeProperty**|<br>|[登録](137_Register_ComplexTypeProperty.html)|[取得](139_Get_ComplexTypeProperty.html)<br>[一覧取得](138_List_ComplexTypeProperty.html)|[更新](140_Update_ComplexTypeProperty.html)|[削除](141_Delete_ComplexTypeProperty.html)|
|<br>|_$links|[登録](142_Register ComplextTypeProperty $ Links.html)|[一覧取得](144_List_ComplextTypeProperty_$links.html)|[更新](145_Update ComplextTypeProperty $ Links.html)|[削除](146_Delete_ComplextTypeProperty_$links.html)|
|**Entity**|[一括操作](170_Entity_Bulk_Operations.html)|[作成](166_Create_Entity.html)|[取得](168_Get_Entity.html)<br>[一覧取得](167_List_Entity.html)|[更新](168_Get_Entity.html)<br>[部分更新](171_Partial_Update_Entity.html)|[削除](172_Delete_Entity.html)|
|（ユーザデータ）|_$links|[登録](175_Register_User_Data_$links.html)|[一覧取得](177_User_Data_List_$links.html)|[更新](178_Update_User_Data_$links.html)|[削除](179_Delete_User_Data_$links.html)|
|（ユーザデータ）|_NavProp経由|[登録](180_Register_using_NavProp.html)|[一覧取得](181_List_using_NavProp.html)|<br>|<br>|

##### データ操作
* [スキーマ取得](172_Get_Schema.html)
* サービスドキュメント取得

<br>
#### 特別なコレクション: Engine Service コレクション
##### [サーバスクリプト(Engine Service Collection)](182_Engine_Service_Collection_APIs.html)
personium.ioアプリケーションやCell利用者が作成したサーバサイドロジックを登録しこれを走行させることができます。  
はじめに、ユーザロジックをファイルとして登録し、サービスコレクションの設定を行ってパスとの関連付けを行うことで、  
コレクション配下の任意のパスからのリクエストに対してユーザーロジックを走行させることができます。

|<br>|<br>|作成・登録<br>|取得<br>|更新<br>|削除<br>|
|:--|:--|:--|:--|:--|:--|
|サービスコレクションソース|<br>|[作成](184_Create_Service_Collection_Source.html)|[取得](185_List_Service_Collection_Source.html)|[設定適用](183_Configure_Service_Collection.html)|[削除](186_Delete_Service_Collection_Source.html)|
|<br>|[サービス実行](187_Service_Execution.html)<br>|<br>|<br>|<br>|<br>|
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
