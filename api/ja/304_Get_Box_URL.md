# BoxURL取得
### 概要
BoxのURLを取得するために用いるリソースです。スキーマ認証を行ったアプリケーションはアクセストークンをつけてこのリソースにアクセスすることでスキーマ認証されたアプリケーションのためのBoxのURLにリダイレクトします。  
スキーマ認証に対応していないアプリケーションはパラメタでSchema urlを与えることで、対応するBoxのURLにリダイレクトします。

### 必要な権限
本リソースのアクセス制御はBoxルートのACLに依存します。以下２点を満たしている必要があります。
* BoxルートACLのrequireSchemaAuthz属性がnoneでない場合（public、confidentialの場合）はスキーマ認証済であること
* 利用者がBoxルートをread可能であること。（Boxルートが全公開である場合は利用者認証不要）

※ACLのrequireSchemaAuthz属性については、[アクセス制御モデル](../../user_guide/002_Access_Control.html)内の「スキーマ権限要求レベル」を参照。

### 制限事項
なし

<br>
### リクエスト
#### リクエストURL
```
/{CellName}/__box
```
#### メソッド
GET

#### リクエストクエリ

|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|p_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|
|Schema<br>|アプリセルURL<br>|URL形式<br>|×<br>|桁数：1&#65374;1024<br>URIの形式に従う<br>|


#### リクエストヘッダ

|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用されます。<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}  override} $: $ {value}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定します。<br>|
|X-Personium-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {AccessToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
#### リクエストボディ
なし

#### リクエストサンプル
なし

<br>
### レスポンス
#### ステータスコード

|コード<br>|メッセージ<br>|概要<br>|
|:--|:--|:--|
|200<br>|FOUND<br>|取得成功<br>|
#### レスポンスヘッダ

|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|Location<br>|Boxメタデータ取得API用URL<br>|リクエストの指定方法により、以下の値となる<br>Schemaクエリを指定<br>Schemaクエリで指定されたアプリセルURLに対応するBoxのURL<br>Schemaクエリなしで、Authorizationヘッダのみを指定<br>トークンに含まれるスキーマURLに対応するBoxのURL<br>|
Locationサンプル
```
Location:https://{UnitFQDN}/{CellName}/{BoxName}
```

#### レスポンスボディ
なし

#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

#### レスポンスサンプル
```
Location:https://{UnitFQDN}/{CellName}/{BoxName}
```
<br>
### cURLサンプル
#### スキーマ認証済
```sh
curl "https://{UnitFQDN}/{CellName}/__box" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```
#### スキーマ認証未対応
```sh
curl "https://{UnitFQDN}/{CellName}/__box?Schema=https://{UnitFQDN}/{CellName}/" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
