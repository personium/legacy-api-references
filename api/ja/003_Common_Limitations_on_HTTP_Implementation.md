# personium.ioのHTTP実装に関する制限事項

<br>
### リクエスト
#### URL
最大サイズ: 50KByte
※URL サイズ = ヘッダサイズ + （URLパスサイズ × ２） + （クエリサイズ × ３）  
※参考）IE7,8の制限値は2048byte、IE9は5120byte、その他の多くブラウザは1Mbyte程度
#### リクエストヘッダ
各ヘッダの上限サイズ： 4096byte （キーを含めての長さかどうかは不明）
##### Accept Encoding
gzip に対応しています。
#### リクエストボディ
##### Transfer-Encoding
chunked のリクエストに対応しています。
##### Content-Encoding
gzip のリクエストに対応予定ですが未対応です。
#### Time-out
60秒（リクエスト開始から、サーバがリクエストをすべて受領し終わるまでの時間）

<br>
### レスポンス
#### レスポンスヘッダ
|ヘッダ名<br>|説明<br>|
|:--|:--|
|Transfer-Encoding<br>|常にchunkedでレスポンスします<br>|
|Content-Encoding<br>|リクエストヘッダで有効なAccept-Encoding値を設定された場合、その値が入ります<br>|
|Date<br>|リクエストを受け付けたUTC時刻を返却します<br>|
#### レスポンスボディ
##### Size Limit
##### Transfer-Encoding
原則chunked でレスポンスを行う。
##### Content-Encoding
リクエストヘッダで有効なAccept-Encoding値を設定された場合は、ボディはその形式で圧縮エンコードされます。
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
