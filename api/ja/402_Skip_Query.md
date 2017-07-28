# $skip クエリ
### 概要
$skipクエリは、コレクションのうち指定した自然数Nの数だけ省き、N+1件目からの情報を取得する際に用いる。  
以下の表記で記述する。
```
$skip={number}
```
### CURLサンプル
例：10セルの取得を省き、11セル目からの情報を取得する場合:
```sh
curl "https://{UnitFQDN}/__ctl/Cell?\$skip=10" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
