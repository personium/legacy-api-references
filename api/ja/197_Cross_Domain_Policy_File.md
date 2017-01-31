# クロスドメインポリシーファイル
### 概要
クロスドメインポリシーファイル(crossdomain.xml)を取得します。
クロスドメインポリシーファイルはすべての利用者が取得できます。
### 必要な権限
なし
### 制限事項
特になし

<br>
### リクエスト
#### リクエストURL
```
/crossdomain.xml
```
#### メソッド
GET
#### リクエストクエリ
なし
#### リクエストヘッダ
なし
#### リクエストボディ
なし
#### リクエストサンプル
なし

<br>
### レスポンス
#### ステータスコード
200
#### レスポンスヘッダ
なし
#### レスポンスボディ
XML形式

|項目名<br>|概要<br>|
|:--|:--|
|/cross-domain-policy/site-control<br>|メタポリシーの許可設定情報。固定で属性値「permitted-cross-domain-policies="all"」を返却する<br>|
|/cross-domain-policy/allow-access-from<br>|現在のドメインにアクセス可能なドメイン。固定で属性値「domain="*"」を返却する<br>|
|/cross-domain-policy/allow-http-request-headers-from<br>|現在のドメインに送信可能なヘッダ情報と、ヘッダ送信元ドメイン。固定で属性値「domain="*" headers="*"」を返却する<br>|
#### エラーメッセージ一覧
[エラーメッセージ一覧](200_Error_Messages.html)を参照

<br>
### CURLサンプル
```sh
curl "https://{UnitFQDN}/crossdomain.xml" -X GET -i
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
