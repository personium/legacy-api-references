# Boxインストール
### 概要
barファイルを使って指定されたパスにBoxをインストールする。barファイルフォーマットについては 「[barファイル](301_Bar_File.html)」を参照。  
本APIは非同期通信方式を採用しているため、本APIではBoxインストールを受け付けた後、即復帰する。  
そのため、Boxインストール状況を確認するには、以下のAPIを使用する。
* [Box メタデータ取得](303_Progress_of_Bar_File_Installation.html) Boxインストールが異常終了した場合は、本APIにてBoxインストール状況を確認することで、エラーとなった原因を参照することができる。以下に、クライアントにおける受付から処理完了までの呼び出し方法を示す。

```
Boxインストールの呼び出し例（クライアントでのポーリングを30秒とした場合)
 1. Boxインストール受付
    -- MKCOL /{Cell}/{Box}
 2. Boxインストール状況確認
    -- GET /{Cell}/{Box}  -> "処理中" で返却。
    -- 30秒ポーリング
 3. GET /{Cell}/{Box}  -> "処理完了" で返却。
 ※上記 2. の処理をループして処理完了までポーリングする。
```

### 必要な権限
box-install

### 制限事項
#### Boxインストール対象Box制限
* 既に同名のBoxが存在する場合は、Boxインストールできない。
* 既に同じscheme URLが設定されたBoxが存在する場合は、Boxインストールできない。
* メインボックスへのBoxインストールはできない。
* barファイル："/bar/00_meta/00_manifest.JSON" のSchemaフィールドにnullは指定できない。

#### barファイル制限
* Boxインストール可能なbarファイルのファイルサイズは以下を上限とする。<br>
上限値を超えた場合はBoxインストールできない。
	- barファイルのファイルサイズ:100MB
	- barファイル内エントリの圧縮前ファイルサイズ：10MB
* barファイルは、barファイル に定義されている順序で各エントリが格納されいること。

#### Boxインストールのログ詳細制限
* Boxインストールのログ詳細は、Boxインストール対象Boxが所属するCellのEventBusへ出力される。 下記※1
	- そのため、ログ詳細を参照する場合は、ログファイル取得APIを使用して参照する。
	- ログファイル取得APIを使用するために、"log-read" の権限が必要である。
	- Boxインストール以外のイベントログも混在するため、"RequestKey" フィールド値でフィルタリングする必要がある。
	- "action" フィールド値が "MKCOL" の "Requestkey" フィールド値を取得し、フィルタリングする。

#### その他制限
* Boxインストール処理中にBoxインストール対象Boxを含む配下のリソースへのデータ操作はできない。
* Boxインストール中にエラーが発生した場合のロールバックは行わない。

##### ※1 Boxインストールのログ詳細フォーマット
|状態<br>|"action"<br>|"object"<br>|"result"<br>|
|:--|:--|:--|:--|
|Boxインストール受付<br>|"MKCOL"<br>|BoxのURL<br>|ステータスコード|
|Boxインストール処理中<br>|処理コード<br>|barファイル内のエントリパス<br>|処理コードに対応するメッセージ|
|Boxインストール完了<br>|処理コード<br>|BoxのURL<br>|処理コードに対応するメッセージ|

##### 処理コード
|処理コード<br>|Message<br>|Description<br>|
|:--|:--|:--|
|PL-BI-0000<br>|bar install completed<br>|Boxインストール完了(正常終了)<br>|
|PL-BI-0001<br>|bar install failed ({原因})<br>|Boxインストール完了(異常終了)<br>|
|PL-BI-1000<br>|bar install started<br>|Boxインストール開始<br>|
|PL-BI-1001<br>|install started<br>|barファイル内エントリインストール開始<br>|
|PL-BI-1003<br>|install completed<br>|barファイル内エントリインストール完了(正常終了)<br>|
|PL-BI-1004<br>|install failed ({cause})<br>|barファイル内エントリインストール完了(異常終了)<br>|
|PL-BI-1005<br>|Unknown Error ({原因})<br>|内部エラー<br>|
<br>
### リクエスト
#### リクエストURL
```
/{CellName}/{BoxName}
```
#### メソッド
MKCOL

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
|Content-Type<br>|リクエストボディの形式を指定する<br>|application/zip<br>|○<br>| <br>|
|Content-Length<br>|リクエストボディのサイズを指定する<br>|半角数字<br>|×<br>| <br>|
#### リクエストボディ
|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|
|インストールするbarファイルをバイナリでリクエストボディに指定する<br>|Content-Typeヘッダで指定した形式<br>|○<br>|barファイル：Zipファイル形式<br>|

barファイルのファイル構成については [bar ファイル](301_Bar_File.html)を参照。

#### リクエストサンプル
なし

<br>
### レスポンス
#### ステータスコード
|コード<br>|メッセージ<br>|概要<br>|備考<br>|
|:--|:--|:--|:--|
|202<br>|Accepted<br>|処理受付成功時<br>|<br>|

#### レスポンスヘッダ
|ヘッダ名<br>|概要<br>|備考<br>|
|:--|:--|:--|
|Location<br>|Boxメタデータ取得API用URL<br>| <br>|
|Access-Control-Allow-Origin<br>|クロスドメイン通信許可ヘッダ<br>|返却値は"*"固定<br>|
|X-Personium-Version<br>|APIの実行バージョン<br>|リクエストが処理されたAPIバージョン<br>|
Locationサンプル
```
Location:https://{UnitFQDN}/{CellName}/{BoxName}
```
Boxメタデータ取得API用URLの詳細は、[Boxメタデータ取得](303_Progress_of_Bar_File_Installation.html)を参照。
#### レスポンスボディ
なし

#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

#### レスポンスサンプル
```
Location: https://{UnitFQDN}/{CellName}/{BoxName}
```
Boxメタデータ取得API用URLの詳細は、[Boxメタデータ取得](303_Progress_of_Bar_File_Installation.html)を参照。
<br>
### cURLサンプル

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}" -X MKCOL -i -H 'Content-type: application/zip' -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' --data-binary @{FileName}
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
