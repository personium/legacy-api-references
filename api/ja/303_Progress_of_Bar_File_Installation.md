# Boxメタデータ取得
### 概要
Boxのメタデータを取得する。メタデータには、以下の情報が含まれる。
* Boxの状態<br>
Boxインストールの状況（インストール結果、進捗率、エラーメッセージ等）
* Boxの状態は以下の3種類が存在する
	- Boxが使用可能
	- Boxインストール処理中
	- Boxインストールの処理が異常終了
* BoxのスキーマURL
* Boxの作成日時

### 必要な権限
read

### 制限事項
* Boxインストール状況が確認可能な有効期限は、Boxインストール完了（異常終了を含む）後Unitで設定されている期限まで
* Boxインストールに失敗した場合は、EventBusに出力されたログを参照する必要がある


### リクエスト
#### リクエストURL
```
/{CellName}/{BoxName}
```
#### メソッド
GET

#### リクエストクエリ

|クエリ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|p_cookie_peer|クッキー認証値|認証時にサーバから返却されたクッキー認証値|×|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する|


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

|コード|メッセージ|概要|備考|
|:--|:--|:--|:--|
|200|OK|成功時|Boxインストール成功可否はレスポンスボディを参照|
#### レスポンスヘッダ

|ヘッダ名|概要|備考|
|:--|:--|:--|
|Access-Control-Allow-Origin|クロスドメイン通信許可ヘッダ|返却値は"*"固定|
|X-Personium-Version|APIの実行バージョン|リクエストが処理されたAPIバージョン|
#### レスポンスボディ
レスポンスはJSON形式で、オブジェクト（サブオブジェクト）に定義される。
キー(名前)と型、並びに値の対応は以下のとおり。

|オブジェクト|名前(キー)|型|値|備考|
|:--|:--|:--|:--|:--|
|ルート|schema|string|Boxが紐づいているスキーマのURL|スキーマなしの場合は null|
|ルート|installed_at|string|Start time (ISO 8610 UTC format)|statusが以下のいずれかの場合は出力しない。<br>&#183; "Installation in Progress"<br>&#183; "installation failed"|
|ルート|started_at|string|Start time (ISO 8610 UTC format)|statusが以下の場合は出力しない。<br>&#183; "Ready"|
|ルート|progress|string|Progress rate (for example, "30%")|statusが以下の場合は出力しない。<br>&#183; "Ready"|
|ルート|message|object|Object (message format)|statusが以下の場合のみ出力する。<br>&#183; "Installation failed"<br>詳細は [エラーメッセージ一覧](004_Error_Messages.html)を参照|
|ルート|status|string|以下のいずれかの文字列:  <br>&#183; "ready"<br>&#183; "installation in progress"<br>&#183; "installation failed"|Boxが使用可能な状態を示す<br>Boxインストール処理中を示す<br>Boxインストール完了（異常終了）を示す|

#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

#### レスポンスサンプル
Boxの作成後（Boxインストール完了時を含む）
```JSON
{
  "schema": "https://example.com/app1/",
  "installed_at": "2017-02-13T09:00:00.000Z",
  "status": "ready"
}
```


Boxインストール処理中の場合
```JSON
{
  "schema": "https://example.com/app1/",
  "started_at": "2017-02-13T09:00:00.000Z",
  "progress": "81%",
  "status": "installation in progress"
}
```


Boxインストール完了時（異常終了）の場合  
（Boxインストールに失敗後、有効期限以内）
```JSON
{
  "schema": "https://example.com/app1/",
  "started_at": "2017-02-13T09:00:00.000Z",
  "progress": "81%",
  "message": {
      "code" : "PR409-OD-0003",
      "message" : {
          "lang" : "en",
          "value" : "The entity already exists."
      }
  },
  "status": "installation failed"

}
```


### cURLサンプル

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```

###### Copyright 2017 FUJITSU LIMITED
