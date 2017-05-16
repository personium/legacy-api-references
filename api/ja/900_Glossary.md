# 用語


##### [A](#anc_a) | [B](#anc_b) | [C](#anc_c) | [D](#anc_d) | [E](#anc_e) | [F](#anc_f) | [G](#anc_g) | [H](#anc_h) | [I](#anc_i) | [J](#anc_j) | [K](#anc_k) | [L](#anc_l) | [M](#anc_m) | [N](#anc_n) | [O](#anc_o) | [P](#anc_p) | [Q](#anc_q) | [R](#anc_r) | [S](#anc_s) | [T](#anc_t) | [U](#anc_u) | [V](#anc_v) | [W](#anc_w) | [X](#anc_x) | [Y](#anc_y) | [Z](#anc_z)


##### <a name="anc_a"> A</a>
###### ACL
[一般] Access Control Listの略称。  
オブジェクトに付与するユーザのアクセス権限を列挙したリスト。PersoniumにおいてはCell,Boxに設定することができる。オブジェクトに付与した権限によって、どのユーザがアクセスを許可されているか、どのような制御命令の使用が許可されているかを定義する。  
（例：Read:読込許可、Write:編集許可、Read/Write:読込/編集許可）


###### Account
[一般] 特定のセルに属するユーザを意味する。アカウント名やパスワードといった情報によって構成される。１セルに複数の登録が可能。  


###### Association
[OData] 2つ以上のEntityType（RDBにおけるテーブル）間の関係性を示す。1対のAssociationEndとその間の$linksによって構成される。  
「1対1」「1対多」「多対多」の種類に分けて扱われる。


###### AssociationEnd
[OData] Associationを構成するエンドポイントとなっているEntityType。1対のAssociationEndとその間の$linksによってAssociationが構成される。  


###### Authentication
[一般] 認証。Personiumのアカウント認証では、作成済みのアカウント名とパスワードでの認証を行い、トークンを取得する方式を採用している。  


##### <a name="anc_b"> B</a>
###### bar ファイル
[Personium] box archive ファイルの略称。Boxの構成内容をまとめたアーカイブファイル。  
フォルダ内の階層構造でWebDAVコレクションの階層構造を表し、その階層毎にファイルを格納することで、WebDAV内に格納するファイルを保持する。
詳細は[barファイル](301_Bar_File.html)参照。  


###### Box
[Personium] アプリケーションに用いるデータを格納する領域。自身もWebDAVコレクションの一つである。一意の名前とスキーマURLを持つ。Cellは、Box未作成でも初期状態で1つのBox（メインボックス） を持ち、削除は不可。


###### Box インストール
[Personium] barファイルを用いて、Boxを作成すること。詳細は[Boxインストール](302_Box_Installation.html)APIを参照。


###### Box レベル ACL
[Personium] Box配下のリソース対するACL。詳細は[アクセス制御モデル](AccessControlOverview.htm)参照。


##### <a name="anc_c"> C</a>
###### Cell
[Personium] データ主体ごとのData Strore。個人で使う場合はPDS(Personal Data Store)となる。Personiumでは、データ主体という概念を人のみでなく組織やモノなどにも拡張したモデル化を行っているため、組織やモノのデータストアとしても使うことが可能。  
（例、私のCell, あなたのCell, ○○会社のCell, ○○部のCell, 私の車のCell）



###### Cell制御オブジェクト
[Personium] セルが持つ機能を個別に定義する定義体。Role,   Account, Box, ExtCell, ExtRoleなどがある。


###### Cell レベル ACL
[Personium] Boxレベルのアクセス権限を除いたCellへのACL。Cell制御オブジェクトの操作や配下のBoxに対するアクセス制御を定義する。


###### Cell Profile
[Personium] Personiumセルの情報を格納する定義体。ホームアプリケーション等のアプリケーションでセル名、イメージ、 そのセルの情報などを表示する項目。


###### Collection （コレクション）
[Personium] セルにあるBoxの中に格納されたデータ集合。「WebDAV」「OData Service Collection」「Engine Service Collection」の3種類がある。


###### ComplexType
[OData] 下位属性を伴った属性を持つPropertyのこと。項目名はComplexTypeProperty。  
（例えば、「住所」をComplexTypeとすると、通り1・通り2・国・郵便番号・都道府県・市区町村などがComplexTypePropertyとなる。）


###### ComplexTypeProperty
[OData] ComplexTypeの下位属性の名称。例えば、 ComplexTypeが「住所」の場合、通り1・通り2・国・郵便番号・都道府県・市区町村などがComplexTypePropertyとなる。


###### CORS
[一般] Cross-Origin Resource Sharingの略称。Webページにおいて、JavaScriptが他のドメインに対しXMLHttpRequestを許可すること。[CORS対応](002_CORS_Support.html)を参照。  
（詳細については[外部サイト](http://www.w3.org/TR/cors/)を参照）


###### Cross Domain Access Control(クロスドメインアクセス制御)
[一般] 異なるドメインを持つサーバに対するアクセスの制御を行うこと。Personiumでは、XMLHttpRequest Level2に基づいた[クロスドメインポリシーファイル](001_Cross_Domain_Policy_File.html)によって制御されている。


##### <a name="anc_d"> D</a>
##### <a name="anc_e"> E</a>
###### Engine Service Collection
[Personium] ユーザがサーバ側のロジックを新たに登録するための特別なコレクション。詳細は[Engineサービスレクション](379_Engine_Service_Collection_APIs.html)を参照。


###### Entity
[OData] データの記録構造のことであり、RDBにおけるテーブル1行分に相当する。例えば、名前・住所・性別といった情報に格納された値を表したもの。


###### EntityType
[OData] データの構造をEntity Data Model(EDM)であらわすための定義体。EntityがRDBにおけるテーブル1行分に相当するのに対し、EntityTypeはテーブルの上位概念を表したもの（顧客、注文内容など）


###### ETag
[Personium] Entity Tagのこと。Webキャッシュの検証に用いる固有の識別子でクライアントの状況に応じたリクエストの送信を可能にする。  
コンテンツの更新がない場合において、レスポンスをすべて返す必要がないときにキャッシュの効果的な使用を高め、データの転送量を減らすことで帯域幅を確保する。


###### Event
[Personium] Personiumの内部および外部から発生するインスタンス。詳細は[イベント概要](277_Event_Summary.html)参照。


###### EventLog
[Personium] 外部および内部イベントの発生ログ。[ログ取得API](285_Retrieve_Log_File.html)によって取得可能。


###### $expand クエリ
[OData] PersoniumでサポートしているODataクエリの1つ。データ取得リクエストに付加し、指定した関連情報を同時に取得するクエリ。（[詳細事項](405_Expand_Query.html)）


###### External Cell
[Personium] 外部セル。セル制御オブジェクトの1つ(ExtCell)。あるセルの外にある他のセル。あらゆるユニットのセルを外部セルとして扱う事ができる。


###### External Role
[Personium] 外部ロール。セル制御オブジェクトの1つ(ExtRole)。特定の関係にある外部セルにて、特定の役割（Role）を付与された利用者主体を表す。


##### <a name="anc_f"> F</a>
###### $filter クエリ
[OData] PersoniumでサポートしているODataクエリの1つ。検索条件を指定しデータを絞り込むクエリ。（[詳細事項](403_Filter_Query.html)）


###### $format クエリ
[OData] PersoniumでサポートしているODataクエリの1つ。HTTPレスポンスにおいてメディアタイプを指定するクエリ。（[詳細事項](404_Format_Query.html)）


###### FQDN
[一般] Fully Qualified Domain Name（完全修飾ドメイン名）の略称。  
インターネット上の特定のコンピュータやホストを一意に定義する完全なドメイン名をあらわす  （例：host-name.domain-name.com）


###### 全文検索クエリ(Full-Text Search Query)
[OData] リクエストに「q="検索語"」を付加することで、EntityTypeに含まれている全データを対象とした全文検索を行うクエリ。（[詳細事項](408_Full_Text_Search_Query.html)）


##### <a name="anc_g"> G</a>
##### <a name="anc_h"> H</a>
##### <a name="anc_i"> I</a>
###### Implicit Flow
[OAuth2.0] <a href="http://tools.ietf.org/pdf/rfc6749.pdf">OAuth2.0</a>で規定された認可付与フローの一つ。 クライアントが (リソースオーナー認可の結果) 認可コードの代わりに直接アクセストークンを受け取る。  
詳細は[外部サイト](http://openid-foundation-japan.github.io/draft-ietf-oauth-v2-draft22.ja.html#grant-implicit)参照。


###### $inlinecount クエリ
[OData] PersoniumでサポートしているODataクエリの1つ。コレクションにおける、エンティティの数のカウントを表示するクエリ。（[詳細事項](407_Inlinecount_Query.html)）


##### <a name="anc_j"> J</a>
##### <a name="anc_k"> K</a>
##### <a name="anc_l"> L</a>
##### <a name="anc_m"> M</a>
###### Main Box　（メインボックス）
[Personium] セル作成時にデフォルトで作成される、”\__”(アンダーバー2つ)と名づけられたBox。動作は通常のBoxと同様だが、削除は不可。  
アプリケーションデータを保管する目的以外にも、そのCellの固有情報（json形式を用いる）の格納に使われる。


###### Message
[Personium] Personiumにおいて、セルの間でメッセージを送受信する機能。ユーザ任意のメッセージの送受信およびセル間の関係性($links)の発行が可能。


###### Multiplicity
[OData] 多重度。AssociationEnd作成時に、関係するEntityTypeの個数を定義する。  
AssociationEndの多重度の表記は、2つのEntityTypeの間の関係では、「1」、「0..1」(0または1)、「\*」(多数)のいずれかを取る。


##### <a name="anc_n"> N</a>
###### NavigationProperty
[OData] Entity Data ModelやODataのデータ構造において、Associationの 一方の End から別の End へのナビゲーションを表すProperty。


##### <a name="anc_o"> O</a>
###### OData
[OData] Open Data Protocolの略称。コレクション（Boxの中に格納されたデータ集合）の1つ。HTTPに準拠した標準データアクセスプロトコルであり、データリソースへのCRUDアクセスが可能。  
ODataについての詳細は[こちら](http://www.odata.org/)。


###### OData Service Collection
[WebDAV] ユーザーがODataを操作するための特別なコレクション。


###### $orderby クエリ
[OData] PersoniumでサポートしているODataクエリの1つ。ユーザーが特定した順序でソートされた値を表示する。デフォルトでは昇順。（[詳細事項](400_Orderby_Query.html)）


##### <a name="anc_p"> P</a>
###### Property
[OData] 各EntityTypeの列頭の値。RDBにおけるテーブルの項目名に相当する。例えば「顧客」というEntityTypeには「ID」「名前」「住所」といったPropertyが考えられる。


###### Privilege
[Personium] Cell内に定義されたRoleに対し、特定のRoleに紐付けられたBox内部のデータにアクセスする権限。  
PersoniumではACLの設定によって定義される。詳細は[アクセス制御モデル](AccessControlOverview.htm)参照。


##### <a name="anc_q"> Q</a>
##### <a name="anc_r"> R</a>
###### RBAC
[一般] Role-based access controlの略称。各アカウントにRole（役割）を定義し、Roleに基づきアクセス制御を設定すること。


###### ReceivedMessage
[Personium] 特定のセルからの、Relation発行リクエストやメッセージを受信する定義体。


###### Refreshトークン
[OAuth2] アクセストークンを再発行するために用いるトークン。リフレッシュトークンの有効時間は24時間である。


###### Refreshトークン 認証
[Personium] アクセストークンを再発行するためのプロセス。


###### Relation
[Personium] セルとその外部セルとの関係を示す定義体。同じまたは異なるユニットに属している外部セルの間で関係を確立できる。  
常にセル固有の関係性になるようにBoxに関連づけられる。


###### Relation クラス URL
[Personium] アプリケーションと定義される関係リソースへのURL。RelationクラスURL構造は以下の通り:  
${Schema URL}/\_\_relation/\_\_/${RelationName}


###### Relation インスタンス URL
[Personium] 要求が1つ以上のExternal Cell(s)に送られる特定のRelationのURL。  
Relation インスタンスURLの構造は以下の通り:  
${Cell URL}/\__relation/${BoxName}/${RelationName}


###### RequireSchemaAuthz
[Personium] ACL要素の属性値であり、Boxのスキーマ権限における要求レベルを定義する。


###### resourcetype
[WebDAV] Collectionの型を表す。ODataCollection/ServiceCollection/DavCollection/fileなど。


###### Role
[Personium] セル制御オブジェクトの1つ。すべてのCellに対して定義される「役割」をあらわす。（例：administrator、teacher、student）  
どのアカウント(ユーザ)が、そのCellにアクセスすることができるかを指定できるようになるので、Cellのアカウント所有権を規定するアクセス権限の異なった設定を持つことができる。  


###### Role クラス URL
[Personium] トランスセルトークン内部に蓄積させるためのRoleのURL。RoleクラスURLの構造は以下の通り:  
${schema URL}/\_\_role/\_\_/${RoleName}


###### Role インスタンス URL
[Personium] 特定のCellに登録されたRoleの、現行状態を提供するURL。Role Resource URLと同様。Schemaは以下の通り:  
${Cell URL}/\__role/${BoxName}/${RoleName}


###### ROPC
[OAuth2.0] Resource Owner Password Credentials (リソースオーナーパスワードクレデンシャル)の略称。[OAuth2.0](http://tools.ietf.org/pdf/rfc6749.pdf)において規定された認可プロセスの1つ。詳細は[外部サイト](http://openid-foundation-japan.github.io/draft-ietf-oauth-v2-draft22.ja.html#anchor7)参照。ID/PWを用いる、Personiumのトークンを取得するための標準的な認証方法。


##### <a name="anc_s"> S</a>
###### Schema URI　スキーマURI
[Personium] Personium内に格納されたスキーマを表すURL。定義はCell URLもしくはURIである。


###### $select クエリ
[OData] PersoniumでサポートしているODataクエリの1つ。データ取得時に、特定のPropertyのみ指定して取得するクエリ。複数指定も可能。（[詳細事項](406_Select_Query.html)）


###### SentMessage
[Personium] 目的のセルに対しRelation設定承認を得るためのメッセージや通常のメッセージを送信する定義体。


###### Service Collection (サービスコレクション)
[Personium] コレクション（Boxの中に格納されたのデータ集合）の1つ。ユーザ定義のサーバサイドロジックを実行するサービスのコレクション。  


###### Service （サービス）登録
[WebDAV] Serviceコレクションにユーザ定義のサーバサイドロジックを登録すること。


###### $skip クエリ
[OData] PersoniumでサポートしているODataクエリの1つ。取得したデータのうち、指定した個数の分だけ、表示から除外して抽出するクエリ。（[詳細事項](402_Skip_Query.html)）


##### <a name="anc_t"> T</a>
###### Token (トークン)
[一般] 主にユーザ認証に用いられるランダムな文字列。Personiumでは、格納したデータやリソースにアクセスする際に用いられる。クライアントがリクエストするcURL内に記載して使用する。  
Personiumでは以下の種類:があり、発行から1時間で変更される。  
(1)セルローカルトークン：認証されたセル内のリソースにアクセスする際に使用  
(2)トランスセルトークン：他のセルに認証されたセル内のリソースにアクセスする際に使用  



###### Token (トークン)認証
[一般] 認証プロセスの一つ。クライアントがリクエストするcURLのアクセストークンによる認証方法。詳細は認証 モデルを参照。


###### $top クエリ
[OData] PersoniumでサポートしているODataクエリの1つ。取得したデータの最大記録数を指定しその個数分を返す。抽出データは、セット内の最初から数えられる。（[詳細事項](401_Top_Query.html)）


##### <a name="anc_u"> U</a>
###### Unit
[Personium] Personiumサーバ内で、複数のセルから構成されるデータ領域。完全修飾ドメイン名（UnitFQDN）を持ち、絶対ドメイン名として参照される。


###### Unit制御オブジェクト
[Personium] ユニットユーザ（管理者）として、セルの作成や管理をするためのオブジェクト群。


###### Unit User
[Personium] ユニットの管理ユーザ。主にCellのCRUD（Create/Read/Update/Delete）の権限を持つ。


##### <a name="anc_v"> V</a>
##### <a name="anc_w"> W</a>
###### WebDAV
[WebDAV] Web-based Distributed Authoring and Versioningの略称。HTTPの拡張であり、Webサーバ内のドキュメントやファイルを異なるユーザで共同で執筆するためのプロトコル。  
Personiumでは、WebDAVコレクションはファイルやフォルダであり、またCRUDの機能が動作する。  
[WebDAV Resources](http://www.webdav.org/)を参照。


##### <a name="anc_x"> X</a>
##### <a name="anc_y"> Y</a>
##### <a name="anc_z"> Z</a>


##### [A](#anc_a) | [B](#anc_b) | [C](#anc_c) | [D](#anc_d) | [E](#anc_e) | [F](#anc_f) | [G](#anc_g) | [H](#anc_h) | [I](#anc_i) | [J](#anc_j) | [K](#anc_k) | [L](#anc_l) | [M](#anc_m) | [N](#anc_n) | [O](#anc_o) | [P](#anc_p) | [Q](#anc_q) | [R](#anc_r) | [S](#anc_s) | [T](#anc_t) | [U](#anc_u) | [V](#anc_v) | [W](#anc_w) | [X](#anc_x) | [Y](#anc_y) | [Z](#anc_z)
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
