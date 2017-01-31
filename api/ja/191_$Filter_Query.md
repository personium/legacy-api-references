﻿﻿# $filterクエリ
### 概要
一覧取得時に検索条件を指定する場合は$filterクエリを使用する
### Operator Support
|演算子<br>|意味<br>|記述例<br>|備考<br>|
|:--|:--|:--|:--|
|eq<br>|等号<br>|文字列の場合 ： $filter=itemKey eq 'searchValue'<br>数値の場合 ： $filter=itemKey eq 10<br>|文字列、数値、真偽値、NULLに対応<br>|
|ne<br>|否定等号<br>|文字列の場合 ： $filter=itemKey ne 'searchValue'<br>数値の場合 ： $filter=itemKey ne 10<br>|文字列、数値、真偽値、NULLに対応<br>|
|gt<br>|より大きい<br>|$filter=itemKey gt 1000<br>|文字列、数値に対応<br>|
|ge<br>|以上<br>|$filter=itemKey ge 1000<br>|文字列、数値に対応<br>|
|lt<br>|より小さい<br>|$filter=itemKey lt 1000<br>|文字列、数値に対応<br>|
|le<br>|以下<br>|$filter=itemKey le 1000<br>|文字列、数値に対応<br>|
|and<br>|論理積<br>|$filter=itemKey1 eq 'searchValue1' and itemKey2 eq 'searchValue2'<br>| <br>|
|or<br>|論理和<br>|$filter=itemKey1 eq 'searchValue1' or itemKey2 eq 'searchValue2'<br>| <br>|
|()<br>|優先グループ<br>|$filter=itemKey eq 'searchValue' or (itemKey gt 500 and itemKey lt 1500)<br>|括弧が片方のみの場合、括弧は無視される<br>|
### サポート関数
|Function<br>|概要<br>|Example<br>|備考<br>|
|:--|:--|:--|:--|
|startswith<br>|前方一致<br>|$filter=startswith(itemKey, 'searchValue')<br>|文字列のみ対応<br>|
|substringof<br>|部分一致<br>|$filter=substringof('searchValue1', itemKey1)<br>|文字列のみ対応<br>※英数字の部分一致は未対応<br>|
### プロパティ型別の検索仕様
#### Edm.String型
検索に指定したキーワードを文字列型に変換して検索する
#### Edm.Int32型
整数値またはnullが指定可能
文字列型で整数値を指定された場合は数値型に変換して検索する
#### Edm.Single型
整数、小数値またはnullの指定が可能
文字列型で整数、小数値を指定された場合は小数値に変換して検索を行う
#### Edm.Boolean型
真偽値またはnullのみ指定可能
### 特記事項
※クエリを指定する際はURLエンコードが必要
※検索対象のプロパティの有効値は半角英数字と-(半角ハイフン),\_(半角アンダーバー)が有効。
※検索文字列にシングルクオートを含めたい場合、シングルクオートを２個「''」記述することでシングルクオートを検索文字列に指定することができる
※$filterに存在しないプロパティ名を指定した場合、検索条件に含めて検索を行う
※\__updated, \__publishedを指定する場合は、UNIX時間（"/Date()"の括弧内の数字）で指定
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
