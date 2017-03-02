# Account登録
### 概要
Accountを登録する

### 必要な権限
auth

### 制限事項
* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
* リクエストボディはjson形式のみ受け付ける
* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはjson形式とする
* $formatクエリオプションにatom または xmlを指定した場合、エラーとはならないが、レスポンスボディのデータの保証はない

<br>
### リクエスト
#### リクエストURL
```
/{CellName}/__ctl/Account
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
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用されます。<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定します。<br>|
|X-Personium-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {UnitUserToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
|Content-Type<br>|リクエストボディの形式を指定する<br>|application / json<br>|×<br>|省略時は[application/json]として扱う<br>|
|Accept<br>|レスポンスボディの形式を指定する<br>|application / json<br>|×<br>|省略時は[application/json]として扱う<br>|
|X-Personium-Credential<br>|パスワード<br>|文字列<br>|×<br>|文字数：6&#65374;32文字<br>文字種:半角英数字と-(半角ハイフン)と_(半角アンダーバー)<br>|
#### リクエストボディ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Name<br>|アカウント名<br>|桁数：1&#65374;128<br>文字種:半角英数字と下記半角記号<br>-_!$*=^`{&#124;}~.@<br>ただし、先頭文字に半角記号は指定不可<br>|○<br>|&#160;<br>|
|Type<br>|アカウントタイプ<br>|basic(ID/PWによる認証)<br>oidc:google(Google OpenID Connectによる認証)<br>または上記２つをスペースで区切る<br>|×<br>|デフォルト：basic<br>|
|LastAuthenticated<br>|最終認証日時<br>|/Date(【long型の時刻】)/の形式で文字列で指定する<br>　【long型の時刻】の有効値は、-6847804800000(1753-01-01T00:00:00.000Z)&#65374;253402300799999(9999-12-31T23:59:59.999Z)<br>|×<br>|デフォルト：null<br>|
#### リクエストサンプル
ID/PW認証用アカウント
```json
{
  "Name": "{AccountName}"
}
```
Google認証用アカウント
```json
{
  "Name": "{AccountName}","Type":"oidc:google"
}
```
ID/PW認証＋Google認証用アカウント
```json
{
  "Name": "{AccountName}","Type":"basic oidc:google"
}
```

<br>
### レスポンス
#### ステータスコード
201

#### レスポンスヘッダ

|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|X-Personium-Version<br>|APIの実行バージョン<br>|リクエストが処理されたAPIバージョン<br>|
|Access-Control-Allow-Origin<br>|クロスドメイン通信許可ヘッダ<br>|返却値は"*"固定<br>|
|Content-Type<br>|返却されるデータの形式<br>|&#160;<br>|
|Location<br>|作成したリソースへのURL<br>|&#160;<br>|
|ETag<br>|リソースのバージョン情報<br>|&#160;<br>|
|DataServiceVersion<br>|ODataのバージョン<br>|&#160;<br>|
#### レスポンスボディ

|オブジェクト<br>|項目名<br>|型<br>|備考<br>|
|:--|:--|:--|:--|
|ルート<br>|d<br>|object<br>|オブジェクト{1}<br>|
|{1}<br>|results<br>|array<br>|オブジェクト{2}の配列<br>|
|{2}<br>|__metadata<br>|object<br>|オブジェクト{3}<br>|
|{3}<br>|uri<br>|string<br>|作成したリソースへのURL<br>|
|{3}<br>|etag<br>|string<br>|Etag値<br>|
|{2}<br>|__published<br>|string<br>|作成日(UNIX時間)<br>|
|{2}<br>|__updated<br>|string<br>|更新日(UNIX時間)<br>|
|{1}<br>|__count<br>|string<br>|$inlinecountクエリでの取得結果件数<br>|

#### Account固有レスポンスボディ

|オブジェクト<br>|項目名<br>|型<br>|備考<br>|
|:--|:--|:--|:--|
|{3}<br>|type<br>|string<br>|CellCtl.Account<br>|
|{2}<br>|Name<br>|string<br>|Account名<br>|
|{2}<br>|LastAuthenticated<br>|string<br>|デフォルト：null<br>|
|{2}<br>|Type<br>|string<br>|デフォルト:"basic"<br>|
|{2}<br>|Cell<br>|string<br>|デフォルト：null<br>|
#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

|コード<br>|メッセージ<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|
|400<br>|Bad Request<br>|リクエストボディの形式が不正&#160;<br>リクエストヘッダの形式が不正<br>|&#160;<br>|
|401<br>|Unauthorized<br>|認証トークンが無効<br>|&#160;<br>|
|403<br>|Forbidden<br>|アクセス権限が不足している場合<br>|&#160;<br>|
|404<br>|Not Found<br>|存在しないリソースを指定<br>|&#160;<br>|
|405<br>|Method Not Allowed<br>|許可していないリクエストメソッドを指定<br>|&#160;<br>|
|409<br>|Conflict<br>|セル内に同じ"Name"のaccountが存在している場合<br>|&#160;<br>
|412<br>|Precondition Failed<br>|存在しないバージョンを指定<br>|&#160;<br>|

&#160;

#### レスポンスサンプル
ID/PW認証用アカウント
```json
{
  "d": {
    "results": {
      "__metadata": {
        "uri": "https://{UnitFQDN}/{CellName}/__ctl/Account('{AccountName}')",
        "etag": "W/\"1-1486462510467\"",
        "type": "CellCtl.Account"
      },
      "Name": "{AccountName}",
      "LastAuthenticated": null,
      "Type": "basic",
      "Cell": null,
      "__published": "/Date(1486462510467)/",
      "__updated": "/Date(1486462510467)/"
    }
  }
}
```
Google認証用アカウント
```json
{
  "d": {
    "results": {
      "__metadata": {
        "uri": "https://{UnitFQDN}/{CellName}/__ctl/Account('{AccountName}')",
        "etag": "W/\"1-1486462510467\"",
        "type": "CellCtl.Account"
      },
      "Name": "{AccountName}",
      "LastAuthenticated": null,
      "Type": "oidc:google",
      "Cell": null,
      "__published": "/Date(1486462510467)/",
      "__updated": "/Date(1486462510467)/"
    }
  }
}
```
ID/PW認証＋Google認証用アカウント
```json
{
  "d": {
    "results": {
      "__metadata": {
        "uri": "https://{UnitFQDN}/{CellName}/__ctl/Account('{AccountName}')",
        "etag": "W/\"1-1486462510467\"",
        "type": "CellCtl.Account"
      },
      "Name": "{AccountName}",
      "LastAuthenticated": null,
      "Type": "basic oidc:google",
      "Cell": null,
      "__published": "/Date(1486462510467)/",
      "__updated": "/Date(1486462510467)/"
    }
  }
}
```

<br>
### CURLサンプル
ID/PW認証用アカウント
```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/Account" -X POST -i -H 'X-Personium-Credential:password' -H 'Authorization: Bearer {UnitUserToken}' -H 'Accept: application/json' -d '{"Name":"{AccountName}"}'
```
Google認証用アカウント
```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/Account" -X POST -i -H 'X-Personium-Credential:password' -H 'Authorization: Bearer {UnitUserToken}' -H 'Accept: application/json' -d '{"Name":"{AccountName}","Type":"oidc:google"}'
```
ID/PW認証＋Google認証用アカウント
```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/Account" -X POST -i -H 'X-Personium-Credential:password' -H 'Authorization: Bearer {UnitUserToken}' -H 'Accept: application/json' -d '{"Name":"{AccountName}","Type":"basic oidc:google"}'
```
<br>
###### Copyright 2017    FUJITSU LIMITED
