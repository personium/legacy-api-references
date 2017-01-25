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
|**Cell**|<br>|[作成](005_Cell作成API.html)|[取得](003_Cell取得API.html)<br>[一覧取得](002_Cell一覧取得API.html)|[更新](001_Cell更新API.html)|[削除](004_Cell削除API.html)<br>[再帰的削除](006_Cell再帰的削除API.html)|
|**UUT**|[昇格設定](007_ULUUT昇格設定API.html)|<br>|<br>|<br>|<br>|
<br>
### [Cell Level API](008_Cell_Level_APIs.html)
Resource Path
```
https://{UnitFQDN}/{CellName}
```

##### Cell内部設定API：単体セルモデルにおける設定

|<br>|<br>|作成・登録<br>|取得<br>|更新<br>|削除<br>|
|:--|:--|:--|:--|:--|:--|
|**Role**|<br>|[登録](010_Role登録API.html)|[取得](012_Role取得API.html)<br>[一覧取得](013_Role一覧取得API.html)|[更新](009_Role更新API.html)|[削除](011_Role削除API.html)|
|<br>|_$links|[登録](014_Role_$links登録API.html)|[一覧取得](016_Role_$links一覧取得API.html)|更新|[削除](015_Role_$links削除API.html)|
|<br>|_NavProp経由|[登録](018_Role_NavProp経由登録API.html)|[取得](019_Role_NavProp経由取得API.html)|<br>|<br>|
|**Account**|<br>|[登録](020_Account登録API.html)|[取得](022_Account取得API.html)<br>[一覧取得](023_Account一覧取得API.html)|[更新](024_Account更新API.html)|[削除](021_Account削除API.html)|
|<br>|_$links|[登録](025_Account_$links登録API.html)|[一覧取得](027_Account_$links一覧取得API.html)|更新|[削除](026_Account_$links削除API.html)|
|<br>|_NavProp経由|[登録](029_Account_NavProp経由登録API.html)|[取得](030_Account_NavProp経由取得API.html)|<br>|<br>|
|**Box**|<br>|[作成](065_Box作成API.html)|[取得](067_Box取得API.html)<br>[一覧取得](068_Box一覧取得API.html)<br>[URL取得](107_BoxURL取得.html)|[更新](064_Box更新API.html)|[削除](066_Box削除API.html)|
|<br>|_$links|[登録](069_Box_$links登録API.html)|[一覧取得](071_Box_$links一覧取得API.html)|更新|[削除](070_Box_$links削除API.html)|
|<br>|_NavProp経由|[登録](073_Box_NavProp経由登録API.html)|[取得](074_Box_NavProp経由取得API.html)|<br>|<br>|
|**認証**|[パスワード変更](102_パスワード変更API.html)|<br>|<br>|<br>|<br>|


##### ソーシャル連携設定API：複数セルモデルにおける設定

|<br>|<br>|作成・登録<br>|取得<br>|更新<br>|削除<br>|
|:--|:--|:--|:--|:--|:--|
|**ExtCell**|<br>|[登録](032_ExtCell登録API.html)|[取得](034_ExtCell取得API.html)<br>[一覧取得](035_ExtCell一覧取得API.html)|[更新](031_ExtCell更新API.html)|[削除](033_ExtCell削除API.html)|
|<br>|_$links|[登録](036_ExtCell_$links登録API.html)|[一覧取得](038_ExtCell_$links一覧取得API.html)|[更新](039_ExtCell_$links更新API.html)|[削除](037_ExtCell_$links削除API.html)|
|<br>|_NavProp経由|[登録](040_ExtCell_NavProp経由登録API.html)|[取得](041_ExtCell_NavProp経由取得API.html)|<br>|<br>|
|**Relation**|<br>|[登録](043_Relation登録API.html)|[取得](045_Relation取得API.html)<br>[一覧取得](046_Relation一覧取得API.html)|[更新](042_Relation更新API.html)|[削除](044_Relation削除API.html)|
|<br>|_$links|[登録](047_Relation_$links登録API.html)|[一覧取得](049_Relation_$links一覧取得API.html)|[更新](050_Relation_$links更新API.html)|[削除](048_Relation_$links削除API.html)|
|<br>|_NavProp経由|[登録](051_Relation_NavProp経由登録API.html)|[取得](052_Relation_NavProp経由取得API.html)|<br>|<br>|
|**ExtRole**|<br>|[登録](054_ExtRole登録API.html)|[取得](055_ExtRole取得API.html)<br>[一覧取得](057_ExtRole一覧取得API.html)|[更新](053_ExtRole更新API.html)|[削除](056_ExtRole削除API.html)|
|<br>|_$links|登録|[一覧取得](060_ExtRole_$links一覧取得API.html)|更新|[削除](059_ExtRole_$links削除API.html)|
|<br>|_NavProp経由|[登録](062_ExtRole_NavProp経由登録API.html)|[取得](063_ExtRole_NavProp経由取得API.html)|<br>|<br>|
|**認証**<br>(/\__auth, /\__authz)|[OAuth2_認可エンドポイント](100_OAuth2_認可エンドポイントAPI.html)<br>[OAuth2Token_エンドポイント認証](101_OAuth2Token_エンドポイント認証API.html)|<br>|<br>|<br>|
|**アクセス制御**|<br>|[制限設定](097_アクセス制限設定API.html)|[プロパティ取得](098_プロパティ取得API.html)|[プロパティ変更](099_プロパティ変更API.html)|<br>|
|[イベント](084_イベント概要.html)|<br>|[イベント受付](085_イベント受付API.html)|[ログファイル取得](091_ログファイル取得API.html)<br>[ログファイル一覧取得](092_ログファイル一覧取得API.html)<br>[ログファイル情報取得](090_ログファイル情報取得API.html)|<br>|[ログファイル削除](093_ログファイル削除API.html)|
|**メッセージ制御**|[送信](096_メッセージ送信API.html)|<br>|[取得](080_SentMessage取得API.html)<br>[一覧取得](081_SentMessage一覧取得API.html)|<br>|[削除](079_SentMessage削除API.html)|
|<br>|**受信**|<br>|[取得](077_ReceivedMessage取得API.html)<br>[一覧取得](078_ReceivedMessage一覧取得API.html)|[状態変更](075_メッセージ状態変更API.html)|[削除](076_ReceivedMessage削除API.html)|
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
|<br>|<br>|[Boxインストール](105_Boxインストール.html)|[Boxメタデータ取得](106_Boxメタデータ取得.html)|<br>|<br>|

##### WebDAV
|<br>|<br>|作成・登録<br>|取得<br>|更新<br>|削除<br>|
|:--|:--|:--|:--|:--|:--|
|コレクション|<br>|[作成](110_コレクション作成API.html)|[設定取得](108_コレクション設定取得API.html)|[設定変更](111_コレクション設定変更API.html)<br>[移動名称変更](501_コレクション移動名称変更API.html)|[削除](109_コレクション削除API.html)|
|ファイル|<br>|[登録更新](114_ファイル登録更新API.html)|[取得](116_ファイル取得API.html)<br>[設定取得](112_ファイル設定取得API.html)|[設定変更](113_ファイル設定変更API.html)|[削除](115_ファイル削除API.html)|
|アクセス制御|[設定](117_アクセス制御設定API.html)|<br>|<br>|<br>|<br>|

##### OData
|<br>|<br>|作成・登録<br>|取得<br>|更新<br>|削除<br>|
|:--|:--|:--|:--|:--|:--|
|**EntityType**|<br>|[登録](147_EntityType登録API.html)|[取得](149_EntityType取得API.html)<br>[一覧取得](150_EntityType一覧取得API.html)|[更新](146_EntityType更新API.html)|[削除](148_EntityType削除API.html)|
|<br>|_$links|[登録](151_EntityType_$links登録API.html)|[一覧取得](153_EntityType_$links一覧取得API.html)|更新|[削除](152_EntityType_$links削除API.html)|
|<br>|_NavProp経由|<br>|一覧取得|<br>|<br>|<br>|
|**AssociationEnd**|<br>|[登録](121_AssociationEnd登録API.html)|[取得](123_AssociationEnd取得API.html)<br>[一覧取得](120_AssociationEnd一覧取得API.html)|[更新](502_AssociationEnd更新API.html)|[削除](122_AssociationEnd削除API.html)|
|<br>|_$links|[登録](124_AssociationEnd_$links登録API.html)|[一覧取得](126_AssociationEnd_$links一覧取得API.html)|<br>|[削除](125_AssociationEnd_$links削除API.html)|
|<br>|_NavProp経由|<br>|一覧取得|<br>|<br>|
|**ComplexType**|<br>|[登録](129_ComplexType登録API.html)|[取得](131_ComplexType取得API.html)<br>[一覧取得](132_ComplexType一覧取得API.html)|更新|[削除](130_ComplexType削除API.html)|
|<br>|_$links|[登録](133_ComplexType_$links登録API.html)|[一覧取得](135_ComplexType_$links一覧取得API.html)|更新|[削除](134_ComplexType_$links削除API.html)|
|**Property**|<br>|[登録](157_Property登録API.html)|[取得](159_Property取得API.html)<br>[一覧取得](160_Property一覧取得API.html)|[更新](156_Property更新API.html)|[削除](158_Property削除API.html)|
|<br>|_$links|[登録](161_Property_$links登録API.html)|[一覧取得](163_Property_$links一覧取得API.html)|更新|[削除](162_Property_$links削除API.html)|
|**ComplexTypeProperty**|<br>|[登録](138_ComplexTypeProperty登録API.html)|[取得](140_ComplexTypeProperty取得API.html)<br>[一覧取得](141_ComplexTypeProperty一覧取得API.html)|[更新](137_ComplexTypeProperty更新API.html)|[削除](139_ComplexTypeProperty削除API.html)|
|<br>|_$links|登録|一覧取得|更新|削除|
|**Entity**|[一括操作](165_Entity一括操作API.html)|[作成](166_Entity作成API.html)|[取得](170_Entity取得API.html)<br>[一覧取得](171_Entity一覧取得API.html)|[更新](168_Entity更新API.html)<br>[部分更新](169_Entity部分更新API.html)|[削除](167_Entity削除API.html)|
|（ユーザデータ）|_$links|[登録](174_ユーザデータ_$links登録API.html)|[一覧取得](176_ユーザデータ_$links一覧取得API.html)|更新|[削除](175_ユーザデータ_$links削除API.html)|
|（ユーザデータ）|_NavProp経由|[登録](178_ユーザデータ_NavProp経由登録API.html)|[一覧取得](179_ユーザデータ_NavProp経由一覧取得API.html)|<br>|<br>|


##### [サーバスクリプト(Engine Service Collection)](180_Engine_Service_Collection_APIs.html)
|<br>|<br>|作成・登録<br>|取得<br>|更新<br>|削除<br>|
|:--|:--|:--|:--|:--|:--|
|サービスコレクションソース|<br>|[作成](182_サービスコレクションソース作成API.html)|[取得](184_サービスコレクションソース取得API.html)|[設定適用](181_サービスコレクション設定適用API.html)|[削除](183_サービスコレクションソース削除API.html)|
|<br>|[サービス実行](185_サービス実行API.html)<br>|<br>|<br>|<br>|
<br>
### Cell/Box 共通 API
##### サービスドキュメント取得/スキーマ取得
|<br>|<br>|作成・登録<br>|取得<br>|更新<br>|削除<br>|
|:--|:--|:--|:--|:--|:--|
|サービスドキュメント|<br>|<br>|[取得](119_サービスドキュメント取得API.html)|<br>|<br>|
|スキーマ|<br>|<br>|[取得](118_スキーマ取得API.html)|<br>|<br>|


##### OData取得共通クエリ
|クエリ|一件取得使用可|一覧取得使用可|備考|
|:--|:--:|:--:|:--|
|[$formatクエリ](190_$formatクエリ.html)|○|○|<br>|
|[$expandクエリ](191_$expandクエリ.html)|○|○|<br>|
|[$selectクエリ](192_$selectクエリ.html)|○|○|<br>|
|[$orderbyクエリ](186_$orderbyクエリ.html)|○|<br>|<br>|
|[$topクエリ](187_$topクエリ.html)|○|<br>|<br>|
|[$skipクエリ](188_$skipクエリ.html)|○|<br>|<br>|
|[$filterクエリ](189_$filterクエリ.html)|○|<br>|<br>|
|[$inlinecount](193_$inlinecountクエリ.html)|○|<br>|<br>|
|[全文検索(q)クエリ](194_全文検索クエリ.html)|○|<br>|<br>|
<br>
### 共通
#### [エラーメッセージ一覧](198_Error_Messages.html)
#### [personiumのHTTP実装に関する制限事項](197_Common_Limitations_on_HTTP_Implementation.html)
#### [CORS対応](196_CORS_Support.html)
#### [クロスドメインポリシーファイル取得](195_Cross_Domain_Policy_File.html)
<br>
##### [用語集](199_Glossary.html)
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
