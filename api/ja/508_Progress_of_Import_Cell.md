# Cellインポート状態取得
### 概要
Cellインポートの状態を取得する。Cellインポート状態には、以下の情報が含まれる。
* Cellインポートの状態
	* Cellインポート受付可能
	* Cellインポート処理中
	* Cellインポート異常終了
* インポート開始日時
* 進捗率
* インポートファイル名
* エラーメッセージ

### 必要な権限
root

### 制限事項
* なし

<br>
### リクエスト
#### リクエストURL
```
/{CellName}/__import
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
|200<br>|OK<br>|成功時<br>|Cellインポートの状態はレスポンスボディを参照<br>|

#### レスポンスヘッダ
|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|Access-Control-Allow-Origin<br>|クロスドメイン通信許可ヘッダ<br>|返却値は"*"固定<br>|
|X-Personium-Version<br>|APIの実行バージョン<br>|リクエストが処理されたAPIバージョン<br>|
|Content-Type<br>|返却されるデータの形式<br>|<br>|

#### レスポンスボディ
レスポンスはJSON形式で、オブジェクト（サブオブジェクト）に定義される。
キー(名前)と型、並びに値の対応は以下のとおり。

|オブジェクト<br>|キー<br>|型<br>|値<br>|備考<br>|
|:--|:--|:--|:--|:--|
|ルート<br>|status<br>|string<br>|以下のいずれかの文字列:  <br>"ready"<br>"importation in progress"<br>"import failed"<br>|"ready":Cellインポート受付可能<br>"importation in progress":Cellインポート処理中<br>"import failed":Cellインポート異常終了|
|ルート<br>|started_at<br>|string<br>|Start time (ISO 8610 UTC format)<br>|statusが以下の場合は出力しない。<br>"ready"<br>|
|ルート<br>|progress<br>|string<br>|Progress rate (for example, "30%")<br>|statusが以下の場合は出力しない。<br>"ready"<br>|
|ルート<br>|importation_name<br>|string<br>|インポートファイル名(拡張子は除く)<br>|statusが以下の場合は出力しない。<br>"ready"<br>|
|ルート<br>|message<br>|object<br>|Object (message format)<br>|statusが以下の場合のみ出力する。<br>"import failed"<br>詳細は [エラーメッセージ一覧](004_Error_Messages.html)を参照<br>|

#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

#### レスポンスサンプル
Cellインポート受付可能（Cellインポート完了時を含む）
```json
{
  "status": "ready"
}
```

Cellインポート処理中の場合
```json
{
  "status": "importation in progress",
  "started_at": "2017-02-13T09:00:00.000Z",
  "progress": "81%",
  "importation_name": "CellExport_2017_01"
}
```

Cellインポート異常終了
```json
{
  "status": "import failed",
  "started_at": "2017-02-13T09:00:00.000Z",
  "progress": "81%",
  "importation_name": "CellExport_2017_01",
  "message": {
      "code" : "PR409-OD-0003",
      "message" : {
          "lang" : "en",
          "value" : "The entity already exists."
      }
  }
}
```

<br>
### cURLサンプル
```sh
curl "https://{UnitFQDN}/{CellName}/__import" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
