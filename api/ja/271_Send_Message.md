# メッセージ送信
### 概要
メッセージを送信する

### 必要な権限
message

### 制限事項
* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
* リクエストボディはJSON形式のみ受け付ける
* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはJSON形式とする
* $formatクエリオプションにatom または xmlを指定した場合、エラーとはならないが、レスポンスボディのデータの保証はない


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

|ヘッダ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|メソッドオーバーライド機能|任意|×|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用されます。|
|X-Override|ヘッダオーバライド機能|${上書きするヘッダ名}:${値}|×|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定します。|
|Authorization|OAuth2.0形式で、認証情報を指定する|Bearer {AccessToken}|×|※認証トークンは認証トークン取得APIで取得したトークン|
|Content-Type|リクエストボディの形式を指定する|application/json|×|省略時は[application/json]として扱う|
|Accept|レスポンスボディの形式を指定する|application/json|×|省略時は[application/json]として扱う|
#### リクエストボディ
JSON

|項目名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|BoxBound|Boxと紐付けるか否か|true / false<br>デフォルト値はfalse|×|Boxに結びつける場合に本項目を「true」にしてスキーマ認証したトークンを送る|
|InReplyTo|返信対象のメッセージID|桁数：32<br>null|×||
|To|送信先セルURL|URL形式<br>null|※ 1|複数Cellに送信する場合はCSV形式で指定する<br>※1 ToまたはRelationのどちらかは必須,<br>ToまたはRelationで指定できる送信先セルURLの最大件数は1000件|
|ToRelation|送信対象の関係名|桁数：1&#65374;128<br>文字種:半角英数字と-(半角ハイフン)と_(半角アンダーバー)と+(プラス)と:(コロン)<br>ただし、先頭文字に_(半角アンダーバー)と:(コロン)は指定不可<br>null|※ 1|※1 ToまたはRelationのどちらかは必須<br>ToまたはRelationで指定できる送信先セルURLの最大件数は1000件|
|Type|メッセージタイプ|message<br>req.relation.build<br>req.relation.break<br>req.role.grant<br>req.role.revoke|×|省略時はmessageとして扱う|
|Title|メッセージタイトル|桁数：256文字以下|×|省略時は空文字として扱う|
|Body|メッセージ本文|桁数：64Kbyte以下|×|省略時は空文字として扱う|
|Priority|優先度|1~5|×|省略時は3として扱う|
|RequestRelation|登録依頼した関係情報|URL形式<br>null|※ 2|※2 メッセージタイプがmessage以外の場合必須<br>登録依頼するリレーションクラスURL、またはリレーション名、またはロールクラスURL、またはロール名を指定<br>リレーション名指定時は以下のURLからの相対URLとみなす<br>BoxBoundがtrue：[対象BoxスキーマURL]\_\_relation/\_\_/<br>BoxBoundがfalse：[送信先セルURL]\_\_relation/\_\_/<br>ロール名指定時は以下のURLからの相対URLとみなす<br>BoxBoundがtrue：[対象BoxスキーマURL]\_\_role/\_\_/<br>BoxBoundがfalse：[送信先セルURL]\_\_role/\_\_/|
|RequestRelationTarget|関係を結ぶセルURL|URL形式<br>null|※ 2|※2 メッセージタイプがmessage以外の場合必須|

#### リクエストサンプル
```JSON
{
  "BoxBound": true,
  "InReplyTo": "hnKXm44TTZCw-bfSEw4f0A",
  "To": "https://{UnitFQDN}/{TargetCellName}",
  "ToRelation": null,
  "Type": "req.relation.build",
  "Title": "友人登録依頼です",
  "Body": "先日はありがとうごさいました。友人登録承認をお願いいたします。",
  "Priority": 3,
  "RequestRelation": "https://{UnitFQDN}/{AppCellName}/__relation/__/{RelationName}",
  "RequestRelationTarget": "https://{UnitFQDN}/{CellName}"
}
```

### レスポンス
#### ステータスコード
201

#### レスポンスヘッダ

|ヘッダ名|概要|備考|
|:--|:--|:--|
|Content-Type|返却されるデータの形式||
|Location|作成したリソースへのURL||
|DataServiceVersion|ODataのバージョン||
|ETag|リソースのバージョン情報||
|Access-Control-Allow-Origin|クロスドメイン通信許可ヘッダ|返却値は"*"固定|
|X-Personium-Version|APIの実行バージョン|リクエストが処理されたAPIバージョン|
#### レスポンスボディ

##### 共通レスポンスボディ
レスポンスはJSONオブジェクトで、オブジェクト（サブオブジェクト）に定義されるキー(名前)と型、並びに値の対応は以下のとおりです。

|オブジェクト|名前【キー）|型|値|
|:--|:--|:--|:--|
|ルート|d|object|オブジェクト{1}|
|{1}|results|array|オブジェクト{2}の配列|
|{2}|__metadata|object|オブジェクト{3}|
|{3}|uri|string|作成したリソースへのURL|
|{3}|etag|string|Etag値|
|{2}|__published|string|作成日(UNIX時間)|
|{2}|__updated|string|更新日(UNIX時間)|
|{1}|__count|string|$inlinecountクエリでの取得結果件数|

##### SentMessage固有レスポンスボディ
|オブジェクト|名前【キー】|型|値|
|:--|:--|:--|:--|
|{3}|type|string|CellCtl.ReceivedMessage|
|{2}|__id|string|受信メッセージID<br>UUIDで「b5d008e9092f489c8d3c574a768afc33」のような32文字の文字列を返却|
|{2}|InReplyTo|string|受信元メッセージID<br>UUIDで「b5d008e9092f489c8d3c574a768afc33」のような32文字の文字列を返却|
|{2}|To|string|送信先CellURL|
|{2}|ToRelation|string|送信対象の関係名|
|{2}|Type|string|メッセージタイプ<br>メッセージ：message<br>関係登録依頼(リレーション)：req.relation.build<br>関係削除依頼(リレーション)：req.relation.break<br>関係登録依頼(ロール)：req.role.grant<br>関係削除依頼(ロール)：req.role.revoke|
|{2}|Title|string|メッセージタイトル|
|{2}|Body|string|メッセージ本文|
|{2}|Priority|string|優先度<br>(高)1&#65374;5(低)|
|{2}|RequestRelation|string|登録依頼するリレーションクラスURL、またはリレーション名、またはロールクラスURL、またはロール名<br>メッセージタイプがmessage以外の場合のみ|
|{2}|RequestRelationTarget|string|関係を結ぶCellURL<br>メッセージタイプがmessage以外の場合のみ|
|{2}|_Box.Name|string|関係対象のボックス名|
|{2}|Result|array|送信先Cell毎の送信結果<br>オブジェクト{4}の配列|
|{4}|To|string|送信先CellURL|
|{4}|Code|string|ステータスコード|
|{4}|Reason|string|詳細メッセージ|
#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

#### レスポンスサンプル
```JSON
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

### cURLサンプル
```sh
curl "https://{UnitFQDN}/{CellName}/__message/send" -X POST -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"BoxBound":false,"InReplyTo":"xnKXmd4TTZCw-bfSEw4f0AxnKXmd4TTZ","To":"https://{UnitFQDN}/{CellName}","Type":"message","Title":"メッセージサンプルタイトル","Body":"メッセージサンプル本文です。","Priority":3}'
```

###### Copyright 2017 FUJITSU LIMITED
