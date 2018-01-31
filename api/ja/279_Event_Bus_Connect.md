# イベントバス接続(Not Implemented Yet)

## 概要

WebSocketでCellで発生するイベントが通るイベントバスに接続して、発生イベント情報を受け取ります。
本APIは最初にアクセストークンの送信を要求します。該当トークンがイベントバス接続可能な権限をもつときのみ接続を許可します。
次に購読条件を指定します。これにより条件に合致するイベントのみがリアルタイムにクライアントに送信されます。

### 必要な権限

 event-read


## 接続

### エンドポイントURL

    wss:{UnitFQDN}/{CellName}/__event


|項目|仕様|
|:--|:--|
|プロトコル|Sec-WebSocket-Version 13(RFC 6455)|
|メッセージフォーマット|JSON|
|ping/pong|アクセストークンメッセージ受領後毎分1回pingをCellから送信|

* クライアント側で10回PONGの返信がないとセッションを切断する
* ブラウザの場合は自動でPONGが返信されるため対応不要


## リクエスト

### 接続開始

    {"access_token" : "abcd8932feae"}

#### 成功時

#### 失敗時

接続が切られます。

### Eventの購読

    {subscribe: {Type:  type, Object: path}}


### Eventの購読解除

    {unsubscribe: {Type:  type, Object: path}}


## レスポンス

購読条件に合致するイベントの発生時にリアルタイムでイベント情報を応答します。

