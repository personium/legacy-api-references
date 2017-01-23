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
|**認証**|[OAuth2_認可エンドポイント](100_OAuth2_認可エンドポイントAPI.html)<br>[OAuth2Token_エンドポイント認証](101_OAuth2Token_エンドポイント認証API.html)<br>[ULUUT昇格設定](007_ULUUT昇格設定API.html)<br>[パスワード変更](102_パスワード変更API.html)|<br>|<br>|<br>|<br>|
|**ODataスキーマ**|<br>|<br>|EDMX取得 ($metadata)<br>AtomPubサービスドキュメント取得<br>|<br>|<br>|
|**Role**|<br>|[登録](010_Role登録API.html)|[取得](012_Role取得API.html)<br>[一覧取得](013_Role一覧取得API.html)|[更新](009_Role更新API.html)|[削除](011_Role削除API.html)|
|<br>|_$links|[登録](014_Role_$links登録API.html)|[一覧取得](016_Role_$links一覧取得API.html)|更新|[削除](015_Role_$links削除API.html)|
|<br>|_NavProp経由|[登録](018_Role_NavProp経由登録API.html)|[取得](019_Role_NavProp経由取得API.html)|<br>|<br>|
|**Account**|<br>|[登録](020_Account登録API.html)|[取得](022_Account取得API.html)<br>[一覧取得](023_Account一覧取得API.html)|[更新](024_Account更新API.html)|[削除](021_Account削除API.html)|
|<br>|_$links|[登録](025_Account_$links登録API.html)|[一覧取得](027_Account_$links一覧取得API.html)|更新|[削除](026_Account_$links削除API.html)|
|<br>|_NavProp経由|[登録](029_Account_NavProp経由登録API.html)|[取得](030_Account_NavProp経由取得API.html)|<br>|<br>|
|**Box**|<br>|[作成](065_Box作成API.html)|[取得](067_Box取得API.html)<br>[一覧取得](068_Box一覧取得API.html)<br>[URL取得](107_BoxURL取得.html)|[更新](064_Box更新API.html)|[削除](066_Box削除API.html)|
|<br>|_$links|[登録](069_Box_$links登録API.html)|[一覧取得](071_Box_$links一覧取得API.html)|更新|[削除](070_Box_$links削除API.html)|
|<br>|_NavProp経由|[登録](073_Box_NavProp経由登録API.html)|[取得](074_Box_NavProp経由取得API.html)|<br>|<br>|
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
|<br>|イベントログ設定|<br>|取得|更新|<br>|
|<br>|<br>|パス接続<br>リスナー登録|リスナー取得|<br>|リスナー削除|
|**メッセージ制御**|[送信](096_メッセージ送信API.html)|<br>|[取得](080_SentMessage取得API.html)<br>[一覧取得](081_SentMessage一覧取得API.html)|<br>|[削除](079_SentMessage削除API.html)|
|<br>|**受信**|<br>|[取得](077_ReceivedMessage取得API.html)<br>[一覧取得](078_ReceivedMessage一覧取得API.html)|[状態変更](075_メッセージ状態変更API.html)|[削除](076_ReceivedMessage削除API.html)|
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
