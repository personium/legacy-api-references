# Common Limitations on HTTP Implementation of Personium

<br>

### Request

#### URL

Maximum Length: 50KByte<br>
\*URL Size = header size + (URL path size * 2) + (query size * 3) <br>
>limit) IE7, 8 Many reference the browser is about 1Mbyte 2048byte, IE9 is other 5120byte,

#### Request Header

The maximum size of each header: 4096 byte (whether or not including the length of the key is unknown)

##### Accept Encoding

Supports GZip

#### Request Body

##### Transfer-Encoding

I have to respond to a request for chunked. (Unconfirmed)

##### Content-Encoding

It does not support is expected to respond to a request for gzip.

#### Time-out

Time of 60 seconds from the start of the request, the server until you have received all of the request (unconfirmed)

<br>

### Response

#### Response Header

| Header Name<br>       | Description<br>                                                               |
|:-- |:-- |
| Transfer-Encoding<br> | It always respond with chunked<br>                                            |
| Content-Encoding<br>  | If set to a valid value Accept-Encoding request header contains the value<br> |
| Date<br>              | It returns the UTC time that the request has been accepted<br>                |

#### Response Body

##### Size Limit

##### Transfer-Encoding

Support to chunked request

##### Content-Encoding

If you set the value of a valid request Accept-Encoding header is compressed and encoded in the form of its body.<br><br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED