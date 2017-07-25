# ファイル取得
### 概要
特定のWebDav情報を取得する  
If-None-Matchヘッダに指定されたETag値によって、返却される内容が異なる  
Rangeヘッダで範囲指定した場合、指定した範囲の内容を返却する
### 必要な権限
write
### 制限事項
未稿

<br>
### リクエスト
#### リクエストURL
```
/{CellName}/{BoxName}/{ResourcePath}
```
|パス<br>|概要<br>|備考<br>|
|:--|:--|:--|
|{CellName}<br>|セル名<br>|<br>|
|{BoxName}<br>|ボックス名<br>|<br>|
|{ResourcePath}<br>|リソースへのパス<br>|有効値 桁数:1&#65374;128<br>使用可能文字種<br>半角英数字、半角ピリオド(.)、半角アンダーバー(_)、半角ハイフン(-)<br>|
#### メソッド
GET
#### リクエストクエリ
|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|p_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|
#### リクエストヘッダ
##### 共通リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用されます。<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定します。<br>|
|X-Personium-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
##### 個別リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {UnitUserToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
|If-None-Match<br>|ETagの値を指定する<br>|String<br>以下のフォーマットでETag値を指定する<br>&quot;*&quot;<br>または、<br>"{半角数字}-{半角数字}"<br>|×<br>|例）ETag値「1-1372742704414」を指定する場合<br>　　"1-1372742704414"<br>|
|Range<br>|リソースの一部を取得する際に範囲を指定する<br>|・データ型：String<br>【前提】<br>範囲指定の開始値は0スタート。取得ファイルのサイズがZ(byte)の場合、終端はZ-1(byte)<br>1.&quot;Range: bytes={半角数字}-{半角数字}&quot;<br>ex) &quot;Range: bytes=10-20&quot;<br>2.&quot;Range: bytes={半角数字}-&quot;<br>→範囲の開始値から取得ファイルの最後まで<br>ex) &quot;Range: bytes=10-&quot;<br>3.&quot;Range: bytes=-{半角数字}&quot;<br>→リソースの終端から指定した分を取得<br>ex) "Range: bytes=-20"<br>|×<br>|・開始値は取得ファイルのサイズより小さい値を指定<br>・指定されたRangeヘッダのフォーマットが不正な場合はRangeヘッダは無視される<br>【制限事項】<br>・マルチパートレスポンスには未対応<br>|
#### リクエストボディ
なし
#### リクエストサンプル
なし

<br>
### レスポンス
#### ステータスコード
* リクエストでIf-None-Matchヘッダが指定されていない場合、またはリクエストでIf-None-MatchヘッダのETag値がWebDavに保存されているリソースのETagと一致しない場合<br>（指定されたETag値のフォーマットが不正な場合を含む）
* リクエストでRangeヘッダが指定されていない場合、またはリクエストでRangeヘッダで指定される開始値が終端値よりも大きい場合

|コード<br>|メッセージ<br>|概要<br>|
|:--|:--|:--|
|200<br>|OK<br>|取得成功時<br>|
* リクエストのRangeヘッダで有効である場合

|コード<br>|メッセージ<br>|概要<br>|
|:--|:--|:--|
|206<br>|Partial Content<br>|部分取得成功時<br>|
* リクエストでIf-None-MatchヘッダのETag値がWebDavに保存されているリソースのETagと一致する場合

|コード<br>|メッセージ<br>|概要<br>|
|:--|:--|:--|
|304<br>|Not Modified<br>|ドキュメントが更新されていない<br>|
#### レスポンスヘッダ
|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|Content-Type<br>|リソースのContent-Type<br>|ステータスコード200の場合のみ返却する<br>|
|ETag<br>|リソースのバージョン情報<br>|<br>|
|Accept-Ranges<br>|リソースへのバイトレンジ(範囲)リクエストの受け入れを示す<br>|<br>|
|Content-Range<br>|バイトレンジリクエストで指定した分がエンティティボディ全体のうちどこに当たるものかを示す<br>|<br>|
Basic認証エラーの場合は 400 + WWW-Authenticated:Basicヘッダを返却する
#### レスポンスボディ
ファイルの内容を返却する  
ただし、ステータスコードが304の場合はレスポンスボディを返却しない  
また、ステータスコードが206の場合はRangeヘッダで指定した範囲のファイルの内容を返却する
#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

|コード<br>|メッセージ<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|
|400<br>|Bad Request<br>|リクエストヘッダの形式が不正<br>|Basic認証エラーの場合、Odataクエリが不正の場合はこれに含む<br>|
|401<br>|Unauthorized<br>|認証トークンが無効<br>|Basic認証エラーの場合を除く<br>|
|403<br>|Forbidden<br>|アクセス権限が不足している場合<br>|<br>|
|404<br>|Not Found<br>|存在しないリソースを指定<br>|<br>|
|405<br>|Method Not Allowed<br>|許可していないリクエストメソッドを指定<br>|<br>|
|416<br>|Requested Range Not Satisfiable<br>|Rangeヘッダの指定範囲の開始値が取得ファイルサイズより大きい場合<br>|<br>|
|503<br>|Service Unavailable<br>|他プロセスがファイルを更新中の場合<br>|<br>|
#### レスポンスサンプル
なし

<br>
### CURLサンプル

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{CollectionName}" -X GET -i -H 'If-None-Match:"1-1372742704414"' -H 'Range:bytes=10-20 ' -H 'Authorization: Bearer {UnitUserToken}' -H 'Accept: application/json'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
