# Cellスナップショットファイル取得
### 概要
Cellスナップショットファイルを取得する。<br>If-None-Matchヘッダに指定されたETag値によって、返却される内容が異なる。

### 必要な権限
root

### 制限事項
* なし

<br>
### リクエスト
#### リクエストURL
```
/{CellName}/__snapshot/{FileName}
```
#### メソッド
GET

#### リクエストクエリ
|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|p_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|

#### リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用される。<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きする。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定する。<br>|
|X-Personium-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {AccessToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
|If-None-Match<br>|ETagの値を指定する<br>|String<br>以下のフォーマットでETag値を指定する<br>"*"または、"{半角数字}-{半角数字}"<br>|×<br>|例）ETag値「1-1372742704414」を指定する場合<br>"1-1372742704414"<br>|

#### リクエストボディ
なし

#### リクエストサンプル
なし

<br>
### レスポンス
#### ステータスコード
- リクエストでIf-None-Matchヘッダが指定されていない場合、またはリクエストでIf-None-MatchヘッダのETag値がWebDavに保存されているリソースのETagと一致しない場合<br>（指定されたETag値のフォーマットが不正な場合を含む）

|コード|メッセージ|概要|
|:--|:--|:--|
|200|OK|取得成功|

- リクエストでIf-None-MatchヘッダのETag値がWebDavに保存されているリソースのETagと一致する場合

|コード|メッセージ|概要|
|:--|:--|:--|
|304|Not Modified|ドキュメントが更新されていない|

#### レスポンスヘッダ
|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|Content-Type<br>|リソースのContent-Type<br>|ステータスコード200の場合のみ返却する<br>|
|ETag<br>|リソースのバージョン情報<br>|<br>|
|Access-Control-Allow-Origin<br>|クロスドメイン通信許可ヘッダ<br>|返却値は"*"固定<br>|
|X-Personium-Version<br>|APIの実行バージョン<br>|リクエストが処理されたAPIバージョン<br>|

#### レスポンスボディ
ファイルの内容を返却する<br>
ただし、ステータスコードが304の場合はレスポンスボディを返却しない

#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

#### レスポンスサンプル
なし

<br>
### cURLサンプル
```sh
curl "https://{UnitFQDN}/{CellName}/__snapshot/{FileName}" -X POST -i -H 'Authorization: Bearer {AccessToken}'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
