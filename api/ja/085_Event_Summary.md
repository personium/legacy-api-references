﻿﻿# イベントのモデル
### 概要
[イベント受付API](086_Event_Reception.html)にて定義された外部イベント、およびpersonium.io内部で定義された内部イベントから構成される。

![イベントモデル](image/eventmodel.png "イベントモデル")


#### 外部イベント
[イベント受付API](086_Event_Reception.html) にて、受け付けるイベントを表す。
イベント受付APIによってイベントが発生し、受け付けたイベントの内容に沿って処理が実行される。
また、将来的には、他のセルに対するメッセージ通知によるイベント受付のリレー処理が可能となる予定。
イベント受付時に、発生したイベントの内容をイベントログとして出力する。
#### 内部イベント
personium.io内部で保持する管理データ（OData/WebDAV/Service等）の状態をもとに実行される処理のことを表す。
代表的な内部イベントとして、personium.io APIのリクエストがある。
personium.io APIのレスポンス返却時に実行結果をイベントログとして出力する。
### イベントのログ
#### イベントログ管理設定
ログ設定更新APIによってイベントログに対する出力設定が可能。
設定した情報を取得するには、ログ設定取得APIを使用する。
#### イベントログアクセス方法
イベントログは、WebDAV上で管理するため、イベントログファイルへのアクセスはWebDAV用のAPIで行う。
* [ログファイル一覧取得](092_Retrieve_Log_File_list.html)
* [ログファイル取得](093_Retrieve_Log_File.html)
* [ログファイル削除](094_Delete_Log_File.html)
<!-- * [ログファイル情報取得](090_ログファイル情報取得API.html) -->

#### イベントログ保管期間
イベント量に応じてイベントログファイルのサイズが増大するため、一定量にてイベントログファイルをローテートする。
ローテートされたイベントログファイルの保持世代数は、最大12世代とする。
なお、ローテート時の最大サイズは、デフォルト50MBとし、ログ設定更新APIにて設定可能とする。
#### 出力例
##### 外部イベントの出力例
```
2013-04-18 14:52:39.778 [thread-pool-1-38888(20)] [ERROR] EventLogger Req_animal-access_1001,client,https://{UnitFQDN}/appCell/,https://{UnitFQDN}/servicemanager/#admin,actionData,/svc/token_keeper,resultData
2013-04-18 14:52:40.688 [thread-pool-1-38888(17)] [INFO ] EventLogger Req_animal-access_2001,client,https://{UnitFQDN}/appCell/,https://{UnitFQDN}/servicemanager/#admin,action,/svc/token_keeper,result
2013-04-18 15:01:46.994 [thread-pool-1-38888(25)] [INFO ] EventLogger Req_animal-access_2001,client,https://{UnitFQDN}/appCell/,https://{UnitFQDN}/servicemanager/#admin,action,/svc/token_keeper,result
2013-04-18 15:06:19.294 [thread-pool-1-38888(15)] [ERROR] EventLogger Req_animal-access_1001,client,https://{UnitFQDN}/appCell/,https://{UnitFQDN}/servicemanager/#admin,actionData,/svc/token_keeper,resultData
2013-04-18 15:06:23.360 [thread-pool-1-38888(17)] [INFO ] EventLogger Req_animal-access_2001,client,https://{UnitFQDN}/appCell/,https://{UnitFQDN}/servicemanager/#admin,action,/svc/token_keeper,result
2013-04-18 15:09:18.073 [thread-pool-1-38888(10)] [ERROR] EventLogger Req_animal-access_1001,client,https://{UnitFQDN}/appCell/,https://{UnitFQDN}/servicemanager/#admin,actionData,/svc/token_keeper,resultData
```
##### 内部イベントの出力例
```
2013-04-18 14:52:39.778 [thread-pool-1-38888(20)] [ERROR] EventLogger Req_animal-access_1001,server,https://{UnitFQDN}/appCell/,https://{UnitFQDN}/appCell/#staff,POST,/homeClinic/__auth,200
2013-04-18 14:52:39.778 [thread-pool-1-38888(20)] [ERROR] EventLogger Req_animal-access_1001,server,https://{UnitFQDN}/appCell/,https://{UnitFQDN}/appCell/#staff,PROPFIND,/homeClinic/box/col/put_blog,207
2013-04-18 14:52:39.779 [thread-pool-1-38888(20)] [ERROR] EventLogger Req_animal-access_1001,server,https://{UnitFQDN}/appCell/,https://{UnitFQDN}/appCell/#staff,PUT,/homeClinic/box/col/put_blog,204
2013-04-18 14:52:39.780 [thread-pool-1-38888(20)] [ERROR] EventLogger Req_animal-access_1001,server,https://{UnitFQDN}/appCell/,https://{UnitFQDN}/appCell/#staff,GET,/homeClinic/box/col/blog_20130418,200
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
