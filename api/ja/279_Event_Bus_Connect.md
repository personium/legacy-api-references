# イベントバスへの Web Socket 接続

## 概要

Cellで発生するイベントが通るイベントバスにWebSocketで接続して、発生イベント情報を受け取ります。
本APIは最初にアクセストークンの送信を要求します。該当トークンがイベントバス接続可能な権限をもつときのみ接続を許可します。
次に購読条件を指定します。これにより条件に合致するイベントのみがリアルタイムにクライアントに送信されます。

### 必要な権限

 event-read


## 接続からセッション開始まで

### エンドポイントURL

    wss:{UnitFQDN}/{CellName}/__event

まず上記URLにWebSocketでの接続を行ってください。アクセストークンを受け付ける状態となります。この状態ではアクセストークン送付以外のいかなるメッセージ送信も意味を持ちません。

### アクセストークン送付

#### リクエスト

権限のあるアクセストークンを以下形式で送付することでイベントバス接続セッションを開始します。  

    {"access_token":"AA~91WT0GNoVGFHJFQ.......e"}

#### レスポンス

有効なトークンであれば以下の応答が返り、セッション開始となります。セッション開始によりイベント購読可能状態となりますが、まだイベント購読はしていないため何もイベントは流れてきません。

    {"response":"success","expires_in":3600,"timestamp":1518612600}

トークンが無効であったり、トークンに必要な権限がない場合はCellはWebSocket接続を切断します。


## セッション開始後の通信

購読条件を送信してイベントを購読することで、購読条件に合致するイベントの発生時にリアルタイムでイベント情報が流れてくるようになります。条件を変えながら任意の数の購読を行うことができます。

### Eventの購読

#### リクエスト

    {"subscribe": {"Type": "${EventType}", "Object": "${EventObject}"}}

#### レスポンス

    {"response":"success","timestamp":1518612600}


### Eventの購読解除

#### リクエスト

    {"unsubscribe": {"Type": "${EventType}", "Object": "${EventObject}"}}

#### レスポンス

    {"response":"success","timestamp":1518612600}


### イベントの受け取り

何らかの購読設定がなされている状態で購読購読条件にに合致するイベントがCellで発生すると、以下形式のメッセージがクライアントに送られます。  

    {
      "Type":"chat", 
      "RequestKey":null,
      "Schema":"",
      "External":true,
      "Object":"general",
      "Info":"Hello World", 
      "cellId":"5ZKpfNSMSwO6GqAgt0O2Jg", 
      "Subject":"https:\/\/demo.personium.io\/john.doe\/#me"
    }


## WebSocket詳細仕様

|項目|仕様|
|:--|:--|
|プロトコル|Sec-WebSocket-Version 13(RFC 6455)|
|メッセージフォーマット|JSON|
|ping/pong|アクセストークンメッセージ受領後毎分1回pingをCellから送信|

* クライアント側で10回PONGの返信がないとセッションを切断する
* ブラウザの場合は自動でPONGが返信されるため対応不要
