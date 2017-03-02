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
|コレクション|<br>|[作成](306_Create_Collection.html)|[設定取得](307_Get_Property.html)|[設定変更](308_Change_Property.html)<br>[移動名称変更](309_Update_Move_Collection.html)|[削除](310_Delete_Collection.html)|
|ファイル|<br>|[登録更新](312_Register_and_Update_WebDAV.html)|[取得](311_Get_WebDav.html)<br>[設定取得](307_Get_Property.html)|[設定変更](313_Change_Property.html)|[削除](314_Delete_WebDAV.html)|
|アクセス制御|[設定](315_Configure_Access_Control.html)|<br>|<br>|<br>|<br>|

※ すべてのファイルやコレクション（特殊コレクションを含む）に対してACL設定(アクセス制御設定)が可能です。  
※ ACL設定 は PROPFINDメソッドで取得できます。

<br>
#### 特別なコレクション: OData Serviceコレクション
##### OData
|<br>|<br>|作成・登録<br>|取得<br>|更新<br>|削除<br>|
|:--|:--|:--|:--|:--|:--|
|**EntityType**|<br>|[登録](345_Create_EntityType.html)|[取得](347_Get_EntityType.html)<br>[一覧取得](346_List_EntityType.html)|[更新](348_Update_EntityType.html)|[削除](349_Delete_EntityType.html)|
|<br>|_$links|登録|一覧取得|更新|削除|
|<br>|_NavProp経由|<br>|一覧取得|<br>|<br>|
|**AssociationEnd**|<br>|[登録](318_Register_AssociationEnd.html)|[取得](320_Get_AssociationEnd.html)<br>[一覧取得](319_List_AssociationEnd.html)|[更新](321_Update_AssociationEnd.html)|[削除](322_Delete_AssociationEnd.html)|
|<br>|_$links|[登録](323_Register_AssociationEnd_links.html)|[一覧取得](324_List_AssociationEnd_links.html)|<br>|[削除](325_Delete_AssociationEnd_links.html)|
|<br>|_NavProp経由|<br>|一覧取得|<br>|<br>|
|**ComplexType**|<br>|[登録](327_Register_ComplexType.html)|[取得](329_Get_ComplexType.html)<br>[一覧取得](328_List_ComplexType.html)|更新|[削除](331_Delete_ComplexType.html)|
|<br>|_$links|登録|一覧取得|更新|削除|
|**Property**|<br>|[登録](355_Register_Property.html)|[取得](357_Get_Property.html)<br>[一覧取得](356_List_Property.html)|更新|[削除](359_Delete_Property.html)|
|<br>|_$links|登録|一覧取得|更新|削除|
|**ComplexTypeProperty**|<br>|[登録](336_Register_ComplexTypeProperty.html)|[取得](338_Get_ComplexTypeProperty.html)<br>[一覧取得](337_List_ComplexTypeProperty.html)|更新|[削除](340_Delete_ComplexTypeProperty.html)|
|<br>|_$links|登録|一覧取得|更新|削除|
|**Entity**|[一括操作](368_Entity_Bulk_Operations.html)|[作成](364_Create_Entity.html)|[取得](366_Get_Entity.html)<br>[一覧取得](365_List_Entity.html)|[更新](367_Update_Entity.html)<br>[部分更新](369_Partial_Update_Entity.html)|[削除](370_Delete_Entity.html)|
|（ユーザデータ）|_$links|[登録](373_Register_User_Data_links.html)|[一覧取得](374_User_Data_List_links.html)|更新|[削除](376_Delete_User_Data_links.html)|
|（ユーザデータ）|_NavProp経由|[登録](377_Register_using_NavProp.html)|[一覧取得](378_List_using_NavProp.html)|<br>|<br>|

##### データ操作
* [スキーマ取得](316_User_Defined_Data_Schema.html)
* [サービスドキュメント取得](317_Document_Acquisition_Service.html)

<br>
#### 特別なコレクション: Engine Service コレクション
##### [サーバスクリプト(Engine Service Collection)](379_Engine_Service_Collection_APIs.html)
PersoniumアプリケーションやCell利用者が作成したサーバサイドロジックを登録しこれを走行させることができます。  
はじめに、ユーザロジックをファイルとして登録し、サービスコレクションの設定を行ってパスとの関連付けを行うことで、  
コレクション配下の任意のパスからのリクエストに対してユーザーロジックを走行させることができます。

|<br>|<br>|作成・登録<br>|取得<br>|更新<br>|削除<br>|
|:--|:--|:--|:--|:--|:--|
|サービスコレクションソース|<br>|[作成](381_Create_Service_Collection_Source.html)|[取得](382_List_Service_Collection_Source.html)|[設定適用](380_Configure_Service_Collection.html)|[削除](383_Delete_Service_Collection_Source.html)|
|<br>|[サービス実行](384_Service_Execution.html)<br>|<br>|<br>|<br>|<br>|
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
