# $expand クエリ
### 概要
$expandクエリにNavigationProperty名を指定することで、関連情報を展開して取得することが可能。  
一覧取得時の関連情報の展開は最大100件まで展開する。  
一件取得時の関連情報の展開は最大10000件まで展開する。  
$expandで取得した関連データのソート順は下記の通り。  

|関連<br>|ソート条件<br>|順序<br>|
|:--|:--|:--|
|0 .. 1:0 .. 1<br>|展開するナビゲーションプロパティのエンティティ作成日時<br>|降順<br>|
|0 .. 1: *<br>|展開するナビゲーションプロパティのエンティティ作成日時<br>|降順<br>|
|*: 0 .. 1<br>|展開するナビゲーションプロパティのエンティティ作成日時<br>|降順<br>|
|*: *<br>|展開するナビゲーションプロパティとのリンク情報作成日時<br>|降順<br>|
※Multiplicityが"1"の場合は、"0..1"と同様のソート結果となる
### リクエストクエリ
```
$expand={NavigationPropertyName}
```
|Path<br>|概要<br>|
|:--|:--|
|{NavigationPropertyName}<br>|展開するナビゲーションプロパティ名<br>複数指定する場合はカンマ区切りで指定する<br>|
※$expandに存在しないNavigationProperty名を指定した場合は「400 Bad Request」を返却する
###### ナビゲーションプロパティ指定可能数
一覧取得時のナビゲーションプロパティは2件まで指定可能  
一件取得時のナビゲーションプロパティは10件まで指定可能  
※指定可能なナビゲーションプロパティ数を超えた場合は「400 Bad Request」を返却する
#### CURLサンプル
ナビゲーションプロパティに紐付く情報を展開して取得する
```
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/{EntityTypeName}('{EntityID}')?\$expand={NavigationPropertyName}" -X GET -i -H 'Authorization: Bearer {UnitUserToken}' -H 'Accept: application/json'
```
### 制限事項
* 関連情報の展開は1階層のみ可能
* 展開した関連情報がすべての件数を返却しているかを示すために、\__countを関連情報一覧の項目として追加する（未サポート）
<br>
<br>
<br>

###### Copyright 2017    FUJITSU LIMITED
