# Role\_$links Acquire List

### Overview

Get a list of OData resources associated with Role  
You can specify the following OData resources

* Account
* ExtCell
* ExtRole
* Relation

### Required Privileges

Unpublished

### Restrictions

* Accept in the request header is ignored
* Always handles Content-Type in the request header as application/json
* Only accepts the request body in the JSON format
* Only application/json is supported for Content-Type in the request header and the JSON format for the response body
* $formatQuery options ignored


### Request

#### Request URL

##### Association with the account

```
/{CellName}/__ctl/Role(Name='{RoleName}',_Box.Name='{BoxName}')/$links/_Account
```

or

```
/{CellName}/__ctl/Role(Name='{RoleName}')/$links/_Account
```

or

```
/{CellName}/__ctl/Role('{RoleName}')/$links/_Account
```

##### Association with the ExtCell

```
/{CellName}/__ctl/Role(Name='{RoleName}',_Box.Name='{BoxName}')/$links/_ExtCell
```

or

```
/{CellName}/__ctl/Role(Name='{RoleName}')/$links/_ExtCell
```

or

```
/{CellName}/__ctl/Role('{RoleName}')/$links/_ExtCell
```

##### Association with the ExtRole

```
/{CellName}/__ctl/Role(Name='{RoleName}',_Box.Name='{BoxName}')/$links/_ExtRole
```

or

```
/{CellName}/__ctl/Role(Name='{RoleName}')/$links/_ExtRole
```

or

```
/{CellName}/__ctl/Role('{RoleName}')/$links/_ExtRole
```

##### Association with the relation

```
/{CellName}/__ctl/Role(Name='{RoleName}',_Box.Name='{BoxName}')/$links/_Relation
```

or

```
/{CellName}/__ctl/Role(Name='{RoleName}')/$links/_Relation
```

or

```
/{CellName}/__ctl/Role('{RoleName}')/$links/_Relation
```

If the \_Box.Name parameter is omitted, it is assumed that null is specified

#### Request Method

GET

#### Request Query

The following query parameters are available

|Query Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|p_cookie_peer|Cookie Authentication Value|The cookie authentication value returned from the server during authentication|No|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used|

[$select  Query](406_Select_Query.html)

[$expand  Query](405_Expand_Query.html)

[$format  Query](404_Format_Query.html)

[$filter  Query](403_Filter_Query.html)

[$inlinecount  Query](407_Inlinecount_Query.html)

[$orderby  Query](400_Orderby_Query.html)

[$top  Query](401_Top_Query.html)

[$skip  Query](402_Skip_Query.html)

[Full-text Search (q) Query](408_Full_Text_Search_Query.html)

#### Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|Method override function|User-defined|No|If you specify this value when requesting with the POST method, the specified value will be used as a method.|
|X-Override|Header override function|${OverwrittenHeaderName}:${Value}|No|Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.|
|X-Personium-RequestKey|RequestKey field value output in the event log|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters|No|PCS-${UNIXtime} by default<br>Supported in V 1.1.7 and later|
|Authorization|Specifies authentication information in the OAuth 2.0 format|Bearer {AccessToken}|No|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API|
|Accept|Specifies the response body format|application/json|No|[application/json] by default|

#### Request Body

None

#### Request Sample

None


### Response

#### Response Code

200

#### Response Header

|Header Name|Overview|Notes|
|:--|:--|:--|
|Content-Type|Format of data to be returned||
|DataServiceVersion|OData version||
|Access-Control-Allow-Origin|Cross domain communication permission header|Return value fixed to "*"|
|X-Personium-Version|API version that the request is processed|Version of the API used to process the request|

#### Response Body

|Object|Item Name|Data Type|Notes|
|:--|:--|:--|:--|
|Root|d|object|Object{1}|
|{1}|results|array|Array object {2}|
|{2}|uri|string|URI of the linked OData resource|

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

```JSON
{
  "d": {
    "results": [
      {
        "uri": "https://{UnitFQDN}/{CellName}/__ctl/Box('{BoxName}')"
      }
    ]
  }
}
```


### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/Role(Name='{RoleName}',_Box.Name='{BoxName}')/\$links/_Box" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```


###### Copyright 2017 FUJITSU LIMITED
