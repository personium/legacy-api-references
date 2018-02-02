# Other object registration via # Rule's Navigation Property
## Overview

Register other Cell control objects via Rule's Navigation Property. By doing this, it is possible to register objects linked from the beginning.

### Required Permissions
* rule
* box

### Limitations
* Accept of request header is ignored
* Treat all Content-Type of the request header as application / json
* Accept only JSON format for request body
* Content-Type of the response header supports only application / json, and the response body is JSON format
* $format query option ignored


## request
### Request URL
#### navigationProperty to Box
```
/{CellName}/__ctl/Rule(Name='{RuleName}',_Box.Name=null)/_Box
```
Alternatively,
```
/{CellName}/__ctl/Rule(Name='{RuleName}')/_Box
```
Alternatively,
```
/{CellName}/__ctl/Rule('{RuleName}')/_Box
```
* When the \_Box.Name parameter is omitted, it is assumed that null is specified
### Method
POST
### Request query
| Query name | Summary | Valid values | Required | Remarks |
|:--|:--|:--|:--|:--|
| p_cookie_peer | cookie authentication value | cookie authentication value returned from the server at the time of authentication | No | valid only when there is no specification of the authorization header <br> specifying when using the authentication information of the cookie |
### Request Header
| Header name | overview | effective value | required | remarks |
|:--|:--|:--|:--|:--|
| X-HTTP-Method-Override | method override function | optional | No | If you specify this value when requesting with the POST method, the specified value will be used as a method. |
| X-Override | header override function | ${overwrite header name}: ${value} | No | overwrites the value of normal HTTP header. To overwrite multiple headers, specify multiple X-Override headers. |
| X-Personium-RequestKey | Value of the RequestKey field to be output to the event log | Single byte alphanumeric characters, - (half size hyphen) and _ (half width underscore) Maximum 128 characters | No | PCS-${ UNIX time}
| Authorization | Specify authentication information in OAuth 2.0 format | Bearer {AccessToken} | No | * Authentication token acquired with the authentication token acquisition API Token |
| Content-Type | Specify the format of the request body | application / json | No | treat it as [application / json] when omitted |
| Accept | Specify the response body format | application / json | No | treat as [application / json] when omitted |
### Request body
| Item name | overview | effective value | required | remarks |
|:--|:--|:--|:--|:--|
| Name | Box name | Number of digits: 1 to 128 <br> Character type: Half size alphanumeric characters - (half size hyphen) and _ (half width underscore) However, in the first letter - (half size hyphen) and _ ) Can not be specified | Yes ||
| Schema | Schema name | Number of digits: 1 to 128 <br> Character type: Half size alphanumeric characters - (half size hyphen) and _ (half width underscore) However, in the first letter - (half size hyphen) and _ ) Can not be specified <br> null | No ||

### Request sample
```JSON
{"Name":"{BoxName}", "Schema":"https://{UnitFQDN}/{CellName}/"}
```
## Response
### Status code
201
### Response header
| Header name | Overview | Remarks |
|:--|:--|:--|
| Content-Type | Format of data to be returned ||
| Location | URL to created resource ||
| DataServiceVersion | OData version ||
| ETag | resource version information ||
| Access-Control-Allow-Origin | Cross-domain communication permission header | Return value is fixed as "*"
| X-Personium-Version | Execution version of API | API version processed request |
### Response body
| Object | Item name | Data Type | Remarks |
|:--|:--|:--|:--|
| Route | d | object | object {1} |
| {1} | results | array | Array of objects {2}
| {2} | __metadata | object | object {3} |
| {3} | uri | string | URL to created resource |
| {3} | etag | string | Etag value |
| {2} | __published | string | creation date (UNIX time) |
| {2} | __updated | string | Update date (UNIX time) |
| {1} | __count | string | $inlinecount number of results obtained by query |

### Box-specific response body
| Object | Item name | Data Type | Remarks |
|:--|:--|:--|:--|
| {3} | type | string | CellCtl.Box |
| {2} | Name | string | Box name |
| {2} | Schema | string | Schema name |

### Response sample
```JSON
{
  "d": {
    "results": {
      "__metadata": {
        "uri": "https://{UnitFQDN}/{CellName}/__ctl/Box('{BoxName}')",
        "etag": "W/\"1-1486368212581\"",
        "type": "CellCtl.Box"
      },
      "Name": "{BoxName}",
      "Schema": null,
      "__published": "/Date(1486368212581)/",
      "__updated": "/Date(1486368212581)/"
    }
  }
}
```
### Error message list
[Error message list] (004_Error_Messages.md) is referred to

## cURL Sample

```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/Rule('{RuleName}')/_Box" -X POST -i -H \
'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"Name":"{BoxName}"}'
```
