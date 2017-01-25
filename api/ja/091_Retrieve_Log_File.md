# ログファイル取得
### 概要
イベントログを取得する
ローテートされたログファイルの保持世代数は最大12世代である。
ログファイルのローテート時に最大保持世代数を超えた場合は、最古のログファイルが削除される。
### 必要な権限
log-read
### 制限事項
* コレクションの階層の深さの最大値は50
* 各コレクション配下の子要素数の最大値は1コレクションあたり10,000
	- Box直下のコレクションを1階層目とし、以下2階層目、3階層目…とカウントする。コレクション配下に作成するファイルは、階層としてカウントしない。
* 内部イベントのログ出力は未サポート
* ログの出力設定、および出力設定の参照は未サポート
  - ログファイル名は"default.log"固定とする
  - ローテートは以下のデフォルト設定に従い行う
    - ローテートのサイズ設定:50MB
* ログの出力レベルは"info"固定（INFO, WARN, ERRORすべて出力）とする
* ローテート時のファイル名は、 default.log.{timestamp} とする。 {timestamp}は、ローテートされたときの時刻で採番される。

|アクション<br>|アーカイブされたログファイル<br>|説明<br>|備考<br>|
|:--|:--|:--|:--|
|First Rotation<br>|archive/<br>default.log.1402910774659<br>|<br>新規にローテートされたファイル<br>|<br>2014-06-16 18:26:14 +0900<br>|
|2nd Rotation<br>|archive/<br>default.log.1402910774659<br>default.log.1403910784659<br>|<br>前回ローテートされたファイル<br>新規にローテートされたファイル<br>|<br>2014-06-16 18:26:14 +0900<br>2014-06-28 08:13:04 +0900<br>|
|3rd Rotation<br>|archive/<br>default.log.1402910774659<br>default.log.1403910784659<br>default.log.1403910784659<br>|<br>前々回にローテートされたファイル<br>前回ローテートされたファイル<br>新規にローテートされたファイル<br>|<br>2014-06-16 18:26:14 +0900<br>2014-06-28 08:13:04 +0900<br>2014-07-09 21:59:44 +0900<br>|

<br>
### リクエスト
#### リクエストURL
|URL<br>|概要<br>|
|:--|:--|
|/{CellName}/__log/current/{LogName}<br>|最新のログファイル<br>|
|/{CellName}/__log/current/{LogName}<br>|ローテートされたログファイル<br>|
※{LogName}は、ログファイル情報取得API で返却されたファイル名を指定する。
#### メソッド
GET
#### リクエストクエリ
|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|dc_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|
#### リクエストヘッダ
##### 共通リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用される<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定する<br>|
|X-Dc-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {UnitUserToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
#### リクエストボディ
なし
#### リクエストサンプル
なし

<br>
### レスポンス
#### ステータスコード
200
#### レスポンスヘッダ
|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|Content-Type<br>|Resourceのデータ形式に応じたMimeType<br>|"text/csv"または"application/zip"<br>|
#### レスポンスボディ
currentのログ取得時にログが存在しない場合は、空のレスポンスボディを返却する。
ローテートのサイズ設定値よりも5MB程度大きなサイズとなる場合がある。
出力形式は以下の通り。
```
{dateTime},[{level}],{RequestKey},{name},{schema},{subject},{action},{object},{result}
```
|項目名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|dateTime<br>|ログ書込み日時（ISO8601 UTC形式）<br>|YYYY-MM-DDTHH:MM:SS.sssZ<br>|
|level<br>|ログレベル INFO,WARN,ERRORのいずれか<br>|文字列<br>|
|RequestKey<br>|X-Dc-RequestKeyヘッダで指定された値<br>X-Dc-RequestKeyヘッダ指定がない場合、PCS-${UNIX時間}<br>|文字列<br>|
|name<br>|外部イベント：client<br>内部イベント：server<br>|文字列<br>|
|schema<br>|受け付けたURLのboxのschema<br>|URL形式<br>|
|subject<br>|イベントの主体<br>|URL形式<br>|
|action<br>|外部イベント：イベント受付で定義されたaction<br>内部イベント：HTTPメソッド名<br>|文字列<br>|
|object<br>|外部イベント：イベント受付で定義されたobject<br>内部イベント：リクエストされたリソースパス<br>|文字列<br>|
|result<br>|外部イベント：イベント受付で定義されたresult<br>内部イベント：HTTPステータスコード<br>|文字列<br>|
#### エラーメッセージ一覧
[エラーメッセージ一覧](198_Error_Messages.html)を参照

|コード<br>|メッセージ<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|
|400<br>|Bad Request<br>|リクエストボディの形式が不正<br>リクエストヘッダの形式が不正<br>既に別のスキーマ型のIndexが作成されている場合<br>|<br>|
|401<br>|Unauthorized<br>|認証トークンが無効<br>|<br>|
|403<br>|Forbidden<br>|アクセス権限が不足している場合<br>|<br>|
|404<br>|Not Found<br>|存在しないリソースを指定<br>|<br>|
|405<br>|Method Not Allowed<br>|許可していないリクエストメソッドを指定<br>|<br>|
#### レスポンスサンプル
外部イベント
```
2013-02-04T00:50:12.761Z,[INFO ],Req_animal-access_1001,client,https://{UnitFQDN}/{CellName}/,https://{UnitFQDN}/servicemanager/#admin,authSchema,/{CellName}/{BoxName}/service_name/token_keeper,[XXXX2033] Success schema authorization. cellUrl=https://{UnitFQDN}/keeper-d4a57bb26eae481486b07d06487051d1/
```

内部イベント
```
2013-04-18T14:52:39.778Z,[ERROR],PCS-1364350331902,server,https://{UnitFQDN}/appCell/,https://{UnitFQDN}/appCell/#staff,POST,/homeClinic/__auth,200
```

<br>
### CURLサンプル
#### CURLコマンド(UNIX)
```sh
curl "https://{UnitFQDN}/{CellName}/__log/current/default.log" -X GET -i -H 'Authorization: Bearer {UnitUserToken}' -H 'Accept: application/json'          
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
