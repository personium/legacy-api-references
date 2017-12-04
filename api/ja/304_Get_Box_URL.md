# BoxURL取得
### 概要
BoxのURLを取得するために用いるリソースです。スキーマ認証を行ったアプリケーションはアクセストークンをつけてこのリソースにアクセスすることでスキーマ認証されたアプリケーションのためのBoxのURLにリダイレクトします。  
スキーマ認証に対応していないアプリケーションはパラメタでschema urlを与えることで、対応するBoxのURLにリダイレクトします。

### 必要な権限
本リソースのアクセス制御はBoxルートのACLに依存します。以下２点を満たしている必要があります。
* BoxルートACLのrequireSchemaAuthz属性がnoneでない場合（public、confidentialの場合）はスキーマ認証済であること
* 利用者がBoxルートをread可能であること。（Boxルートが全公開である場合は利用者認証不要）

※ACLのrequireSchemaAuthz属性については、[アクセス制御モデル](../../user_guide/002_Access_Control.html)内の「スキーマ権限要求レベル」を参照。

### 制限事項
なし


### リクエスト
#### リクエストURL
```
/{CellName}/__box
```
#### メソッド
GET

#### リクエストクエリ

|クエリ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|p_cookie_peer|クッキー認証値|認証時にサーバから返却されたクッキー認証値|×|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する|
|schema|アプリセルURL|URL形式|×|桁数：1&#65374;1024<br>URIの形式に従う|


#### リクエストヘッダ

|ヘッダ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|メソッドオーバーライド機能|任意|×|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用されます。|
|X-Override|ヘッダオーバライド機能|${上書きするヘッダ名}:${値}  override} $: $ {value}|×|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定します。|
|X-Personium-RequestKey|イベントログに出力するRequestKeyフィールドの値|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字|×|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応|
|Authorization|OAuth2.0形式で、認証情報を指定する|Bearer {AccessToken}|×|※認証トークンは認証トークン取得APIで取得したトークン|
#### リクエストボディ
なし

#### リクエストサンプル
なし


### レスポンス
#### ステータスコード

|コード|メッセージ|概要|
|:--|:--|:--|
|200|FOUND|取得成功|
#### レスポンスヘッダ

|ヘッダ名|概要|備考|
|:--|:--|:--|
|Location|Boxメタデータ取得API用URL|リクエストの指定方法により、以下の値となる<br>schemaクエリを指定<br>schemaクエリで指定されたアプリセルURLに対応するBoxのURL<br>schemaクエリなしで、Authorizationヘッダのみを指定<br>トークンに含まれるスキーマURLに対応するBoxのURL|
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

### cURLサンプル
#### スキーマ認証済
```sh
curl "https://{UnitFQDN}/{CellName}/__box" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```
#### スキーマ認証未対応
```sh
curl "https://{UnitFQDN}/{CellName}/__box?schema=https://{UnitFQDN}/{CellName}/" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```

###### Copyright 2017 FUJITSU LIMITED
