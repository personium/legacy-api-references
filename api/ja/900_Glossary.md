# 用語


##### [A](#anc_a) | [B](#anc_b) | [C](#anc_c) | [D](#anc_d) | [E](#anc_e) | [F](#anc_f) | [G](#anc_g) | [H](#anc_h) | [I](#anc_i) | [J](#anc_j) | [K](#anc_k) | [L](#anc_l) | [M](#anc_m) | [N](#anc_n) | [O](#anc_o) | [P](#anc_p) | [Q](#anc_q) | [R](#anc_r) | [S](#anc_s) | [T](#anc_t) | [U](#anc_u) | [V](#anc_v) | [W](#anc_w) | [X](#anc_x) | [Y](#anc_y) | [Z](#anc_z)


##### <a name="anc_a"> A
###### ACL
[一般] Access Control Listの略称。  
オブジェクトに付与した権限によって、どのユーザがアクセスを許可されているか、どのような制御命令の使用が許可されているかを定義する。  
（例：Read:読込許可、Write:編集許可、Read/Write:読込/編集許可）


###### Account
[Personium] 特定のセルに属するユーザを意味する。アカウント名やパスワードといった情報によって保持される。１セルに複数の登録が可能。  
詳細はセル制御オブジェクト参照。


###### Association
[OData] 2つ以上のEntityType（RDBにおけるテーブル）間の関係性を示す。1対のAssociationEndとその間の$linksによって定義される。  
「1対1」「1対多」「多対多」の種類に分けて扱われる。詳細はユーザODataモデル参照。


###### AssociationEnd
[OData] AssociationのエンドポイントとなっているEntityTypeを定義する。1対のAssociationEndとその間の$linksによってAssociationが定義される。  
詳細はユーザODataモデル参照。


###### Authentication
[一般] 認証。Personiumのアカウント認証では、作成済みのアカウント名とパスワードでの認証を行い、トークンを取得する方式を採用している。  
詳細は、認証モデルを参照。


##### <a name="anc_b"> B
###### bar ファイル
[Personium] box archive ファイルの略称。Boxの構成内容をまとめたアーカイブファイル。  
フォルダ内の階層構造でWebDAVコレクションの階層構造を表し、その階層毎にファイルを格納することで、WebDAV内に格納するファイルを保持する。
詳細はbarファイル参照。  


###### Box
[Personium] アプリケーションに用いるデータを格納する領域であるが、自身もWebDAVコレクションの一つである。一意の名前とスキーマURLを持つ。Cellは、Box未作成でも初期状態で1つのBox（メインボックス） を持つが、削除は不可。詳細はセル制御オブジェクトを参照。


###### Box インストール
[Personium] barファイルを用いて、Boxを作成すること。詳細はBoxインストールAPIを参照。


###### Box レベル ACL
[Personium] Box配下のリソース対するACL。詳細はアクセス制御モデル参照。


##### <a name="anc_c"> C
###### Cell
[Personium] 「Personium」の別名であり、データやアプリケーション共有の中心となるデータ領域。


###### Cell制御オブジェクト
[Personium] セルが持つ機能を個別に定義する定義体で、Role,   Account, Box, ExtCell, ExtRoleなどがある。セル制御オブジェクト参照。
###### Cell レベル ACL
[Personium] Boxレベルのアクセス権限を除いたCellへのACL。詳細は、認証モデルを参照。


###### Cell Manager
[Personium] セルの管理者のことで、そのセルへのアクセス権限や動作権限を持つ。


###### Cell Profile
[Personium Portal] Personiumポータル内でセル名、イメージ、 そのセルの情報などを表示する項目。


###### Collection （コレクション）
[WebDAV] セルにあるBoxの中に格納されたのデータ集合を表す。「WebDAV」「OData Service Collection」「Engine Service Collection」の3種類がある。


###### ComplexType
[OData] 下位属性を伴った属性を持つPropertyのこと。項目名はComplexTypeProperty。  
（例えば、「住所」をComplexTypeとすると、通り1・通り2・国・郵便番号・都道府県・市区町村などが考えられる。）


###### ComplexTypeProperty
[OData] ComplexTypeの下位属性の名称。例えば、 ComplexTypeが「住所」の場合、通り1・通り2・国・郵便番号・都道府県・市区町村などが考えられる。


###### CORS
[一般] Cross-Origin Resource Sharingの略称。Webページにおいて、JavaScriptが他のドメインに対しXMLHttpRequestを許可すること。CORS対応を参照。  
（詳細については外部サイトを参照）


###### Cross Domain Access Control(クロスドメインアクセス制御)
[一般] 異なるドメインを持つサーバに対するアクセスの制御行うこと。Personiumでは、XMLHttpRequest Level2に基づいたクロスドメインポリシーファイルによって制御される。


##### <a name="anc_d"> D
##### <a name="anc_e"> E
###### Engine Service Collection
[WebDAV] ユーザーがサーバー側のロジックを新たに登録するための特別なコレクション。詳細はEngineサービスレクションを参照。


###### Entity
[OData] データの記録構造のことであり、RDBにおけるテーブル1行分に相当する。例えば、名前・住所・性別といった情報の行を表したもの。


###### EntityType
[OData] データの構造をEntity Data Model(EDM)であらわすための定義体。EntityTypeは、上位概念を表したもの（顧客、注文内容など）


###### Environment
[Personium Portal] Personiumポータル内の用語。管理権限のあるユーザが複数のセルを包括管理する際に括られたセルの集合。Personiumのデータ構造を参照。


###### ETag
[Personium] Entity Tagのこと。Webキャッシュの検証に用いる固有の識別子でクライアントの状況に応じたリクエストの送信が可能。  
コンテンツの更新がない場合において、レスポンスをすべて返す必要がないときにキャッシュの効果的な使用を高め、帯域幅を確保する。


###### Event
[Personium] Personiumの内部および外部から発生するインスタンス。詳細はイベント概要参照。


###### EventLog
[Personium] 外部および内部イベントの発生ログ。ログ取得APIによって取得可能。


###### $expand クエリ
[OData] サポートしているODataクエリの1つ。データ取得リクエストに付加し、指定した関連情報を同時に取得するクエリ。（詳細事項）


###### External Cell
[Personium] 外部セル。セル制御オブジェクトの1つ(ExtCell)。あるセルの外にある他のセル。あらゆるユニットのセルを外部セルとして扱う事ができる。詳細はセル制御オブジェクト参照。


###### External Role
[Personium] 外部ロール。セル制御オブジェクトの1つ(ExtRole)。特定の関係にある外部セルにて、特定の役割（Role）を付与された利用者主体を表す。詳細はセル制御オブジェクト参照。


##### <a name="anc_f"> F
###### $filter クエリ
[OData] サポートしているODataクエリの1つ。検索条件を指定しデータを絞り込むクエリ。（詳細事項）


###### $format クエリ
[OData] サポートしているODataクエリの1つ。HTTPレスポンスにおいてメディアタイプを指定するクエリ。（詳細事項）


###### UnitFQDN
[一般] Fully Qualified Domain Name（完全修飾ドメイン名）の略称。  
インターネット上の特定のコンピュータやホストを一意に定義する完全なドメイン名をあらわす  （例：host-name.domain-name.com）


###### 全文検索クエリ(Full-Text Search Query)
[OData] リクエストに「q="検索語"」を付加することで、EntityTypeに含まれている全データを対象とした全文検索を行うクエリ。（詳細事項）


##### <a name="anc_g"> G
##### <a name="anc_h"> H
##### <a name="anc_i"> I
###### Implicit Flow
[OAuth2.0] OAuth2.0で規定された認可付与フローの一つ。 クライアントが (リソースオーナー認可の結果) 認可コードの代わりに直接アクセストークンを受け取る。  
詳細は外部サイト参照。


###### $inlinecount クエリ
[OData] サポートしているODataクエリの1つ。コレクションにおける、エンティティの数のカウントを表示するクエリ。（詳細事項）


##### <a name="anc_j"> J
##### <a name="anc_k"> K
##### <a name="anc_l"> L
##### <a name="anc_m"> M
###### Main Box　メインボックス
[Personium] セル作成時にデフォルトで作成される、”\__”(アンダーバー2つ)と名づけられたBox。動作は通常のBoxと同様だが、削除は不可。  
アプリケーションデータを保管する目的以外にも、そのCellの固有情報（json形式を用いる）の格納に使われる。


###### Message
[Personium] Personiumにおいて、セルの間でメッセージを送受信する機能。ユーザ任意のメッセージの送受信およびセル間の関係性($links)の発行が可能。詳細はメッセージのモデルを参照。


###### Multiplicity
[OData] 多重度。AssociationEnd作成時に、関係するEntityTypeの個数を定義する。  
AssociationEndの多重度の表記は、2つのEntityTypeの間の関係では、「1」、「0..1」(0または1)、「\*」(多数)のいずれかを取る。


##### <a name="anc_n"> N
###### NavigationProperty
[一般] Entity Data ModelやODataのデータ構造において、Associationの 一方の End から別の End へのナビゲーションを表すProperty。


##### <a name="anc_o"> O
###### OData
[一般] Open Data Protocolの略称。コレクション（Boxの中に格納されたのデータ集合）の1つ。HTMLに準拠した標準データアクセスプロトコルであり、データリソースへのCRUDアクセスが可能。  
ODataについての詳細はこちら。


###### OData Service Collection
[WebDAV] ユーザーがODataを操作するための特別なコレクション。ユーザODataモデル参照。


###### $orderby クエリ
[OData] サポートしているODataクエリの1つ。ユーザーが特定した順序でソートされた値を表示する。デフォルトでは昇順。（詳細事項）


##### <a name="anc_p"> P
###### Property
[OData] 各EntityTypeの列頭の値。例えば「顧客」というEntityTypeには「ID」「名前」「住所」といったPropertyが考えられる。


###### Privilege
[一般] Cell内に定義されたRoleに対し、特定のRoleに紐付けられたBox内部のデータにアクセスする権限。  
PersoniumではACLの設定によって定義される。詳細はアクセス制御モデル参照。


##### <a name="anc_q"> Q
##### <a name="anc_r"> R
###### RBAC
[一般] Role-based access controlの略称。各アカウントにRole（役割）を定義し、Roleに基づきアクセス制御を設定すること。


###### ReceivedMessage
[Personium] 特定のセルからの、Relation発行リクエストやメッセージを受信する定義体。詳細はメッセージのモデルを参照。


###### Refreshトークン
[Personium] アクセストークンを再発行するために用いるトークン。リフレッシュトークンの有効時間は24時間である。詳細は認証 モデルを参照。


###### Refreshトークン 認証
[Personium] アクセストークンを再発行するプロセス。詳細は認証 モデルを参照。


###### Relation
[Personium] セルとその外部セルとの関係を示す定義体。同じまたは異なるユニットに属している外部セルの間で関係を確立できる。  
常にセル固有の関係性になるようにBoxに関連づけられる。セル制御オブジェクト参照。


###### Relation クラス URL
[Personium] アプリケーションと定義される関係リソースへのURL。RelationクラスURL構造は以下の通り:  
${Schema URL}/\__relation/\__/${RelationName}


###### Relation インスタンス URL
[Personium] 要求が1つ以上のExternal Cell(s)に送られる特定のRelationのURL。  
Relation インスタンスURLの構造は以下の通り:  
${Cell URL}/\__relation/${BoxName}/${RelationName}


###### RequireSchemaAuthz
[WebDAV] ACL要素の属性値であり、Boxのスキーマ権限における要求レベルを定義する。


###### resourcetype
[WebDAV] Collectionの型を表す。ODataCollection/ServiceCollection/DavCollection/fileなど。


###### Role
[Personium] セル制御オブジェクトの1つ。すべてのCellに対して定義される「役割」をあらわす。（例：administorator、teacher、student）  
どのアカウント(ユーザ)が、そのCellにアクセスすることができるかを指定できるようになるので、Cellのアカウント所有権を規定するアクセス権限の異なった設定を持つことができる。  
セル制御オブジェクト参照。


###### Role クラス URL
[Personium] トランスセルトークン内部に蓄積させるためのRoleのURL。RoleクラスURLの構造は以下の通り:  
${schema URL}/\__role/\__/${RoleName}


###### Role インスタンス URL
[Personium] 特定のCellに登録されたRoleの、現行状態を提供するURL。Role Resource URLと同様。Schemaは以下の通り:  
${Cell URL}/\__role/${BoxName}/${RoleName}


###### ROPC
[OAuth2.0] Resource Owner Password Credentials (リソースオーナーパスワードクレデンシャル)の略称。OAuth2.0において規定された認可プロセスの1つ。詳細は外部サイト参照。


##### <a name="anc_s"> S
###### Schema URL　スキーマURL
[Personium] Personium内に格納されたスキーマを表すURL。


###### $select クエリ
[OData] サポートしているODataクエリの1つ。データ取得時に、特定のPropertyのみ指定して取得するクエリ。複数指定も可能。（詳細事項）


###### SentMessage
[Personium] 目的のセルに対しRelation設定承認を得るためのメッセージや通常のメッセージを送信する定義体。詳細はメッセージのモデルを参照。


###### Service Collection (サービスコレクション)
[WebDAV] コレクション（Boxの中に格納されたのデータ集合）の1つ。ユーザー定義のサーバーサイドロジックを実行するサービスのコレクション。  
EngineサービスコレクションとODataサービスコレクションがある。


###### Service （サービス）登録
[WebDAV] コレクションにユーザー定義のサーバーサイドロジックを登録すること。


###### $skip クエリ
[OData] サポートしているODataクエリの1つ。取得したデータのうち、指定した個数の分だけ、表示から除外して抽出するクエリ。（詳細事項）


##### <a name="anc_t"> T
###### Token (トークン)
[一般] 主にユーザ認証に用いられるランダムな文字列。Personiumでは、格納したデータやリソースにアクセスする際に用いられる。クライアントがリクエストするCURL内に記載して使用する。  
Personiumでは以下の種類:があり、発行から1時間で変更される。  
(1)セルローカルトークン：認証されたセル内のリソースにアクセスする際に使用  
(2)トランスセルトークン：他のセルに認証されたセル内のリソースにアクセスする際に使用  
詳細は認証モデルを参照。


###### Token (トークン)認証
[一般] 認証プロセスの一つ。クライアントがリクエストするCURLのアクセストークンによる認証方法。詳細は認証 モデルを参照。


###### $top クエリ
[OData] サポートしているODataクエリの1つ。取得したデータの最大記録数を指定しその個数分を返す。抽出データは、セット内の最初から数えられる。（詳細事項）


##### <a name="anc_u"> U
###### Unit
[Personium] Personium基盤サーバ内で、1つ以上のセルから構成されるデータ領域。完全修飾ドメイン名（UnitFQDN）を持ち、絶対ドメイン名として参照される。


###### Unit制御オブジェクト
[Personium] ユニットユーザ（管理者）として、セルの作成や管理をするためのオブジェクト群。


###### Unit User
[Personium] ユニットの管理ユーザ。ユニット内部のCRUD（Create/Read/Update/Delete）の権限を持つ。


##### <a name="anc_v"> V
##### <a name="anc_w"> W
###### WebDAV
[一般] Web-based Distributed Authoring and Versioningの略称。HTTPの拡張であり、Webサーバ内のドキュメントやファイルを異なるユーザで共同で執筆するためのプロトコル。  
Personiumポータルでは、WebDAVコレクションはファイルやフォルダであり、またCRUDの機能が動作する。  
WebDAV Resourcesを参照。


##### <a name="anc_x"> X
##### <a name="anc_y"> Y
##### <a name="anc_z"> Z


##### [A](#anc_a) | [B](#anc_b) | [C](#anc_c) | [D](#anc_d) | [E](#anc_e) | [F](#anc_f) | [G](#anc_g) | [H](#anc_h) | [I](#anc_i) | [J](#anc_j) | [K](#anc_k) | [L](#anc_l) | [M](#anc_m) | [N](#anc_n) | [O](#anc_o) | [P](#anc_p) | [Q](#anc_q) | [R](#anc_r) | [S](#anc_s) | [T](#anc_t) | [U](#anc_u) | [V](#anc_v) | [W](#anc_w) | [X](#anc_x) | [Y](#anc_y) | [Z](#anc_z)
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
