# Account_NavProp経由登録
### 概要
RoleをAccountのNavigation Property経由で登録する
### 必要な権限
write
### 制限事項
* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
* リクエストボディはjson形式のみ受け付ける
* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはjson形式とする
* $formatクエリオプションにatom または xmlを指定した場合、エラーとはならないが、レスポンスボディのデータの保証はない

<br>
### リクエスト
#### リクエストURL
##### RoleへのnavigationProperty
```
/{CellName}/__ctl/Account(Name='{AccountName}')/_Role
```
または、
```
/{CellName}/__ctl/Account('{AccountName}')/_Role
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
#### リクエストボディ
Roleを登録する場合

|項目名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Name|アカウント名|文字種:半角英数字と左記半角記号（-_!$*=^`{&#124;}~.@）<br>ただし、先頭文字に半角記号は指定不可|○|<br>|
#### リクエストサンプル
```json
{
  "Name": "{AccountName}"  
}
```

<br>
### レスポンス
#### ステータスコード
201
#### レスポンスヘッダ
|項目名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|X-Personium-Version<br>|APIの実行バージョン<br>|リクエストが処理されたAPIバージョン<br>|
|Access-Control-Allow-Origin<br>|クロスドメイン通信許可ヘッダ<br>|返却値は"*"固定<br>|
|Content-Type<br>|返却されるデータの形式<br>|<br>|
|Location<br>|作成したリソースへのURL<br>|<br>|
|ETag<br>|リソースのバージョン情報<br>|<br>|
|DataServiceVersion<br>|ODataのバージョン<br>|<br>|
#### レスポンスボディ
|項目名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|d<br>|<br>|<br>|
|d / results<br>|<br>|<br>|
|d / results / __published<br>|作成日<br>|<br>|
|d / results / __updated<br>|更新日<br>|<br>|
|d / results / __metadata<br>|<br>|<br>|
|d / results / __metadata / etag<br>|ETag値<br>|<br>|
|d / results / __metadata / uri<br>|作成したリソースへのURL<br>|<br>|
|d / results / __metadata / type<br>|EntityType<br>|<br>|
|d / results / Name<br>|Role名<br>|<br>|
|d / results / _Box.Name<br>|関係対象のBox名<br>|<br>|
##### Accountを登録した場合
Account固有レスポンスボディ

|オブジェクト<br>|項目名<br>|Data Type<br>|備考<br>|
|:--|:--|:--|:--|
|{2}<br>|Name<br>|string<br>|Account名 <br>|
|{2}<br>|Cell<br>|string<br>|null<br>|
|{2}<br>|Type<br>|string<br>|basic<br>|
|{3}<br>|Type<br>|string<br>|CellCtl.Account<br>|

#### レスポンスサンプル
```json
{
  "d": {
    "results": {
      "Name": "{AccountName}",
      "__published": "/Date(1349355810698)/",
      "Cell": null,
      "__updated": "/Date(1349355810698)/",
      "Type": "basic",
      "__metadata": {
        "etag": "1-1349355810698",
        "type": "CellCtl.Account",
        "uri": "https://{UnitFQDN}/{CellName}/__ctl/Account('{AccountName}')"
      }
    }
  }
}   
```
#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

<br>
### CURLサンプル

##### AccountとRoleのnavigationProperty経由登録
```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/Account('acount_name')/_Role" -X POST -i -H 'Authorization: Bearer {UnitUserToken}' -H 'Accept: application/json' -d '{"Name":"{RoleName}"}'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
