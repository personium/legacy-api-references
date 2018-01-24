# SentMessage取得
## 概要
送信メッセージ情報を取得する
### 必要な権限
message または message-read
### 制限事項
* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
* リクエストボディはJSON形式のみ受け付ける
* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはJSON形式とする
* $formatクエリオプションにatom または xmlを指定した場合、エラーとはならないが、レスポンスボディのデータの保証はない


## リクエスト
### リクエストURL
```
/{CellName}/__ctl/SentMessage('{MessageID}')
```
### メソッド
GET

### リクエストクエリ
以下のクエリパラメタが利用可能です。

|クエリ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|p_cookie_peer|クッキー認証値|認証時にサーバから返却されたクッキー認証値|×|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する|

[$select クエリ](406_Select_Query.md)

[$expand クエリ](405_Expand_Query.md)

[$format クエリ](404_Format_Query.md)

### リクエストヘッダ
#### 共通リクエストヘッダ
|ヘッダ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|メソッドオーバーライド機能|任意|×|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用される|
|X-Override|ヘッダオーバライド機能|${上書きするヘッダ名}:${値}|×|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定する|
|X-Personium-RequestKey|イベントログに出力するRequestKeyフィールドの値|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字|×|指定がない場合、PCS-${UNIX時間}を設定する|
#### OData共通リクエストヘッダ
|ヘッダ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|Authorization|OAuth2.0形式で、認証情報を指定する|Bearer {AccessToken}|×|※認証トークンは認証トークン取得APIで取得したトークン|
#### OData取得リクエストヘッダ
|ヘッダ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|Accept|レスポンスボディの形式を指定する|application/json|×|省略時は[application/json]として扱う|
|If-None-Match|対象ETag値を指定する|ETag値|×|未対応|
### リクエストボディ
なし

## レスポンス
### ステータスコード
200
### レスポンスヘッダ
なし
### レスポンスボディ
#### 共通レスポンスボディ
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

#### SentMessage固有レスポンスボディ
|オブジェクト|名前【キー】|型|値|
|:--|:--|:--|:--|
|{3}|type|string|CellCtl.ReceivedMessage|
|{2}|__id|string|受信メッセージID<br>UUIDで「b5d008e9092f489c8d3c574a768afc33」のような32文字の文字列を返却|
|{2}|_Box.Name|string|関係対象のボックス名|
|{2}|InReplyTo|string|受信元メッセージID<br>UUIDで「b5d008e9092f489c8d3c574a768afc33」のような32文字の文字列を返却|
|{4}|To|string|送信先CellURL|
|{2}|ToRelation|string|送信対象の関係名|
|{2}|Type|string|メッセージタイプ<br>メッセージ：message<br>関係登録依頼(リレーション)：req.relation.build<br>関係削除依頼(リレーション)：req.relation.break<br>関係登録依頼(ロール)：req.role.grant<br>関係削除依頼(ロール)：req.role.revoke|
|{2}|Title|string|メッセージタイトル|
|{2}|Body|string|メッセージ本文|
|{2}|Priority|string|優先度<br>(高)1&#65374;5(低)|
|{2}|RequestRelation|string|登録依頼するリレーションクラスURL、またはリレーション名、またはロールクラスURL、またはロール名<br>メッセージタイプがmessage以外の場合のみ|
|{2}|RequestRelationTarget|string|関係を結ぶCellURL<br>メッセージタイプがmessage以外の場合のみ|
|{2}|Result|array|送信先Cell毎の送信結果<br>オブジェクト{4}の配列|
|{4}|To|string|送信先CellURL|
|{4}|Code|string|ステータスコード|
|{4}|Reason|string|詳細メッセージ|

### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.md)を参照

### レスポンスサンプル
```JSON
{
  "d": {
    "results": {
      "__metadata": {
        "uri": "https://{UnitFQDN}/{CellName}/__ctl/SentMessage('c87b42e10df846a9bee842225d1383fe')",
        "etag": "W/\"1-1486683974323\"",
        "type": "CellCtl.SentMessage"
      },
      "__id": "c87b42e10df846a9bee842225d1383fe",
      "_Box.Name": null,
      "InReplyTo": "xnKXmd4TTZCw-bfSEw4f0AxnKXmd4TTZ",
      "To": "https://{UnitFQDN}/{CellName}",
      "ToRelation": null,
      "Type": "message",
      "Title": "メッセージサンプルタイトル",
      "Body": "メッセージサンプル本文です。",
      "Priority": 3,
      "RequestRelation": null,
      "RequestRelationTarget": null,
      "Result": [
        {
          "To": "https://{UnitFQDN}/{CellName}/",
          "Code": "201",
          "Reason": "Created."
        }
      ],
      "__published": "/Date(1486683974323)/",
      "__updated": "/Date(1486683974323)/",
      "_Box": {
        "__deferred": {
          "uri": "https://{UnitFQDN}/{CellName}/__ctl/SentMessage('c87b42e10df846a9bee842225d1383fe')
/_Box"
        }
      }
    }
  }
}
```


## cURLサンプル

```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/SentMessage('{MessageID}')" -X GET -i -H \
'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```

