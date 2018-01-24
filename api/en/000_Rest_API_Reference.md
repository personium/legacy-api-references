# Personium REST API Reference

Welcome to the Personium REST API Reference.  
The REST API Reference describes technical detailed specifications related to all of the REST APIs provided by Personium.

## Unit Level API
The Unit Level API's belong to the unit that hosts a group of cells (for creating cells and managing a group of created cells).  
In principle, these APIs cannot be accessed using the access tokens issued by normal cells.

Unit Root URL
```
https://{UnitFQDN}/
```

||Create/Register|Acquire|Update|Delete|
|:--|:--|:--|:--|:--|
|**Cell**|[Create](100_Create_Cell.md)|[Acquire List](101_List_Cell.md)<br>[Acquire](102_Get_Cell.md)|[Update](103_Update_Cell.md)|[Delete](104_Delete_Cell.md)<br>[Recursive Delete](105_Cell_Recursive_Delete.md)|


## Cell Level API

Cell Level API

*  Ability to authenticate people and applications accessing Cell
*  Function to set access control for Cell
*  Function to build relations between Cells
*  Function to create and manage Box
*  Ability to send and receive messages between Cells
*  Function of event processing
*  Function to export / import Cell

And so on. Many of these functions are implemented in the form of control objects that can be operated with the OData protocol, which is a standard for performing relational data manipulation based on REST.

Cell Root URL
```
https://{UnitFQDN}/{CellName}/
```

### Authenticating user and applications accessing the Cell

||Create/Register|Acquire|Update|Delete|Other|
|:--|:--|:--|:--|:--|:--|
|**Account**|[Register](212_Create_Account.md)|[Acquire List](214_Search_Account.md)<br>[Acquire](213_Retrieve_Account.md)|[Update](215_Update_Account.md)<br>[Change Password](294_Password_Change.md)|[Delete](216_Delete_Account.md)||
|_$links|[Register](217_Register_Account_links.md)|[Acquire List](218_Acquire_Account_links_List.md)|Update|[Delete](220_Delete_Account_links.md)||
|_via NavProp|[Register](221_Register_Account_Navigation_Property.md)|[Acquire](222_Acquire_Account_Navigation_Property.md)||||
|**Authentication**<br>(__token, __authz)|||||[OAuth2.0 Authorization Endpoint](292_OAuth2_Authorization_Endpoint.md)<br>[OAuth2.0 Token Endpoint](293_OAuth2_Token_Endpoint.md)|

### Functions to set access control for the Cell

||Create/Register|Acquire|Update|Delete|
|:--|:--|:--|:--|:--|
|**Role**|[Register](201_Create_Role.md)|[Acquire List](202_Retrieve_Role.md)<br>[Acquire](203_Search_Role.md)|[Update](204_Update_Role.md)|[Delete](205_Delete_Role.md)|
|_$links|[Register](206_Create_Role_links.md)|[Acquire List](207_List_Role_links.md)|Update|[Delete](209_Delete_Role_links.md)|
|_via NavProp|[Register](210_Register_Role_Using_NavProp.md)|[Acquire](211_List_Using_Role_NavProp.md)|||
|**Access Control**|[Restrict](289_Cell_ACL.md)|[Acquire Properties](290_Cell_Get_Property.md)|[Change Properties](291_Cell_Change_Property.md)||

### Functions to build relationship between cells

||Create/Register|Acquire|Update|Delete|
|:--|:--|:--|:--|:--|
|**ExtCell**|[Register](223_Create_External_Cell.md)|[Acquire List](224_List_External_Cell.md)<br>[Acquire](225_Get_External_Cell.md)|[Update](226_Update_External_Cell.md)|[Delete](227_Delete_External_Cell.md)|
|_$links|[Register](228_Register_External_Cell_links.md)|[Acquire List](229_List_External_Cell_links.md)|Update|[Delete](231_Delete_External_Cell_links.md)|
|_via NavProp|[Register](232_Register_External_Cell_Using_NavProp.md)|[Acquire List](233_List_External_Cell_NavProp.md)|||
|**Relation**|[Register](234_Create_Relation.md)|[Acquire List](235_List_Relation.md)<br>[Acquire](236_Retrieve_Relation.md)|[Update](237_Update_Relation.md)|[Delete](238_Delete_Relation.md)|
|_$links|[Register](239_Register_Relation_links.md)|[Acquire List](240_List_Relation_links.md)|Update|[Delete](242_Delete_Relation_links.md)|
|_via NavProp|[Register](243_Register_Using_Relation_NavProp.md)|[Acquire](244_List_Using_Relation_NavProp.md)|||
|**ExtRole**|[Register](245_Create_External_Role.md)|[Acquire](247_Get_External_Role.md)<br>[Acquire List](246_List_External_Role.md)|[Update](248_Update_External_Role.md)|[Delete](249_Delete_External_Role.md)|
|_$links|[Register](250_Register_External_Role_links.md)|[Acquire List](251_Retrieve_External_Role_links.md)|Update|[Delete](253_Delete_External_Role_links.md)|
|_via NavProp|[Register](254_Register_Using_Role_NavProp.md)|[Acquire](255_List_External_Role_NavProp.md)|||

### Box creation and management inside the Cell

[Install Box](302_Box_Installation.md)
[Acquire Box Meta Data](303_Progress_of_Bar_File_Installation.md)


||Create/Register|Acquire|Update|Delete|
|:--|:--|:--|:--|:--|
|**Box**|[Create](256_Create_Box.md)|[Acquire](258_Retrieve_Box.md)<br>[Acquire List](257_Search_Box.md)<br>[Acquire URL](304_Get_Box_URL.md)|[Update](259_Update_Box.md)|[Delete](260_Delete_Box.md)<br>[Recursive Delete](295_Box_Recursive_Delete.md)|
|_$links|[Register](261_Register_Box_links.md)|[Acquire List](262_List_Box_links.md)|Update|[Delete](264_Delete_Box_links.md)|
|_via NavProp|[Register](265_Register_Using_Box_NavProp.md)|[Acquire](266_List_Box_NavProp.md)|||

### Sending and receiving messages between Cells

||Create/Register|Acquire|Update|Delete|
|:--|:--|:--|:--|:--|
|**Message Control**<br>[Transmit](271_Send_Message.md)||[Acquire](272_Retrieve_Sent_Message.md)<br>[Acquire List](273_List_Sent_Messages.md)||[Delete](274_Delete_Sent_Message.md)|
|**Message Control**<br>Receive||[Acquire](269_Get_Received_Message.md)<br>[Acquire List](268_List_Received_Messages.md)|[Change Status](267_Received_Message_Approval.md)|[Delete](270_Delete_an_Incoming_Message.md)|

### Event processing functions

* [Events Overview](277_Event_Summary.md)
* [Accept External Events](278_Event_Reception.md)

#### Event Processing Rule (Cell Control Object)

|Rule|Operations|
|:--|:--|
|Basic Operations|[Create](2A0_Create_Rule.md) &nbsp; &nbsp; [List](2A2_Search_Rule.md) &nbsp; &nbsp; [Retrieve](2A1_Retrieve_Rule.md) &nbsp; &nbsp; [Update](2A3_Update_Rule.md) &nbsp; &nbsp; [Delete](2A4_Delete_Rule.md) |
|&nbsp; &nbsp;Linking with other objects|[Link](2A5_Create_Rule_links.md) &nbsp; &nbsp; [Unlink](2A6_List_Rule_links.md) &nbsp; &nbsp; [List of links](2A6_List_Rule_links.md) &nbsp; &nbsp; |
|&nbsp; &nbsp;via Navigation Property|[Create](2A8_Register_Rule_Using_NavProp.md) &nbsp; &nbsp; [List](2A9_List_Using_Rule_NavProp.md)|


#### Event Log 

* [Retrieve Log File](285_Retrieve_Log_File.md)
* [List of Log Files](284_Retrieve_Log_File_list.md)
* [Meatadata of Log File](283_Log_File_Information_Acquisition.md)
* [Delete Log File](286_Delete_Log_File.md)

### Exporting / Importing the contents inside the Cell

Snapshot file of Cell is created by export execution.  
Import imports the contents of the snapshot file into Cell.  
Snapshot file can be operated with WebDAV interface.  

||Create/Register|Acquire|Update|Delete|
|:--|:--|:--|:--|:--|
|**Export**|[Execute](501_Export_Cell.md)|[Get progress](502_Progress_of_Export_Cell.md)|||
|**Import**|[Execute](507_Import_Cell.md)|[Get progress](508_Progress_of_Import_Cell.md)|||
|**Snapshot**|[Register/Update](503_Register_and_Update_Snapshot_Cell.md)|[Acquire](504_Get_Snapshot_Cell.md)<br>[Acquire Settings](505_Get_Property_Snapshot_Cell.md)||[Delete](506_Delete_Snapshot_Cell.md)|


## Box Level API

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
https://{UnitFQDN}/{CellName}/{BoxName}/
https://{UnitFQDN}/{CellName}/{BoxName}/{ResourcePath}
```

### Box Management


### WebDAV

||Create/Register|Acquire|Update|Delete|Other|
|:--|:--|:--|:--|:--|:--|
|Collection|[Create](306_Create_Collection.md)|[Acquire Settings](305_Get_Property.md)|[Change Settings](308_Change_Property.md)<br>[Move/Rename](309_Update_Move_Collection.md)|[Delete](310_Delete_Collection.md)||
|File|[Register/Update](312_Register_and_Update_WebDAV.md)|[Acquire](311_Get_WebDav.md)<br>[Acquire Settings](307_Get_Property.md)|[Change Settings](313_Change_Property.md)|[Delete](314_Delete_WebDAV.md)||
|Access Control|||||[Configuring the Settings](315_Configure_Access_Control.md)|

\* ACL setting (access control setting) is possible for all files and collections (including special collections).  
\* ACL setting can be acquired with the PROPFIND method.

##### OData

||Create/Register|Acquire|Update|Delete|Other|
|:--|:--|:--|:--|:--|:--|
|**EntityType**|[Register](345_Create_EntityType.md)|[Acquire](347_Get_EntityType.md)<br>[Acquire List](346_List_EntityType.md)|[Update](348_Update_EntityType.md)|[Delete](349_Delete_EntityType.md)||
|_$links|Register|Acquire List|Update|Delete||
|_via NavProp||Acquire List||||
|**AssociationEnd**|[Register](318_Register_AssociationEnd.md)|[Acquire](320_Get_AssociationEnd.md)<br>[Acquire List](319_List_AssociationEnd.md)|[Update](321_Update_AssociationEnd.md)|[Delete](322_Delete_AssociationEnd.md)||
|_$links|[Register](323_Register_AssociationEnd_links.md)|[Acquire List](324_List_AssociationEnd_links.md)||[Delete](325_Delete_AssociationEnd_links.md)||
|_via NavProp||Acquire List||||
|**ComplexType**|[Register](327_Register_ComplexType.md)|[Acquire](329_Get_ComplexType.md)<br>[Acquire List](328_List_ComplexType.md)|Update|[Delete](331_Delete_ComplexType.md)||
|_$links|Register|Acquire List|Update|Delete||
|**Property**|[Register](355_Register_Property.md)|[Acquire](357_Get_Property.md)<br>[Acquire List](356_List_Property.md)|Update|[Delete](359_Delete_Property.md)||
|_$links|Register|Acquire List|Update|Delete||
|**ComplexTypeProperty**|[Register](336_Register_ComplexTypeProperty.md)|[Acquire](338_Get_ComplexTypeProperty.md)<br>[Acquire List](337_List_ComplexTypeProperty.md)|[Update](339_Update_ComplexTypeProperty.md)|[Delete](340_Delete_ComplexTypeProperty.md)||
|_$links|Register|Acquire List|Update|Delete||
|**Entity**|[Create](364_Create_Entity.md)|[Acquire](366_Get_Entity.md)<br>[Acquire List](365_List_Entity.md)|[Update](367_Update_Entity.md)<br>[Partial Update](369_Partial_Update_Entity.md)|[Delete](370_Delete_Entity.md)|[Batch Operation](368_Entity_Bulk_Operations.md)|
|_$links|[Register](373_Register_User_Data_links.md)|[Acquire List](374_User_Data_List_links.md)|Update|[Delete](376_Delete_User_Data_links.md)||
|_via NavProp|[Register](377_Register_using_NavProp.md)|[Acquire List](378_List_using_NavProp.md)||||

##### Server Script (Engine Service Collection)

You can register Personium application and server side logic created by Cell user and run it.  
First, register the user logic as a file, set the service collection and associate with the path, so that  
You can run the user logic for requests from any path under the collection.

||Create/Register|Acquire|Update|Delete|Other|
|:--|:--|:--|:--|:--|:--|
|Service Collection Source|[Create](381_Create_Service_Collection_Source.md)|[Acquire](382_List_Service_Collection_Source.md)|[Apply Settings](380_Configure_Service_Collection.md)|[Delete](383_Delete_Service_Collection_Source.md)|[Service Execute](384_Service_Execution.md)|

##### Service Document Acquire/Schema Acquire

||Acquire|
|:--|:--|
|Service Document|[Acquire](317_Document_Acquisition_Service.md)|
|Schema|[Acquire](316_User_Defined_Data_Schema.md)|

##### OData Acquisition Common Queries

|Query|Single Acquisition|List Acquisition|
|:--|:--|:--|
|[$format Query](404_Format_Query.md)|Yes|Yes|
|[$expand Query](405_Expand_Query.md)|Yes|Yes|
|[$select Query](406_Select_Query.md)|Yes|Yes|
|[$orderby Query](400_Orderby_Query.md)|Yes|Yes|
|[$top Query](401_Top_Query.md)|Yes|Yes|
|[$skip Query](402_Skip_Query.md)|Yes|Yes|
|[$filter Query](403_Filter_Query.md)|Yes|Yes|
|[$inlinecount](407_Inlinecount_Query.md)|Yes|Yes|
|[Full-text Search (q) Query](408_Full_Text_Search_Query.md)|Yes|Yes|


### Common

#### [Error Messages](004_Error_Messages.md)

#### [Restrictions on Personium HTTP Implementation](003_Common_Limitations_on_HTTP_Implementation.md)

#### [CORS Support](002_CORS_Support.md)

#### [Cross Domain Policy File Acquire](001_Cross_Domain_Policy_File.md)

