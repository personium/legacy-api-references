# Personium REST API Reference

Welcome to the Personium REST API Reference.  
The REST API Reference describes technical detailed specifications related to all of the REST APIs provided by Personium.

### Unit Level API

The Unit Level API is an API belonging to the unit that hosts a group of cells (for creating cells and managing a group of created cells).  
In principle, these APIs cannot be accessed using the access tokens issued from the cell.

Resource Path

```
https://{UnitFQDN}/
```

||Create/Register|Acquire|Update|Delete|
|:--|:--|:--|:--|:--|
|**Cell**|[Create](100_Create_Cell.html)|[Acquire List](101_List_Cell.html)<br>[Acquire](102_Get_Cell.html)|[Update](103_Update_Cell.html)|[Delete](104_Delete_Cell.html)<br>[Recursive Delete](105_Cell_Recursive_Delete.html)|


### Cell Level API

Cell Level API

*  Ability to authenticate people and applications accessing Cell
*  Function to set access control for Cell
*  Function to build relations between Cells
*  Function to create and manage Box
*  Ability to send and receive messages between Cells
*  Function of event processing
*  Function to export / import Cell

And so on. Many of these functions are implemented in the form of control objects that can be operated with the OData protocol, which is a standard for performing relational data manipulation based on REST.

Resource Path

```
https://{UnitFQDN}/{CellName}
```

##### Ability to authenticate people and applications accessing Cell

||Create/Register|Acquire|Update|Delete|Other|
|:--|:--|:--|:--|:--|:--|
|**Account**|[Register](212_Create_Account.html)|[Acquire List](214_Search_Account.html)<br>[Acquire](213_Retrieve_Account.html)|[Update](215_Update_Account.html)<br>[Change Password](294_Password_Change.html)|[Delete](216_Delete_Account.html)||
|_$links|[Register](217_Register_Account_links.html)|[Acquire List](218_Acquire_Account_links_List.html)|Update|[Delete](220_Delete_Account_links.html)||
|_via NavProp|[Register](221_Register_Account_Navigation_Property.html)|[Acquire](222_Acquire_Account_Navigation_Property.html)||||
|**Authentication**<br>(__token, __authz)|||||[OAuth2.0 Authorization Endpoint](292_OAuth2_Authorization_Endpoint.html)<br>[OAuth2.0 Token Endpoint](293_OAuth2_Token_Endpoint.html)|

##### Function to set access control for Cell

||Create/Register|Acquire|Update|Delete|
|:--|:--|:--|:--|:--|
|**Role**|[Register](201_Create_Role.html)|[Acquire List](202_Retrieve_Role.html)<br>[Acquire](203_Search_Role.html)|[Update](204_Update_Role.html)|[Delete](205_Delete_Role.html)|
|_$links|[Register](206_Create_Role_links.html)|[Acquire List](207_List_Role_links.html)|Update|[Delete](209_Delete_Role_links.html)|
|_via NavProp|[Register](210_Register_Role_Using_NavProp.html)|[Acquire](211_List_Using_Role_NavProp.html)|||
|**Access Control**|[Restrict](289_Cell_ACL.html)|[Acquire Properties](290_Cell_Get_Property.html)|[Change Properties](291_Cell_Change_Property.html)||

##### Function to build relationship between cells

||Create/Register|Acquire|Update|Delete|
|:--|:--|:--|:--|:--|
|**ExtCell**|[Register](223_Create_External_Cell.html)|[Acquire List](224_List_External_Cell.html)<br>[Acquire](225_Get_External_Cell.html)|[Update](226_Update_External_Cell.html)|[Delete](227_Delete_External_Cell.html)|
|_$links|[Register](228_Register_External_Cell_links.html)|[Acquire List](229_List_External_Cell_links.html)|Update|[Delete](231_Delete_External_Cell_links.html)|
|_via NavProp|[Register](232_Register_External_Cell_Using_NavProp.html)|[Acquire List](233_List_External_Cell_NavProp.html)|||
|**Relation**|[Register](234_Create_Relation.html)|[Acquire List](235_List_Relation.html)<br>[Acquire](236_Retrieve_Relation.html)|[Update](237_Update_Relation.html)|[Delete](238_Delete_Relation.html)|
|_$links|[Register](239_Register_Relation_links.html)|[Acquire List](240_List_Relation_links.html)|Update|[Delete](242_Delete_Relation_links.html)|
|_via NavProp|[Register](243_Register_Using_Relation_NavProp.html)|[Acquire](244_List_Using_Relation_NavProp.html)|||
|**ExtRole**|[Register](245_Create_External_Role.html)|[Acquire](247_Get_External_Role.html)<br>[Acquire List](246_List_External_Role.html)|[Update](248_Update_External_Role.html)|[Delete](249_Delete_External_Role.html)|
|_$links|[Register](250_Register_External_Role_links.html)|[Acquire List](251_Retrieve_External_Role_links.html)|Update|[Delete](253_Delete_External_Role_links.html)|
|_via NavProp|[Register](254_Register_Using_Role_NavProp.html)|[Acquire](255_List_External_Role_NavProp.html)|||

##### Function to create and manage Box

||Create/Register|Acquire|Update|Delete|
|:--|:--|:--|:--|:--|
|**Box**|[Create](256_Create_Box.html)|[Acquire](258_Retrieve_Box.html)<br>[Acquire List](257_Search_Box.html)<br>[Acquire URL](304_Get_Box_URL.html)|[Update](259_Update_Box.html)|[Delete](260_Delete_Box.html)|
|_$links|[Register](261_Register_Box_links.html)|[Acquire List](262_List_Box_links.html)|Update|[Delete](264_Delete_Box_links.html)|
|_via NavProp|[Register](265_Register_Using_Box_NavProp.html)|[Acquire](266_List_Box_NavProp.html)|||
|Install|[Install Box](302_Box_Installation.html)|[Acquire Box Meta Data](303_Progress_of_Bar_File_Installation.html)|||

##### Ability to send and receive messages between cells

||Create/Register|Acquire|Update|Delete|
|:--|:--|:--|:--|:--|
|**Message Control**<br>[Transmit](271_Send_Message.html)||[Acquire](272_Retrieve_Sent_Message.html)<br>[Acquire List](273_List_Sent_Messages.html)||[Delete](274_Delete_Sent_Message.html)|
|**Message Control**<br>Receive||[Acquire](269_Get_Received_Message.html)<br>[Acquire List](268_List_Received_Messages.html)|[Change Status](267_Received_Message_Approval.html)|[Delete](270_Delete_an_Incoming_Message.html)|

##### Event processing function

||Create/Register|Acquire|Update|Delete|
|:--|:--|:--|:--|:--|
|[Events](277_Event_Summary.html)|[Accept Event](278_Event_Reception.html)|[Log File Acquire](285_Retrieve_Log_File.html)<br>[Log File Acquire List](284_Retrieve_Log_File_list.html)<br>[Log File Acquire Information](283_Log_File_Information_Acquisition.html)||[Delete Log File](286_Delete_Log_File.html)|

##### Ability to export/import Cell

Snapshot file of Cell is created by export execution.  
Import imports the contents of the snapshot file into Cell.  
Snapshot file can be operated with WebDAV interface.  

||Create/Register|Acquire|Update|Delete|
|:--|:--|:--|:--|:--|
|**Export**|[Execute](501_Export_Cell.html)|[Get progress](502_Progress_of_Export_Cell.html)|||
|**Import**|[Execute](507_Import_Cell.html)|[Get progress](508_Progress_of_Import_Cell.html)|||
|**Snapshot**|[Register/Update](503_Register_and_Update_Snapshot_Cell.html)|[Acquire](504_Get_Snapshot_Cell.html)<br>[Acquire Settings](505_Get_Property_Snapshot_Cell.html)||[Delete](506_Delete_Snapshot_Cell.html)|


### Box Level API

The Box Level API is an API for applications and others to manipulate data, and is a group of APIs based on WebDAV as a file system idea.  
Like ordinary file systems, it is possible to arrange / acquire files, create / manage folders (collection), get list of files and folders, set / refer to access control, etc.

Also, because it supports the following special collections, it can handle not only file-like data but also various forms of data.  
These special collections can be created in any path on the WebDAV space provided by Box.

|Special collection|Use|Notes|
|:--|:--|:--|
|OData Service Collection|Relational data||
|Engine Service Collection|Run customized logic||
|CALDAV Collection|Calendar data|Unimplemented|
|Link Collection|Aliases to specific areas of other cells or other Box|Unimplemented|

Resource Path (* with certain exceptions)

```
https://{UnitFQDN}/{CellName}/{BoxName}
https://{UnitFQDN}/{CellName}/{BoxName}/{ResourcePath}
```

##### Box Management

|Create/Register|Acquire|
|:--|:--|
|[Install Box](302_Box_Installation.html)|[Acquire Box Meta Data](303_Progress_of_Bar_File_Installation.html)|

##### WebDAV

||Create/Register|Acquire|Update|Delete|Other|
|:--|:--|:--|:--|:--|:--|
|Collection|[Create](306_Create_Collection.html)|[Acquire Settings](305_Get_Property.html)|[Change Settings](308_Change_Property.html)<br>[Move/Rename](309_Update_Move_Collection.html)|[Delete](310_Delete_Collection.html)||
|File|[Register/Update](312_Register_and_Update_WebDAV.html)|[Acquire](311_Get_WebDav.html)<br>[Acquire Settings](307_Get_Property.html)|[Change Settings](313_Change_Property.html)|[Delete](314_Delete_WebDAV.html)||
|Access Control|||||[Configuring the Settings](315_Configure_Access_Control.html)|

\* ACL setting (access control setting) is possible for all files and collections (including special collections).  
\* ACL setting can be acquired with the PROPFIND method.

##### OData

||Create/Register|Acquire|Update|Delete|Other|
|:--|:--|:--|:--|:--|:--|
|**EntityType**|[Register](345_Create_EntityType.html)|[Acquire](347_Get_EntityType.html)<br>[Acquire List](346_List_EntityType.html)|[Update](348_Update_EntityType.html)|[Delete](349_Delete_EntityType.html)||
|_$links|Register|Acquire List|Update|Delete||
|_via NavProp||Acquire List||||
|**AssociationEnd**|[Register](318_Register_AssociationEnd.html)|[Acquire](320_Get_AssociationEnd.html)<br>[Acquire List](319_List_AssociationEnd.html)|[Update](321_Update_AssociationEnd.html)|[Delete](322_Delete_AssociationEnd.html)||
|_$links|[Register](323_Register_AssociationEnd_links.html)|[Acquire List](324_List_AssociationEnd_links.html)||[Delete](325_Delete_AssociationEnd_links.html)||
|_via NavProp||Acquire List||||
|**ComplexType**|[Register](327_Register_ComplexType.html)|[Acquire](329_Get_ComplexType.html)<br>[Acquire List](328_List_ComplexType.html)|Update|[Delete](331_Delete_ComplexType.html)||
|_$links|Register|Acquire List|Update|Delete||
|**Property**|[Register](355_Register_Property.html)|[Acquire](357_Get_Property.html)<br>[Acquire List](356_List_Property.html)|Update|[Delete](359_Delete_Property.html)||
|_$links|Register|Acquire List|Update|Delete||
|**ComplexTypeProperty**|[Register](336_Register_ComplexTypeProperty.html)|[Acquire](338_Get_ComplexTypeProperty.html)<br>[Acquire List](337_List_ComplexTypeProperty.html)|[Update](339_Update_ComplexTypeProperty.html)|[Delete](340_Delete_ComplexTypeProperty.html)||
|_$links|Register|Acquire List|Update|Delete||
|**Entity**|[Create](364_Create_Entity.html)|[Acquire](366_Get_Entity.html)<br>[Acquire List](365_List_Entity.html)|[Update](367_Update_Entity.html)<br>[Partial Update](369_Partial_Update_Entity.html)|[Delete](370_Delete_Entity.html)|[Batch Operation](368_Entity_Bulk_Operations.html)|
|_$links|[Register](373_Register_User_Data_links.html)|[Acquire List](374_User_Data_List_links.html)|Update|[Delete](376_Delete_User_Data_links.html)||
|_via NavProp|[Register](377_Register_using_NavProp.html)|[Acquire List](378_List_using_NavProp.html)||||

##### Server Script (Engine Service Collection)

You can register Personium application and server side logic created by Cell user and run it.  
First, register the user logic as a file, set the service collection and associate with the path, so that  
You can run the user logic for requests from any path under the collection.

||Create/Register|Acquire|Update|Delete|Other|
|:--|:--|:--|:--|:--|:--|
|Service Collection Source|[Create](381_Create_Service_Collection_Source.html)|[Acquire](382_List_Service_Collection_Source.html)|[Apply Settings](380_Configure_Service_Collection.html)|[Delete](383_Delete_Service_Collection_Source.html)|[Service Execute](384_Service_Execution.html)|

##### Service Document Acquire/Schema Acquire

||Acquire|
|:--|:--|
|Service Document|[Acquire](317_Document_Acquisition_Service.html)|
|Schema|[Acquire](316_User_Defined_Data_Schema.html)|

##### OData Acquisition Common Queries

|Query|Single Acquisition|List Acquisition|
|:--|:--:|:--:|
|[$format Query](404_Format_Query.html)|Yes|Yes|
|[$expand Query](405_Expand_Query.html)|Yes|Yes|
|[$select Query](406_Select_Query.html)|Yes|Yes|
|[$orderby Query](400_Orderby_Query.html)|Yes|Yes|
|[$top Query](401_Top_Query.html)|Yes|Yes|
|[$skip Query](402_Skip_Query.html)|Yes|Yes|
|[$filter Query](403_Filter_Query.html)|Yes|Yes|
|[$inlinecount](407_Inlinecount_Query.html)|Yes|Yes|
|[Full-text Search (q) Query](408_Full_Text_Search_Query.html)|Yes|Yes|


### Common

#### [Error Messages](004_Error_Messages.html)

#### [Restrictions on Personium HTTP Implementation](003_Common_Limitations_on_HTTP_Implementation.html)

#### [CORS Support](002_CORS_Support.html)

#### [Cross Domain Policy File Acquire](001_Cross_Domain_Policy_File.html)


###### Copyright 2017 FUJITSU LIMITED
