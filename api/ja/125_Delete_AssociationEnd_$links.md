# AssociationEnd_$links削除
### 概要
AssociationEndの$link情報を削除する
### 必要な権限
alter-schema
### 制限事項
* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
* リクエストボディはjson形式のみ受け付ける
* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはjson形式とする
* $formatクエリオプションにatom または xmlを指定した場合、エラーとはならないが、レスポンスボディのデータの保証はない

<br>
### リクエスト
#### リクエストURL
##### Linking with EntityType
```
AssociationEndとの$links
/{Cell_name}/{Box_name}/{Collection_name}/EntityType('{EntityType_name}')/$links/_AssociationEnd(Name='{AssociationEnd_name}',_EntityType.Name='{EntityType_name}')
または、
/{Cell_name}/{Box_name}/{Collection_name}/EntityType('{EntityType_name}')/$links/_AssociationEnd(Name='{AssociationEnd_name}')
または、
/{Cell_name}/{Box_name}/{Collection_name}/EntityType('{EntityType_name}')/$links/_AssociationEnd('{AssociationEnd_name}')
```
##### Linking with AssociationEnd
```
EntityTypeとの$links
/{Cell_name}/{Box_name}/{Collection_name}/AssociationEnd(Name='{AssociationEnd_name}',_EntityType.Name='{entitytype_name}')/$links/_EntityType('{entitytype_name}')
または、
/{Cell_name}/{Box_name}/{Collection_name}/AssociationEnd(Name='{AssociationEnd_name}'')/$links/_EntityType('{entitytype_name}')
または、
/{Cell_name}/{Box_name}/{Collection_name}/AssociationEnd('{AssociationEnd_name}')/$links/_EntityType('{entitytype_name}')
AssociationEndとの$links
/{Cell_name}/{Box_name}/{Collection_name}/AssociationEnd(Name='{AssociationEnd_name}',_EntityType.Name='{entitytype_name}')/$links/_AssociationEnd(Name='{AssociationEnd_name}',_EntityType.Name='{entitytype_name}')
または、
/{Cell_name}/{Box_name}/{Collection_name}/AssociationEnd(Name='{AssociationEnd_name}',_EntityType.Name='{entitytype_name}')/$links/_AssociationEnd(Name='{AssociationEnd_name}')
または、
/{Cell_name}/{Box_name}/{Collection_name}/AssociationEnd(Name='{AssociationEnd_name}',_EntityType.Name='{entitytype_name}')/$links/_AssociationEnd({AssociationEnd_name}')
または、
/{Cell_name}/{Box_name}/{Collection_name}/AssociationEnd(Name='{AssociationEnd_name}')/$links/_AssociationEnd(Name='{AssociationEnd_name}',_EntityType.Name='{entitytype_name}')
または、
/{Cell_name}/{Box_name}/{Collection_name}/AssociationEnd(Name='{AssociationEnd_name}')/$links/_AssociationEnd(Name='{AssociationEnd_name}')
または、
/{Cell_name}/{Box_name}/{Collection_name}/AssociationEnd(Name='{AssociationEnd_name}')/$links/_AssociationEnd('{AssociationEnd_name}')
または、
/{Cell_name}/{Box_name}/{Collection_name}/AssociationEnd('{AssociationEnd_name}')/$links/_AssociationEnd(Name='{AssociationEnd_name}',_EntityType.Name='{entitytype_name}')
または、
/{Cell_name}/{Box_name}/{Collection_name}/AssociationEnd('{AssociationEnd_name}')/$links/_AssociationEnd(Name='{AssociationEnd_name}')
または、
/{Cell_name}/{Box_name}/{Collection_name}/AssociationEnd('{AssociationEnd_name}')/$links/_AssociationEnd('{AssociationEnd_name}')
```
#### メソッド
DELETE
#### リクエストクエリ
なし
#### リクエストヘッダ
##### 共通リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用される<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定する<br>|
|X-Dc-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
##### OData共通リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {TokenValue}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
##### OData削除リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|If-Match<br>|対象ETag値を指定する<br>|ETag値<br>|×<br>|省略時は[*]として扱う<br>|
#### リクエストボディ
なし
#### リクエストサンプル
なし

<br>
### レスポンス
#### ステータスコード
204
#### レスポンスヘッダ
##### 共通レスポンスヘッダ
|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|X-Dc-Version<br>|APIの実行バージョン<br>|リクエストが処理されたAPIバージョン<br>|
|Access-Control-Allow-Origin<br>|クロスドメイン通信許可ヘッダ<br>|返却値は"*"固定<br>|
##### ODataレスポンスヘッダ
|項目名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|DataServiceVersion<br>|ODataProtocolのバージョン情報<br>|正常にAssociationEndが作成できた場合のみ返却する<br>動作確認済み<br>|
#### レスポンスボディ
なし
#### エラーメッセージ一覧
[エラーメッセージ一覧](198_Error_Messages.html)を参照
#### レスポンスサンプル
なし

<br>
### CURLサンプル
#### CURLコマンド(UNIX) EntityType
```sh
curl "https://fqdn/cell_name/box_name/collection_name/$metadata/EntityType(Name='entitytype_name')/$links/_AssociationEnd(Name='AssociationEnd_name',_EntityType.Name='entitytype_name')" -X DELETE -i -H 'If-Match: *' -H 'Authorization: Bearer auth_token' -H 'Accept: application/json'
```
#### CURLコマンド(UNIX) AssociationEnd
```sh
curl "https://fqdn/cell_name/box_name/collection_name/$metadata/AssociationEnd(Name='associationEnd_name',_EntityType.Name='entitytype_name')/$links/_AssociationEnd(Name='associationEnd_name2',
_EntityType.Name='entitytype_name2')" -X DELETE -i -H 'If-Match: *' -H 'Authorization: Bearer auth_token' -H 'Accept: application/json'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
