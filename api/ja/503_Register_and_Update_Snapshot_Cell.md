# Cellスナップショットファイル登録更新
### 概要
Cellスナップショットを登録・更新する。

### 必要な権限
root

### 制限事項
なし

<br>
### リクエスト
#### リクエストURL
```
/{CellName}/__snapshot/{FileName}
```

#### メソッド
PUT

#### リクエストクエリ
|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|p_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|

#### リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用される<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定する<br>|
|X-Personium-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {AccessToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
|If-Match<br>|対象ETag値を指定する<br>|ETag値<br>|×<br>|省略時は[*]として扱う<br>|
|Content-Type<br>|登録・更新ファイルのコンテンツ形式を指定する<br>|String<br>|×<br>|ZIP形式で登録・更新する場合<br>Content-Type:application/zip<br>|

#### リクエストボディ
|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|
|登録・更新するコンテキスト情報をバイナリでリクエストボディに指定する<br>|Content-Typeヘッダで指定した方式<br>|○<br>|<br>|

#### リクエストサンプル
なし

<br>
### レスポンス
#### ステータスコード
|コード<br>|メッセージ<br>|概要<br>|
|:--|:--|:--|
|201<br>|Created<br>|登録成功<br>|
|204<br>|No Content<br>|更新成功<br>|

#### レスポンスヘッダ
|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|Content-Type<br>|返却されるデータの形式<br>|更新・作成時に失敗した場合のみ返却する<br>|
|Access-Control-Allow-Origin<br>|クロスドメイン通信許可ヘッダ<br>|返却値は"*"固定<br>|
|X-Personium-Version<br>|APIの実行バージョン<br>|リクエストが処理されたAPIバージョン<br>|
|ETag<br>|リソースのバージョン情報<br>|<br>|

#### レスポンスボディ
更新・作成時に失敗した場合のみ返却する

#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

#### レスポンスサンプル
なし

<br>
### cURLサンプル
```sh
curl "https://{UnitFQDN}/{CellName}/__snapshot/CellExport_2017_01.zip" -X PUT -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{ファイル内容}'
```
```sh
curl "https://{UnitFQDN}/{CellName}/__snapshot/CellExport_2017_01.zip" -X PUT -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -T "/home/user/CellExport.zip"
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
