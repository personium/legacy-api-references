# メッセージ送信
### 概要
メッセージを送信する

### 必要な権限
message

### 制限事項
* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
* リクエストボディはjson形式のみ受け付ける
* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはjson形式とする
* $formatクエリオプションにatom または xmlを指定した場合、エラーとはならないが、レスポンスボディのデータの保証はない

<br>
### リクエスト
#### リクエストURL
```
/{CellName}/__message/send
```
#### メソッド
POST

#### リクエストクエリ
クエリは無視する

#### リクエストヘッダ

|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用されます。<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定します。<br>|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {UnitUserToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
|Content-Type<br>|リクエストボディの形式を指定する<br>|application / json<br>|×<br>|省略時は[application/json]として扱う<br>|
|Accept<br>|レスポンスボディの形式を指定する<br>|application / json<br>|×<br>|省略時は[application/json]として扱う<br>|
#### リクエストボディ
JSON

|項目名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|BoxBound<br>|Boxと紐付けるか否か<br>|true / false<br>デフォルト値はflase<br>|×<br>|Boxに結びつける場合に本項目を「true」にしてスキーマ認証したトークンを送る(未実装)<br>|
|InReplyTo<br>|返信対象のメッセージID<br>|桁数：32<br>null<br>|×<br>|<br>|
|To<br>|送信先セルURL<br>|URL形式<br>null<br>|※ 1<br>|複数Cellに送信する場合はCSV形式で指定する<br>※1 ToまたはRelationのどちらかは必須,<br>ToまたはRelationで指定できる送信先セルURLの最大件数は1000件<br>|
|ToRelation<br>|送信対象の関係名<br>|桁数：1&#65374;128<br>文字種:半角英数字と-(半角ハイフン)と_(半角アンダーバー)と+(プラス)と:(コロン)<br>ただし、先頭文字に_(半角アンダーバー)と:(コロン)は指定不可<br>null<br>|※ 1<br>|※1 ToまたはRelationのどちらかは必須<br>ToまたはRelationで指定できる送信先セルURLの最大件数は1000件<br>|
|Type<br>|メッセージタイプ<br>|message<br>Req.Relation.Build<br>Req.Relation.Break<br>|×<br>|省略時はmessageとして扱う<br>|
|Title<br>|メッセージタイトル<br>|桁数：256文字以下<br>|×<br>|省略時は空文字として扱う<br>|
|Body<br>|メッセージ本文<br>|桁数：64Kbyte以下<br>|×<br>|省略時は空文字として扱う<br>|
|Priority<br>|優先度<br>|1~5<br>|×<br>|省略時は3として扱う<br>|
|RequestRelation<br>|登録依頼した関係情報<br>|URL形式<br>null<br>|※ 2<br>|※2 メッセージタイプが関係登録/削除依頼の場合のみ必須<br>登録依頼するリレーションクラスURL、またはリレーションインスタンスURL<br>|
|RequestRelationTarget<br>|関係を結ぶセルURL<br>|URL形式<br>null<br>|※ 2<br>|※2 メッセージタイプが関係登録/削除依頼の場合のみ必須<br>|

#### リクエストサンプル
```json
{
  "BoxBound": true,
  "InReplyTo": "hnKXm44TTZCw-bfSEw4f0A",
  "To": "https://{UnitFQDN}/target{CellName}",
  "ToRelation": null,
  "Type": "req.relation.build",
  "Title": "友人登録依頼です",
  "Body": "先日はありがとうごさいました。友人登録承認をお願いいたします。",
  "Priority": 3,
  "RequestRelation": "https://{UnitFQDN}/appcell/__relation/__/+:Friend",
  "RequestRelationTarget": "https://{UnitFQDN}/{CellName}"
}
```
<br>
### レスポンス
#### ステータスコード
201

#### レスポンスヘッダ

|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|Content-Type<br>|返却されるデータの形式<br>|<br>|
|Location<br>|作成したリソースへのURL<br>|<br>|
|DataServiceVersion<br>|ODataのバージョン<br>|<br>|
|ETag<br>|リソースのバージョン情報<br>|<br>|
|Access-Control-Allow-Origin<br>|クロスドメイン通信許可ヘッダ<br>|返却値は"*"固定<br>|
|X-Personium-Version<br>|APIの実行バージョン<br>|リクエストが処理されたAPIバージョン<br>|
#### レスポンスボディ

##### 共通レスポンスボディ
レスポンスはJSONオブジェクトで、オブジェクト（サブオブジェクト）に定義されるキー(名前)と型、並びに値の対応は以下のとおりです。

|オブジェクト<br>|名前【キー）<br>|型<br>|値<br>|
|:--|:--|:--|:--|
|ルート<br>|d<br>|object<br>|オブジェクト{1}<br>|
|{1}<br>|__count<br>|string<br>|$inlinecountクエリでの取得結果件数<br>|
|{1}<br>|results<br>|array<br>|オブジェクト{2}の配列<br>|
|{2}<br>|__metadata<br>|object<br>|オブジェクト{3}<br>|
|{3}<br>|uri<br>|string<br>|作成したリソースへのURL<br>|
|{3}<br>|etag<br>|string<br>|Etag値<br>|
|{2}<br>|__published<br>|string<br>|作成日(UNIX時間)<br>|
|{2}<br>|__updated<br>|string<br>|更新日(UNIX時間)<br>|
##### SentMessage固有レスポンスボディ
|オブジェクト<br>|名前【キー】<br>|型<br>|値<br>|
|:--|:--|:--|:--|
|{3}<br>|type<br>|string<br>|CellCtl.ReceivedMessage<br>|
|{2}<br>|__id<br>|string<br>|受信メッセージID<br>UUIDで「b5d008e9092f489c8d3c574a768afc33」のような32文字の文字列を返却<br>|
|{2}<br>|InReplyTo<br>|string<br>|受信元メッセージID<br>UUIDで「b5d008e9092f489c8d3c574a768afc33」のような32文字の文字列を返却<br>|
|{2}<br>|To<br>|string<br>|送信先CellURL<br>|
|{2}<br>|ToRelation<br>|string<br>|送信対象の関係名<br>|
|{2}<br>|Type<br>|string<br>|メッセージタイプ<br>メッセージ：message<br>関係登録依頼：req.relation.build<br>関係削除依頼：req.relation.break<br>|
|{2}<br>|Title<br>|string<br>|メッセージタイトル<br>|
|{2}<br>|Body<br>|string<br>|メッセージ本文<br>|
|{2}<br>|Priority<br>|string<br>|優先度<br>(高)1&#65374;5(低)<br>|
|{2}<br>|RequestRelation<br>|string<br>|登録依頼するリレーションクラスURL、またはリレーションインスタンスURL<br>メッセージタイプが関係登録/削除依頼の場合のみ<br>|
|{2}<br>|RequestRelationTarget<br>|string<br>|関係を結ぶCellURL<br>メッセージタイプが関係登録/削除依頼の場合のみ<br>|
|{2}<br>|_Box.Name<br>|string<br>|関係対象のボックス名<br>|
|{2}<br>|Result<br>|array<br>|送信先Cell毎の送信結果<br>オブジェクト{4}の配列<br>|
|{4}<br>|To<br>|string<br>|送信先CellURL<br>|
|{4}<br>|Code<br>|string<br>|ステータスコード<br>|
|{4}<br>|Reason<br>|string<br>|詳細メッセージ<br>|
#### エラーメッセージ一覧
[エラーメッセージ一覧](200_Error_Messages.html)を参照

#### レスポンスサンプル
```json
{
  "d": {
    "results": {
      "__metadata": {
        "uri": "https://{UnitFQDN}/{CellName}/__ctl/SentMessage('3afcc60e35fc49ee9a4e4f6c1ebee426')",
        "etag": "W/\"1-1486638759524\"",
        "type": "CellCtl.SentMessage"
      },
      "__id": "3afcc60e35fc49ee9a4e4f6c1ebee426",
      "InReplyTo": "xnKXmd4TTZCw-bfSEw4f0AxnKXmd4TTZ",
      "To": "https://{UnitFQDN}/{CellName}",
      "ToRelation": null,
      "Type": "message",
      "Title": "メッセージサンプルタイトル",
      "Body": "メッセージサンプル本文です。",
      "Priority": 3,
      "RequestRelation": null,
      "RequestRelationTarget": null,
      "_Box.Name": null,
      "Result": [
        {
          "To": "https://{UnitFQDN}/{CellName}/",
          "Code": "201",
          "Reason": "Created."
        }
      ],
      "__published": "/Date(1486638759524)/",
      "__updated": "/Date(1486638759524)/"
    }
  }
}
```
<br>
### CURLサンプル
```sh
curl "https://{UnitFQDN}/{CellName}/__message/send" -X POST -i -H 'Authorization: Bearer {UnitUserToken}' -H 'Accept: application/json' -d '{"BoxBound":false,"InReplyTo":"xnKXmd4TTZCw-bfSEw4f0AxnKXmd4TTZ","To":"https://{UnitFQDN}/{CellName}","Type":"message","Title":"メッセージサンプルタイトル","Body":"メッセージサンプル本文です。",
"Priority":3}'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
