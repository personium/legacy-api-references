# $formatクエリ
## 概要
$formatクエリを指定すると、クエリオプションによって指定されたメディアタイプでレスポンスが返却される。  
$formatのクエリオプション有効値は下表の通り。
## リクエストクエリ
|$format オプション|レスポンスされるデータの形式|
|:--|:--|
|atom|application/atom+xml|
|xml|application/xml|
|JSON|application/json|
|上記以外のIANA定義コンテンツ形式|IANA定義コンテンツ形式|
|ある独自ODataサービスに固有のフォーマットを表すサービス特化型の値|IANA定義コンテンツ形式|
## cURLサンプル
例：セル一覧をJSON形式で取得する場合:
```sh
curl "https://{UnitFQDN}/__ctl/Cell?\$format=JSON" -X GET -i -H 'Authorization: Bearer {AccessToken}'
```


