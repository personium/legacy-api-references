# Get Cell Root

## Overview

Get the HTML file set as the cell route.

### Precondition

It is necessary to [set property](./291_Cell_Change_Property.md) of target cell.
```xml
<p:relayhtmlurl>{URL that html can obtain}</p:relayhtmlurl>
```
The schemes that can be specified as URL are "http", "https", "personium-localunit", "personium-localcell".

### Required Privileges

None

### Restrictions

None


## Request

### Request URL

```
/{CellName}
```

### Request Method

GET

### Request Query

None

### Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|Accept|Specifies the response body format|text/html|No|[text/html] by default|

### Request Body

None

### Request Sample

None


## Response

### Response Code

200

### Response Header

|Header Name|Overview|Notes|
|:--|:--|:--|
|Content-Type|Format of data to be returned||
|Access-Control-Allow-Origin|Cross domain communication permission header|Return value fixed to "*"|
|X-Personium-Version|Personium API version|Version of the API used to process the request|

### Response Body

HTML obtained from the URL set in the property of the target cell.

### Error Messages

Refer to [Error Message List](004_Error_Messages.md)

### Response Sample

None


## cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}" -X GET -i -H 'Accept: text/html'
```
