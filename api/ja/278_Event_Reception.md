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

<br>
### リクエスト
#### リクエストURL
```
/{CellName}/__event
```

#### メソッド
POST
#### リクエストクエリ
|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|p_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|
#### リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用されます。<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定します。<br>|
|X-Personium-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
#### リクエストボディ
JSON

|項目名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|level<br>|ログ出力レベル<br>|"INFO"/"WARN"/"ERROR"のいずれか<br>|○<br>|小文字の"info"/"warn"/"error"が指定された場合は、大文字として扱う。<br>ただし、小文字での指定は非推奨（deprecated）。<br>|
|action<br>|イベントのアクション<br>|文字列型<br>桁数：0&#65374;51200 byte<br>|○<br>|<br>|
|object<br>|イベントの対象オブジェクト<br>|文字列型<br>桁数：0&#65374;51200 byte<br>|○<br>|<br>|
|result<br>|イベントの結果<br>|文字列型<br>桁数：0&#65374;51200 byte<br>|○<br>|<br>|
#### リクエストサンプル
```json
{
  "level":"INFO",
  "action":"authSchema",
  "object":"/{CellName}/{BoxName}/service_name/token_keeper",
  "result":"[XXXX2033] Success schema authorization. cellUrl=https://{UnitFQDN}/keeper-d4a57bb26eae481486b07d06487051d1/"
}
```

<br>
### レスポンス
#### ステータスコード
|コード<br>|メッセージ<br>|概要<br>|
|:--|:--|:--|
|200<br>|OK<br>|受付成功時<br>|
#### レスポンスヘッダ
|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|Access-Control-Allow-Origin<br>|クロスドメイン通信許可ヘッダ<br>|返却値は"*"固定<br>|
|X-Personium-Version<br>|APIの実行バージョン<br>|リクエストが処理されたAPIバージョン<br>|
#### レスポンスボディ
なし
#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

### cURLサンプル
```sh
curl "https://{UnitFQDN}/{CellName}/__event" -X POST -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"level":"INFO", "action":"authSchema", "object":"/{CellName}/{BoxName}/service_name/token_keeper", "result":"[XXXX2033] Success schema authorization. cellUrl=https://{UnitFQDN}/keeper-d4a57bb26eae481486b07d06487051d1/"}'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
