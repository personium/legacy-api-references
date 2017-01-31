# コレクション削除
### 概要
コレクションを削除する

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
|{CellName}<br>|セル名<br>| <br>
|{BoxName}<br>|ボックス名<br>| <br>
|{ResourcePath}<br>|リソースへのパス<br>|有効値 桁数:1&#65374;128<br>使用可能文字種<br>半角英数字、半角ピリオド(.)、半角アンダーバー(_)、半角ハイフン(-)<br>|

#### メソッド
DELETE

#### リクエストクエリ
共通リクエストクエリ

|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|dc_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|



####WebDav 共通リクエストクエリ

なし

#### リクエストヘッダ
##### 共通リクエストヘッダ

|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用される<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定する<br>|
|X-Dc-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
###### リクエストヘッダ

|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {UnitUserToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
#### リクエストボディ
なし

#### リクエストサンプル
なし

<br>
### レスポンス
#### ステータスコード
204

#### レスポンスヘッダ

|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|X-Dc-Version<br>|APIの実行バージョン<br>|リクエストが処理されたAPIバージョン<br>|
|Access-Control-Allow-Origin<br>|クロスドメイン通信許可ヘッダ<br>|返却値は"*"固定<br>|
|Content-Type<br>|返却されるデータの形式<br>|テスト未実施<br>|
|Location<br>|作成したコレクションのリソースURL<br>|正常にコレクションが作成できた場合のみ返却する<br>|
|ETag<br>|リソースのバージョン情報<br>|正常にコレクションが作成できた場合のみ返却する<br>|
#### レスポンスボディ
なし

#### エラーメッセージ一覧
[エラーメッセージ一覧](200_Error_Messages.html)を参照

#### レスポンスサンプル
なし

<br>
### CURLサンプル
#### CURLコマンド(UNIX)
```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{CollectionName}" -X DELETE -i -H 'Authorization: Bearer {UnitUserToken}' -H 'Accept: application/json'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
