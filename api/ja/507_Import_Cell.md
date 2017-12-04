# Cellインポート
### 概要
CellスナップショットファイルからCellのデータをインポートする。<br>CellスナップショットファイルはPersoniumUnit内の特殊な領域(Cellスナップショット領域)のものを使用する。<br>処理に失敗した場合、Cellのステータスを"import failed"に変更する。<br>処理に成功した場合、Cellのステータスが"import failed"であれば"normal"に変更する。<br>Cellのステータスが"import failed"の場合、対象のCellは一部API以外を受け付けない。詳細は[プロパティ取得](290_Cell_Get_Property.html)を参照。<br>本APIは非同期通信方式を採用しているため、APIを受け付けた後、即復帰する。<br>Cellインポートの状況を確認するには[Cellインポート状態取得](508_Progress_of_Import_Cell.html)を使用する。<br>クライアントにおける受付から処理完了までの呼び出し例を以下に示す。
```
Cellインポートの呼び出し例（クライアントでのポーリングを10秒とした場合)
 1. Cellインポート受付
    -- POST /{CellName}/__import
 2. Cellインポート状態確認
    -- GET /{CellName}/__import -> "処理中"で返却。
    -- 10秒ポーリング
 3. Cellインポート完了
    -- GET /{CellName}/__import -> "受付可能"で返却。
 ※上記 2. の処理はループして処理完了までポーリングする。
```

### 必要な権限
root

### 制限事項
- リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
- リクエストボディはjson形式のみ受け付ける
- 処理が途中で失敗した場合でもロールバックは行わない<br>※Cellインポートを実施する前に、一旦Cellのエクスポートを実施しておくことを推奨します。

<br>
### リクエスト
#### リクエストURL
```
/{CellName}/__import
```

#### メソッド
POST

#### リクエストクエリ
|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|p_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|

#### リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用される<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定する<br>|
|X-Personium-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {AccessToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
|Content-Type<br>|リクエストボディの形式を指定する<br>|application / json<br>|×<br>|省略時は[application/json]として扱う<br>|

#### リクエストボディ
##### Format
JSON

|項目名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Name<br>|スナップショットファイル名(拡張子は除く)<br>|桁数：1～192<br>文字種:半角英数字と-(半角ハイフン)と_(半角アンダーバー)<br>|○<br>|<br>|

#### リクエストサンプル
```json
{"Name":"CellExport_2017_01"}
```

<br>
### レスポンス
#### ステータスコード
|コード|メッセージ|概要|
|:--|:--|:--|
|202|Accepted|処理受付|

#### レスポンスヘッダ
|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|Content-Type<br>|返却されるデータの形式<br>|作成に失敗した場合のみ返却する<br>|
|Location<br>|Cellインポート状態取得用のURL<br>|<br>|
|Access-Control-Allow-Origin<br>|クロスドメイン通信許可ヘッダ<br>|返却値は"*"固定<br>|
|X-Personium-Version<br>|APIの実行バージョン<br>|リクエストが処理されたAPIバージョン<br>|

#### レスポンスボディ
作成に失敗した場合のみエラーメッセージを返却する

#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

#### レスポンスサンプル
なし

<br>
### cURLサンプル
```sh
curl "https://{UnitFQDN}/{CellName}/__import" -X POST -i -H 'Authorization: Bearer {AccessToken}' -d '{"Name":"CellExport_2017_01"}'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
