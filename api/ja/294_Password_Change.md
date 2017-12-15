# パスワード変更(\__mypassword)
### 概要
Accountのパスワードに関する操作を行うAPI
##### 自アカウントのパスワード変更
自アカウントのパスワード変更を行う。  
※ Account更新APIでもAccountのパスワード変更を行うことができるが、これはCell Level ACLのauth権限が必要で管理目的で利用する。  
※ アカウントに対する変更のため、UnitUserTokenではなく、アカウント認証によるCellLocalTokenが必須となる。

### 必要な権限
Cell Level ACLのauth権限
### 制限事項
なし


### リクエスト
#### リクエストURL
```
{CellName}/__mypassword
```
#### メソッド
PUT
#### リクエストクエリ
クエリは無視する
#### リクエストヘッダ
|ヘッダ名|概要|有効値|必須|備考|
|:--|:--|:--|:--|:--|
|Authorization|OAuth2.0形式で、認証情報を指定する|Bearer {CellLocalToken}|○|認証トークンは認証トークン取得APIで取得したセルローカルトークン|
|X-Personium-Credential|変更後パスワード|文字列|○|文字数：6&#65374;32文字<br>文字種:半角英数字と-(半角ハイフン)と_(半角アンダーバー)|
#### リクエストボディ
なし

#### リクエストサンプル
なし


### レスポンス
#### ステータスコード
204
#### レスポンスヘッダ
なし
#### レスポンスボディ
なし
#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.md)を参照

#### レスポンスサンプル
なし


### cURLサンプル
```sh
curl "https://{UnitFQDN}/{CellName}/__mypassword" -X PUT -i -H 'X-Personium-Credential: change_password' -H 'Authorization: Bearer {CellLocalToken}' -H 'Accept: application/json'
```

###### Copyright 2017 FUJITSU LIMITED
