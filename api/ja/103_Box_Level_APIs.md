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
|コレクション|<br>|[作成](110_コレクション作成API.html)|[設定取得](108_コレクション設定取得API.html)|[設定変更](111_コレクション設定変更API.html)<br>[移動名称変更](501_コレクション移動名称変更API.html)|[削除](109_コレクション削除API.html)|
|ファイル|<br>|[登録更新](114_ファイル登録更新API.html)|[取得](116_ファイル取得API.html)<br>[設定取得](112_ファイル設定取得API.html)|[設定変更](113_ファイル設定変更API.html)|[削除](115_ファイル削除API.html)|
|アクセス制御|[設定](117_アクセス制御設定API.html)|<br>|<br>|<br>|<br>|

※ すべてのファイルやコレクション（特殊コレクションを含む）に対してACL設定(アクセス制御設定)が可能です。
※ ACL設定 は PROPFINDメソッドで取得できます。

<br>
#### 特別なコレクション: OData Serviceコレクション
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

##### データ操作
* [スキーマ取得](172_Get_Schema.html)
* サービスドキュメント取得

<br>
#### 特別なコレクション: Engine Service コレクション
##### [サーバスクリプト(Engine Service Collection)](180_Engine_Service_Collection_APIs.html)
personium.ioアプリケーションやCell利用者が作成したサーバサイドロジックを登録しこれを走行させることができます。
はじめに、ユーザロジックをファイルとして登録し、サービスコレクションの設定を行ってパスとの関連付けを行うことで、
コレクション配下の任意のパスからのリクエストに対してユーザーロジックを走行させることができます。

|<br>|<br>|作成・登録<br>|取得<br>|更新<br>|削除<br>|
|:--|:--|:--|:--|:--|:--|
|サービスコレクションソース|<br>|[作成](182_サービスコレクションソース作成API.html)|[取得](184_サービスコレクションソース取得API.html)|[設定適用](181_サービスコレクション設定適用API.html)|[削除](183_サービスコレクションソース削除API.html)|
|<br>|[サービス実行](185_サービス実行API.html)<br>|<br>|<br>|<br>|
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
