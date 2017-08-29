# $formatクエリ
### 概要
$formatクエリを指定すると、クエリオプションによって指定されたメディアタイプでレスポンスが返却される。  
$formatのクエリオプション有効値は下表の通り。
### リクエストクエリ
|$format オプション<br>|レスポンスされるデータの形式<br>|
|:--|:--|
|atom<br>|application/atom+xml<br>|
|xml<br>|application/xml<br>|
|json<br>|application/json<br>|
|上記以外のIANA定義コンテンツ形式<br>|IANA定義コンテンツ形式<br>|
|ある独自ODataサービスに固有のフォーマットを表すサービス特化型の値  <br>|IANA定義コンテンツ形式<br>|
### cURLサンプル
例：セル一覧をJSON形式で取得する場合:
```sh
curl "https://{UnitFQDN}/__ctl/Cell?\$format=json" -X GET -i -H 'Authorization: Bearer {AccessToken}'
```

<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
