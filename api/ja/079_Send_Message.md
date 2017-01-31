﻿﻿# メッセージ送信
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

|Header name<br>|Summary<br>|Valid value<br>|Required<br>|Remarks<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用されます。<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定します。<br>|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {UnitUserToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
|Content-Type<br>|リクエストボディの形式を指定する<br>|application / json<br>|×<br>|省略時は[application/json]として扱う<br>|
|Accept<br>|レスポンスボディの形式を指定する<br>|application / json<br>|×<br>|省略時は[application/json]として扱う<br>|
#### リクエストボディ
JSON

|Item name<br>|Summary<br>|Valid value<br>|Required<br>|Remarks<br>|
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

|Header name<br>|Summary<br>|Remarks<br>|
|:--|:--|:--|
|X-Dc-Version<br>|APIの実行バージョン<br>|リクエストが処理されたAPIバージョン<br>|
|Access-Control-Allow-Origin<br>|クロスドメイン通信許可ヘッダ<br>|返却値は"*"固定<br>|
|Content-Type<br>|返却されるデータの形式<br>|<br>|
|Location<br>|作成したリソースへのURL<br>|<br>|
|ETag<br>|リソースのバージョン情報<br>|<br>|
|DataServiceVersion<br>|ODataのバージョン<br>|<br>|
#### レスポンスボディ

|Item name<br>|Summary<br>|Remarks<br>|
|:--|:--|:--|
|d<br>|<br>|<br>|
|d / results<br>|<br>|<br>|
|d / results / __published<br>|作成日<br>|<br>|
|d / results / __updated<br>|更新日<br>|<br>|
|d / results / __metadata<br>|<br>|<br>|
|d / results / __metadata / etag<br>|ETag値<br>|<br>|
|d / results / __metadata / uri<br>|作成したリソースへのURL<br>|<br>|
|d / results / __metadata / type<br>|SentMessage<br>|<br>|
|d / results / _Box.Name<br>|関係対象のボックス名<br>|<br>|
|d / results / InReplyTo<br>|返信対象のメッセージID<br>|UUIDで「hnKXm44TTZCw-bfSEw4f0A」のような22文字の文字列を返却<br>|
|d / results / To<br>|送信先セルURL<br>|複数Cellに送信した場合はCSV形式となる<br>|
|d / results / ToRelation<br>|送信対象の関係名<br>|<br>|
|d / results / Type<br>|メッセージタイプ<br>|メッセージ　：　message<br>関係登録依頼　：　req.relation.build<br>関係削除依頼　：　req.relation.break<br>|
|d / results / Title<br>|メッセージタイトル<br>|<br>|
|d / results / Body<br>|メッセージ本文<br>|<br>|
|d / results / Priority<br>|優先度<br>|(高)1&#65374;5(低)<br>|
|d / results / RequestRelation<br>|登録依頼するリレーションクラスURL、またはリレーションインスタンスURL<br>|メッセージタイプが関係登録/削除依頼の場合のみ<br>|
|d / results / RequestRelationTarget<br>|関係を結ぶCellURL<br>|メッセージタイプが関係登録/削除依頼の場合のみ<br>|
|d / results / Result<br>|送信先Cell毎の送信結果<br>|<br>|
|d / results / Result / To<br>|送信先CellURL<br>|<br>|
|d / results / Result / Code<br>|ステータスコード<br>|<br>|
|d / results / Result / Reason<br>|詳細メッセージ<br>|<br>|
#### エラーメッセージ一覧
[エラーメッセージ一覧](200_Error_Messages.html)を参照

#### レスポンスサンプル
```json
{
  "d": {
    "results": {
      "__id": "hnKXm44TTZCw-bfSEw4f0A",
      "__published": "/Date(1349435294656)/",
      "__updated": "/Date(1349435294656)/",
      "_Box.Name ": "{BoxName}",
      "__metadata": {
        "etag": "1-1349435294656",
        "type": "CellCtl.SentMessage",
        "uri": "https://{UnitFQDN}/{CellName}/__ctl/SentMessage('hnKXm44TTZCw-bfSEw4f0A')"
      },
      "InReplyTo": "xnKXmd4TTZCw-bfSEw4f0A",
      "To": "https://{UnitFQDN}/targetcel_lname",
      "ToRelation": "",
      "Type": "req.relation.build",
      "Title": "友人登録依頼です",
      "Body": "先日はありがとうごさいました。友人登録承認をお願いいたします。",
      "Priority": 3,
      "RequestRelation": "https://{UnitFQDN}/appcell/__relation/__/+:Friend",
      "RequestRelationTarget": "https://{UnitFQDN}/{CellName}",
      "Result": [
        {
          "To": "https://{UnitFQDN}/{CellName}-sample1",
          "Code": "201",
          "Reason": "Created."
        },
        {
          "To": "https://{UnitFQDN}/{CellName}-sample2",
          "Code": "404",
          "Reason": "Cell not found."
        },
        {
          "To": "https://{UnitFQDN}/{CellName}-sample3",
          "Code": "201",
          "Reason": "Created."
        }
      ]
    }
  }
}
```
<br>
### CURLサンプル
```sh
curl "https://{UnitFQDN}/{CellName}/__message/send" -X POST -i -H 'Authorization: Bearer {UnitUserToken}' -H 'Accept: application/json' -d '{"BoxBound":true,"InReplyTo":"xnKXmd4TTZCw-bfSEw4f0A","To":"https://{UnitFQDN}/target{CellName}", "Relation":"","Type":"req.relation.build","Title":"友人登録依頼です","Body":"先日はありがとうごさいました。友人登録承認をお願いいたします。",
"Priority":3,"RequestRelation":"https://{UnitFQDN}/appcell/__relation/__/+:Friend","RequestRelationTarget":"https://{UnitFQDN}/{CellName}"}'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
