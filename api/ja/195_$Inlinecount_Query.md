# $inlinecount クエリ
### 概要
一覧取得時に取得結果件数を指定する場合は$inlinecountクエリを使用する  
省略した場合は、レスポンスに取得結果件数を含まない
### 有効値
|Value<br>|概要<br>|Example<br>|備考<br>|
|:--|:--|:--|:--|
|allpages<br>|取得結果件数を含める  <br>|$inlinecount=allpages<br>| <br>|
|none<br>|取得結果件数を含めない  <br>|$inlinecount=none<br>| <br>|
### CURLサンプル
例：セル一覧を取得結果件数を含め取得する場合:
```sh
curl "https://{UnitFQDN}/__ctl/Cell?\$inlinecount=allpages" -X GET -i -H 'Authorization: Bearer {UnitUserToken}' -H 'Accept: application/json'
```

<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
