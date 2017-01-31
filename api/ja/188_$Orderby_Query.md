﻿﻿# $orderby クエリ
### 概要
一覧取得時に検索結果を並べ替える場合は$orderbyクエリを使用する
※$orderbyに存在しないプロパティ名を指定した場合は、指定された項目を無視する
### リクエストクエリ
```
$orderby={sortKey} {option}, ・・・
```
※ sortkey} = {propertyName} {
※ {sortkey}} {option}はカンマ区切りで複数指定可能

|Path<br>|概要<br>|
|:--|:--|
|{PropertyName}<br>|並び替えのキーに指定するプロパティ名<br>|
|{Option}<br>|並び替え方法<br>asc:昇順<br>desc:降順<br>デフォルト値:asc<br>|
##### CURLサンプル
```
$orderby=Test%20desc
```
```
$orderby=ID%20asc,Test%20desc
```
### 動作詳細
* null値を含む場合のソート順序
	昇順を指定した場合、降順を指定した場合ともにnullはソート結果の末尾になるようにソートします。
	※ただし、マイナーバージョン 0.19.9を使用している場合は、以下の規則に従いソートします。

* 文字列型に対するソート
	* asc
		null⇒文字列
	* desc
		文字列⇒null
* 数値型
	* asc
		負数⇒null⇒0⇒なし⇒正数
	* desc
		正数⇒null⇒0⇒なし⇒負数

* $orderbyに存在しないプロパティ名を指定した場合は、指定された項目を無視する
* $orderbyに配列型のプロパティ名を指定した場合は、400エラーを返却する
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
