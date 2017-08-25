# サービスコレクションソース作成
### 概要
サービスソース情報を登録・更新する
### 必要な権限
write
### 制限事項
* 共通制限
	* なし
* WebDAV制限
	* 未稿

<br>
### リクエスト
#### リクエストURL
```
/{CellName}/{BoxName}/{CollectionName}/__src/{ResourceName}
```
|パス<br>|概要<br>|備考<br>|
|:--|:--|:--|
|{CellName}<br>|セル名<br>|<br>|
|{BoxName}<br>|ボックス名<br>|<br>|
|{CollectionName}<br>|サービスコレクション名<br>|有効値 桁数:1&#65374;128<br>使用可能文字種<br>半角英数字、半角ピリオド(.)、半角アンダーバー(_)、半角ハイフン(-)<br>|
|{ResourceName}<br>|リソース名<br>|有効値(制限) 桁数:1&#65374;128<br>使用可能文字種<br>半角英数字、半角ピリオド(.)、半角アンダーバー(_)、半角ハイフン(-)<br>|
#### メソッド
PUT
#### リクエストクエリ
##### 共通リクエストクエリ
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
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {AccessToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
|If-Match<br>|リソースのバージョン情報を指定する<br>|String<br>|×<br>|バージョン指定しない場合は*（アスタリスク）<br>|
|Content-Type<br>|登録・更新ファイルのコンテンツ形式を指定する <br>|String<br>|○<br>|JSでのリソースを形式で登録・更新する場合<br>Content-Type:text/javascript<br>|
#### リクエストボディ
|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|
|登録・更新するコンテキスト情報をバイナリでリクエストボディに指定する<br>|Content-Typeヘッダで指定した形式<br>|○<br>|<br>|
#### リクエストサンプル
なし

<br>
### レスポンス
#### ステータスコード
204
#### レスポンスヘッダ
|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|Content-Type<br>|返却されるデータの形式<br>|更新・作成に失敗した場合のみ返却する<br>|
|ETag<br>|リソースのバージョン情報<br>|<br>|
#### レスポンスボディ
なし
#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

#### レスポンスサンプル
なし

<br>
### CURLサンプル

```sh
curl "https://{CellName}/{BoxName}/{CollectionName}/__src/{ResourceName}" -X PUT -i  -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -H 'Content-Type:text/javascript' -d '【ファイル内容】'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
