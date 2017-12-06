# イベント受付
### 概要
リクエストされたイベントを受付ける
### 必要な権限
event
### Supported Versions
1.1.7
### 制限事項
なし

#### イベントAPI共通
* ログの出力設定は未サポート
  - ログファイル名は"default.log"固定とする
  - ローテートは以下のデフォルト設定に従い行う
    - ローテート数:12世代
    - ローテートのサイズ設定:50MB
    - ローテート後の圧縮方式:zip<br>ログの出力レベルは"info"固定（INFO, WARN, ERRORすべて出力）とする
* ログ出力設定の参照は未サポート


### リクエスト
#### リクエストURL
```
/{CellName}/__event
```

#### メソッド
POST
#### リクエストクエリ
|クエリ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|p_cookie_peer|クッキー認証値|認証時にサーバから返却されたクッキー認証値|×|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する|
#### リクエストヘッダ
|ヘッダ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|メソッドオーバーライド機能|任意|×|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用されます。|
|X-Override|ヘッダオーバライド機能|${上書きするヘッダ名}:${値}|×|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定します。|
|X-Personium-RequestKey|イベントログに出力するRequestKeyフィールドの値|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字|×|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応|
#### リクエストボディ
JSON

|項目名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|level|ログ出力レベル|"INFO"/"WARN"/"ERROR"のいずれか|○|小文字の"info"/"warn"/"error"が指定された場合は、大文字として扱う。<br>ただし、小文字での指定は非推奨（deprecated）。|
|action|イベントのアクション|文字列型<br>桁数：0&#65374;51200 byte|○||
|object|イベントの対象オブジェクト|文字列型<br>桁数：0&#65374;51200 byte|○||
|result|イベントの結果|文字列型<br>桁数：0&#65374;51200 byte|○||
#### リクエストサンプル
```JSON
{
  "level":"INFO",
  "action":"authSchema",
  "object":"/{CellName}/{BoxName}/service_name/token_keeper",
  "result":"[XXXX2033] Success schema authorization. cellUrl=https://{UnitFQDN}/keeper-d4a57bb26eae481486b07d06487051d1/"
}
```


### レスポンス
#### ステータスコード
|コード|メッセージ|概要|
|:--|:--|:--|
|200|OK|受付成功時|
#### レスポンスヘッダ
|ヘッダ名|概要|備考|
|:--|:--|:--|
|Access-Control-Allow-Origin|クロスドメイン通信許可ヘッダ|返却値は"*"固定|
|X-Personium-Version|APIの実行バージョン|リクエストが処理されたAPIバージョン|
#### レスポンスボディ
なし
#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

### cURLサンプル
```sh
curl "https://{UnitFQDN}/{CellName}/__event" -X POST -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"level":"INFO", "action":"authSchema", "object":"/{CellName}/{BoxName}/service_name/token_keeper", "result":"[XXXX2033] Success schema authorization. cellUrl=https://{UnitFQDN}/keeper-d4a57bb26eae481486b07d06487051d1/"}'
```

###### Copyright 2017 FUJITSU LIMITED
