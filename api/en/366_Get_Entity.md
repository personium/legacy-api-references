# Get Entity

### Overview

We acquire one entity Entity of user data.

### Required Privileges

read

### Restrictions

* Always handles Content-Type in the request header as application/json
* Only accepts the request body in the JSON format
* Only application/json is supported for Content-Type in the request header and the JSON format for the response body
* Response body data is not ensured if atom or xml is specified in the $format query option, although it does not result in an error
* User data restrictions
* Property scope of Edm.DateTime type is not properly checked
* Array of Edm.DateTime type is not supported
* If SYSUTCDATETIME () is specified as the property of Edm.DateTime type, the set system time may be different

    * When setting in request body and setting with DefaultValue (__published, __ updated is the latter timing)

* For EntityType, you can create up to 400 DynamicProperty / DeclaredProperty / ComplexTypeProperty

<br>

### Request

#### Request URL

```
/{CellName}/{BoxName}/{ODataCollecitonName}/{EntityTypeName}({EntityID})}
```

| Path<br>                  | Overview<br>                    |
|:-- |:-- |
| {CellName}<br>            | Cell Name<br>                   |
| {BoxName}<br>             | Box Name<br>                    |
| {ODataCollecitonName}<br> | Collection Name<br>             |
| {EntityTypeName}<br>      | EntityType name<br>             |
| {EntityID}<br>            | ID of the Entity to acquire<br> |

#### Request Method

GET

#### Request Query

The following query parameters are available

| Header Name<br>   | Overview<br>                    | Effective Value<br>                                                                | Required<br> | Notes<br>                                                                                                        |
|:-- |:-- |:-- |:-- |:-- |
| p_cookie_peer<br> | Cookie Authentication Value<br> | The cookie authentication value returned from the server during authentication<br> | No<br>       | Valid only when no Authorization header is specified Specify when using authentication information of cookie<br> |

[\$select  Query](406_Select_Query.html)

[\$expand  Query](405_Expand_Query.html)

[\$format  Query](404_Format_Query.html)

#### Request Header

##### Common Request Header

| Header Name<br>            | Overview<br>                                       | Effective Value<br>                                                                                        | Required<br> | Notes<br>                                                                                                                    |
|:-- |:-- |:-- |:-- |:-- |
| X-HTTP-Method-Override<br> | Method override function<br>                       | User-defined<br>                                                                                           | No<br>       | Specifying this value in a request with the POST method indicates that the specified value is used as the method<br>         |
| X-Override<br>             | Header override function<br>                       | ${OverwrittenHeaderName}:${Value}<br>                                                                      | No<br>       | The normal HTTP header value is overwritten. Specify multiple X-Override headers for the overwriting of multiple headers<br> |
| X-Personium-RequestKey<br> | RequestKey field value output in the event log<br> | Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br> | No<br>       | Supported in V 1.1.7 and later<br>                                                                                           |

##### OData Common Request Header

| Header Name<br>   | Overview<br>                                                     | Effective Value<br>      | Required<br> | Notes<br>                                                                                          |
|:-- |:-- |:-- |:-- |:-- |
| Authorization<br> | Specifies authentication information in the OAuth 2.0 format<br> | Bearer {AccessToken}<br> | No<br>       | * Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br> |

##### OData Request Header

| Header Name<br>   | Overview<br>                                                                                           | Effective Value<br>  | Required<br> | Notes<br>                                                               |
|:-- |:-- |:-- |:-- |:-- |
| Accept <br>       | Specify the format of the response body <br>                                                           | application/json<br> | No<br>       | When omitted, treat it as [application/json] <br>                       |
| If-None-Match<br> | Specify the value of ETag, if there is no change, 304, if it is changed return the latest resource<br> | <br>                 | No<br>       | Specified when acquiring Entity not matching ETag<br>Not compatible<br> |

#### Request Body

None

#### Request Sample

None

<br>

### Response

#### Response Code

200

#### Response Header

| Item Name<br>          | Overview<br>                      | Notes<br>                                                |
|:-- |:-- |:-- |
| Content-Type<br>       | Format of data to be returned<br> | <br>                                                     |
| DataServiceVersion<br> | OData version information<br>     | Return only when Entity can be successfully acquired<br> |

#### Response Body

The response is a JSON object, the correspondence between the key (name) and type defined in the object (subobject) and the value is as follows

| Object<br> | Name(Key)<br>   | Type<br>   | Value<br>                                                                                      |
|:-- |:-- |:-- |:-- |
| Root<br>   | d<br>           | object<br> | Object{1}<br>                                                                                  |
| {1}<br>    | results<br>     | array<br>  | Array object {2}<br>                                                                           |
| {2}<br>    | __metadata<br>  | object<br> | Object{3}<br>                                                                                  |
| {3}<br>    | uri<br>         | string<br> | URL to the resource that was created<br>                                                       |
| {3}<br>    | etag<br>        | string<br> | Etag value<br>                                                                                 |
| {3}<br>    | type<br>        | string<br> | UserData.{EntityTypeName}<br>                                                                  |
| {2}<br>    | __id<br>        | string<br> | EntityID(__id)<br>                                                                             |
| {2}<br>    | __published<br> | string<br> | Creation date (UNIX time)<br>                                                                  |
| {2}<br>    | __updated<br>   | string<br> | Update date (UNIX time)<br>                                                                    |
| {2}<br>    | _{NP name}<br>  | string<br> | Object{4}<br>It is returned only when Link is connected. {NP name}: NavigationPropert name<br> |
| {4}<br>    | __deferred<br>  | object<br> | Object{5}<br>                                                                                  |
| {5}<br>    | uri<br>         | string<br> | uri of the resource that has the relationship<br>Not tested<br>                                |
| {1}<br>    | __count<br>     | string<br> | Get number of results in $inlinecount query<br>                                                |

Return items that were schema-set other than the above, or dynamic items specified at registration

##### Numerical treatment

##### Decimal value (Edm.Single type)

* The handling when acquiring UserOData in JSON format is as follows

    * The value that the decimal part such as 10.0 becomes 0 is returned as an integer value

##### Numerical value (Edm.Double type)

\*Double type handling in Personium follows the Java Double specification

* The handling when acquiring UserOData in JSON format is as follows

    * The value that the decimal part such as 10.0 becomes 0 is returned as an integer value

* About the value to be returned

    * When the input value at the time of registration is a number having accuracy of double precision or more, data is registered by being rounded to double precision

        * Internally, it is managed as a floating point number, but at the time of output, it converts it to fixed point number representation within the range where no information drop occurs and outputs it<br>
            When output fixed-point number is used for input, the same number of inputs and original number is guaranteed

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

```JSON
{
  "d": {
    "results": {
      "__metadata": {
        "uri": "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/{EntityTypeName}('{EntityID}')",
        "etag": "W/\"1-1487662179733\"",
        "type": "UserData.{EntityTypeName}"
      },
      "__id": "{EntityID}",
      "__published": "/Date(1487662179733)/",
      "__updated": "/Date(1487662179733)/",
      "PetName": null,
      "animalId": "100-1",
      "endedAt": "",
      "episodeType": "care",
      "name": "episode",
      "outcome": "During treatment",
      "startedAt": "2010-11-08"
    }
  }
}
```

<br>

### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{ODataCollecitonName}/{EntityTypeName}('{EntityID}')" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED