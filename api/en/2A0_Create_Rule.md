# Create Rule

## Overview

API for creating a new Event Processing Rule.

## Required Privileges

rule

## Restrictions

* Accept in the request header is ignored
* Always handles Content-Type in the request header as application/json
* Only accepts the request body in the JSON format
* Only application/json is supported for Content-Type in the request header and the JSON format for the response body
* $formatQuery options ignored


## Request

### Request URL

```
/{CellName}/__ctl/Rule
```

### Request Method

POST

### Request Query

|Query Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|p_cookie_peer|Cookie Authentication Value|The cookie authentication value returned from the server during authentication|No|Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used|

### Request Header

|Header Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override|Method override function|User-defined|No|If you specify this value when requesting with the POST method, the specified value will be used as a method.|
|X-Override|Header override function|${OverwrittenHeaderName}:${Value}|No|Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.|
|X-Personium-RequestKey|RequestKey field value output in the event log|Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters|No|PCS-${UNIXtime} by default|
|Authorization|Specifies authentication information in the OAuth 2.0 format|Bearer {AccessToken}|No|* Authentication tokens are the tokens acquired using the Authentication Token Acquisition API|
|Content-Type|Specifies the request body format|application/json|No|[application/json] by default|
|Accept|Specifies the response body format|application/json|No|[application/json] by default|

### Request Body

#### Format

JSON

|Item Name|Overview|Effective Value|Required|Notes|
|:--|:--|:--|:--|:--|
|_Box.Name|Name of the box to which the rule should be bound.|Valid box name. Request without this key or with this key null value means the Rule should not be bound to any box.|No||
|Name|Arbitrary name to make the rule to be created identifiable|Should be unique within the scope of the bound box, or the cell (in the case of box-unbound rules)|No|if omitted, an uuid will automatically be assigned.|
|EventType|Type of the event to trigger the rule|Number of digits: 1 - 128<br>Character type: Single-byte alphanumeric characters, hyphens ("-"), and underscores ("\_")<br>However, the string cannot start with a single-byte hyphen ("-") or underscore ("\_")<br>null|No||
|EventSubject|Event Subject prefix to trigger the rule|Since the Event Subject is basically Cell URL, effective value is its substring.|No||
|EventObject|Event Object prefix to trigger the rule|Event object values vary with the types of events. Meaningful value depends of the type. though any string can be accepted. |No||
|EventInfo|Event Info prefix to trigger the rule|Event info values vary with the types of events. Meaningful value depends of the type. though any string can be accepted.|No||
|EventExternal|Flag if the triggering event should be external|Boolean value. Set true to pick external events. |No|default false|
|Action|Action to invoke when the matching event is met|Valid values are listed in the separate table below|Yes||
|TargetUrl|Specific target url of the action|Meaning of this field changes with the Action field. |No||

Valid Actions

|Action|Description|TargetUrl|Note|
|:--|:--|:--|:--|
|log|Events will be logged at info level|-|-|
|log.info|Events will be logged at info level|-|-|
|log.warn|Events will be logged at warn level|-|-|
|log.error|Events will be logged at error level|-|-|
|exec|Engine script will be invoked with post method|engine service endpoint url|-|
|relay|Events will be relayed to TargetUrl|Url to which event info should be relayed.|-|


### Request Body Sample

```JSON
{
  "Name":"warninglog", 
  "EventType":"", 
  "Action": "log.warn"
}
```


## Response

### Successful Response Code

201

### Response Header

|Header Name|Overview|Notes|
|:--|:--|:--|
|X-Personium-Version|API version that the request is processed|Version of the API used to process the request|
|Access-Control-Allow-Origin|Cross domain communication permission header|Return value fixed to "*"|
|Content-Type|Format of data to be returned||
|Location|URL to the resource that was created||
|ETag|Resource version information||
|DataServiceVersion|OData version||

### Response Body

The response is a JSON object, the correspondence between the key (name) and type defined in the object (subobject) and the value are as follows

|Object|Item Name|Data Type|Notes|
|:--|:--|:--|:--|
|Root|d|object|Object{1}|
|{1}|results|array|Array object {2}|
|{2}|__metadata|object|Object{3}|
|{3}|uri|string|URL to the resource that was created|
|{3}|etag|string|Etag value|
|{2}|__published|string|Creation date (UNIX time)|
|{2}|__updated|string|Update date (UNIX time)|
|{1}|__count|string|Get number of results in $inlinecount query|

### Box specific response body

|Object|Item Name|Data Type|Notes|
|:--|:--|:--|:--|
|{3}|type|string|CellCtl.Box|
|{2}|Name|string|Box Name|
|{2}|Schema|string|Schema Name|

### Response Body Sample

```JSON
{
  "d": {
    "results": {
      "__metadata": {
        "uri": "https://{UnitFQDN}/{CellName}/__ctl/Rule('{BoxName}')",
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

### Error Responses

Refer to [Error Message List](004_Error_Messages.md)


## cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/Rule" -X POST -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json' -d '{"Name":"{RuleName}"}'
```



