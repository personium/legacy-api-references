# コレクション移動名称変更
### 概要
WebDAVコレクションファイルの移動/名前の変更。  
※ プロパティの変更は出来ない
### 必要な権限
write
### 制限事項
#### 共通制限
なし
#### WebDAV制限
未稿

<br>
### リクエスト
#### リクエストURL
```
/{CellName}/{BoxName}/{CollectionName}/
または、
/{CellName}/{BoxName}/{CollectionName}/{FileName}/
```
|パス<br>|概要<br>|備考<br>|
|:--|:--|:--|
|{CellName}<br>|セル名<br>| <br>
|{BoxName}<br>|ボックス名<br>| <br>
#### メソッド
MOVE
#### リクエストクエリ
##### 共通リクエストクエリ
|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|p_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|
##### WebDav 共通リクエストクエリ
なし
#### リクエストヘッダ
##### 共通リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用される<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定する<br>|
|X-Personium-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
##### 個別リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Destination<br>|変更先<br>|Absolute URI<br>|○<br>|移動/名前を変更するファイル名を指定する。<br> |
|Overwrite<br>|上書き<br>|"T" or "F"<br>|×<br>|上書き可("T")、上書き不可("F")を指定する。(初期値は"F")<br>|
|Depth<br>|移動階層<br>|"infinity"<br>|×<br>|移動するコレクション階層の深さを指定する。(初期値は無限)<br>|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {UnitUserToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
#### リクエストボディ
なし
#### リクエストサンプル
なし

<br>
### レスポンス
#### ステータスコード
|コード<bar>|概要<bar>|備考<bar>|
|:--|:--|:--|
|201<bar>|Created<bar>|移動または名称変更に成功(作成)<bar>|
|204<bar>|No Content<bar>|移動または名称変更に成功(上書き)<bar>|
#### レスポンスヘッダ
|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|Location<br>|作成したコレクションのリソースURL<br>|正常にコレクションが作成できた場合のみ返却する<br>|
|ETag<br>|リソースのバージョン情報<br>|正常にコレクションが作成できた場合のみ返却する<br>|
|Access-Control-Allow-Origin<br>|クロスドメイン通信許可ヘッダ<br>|返却値は"*"固定<br>|
|X-Personium-Version<br>|APIの実行バージョン<br>|リクエストが処理されたAPIバージョン<br>|

#### レスポンスボディ
なし
#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

|コード<bar>|概要<bar>|備考<bar>|
|:--|:--|:--|
|400<bar>|Bud Request<bar>|リクエストヘッダの形式が不正<bar>|
|409<bar>|Conflict<bar>|指定されたコレクション/セルが不正<bar>|
|412<bar>|Precondition Failed<bar>|上書き禁止が指定されている<bar>|

#### レスポンスサンプル
なし
<br>
### CURLサンプル

コレクション名変更(終端の"/"は必須)
```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{OldCollectionName}/" -X MOVE -i -H 'Destination:https://{UnitFQDN}/{CellName}/{BoxName}/{NewCollectionName}/' -H 'Authorization: Bearer {UnitUserToken}'
```

ファイル名変更
```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{CollectionName}/{OldFileName}/" -X MOVE -i -H 'Destination:https://{UnitFQDN}/{CellName}/{BoxName}/{CollectionName}/{NewFileName}' -H 'Authorization: Bearer {UnitUserToken}'
```

ファイル移動
```sh
curl  "https://{UnitFQDN}/{CellName}/{BoxName}/{CollectionNameA}/{FileName}" -X MOVE -i -H 'Destination:https://{UnitFQDN}/{CellName}/{BoxName}/{CollectionNameB}/{FileName}' -H 'Authorization: Bearer {UnitUserToken}'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
