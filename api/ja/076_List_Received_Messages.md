# ReceivedMessage一覧取得
### 概要
受信メッセージ情報を取得する
### 必要な権限
message または message-read
### 制限事項
* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
* リクエストボディはjson形式のみ受け付ける
* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはjson形式とする
* $formatクエリオプションにatom または xmlを指定した場合、エラーとはならないが、レスポンスボディのデータの保証はない

<br>
### リクエスト
#### リクエストURL
```
/{CellName}/__ctl/ReceivedMessage
```
#### メソッド
GET
#### リクエストクエリ
以下のクエリパラメタが利用可能です。

|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|dc_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|

[$select クエリ](193_$Select_Query.html)

[$expand クエリ](192_$Expand_Query.html)

[$format クエリ](191_$Format_Query.html)

[$filter クエリ](190_$Filter_Query.html)

[$inlinecount クエリ](194_$Inlinecount_Query.html)

[$orderby クエリ](187_$Orderby_Query.html)

[$top クエリ](188_$Top_Query.html)

[$skip クエリ](189_$Skip_Query.html)

[全文検索(q)クエリ](195_Full_Text_Search_Query.html)

#### リクエストヘッダ
##### 共通リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用される<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定する<br>|
|X-Dc-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
##### OData共通リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {UnitUserToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
##### OData取得リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Accept<br>|レスポンスボディの形式を指定する<br>|application/json<br>|×<br>|省略時は[application/json]として扱う<br>|
|If-None-Match<br>|対象ETag値を指定する<br>|ETag値<br>|○<br>|未対応<br>|
#### リクエストボディ
なし
#### リクエストサンプル
なし

<br>
### レスポンス
#### ステータスコード
200
#### レスポンスヘッダ
なし
#### レスポンスボディ
##### 共通レスポンスボディ
レスポンスはJSONオブジェクトで、オブジェクト（サブオブジェクト）に定義されるキー(名前)と型、並びに値の対応は以下のとおりです。

|オブジェクト<br>|名前【キー）<br>|型<br>|値<br>|
|:--|:--|:--|:--|
|ルート<br>|d<br>|object<br>|オブジェクト{1}<br>|
|{1}<br>|__count<br>|string<br>|$inlinecountクエリでの取得結果件数<br>|
|{1}<br>|results<br>|array<br>|オブジェクト{2}の配列<br>|
|{2}<br>|__published<br>|string<br>|作成日(UNIX時間)<br>|
|{2}<br>|__updated<br>|string<br>|更新日(UNIX時間)<br>|
|{2}<br>|__metadata<br>|object<br>|オブジェクト{3}<br>|
|{3}<br>|etag<br>|string<br>|Etag値<br>|
|{3}<br>|uri<br>|string<br>|作成したリソースへのURL<br>|
##### ReceivedMessage固有レスポンスボディ
|オブジェクト<br>|名前（キー）<br>|型<br>|値<br>|
|:--|:--|:--|:--|
|{3}<br>|type<br>|string<br>|CellCtl.ReceivedMessage<br>|
|{2}<br>|__id<br>|string<br>|受信メッセージID<br>UUIDで「b5d008e9092f489c8d3c574a768afc33」のような32文字の文字列を返却<br>|
|{2}<br>|_Box.Name<br>|string<br>|関係対象のボックス名<br>|
|{2}<br>|InReplyTo<br>|string<br>|受信元メッセージID<br>UUIDで「b5d008e9092f489c8d3c574a768afc33」のような32文字の文字列を返却<br>|
|{2}<br>|From<br>|string<br>|受信元セルURL<br>|
|{2}<br>|MulticastTo<br>|string<br>|受信先セルURL<br>複数セルが送信先の場合にCSV形式でセルのURLを返却<br>|
|{2}<br>|Type<br>|string<br>|メッセージタイプ<br>メッセージ：message<br>関係登録依頼：req.relation.build<br>関係削除依頼：req.relation.break<br>|
|{2}<br>|Title<br>|string<br>|メッセージタイトル<br>|
|{2}<br>|Body<br>|string<br>|メッセージ本文<br>|
|{2}<br>|Priority<br>|string<br>|優先度<br>(高)1&#65374;5(低)<br>|
|{2}<br>|Status<br>|string<br>|メッセージステータス<br>Typeがmessageの場合<br>　read：既読<br>　unread：未読<br>Typeがreq.relation.build/req.relation.breakの場合<br>　approved：承認<br>　rejected：拒否<br>　none：未決<br>|
|{2}<br>|RequestRelation<br>|string<br>|登録依頼するリレーションクラスURL、またはリレーションインスタンスURL<br>メッセージタイプが関係登録/削除依頼の場合のみ<br>|
|{2}<br>|RequestRelationTarget<br>|string<br>|関係を結ぶCellURL<br>メッセージタイプが関係登録/削除依頼の場合のみ<br>|
#### エラーメッセージ一覧
[エラーメッセージ一覧](199_Error_Messages.html)を参照

#### レスポンスサンプル
```json
{
  "d": {
    "results": [
      {
        "__id": "hnKXm44TTZCw-bfSEw4f0A",
        "__published": "/Date(1349435294656)/",
        "__updated": "/Date(1349435294656)/",
        "_Box.Name ": "{BoxName}",
        "__metadata": {
          "etag": "1-1349435294656",
          "type": "CellCtl.ReceivedMessage",
          "uri": "https://{UnitFQDN}/{CellName}/__ctl/ReceivedMessage('hnKXm44TTZCw-bfSEw4f0A')"
        },
        "InReplyTo": "xnKXmd4TTZCw-bfSEw4f0A",
        "From": "https://{UnitFQDN}/my{CellName}",
        "MulticastTo": "",
        "Type": "req.relation.build",
        "Title": "友人登録依頼です",
        "Body": "先日はありがとうごさいました。友人登録承認をお願いいたします。",
        "Priority": 3,
        "Status": "none",
        "RequestRelation": "https://{UnitFQDN}/appcell/__relation/__/+:Friend",
        "RequestRelationTarget": "https://{UnitFQDN}/{CellName}"  
      },
      {
        "__id": "HnKXm44abZCw-bfSEw4fyz",
        "__published": "/Date(1349435294123)/",
        "__updated": "/Date(1349435294123)/",
        "_Box.Name ": "{BoxName}",
        "__metadata": {
          "etag": "1-1349435294656",
          "type": "CellCtl.ReceivedMessage",
          "uri": "https://{UnitFQDN}/{CellName}/__ctl/ReceivedMessage('HnKXm44abZCw-bfSEw4fyz')"
        },
        "InReplyTo": "xnKXmd4TTZCw-bfSEw4f0A",
        "From": "https://{UnitFQDN}/my{CellName}",
        "MulticastTo": "",
        "Type": "message",
        "Title": "御礼",
        "Body": "友人承認していただきありがとうごさいます。",
        "Priority": 3,
        "Status": "unread",
        "RequestRelation": null,
        "RequestRelationTarget": null
      }
    ]
  }
}
```

<br>
### CURLサンプル
#### CURLコマンド(UNIX)
```sh
curl "https://{UnitFQDN}/__ctl/ReceivedMessage" -X GET -i -H 'Authorization: Bearer {UnitUserToken}' -H 'Accept: application/json'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
