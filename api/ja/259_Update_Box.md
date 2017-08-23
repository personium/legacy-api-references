# Box更新
### 概要
既存のBox情報を更新する

### 必要な権限
なし

### 制限事項
* リクエストヘッダのAcceptは無視される
* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
* リクエストボディはjson形式のみ受け付ける
* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはjson形式とする
* $formatクエリオプションは無視される

<br>
### リクエスト
#### リクエストURL
```
/{CellName}/__ctl/Box('{BoxName}')
```
または、
```
/{CellName}/__ctl/Box(Name='{BoxName}')
```
#### メソッド
PUT

#### リクエストクエリ

|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|p_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|

#### リクエストヘッダ

|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用されます。<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}override} $: $ {value}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定します。<br>|
|X-Personium-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {AccessToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
|Contents-Type<br>|リクエストボディの形式を指定する<br>|application / json<br>|×<br>|省略時は[application/json]として扱う<br>|
|Accept<br>|レスポンスボディの形式を指定する<br>|application / json<br>|×<br>|省略時は[application/json]として扱う<br>|
|If-Match<br>|対象ETag値を指定する<br>|ETag値<br>|○<br>|<br>|
#### リクエストボディ
##### Format
JSON

|項目名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Name<br>|Box名<br>|桁数：1&#65374;128<br>文字種:半角英数字と-(半角ハイフン)と_(半角アンダーバー)<br>ただし、先頭文字に-(半角ハイフン)と_(半角アンダーバー)は指定不可<br>null<br>|○<br>|<br>|
|Schema<br>|Schema名<br>|桁数：1&#65374;128<br>文字種:半角英数字と-(半角ハイフン)と_(半角アンダーバー)<br>ただし、先頭文字に-(半角ハイフン)と_(半角アンダーバー)は指定不可<br>null<br>|×<br>|<br>|

#### リクエストサンプル
```json
{"Name":"{BoxName}", "Schema":"https://{UnitFQDN}/{CellName}/"}
```
<br>
### レスポンス
#### ステータスコード
204

#### レスポンスヘッダ
なし

#### レスポンスボディ
なし

#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

#### レスポンスサンプル
なし

<br>
### CURLサンプル

```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/Box('{BoxName}')" -X PUT -i -H 'If-Match: *' -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"Name":"{BoxName}"}'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
