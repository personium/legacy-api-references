# Entity作成
### 概要
ユーザデータのEntityを作成します。
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
       - ※リクエストボディでの設定時とDefaultValueでの設定時（\__published、\__updatedは後者のタイミング）
	- 1つのEntityTypeに対して作成出来るのは、DynamicProperty・DeclaredProperty・ComplexTypeProperty合わせて400個まで

<br>
### リクエスト
#### リクエストURL
```
/{Cell_name}/{Box_name}/{odata_colleciton_path}/{entity_set}
```
|パス<br>|概要<br>|
|:--|:--|
|Cell_name<br>|セル名<br>|
|Box_name<br>|ボックス名<br>|
|odata_colleciton_path<br>|コレクション名<br>|
|entity_set<br>|EntitySet名<br>|
|navigation_property<br>|NavigationProperty名<br>|
#### メソッド
POST
#### リクエストクエリ
##### 共通リクエストクエリ
|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|dc_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|
##### OData 共通リクエストクエリ
なし
#### リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {TokenValue}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
|Accept<br>|レスポンスボディの形式を指定する<br>|application / json<br>|×<br>|省略時は[application/json]として扱う<br>未対応<br>|
|Content-Type<br>|リクエストボディの形式を指定する<br>|application / json<br>|×<br>|省略時は[application/json]として扱う <br>未対応<br>|
#### リクエストボディ
|項目名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|__id<br>|EntityのID<br>|桁数：1&#65374;200<br>文字種:半角英数字と-(半角ハイフン)と_(半角アンダーバー)と:(半角コロン)<br>※ただし、先頭文字に-(半角ハイフン)と_(半角アンダーバー)と:(半角コロン)は指定不可<br>|×<br>|指定しない場合ユニークなIDが割り当てられます<br>有効値のチェック未対応<br>|
##### プロパティ
スキーマ定義済みのプロパティと動的（スキーマ未定義）プロパティ、合わせて最大で400個のプロパティを設定可能
上記にはComplexTypeで定義されているプロパティ数を含む
###### スキーマ定義済みのプロパティ
|項目名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|EntityTypeに紐づくProperty<br>|ユーザ定義項目  <br>|デフォルト値 PropertyのDefaultValueに基づく<br>|PropertyのNullableに基づく<br>| <br>|
###### スキーマ定義済みプロパティのvalueの有効値  
|データ型<br>|有効値<br>|
|:--|:--|
|文字列<br>|桁数：0&#65374;51200 byte<br>文字種: 文字列の値に制御コードを使用した場合、取得時にエスケープした状態で返却する<br>「\」を使用する場合、「\\」で指定する必要がある<br>文字列型のプロパティに整数値、小数値、真偽値、日付型の値を設定した場合、文字列型に変換して登録される<br>|
|整数値<br>|-2147483648 &#65374; 2147483647<br>|
|小数値<br>|整数部分の桁数：1&#65374;5桁<br>小数部分の桁数：1&#65374;5桁<br>|
|真偽値<br>|true / false / null(nullを指定した場合はfalseとして扱う)<br>|
|日付<br>|/Date(【long型の時刻】)/の形式で文字列で指定する<br>【long型の時刻】の有効値は、-6847804800000(1753-01-01T00:00:00.000Z)&#65374;253402300799999(9999-12-31T23:59:59.999Z)<br>また、予約語として以下を指定可能<br>SYSUTCDATETIME()：サーバ時間<br>|
#### 動的（スキーマ未定義）プロパティ
スキーマ定義済みのプロパティと動的（スキーマ未定義）プロパティ、合わせて最大で400個のプロパティを設定可能
上記にはComplexTypeで定義されているプロパティ数を含む
##### 動的プロパティのkeyの有効値
|データ型<br>|有効値<br>|
|:--|:--|
|文字列<br>|桁数：1&#65374;128 :<br>文字種:半角英数字と-(半角ハイフン)と_(半角アンダーバー)<br>_ただし、先頭文字に-(半角ハイフン)と_(半角アンダーバー)は指定不可  <br>_published、_updatedは、予約語であるためリクエストボディの指定は不可<br>|
#####動的プロパティのvalueの有効値
スキーマ定義済みプロパティのvalueの有効値と同様
配列、連想配列は指定不可
#### リクエストサンプル
```json
{"__id": "100-1_20101108-111352093","animalId": "100-1","name": "episode","startedAt": "2010-11-08","episodeType": "care","endedAt": "","outcome": "治療中"}
```
```json
{"__id": "100-1_20101108-111352093","animalId": "100-1","name": "episode","update": "SYSUTCDATETIME()"}
```
```json
{"__id": "100-1_20101108-111352093","animalId": "100-1","name": "episode","update": "\/Date(1350451322147)\/"}
```

<br>
### レスポンス
#### ステータスコード
|コード<br>|概要<br>|備考<br>|
|:--|:--|:--|
|201<br>|成功<br>| <br>|
|400<br>|リクエストクエリの指定誤り<br>|テスト未実施<br>|
||リクエストヘッダの指定誤り<br>|テスト未実施<br>|
||リクエストボディが有効値でない場合<br>|テスト未実施<br>|
||リクエストボディに最大個数より多くユーザデータを指定 リクエストボディの__idにnullを指定<br>| <br>|
|401<br>|認証トークンが無効<br>|テスト未実施<br>|
|403<br>|アクセス権限が不足している場合<br>| <br>|
|404<br>|存在しないCellを指定した場合<br>存在しないBoxを指定した場合<br>存在しないODataCollectionを指定した場合<br>|テスト未実施<br>|
|405<br>|許可していないリクエストメソッドを指定<br>|テスト未実施<br>|
|409<br>|既に同一のIDが作成されている場合<br>|テスト未実施<br>|
#### レスポンスヘッダ
|項目名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|Content-Type<br>|返却されるデータの形式<br>|テスト未実施<br>|
|Location<br>|作成したEntityのリソースURL<br>|正常にEntityが作成できた場合のみ返却する<br>|
|DataServiceVersion<br>|ODataのバージョン情報<br>|正常にEntityが作成できた場合のみ返却する<br>|
|ETag<br>|リソースのバージョン情報<br>|正常にEntityが作成できた場合のみ返却する<br>|
#### レスポンスボディ
##### 共通レスポンスボディ
レスポンスはJSONオブジェクトで、オブジェクト（サブオブジェクト）に定義されるキー(名前)と型、並びに値の対応は以下のとおりです。

|オブジェクト<br>|名前（キー）<br>|型<br>|値<br>|
|:--|:--|:--|:--|
|ルート<br>|d<br>|object<br>|オブジェクト{1}<br>|
|{1}<br>|__count<br>|string<br>|$inlinecountクエリでの取得結果件数<br>|
|{1}<br>|results<br>|array<br>|オブジェクト{2}の配列<br>|
|{2}<br>|__published<br>|string<br>|作成日(UNIX時間)<br>|
|{2}<br>|__updated<br>|string<br>|更新日(UNIX時間)<br>|
|{2}<br>|__metadata<br>|object<br>|オブジェクト{3}<br>|
|{3}<br>|etag<br>|string<br>|Etag値<br>|
|{3}<br>|uri<br>|string<br>|作成したリソースへのURL<br>|
上記以外に登録時に指定した動的なユーザデータを返却
#### エラーメッセージ一覧
[エラーメッセージ一覧](198_Error_Messages.html)を参照

#### レスポンスサンプル
```json
{
     "d": {
        "results": {
            "name": "episode",
            "outcome": "6cbb76424e2d",
            "episodeType": "care",
            "endedAt": "",
            "animalId": "100-1",
            "__published": "/Date(1336546944234)/",
            "__metadata": {
                "uri": "https://fqdn/cell_name/box_name/odata_colleciton_path/parent(100-1_20101108-111352093)",
                "etag": "1-de6910ec8b1333b48a4708ededc2942d",
                "type": "UserData.parent"  
            },
            "__id": "100-1_20101108-111352093",
            "startedAt": "2010-11-08",
            "__updated": "/Date(1336546944234)/"  
        }
    }
}
```

<br>
### CURLサンプル
#### CURLコマンド(UNIX)
```sh
curl 'https://fqdn/cell_name/box_name/odata_colleciton_path/parent' \
-X POST -H 'Authorization:Bearer auth_token' -k -i -d '{"__id": "100-1_20101108-111352093","animalId":
"100-1","name": "episode","startedAt": "2010-11-08","episodeType": "care","endedAt":
"","outcome": "治療中"}'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
