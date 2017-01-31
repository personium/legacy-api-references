﻿﻿# $select クエリ
### 概要
返却するプロパティを指定する場合、$selectクエリを使用する
省略した場合は、取得したすべてのプロパティを返却する
ただし、以下のプロパティについては$selectに指定しなくても、必ず返却する  
* \__id
* \__published
* \__updated
* \__metadata

※$selectに存在しないプロパティ名を指定した場合、指定された項目を無視する
※$selectで指定したDynamicプロパティの値がNULLの場合は、プロパティ値を取得することはできない
※プロパティ名は「'」(シングルクォート)で囲わずに指定する
### リクエストクエリ
```
$select={propertyName}
```
※省略時は $select=* となる

|Path<br>|概要<br>|
|:--|:--|
|{PropertyName}<br>|返却するプロパティ名<br>複数指定する場合はカンマ区切りで指定する<br>|
#### CURLサンプル
1. NameとCategoryを返却する
```
$select=Name,Category
```
2. すべてのプロパティを返却する
```
$select=*
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
