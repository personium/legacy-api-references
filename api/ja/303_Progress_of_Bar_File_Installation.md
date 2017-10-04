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

<br>
### リクエスト
#### リクエストURL
```
/{CellName}/{BoxName}
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

|コード<br>|メッセージ<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|
|200<br>|OK<br>|成功時<br>|Boxインストール成功可否はレスポンスボディを参照<br>|
#### レスポンスヘッダ

|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|Access-Control-Allow-Origin<br>|クロスドメイン通信許可ヘッダ<br>|返却値は"*"固定<br>|
|X-Personium-Version<br>|APIの実行バージョン<br>|リクエストが処理されたAPIバージョン<br>|
#### レスポンスボディ
レスポンスはJSON形式で、オブジェクト（サブオブジェクト）に定義される。
キー(名前)と型、並びに値の対応は以下のとおり。

|オブジェクト<br>|名前(キー)<br>|型<br>|値<br>|備考<br>|
|:--|:--|:--|:--|:--|
|ルート<br>|Schema<br>|string<br>|Boxが紐づいているスキーマのURL<br>|スキーマなしの場合は null<br>|
|ルート<br>|installed_at<br>|string<br>|Start time (ISO 8610 UTC format)<br>|statusが以下のいずれかの場合は出力しない。<br>&#183;        "Installation in Progress"<br>&#183;        "installation failed"<br>|
|ルート<br>|started_at<br>|string<br>|Start time (ISO 8610 UTC format)<br>|statusが以下の場合は出力しない。<br>&#183;        "Ready"<br>|
|ルート<br>|progress<br>|string<br>|Progress rate (for example, "30%")<br>|statusが以下の場合は出力しない。<br>&#183;        "Ready"<br>|
|ルート<br>|message<br>|object<br>|Object (message format)<br>|statusが以下の場合のみ出力する。<br>&#183;        "Installation failed"<br>詳細は [エラーメッセージ一覧](004_Error_Messages.html)を参照<br>|
|ルート<br>|status<br>|string<br>|以下のいずれかの文字列:  <br>&#183;        "ready"<br>&#183;        "installation in progress"<br>&#183;        "installation failed"<br>|Boxが使用可能な状態を示す<br>Boxインストール処理中を示す<br>Boxインストール完了（異常終了）を示す<br>|

#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

#### レスポンスサンプル
Boxの作成後（Boxインストール完了時を含む）
```JSON
{
  "Schema": "https://example.com/app1/",
  "installed_at": "2017-02-13T09:00:00.000Z",
  "status": "ready"
}
```



Boxインストール処理中の場合
```JSON
{
  "Schema": "https://example.com/app1/",
  "started_at": "2017-02-13T09:00:00.000Z",
  "progress": "81%",
  "status": "installation in progress"
}
```



Boxインストール完了時（異常終了）の場合  
（Boxインストールに失敗後、有効期限以内）
```JSON
{
  "Schema": "https://example.com/app1/",
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

<br>
### cURLサンプル

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
