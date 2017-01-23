# Entity削除
### 概要
ユーザデータのEntityを一件削除します。
### 必要な権限
write
### 制限事項
* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
* リクエストボディはjson形式のみ受け付ける
* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはjson形式とする
* $formatクエリオプションにatom または xmlを指定した場合、エラーとはならないが、レスポンスボディのデータの保証はない
* ユーザデータ制限事項
	- Edm.DateTime型のプロパティの有効範囲のチェックが適切に行われない
	- Edm.DateTime型の配列は未サポート
	- Edm.DateTime型のプロパティにSYSUTCDATETIME()を指定した場合、設定されるシステム時間が異なる場合がある
      - リクエストボディでの設定時とDefaultValueでの設定時（\__published、\__updatedは後者のタイミング）
	- 1つのEntityTypeに対して作成出来るのは、DynamicProperty・DeclaredProperty・ComplexTypeProperty合わせて400個まで

<br>
### リクエスト
#### リクエストURL
```
/{Cell_name}/{Box_name}/{odata_colleciton_path}/{entity_set}({KeyPredicate})}
```
|パス<br>|概要<br>|
|:--|:--|
|Cell_name<br>|セル名<br>|
|Box_name<br>|ボックス名<br>|
|odata_colleciton_path<br>|コレクション名<br>|
|entity_set<br>|EntitySet名<br>|
|KeyPredicate<br>|削除するEntityのID<br>|
#### メソッド
DELETE
#### リクエストクエリ
##### 共通リクエストクエリ
|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|dc_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|
##### OData 共通リクエストクエリ
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
|コード<br>|概要<br>|備考<br>|
|:--|:--|:--|
|204<br>|成功<br>| <br>
|400<br>|リクエストヘッダの指定誤り<br>|テスト未実施<br>|
|401<br>|認証トークンが無効<br>|テスト未実施<br>|
|403<br>|アクセス権限が不足している場合<br>| <br>
|404<br>|存在しないCellを指定した場合<br>存在しないBoxを指定した場合<br>存在しないODataCollectionを指定した場合<br>存在しないEntitySetを指定した場合<br>存在しないEntityを指定した場合<br>|テスト未実施<br>|
|405<br>|許可していないリクエストメソッドを指定<br>|テスト未実施<br>|
|409<br>|既に同一のIDが作成されている場合<br>|未対応<br>|
|412<br>|If-Matchの指定誤り<br>| <br>|
#### レスポンスヘッダ
|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|Content-Type<br>|返却されるデータの形式<br>|Entityの削除に失敗した場合のみ返却する<br>テスト未実施<br>|
|DataServiceVersion<br>|ODataのバージョン情報<br>|正常にEntityが削除できた場合のみ返却する<br>|
#### レスポンスボディ
正常時：なし
エラー時：エラーの詳細情報
#### エラーメッセージ一覧
[エラーメッセージ一覧](198_Error_Messages.html)を参照
#### レスポンスサンプル
なし

<br>
### CURLサンプル
#### CURLコマンド(UNIX)
```sh
curl 'https://fqdn/cell_name/box_name/odata_colleciton_path/entity(%270022b630db5c4aedade200a955e82285%27)'\
-X DELETE -H 'Authorization:Bearer auth_token' -H 'If-Match:*' -k -i
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
