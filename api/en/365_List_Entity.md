# Get Entity list

### Overview

Get a list of Entities of user data.

### Required Privileges

read

### Restrictions

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
/{CellName}/{BoxName}/{ODataCollecitonName}/{EntityTypeName}
```

| Path<br>                  | Overview<br>        |
|:-- |:-- |
| {CellName}<br>            | Cell Name<br>       |
| {BoxName}<br>             | Box Name<br>        |
| {ODataCollecitonName}<br> | Collection Name<br> |
| {EntityTypeName}<br>      | EntityType name<br> |

#### Request Method

GET

#### Request Query

The following query parameters are available

| Query Name<br>    | Overview<br>                    | Effective Value<br>                                                                | Required<br> | Notes<br>                                                                                                                |
|:-- |:-- |:-- |:-- |:-- |
| p_cookie_peer<br> | Cookie Authentication Value<br> | The cookie authentication value returned from the server during authentication<br> | No<br>       | Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br> |

[\$select  Query](406_Select_Query.html)

[\$expand  Query](405_Expand_Query.html)

[\$format  Query](404_Format_Query.html)

[\$filter  Query](403_Filter_Query.html)

[\$inlinecount  Query](407_Inlinecount_Query.html)

[\$orderby  Query](400_Orderby_Query.html)

[\$top  Query](401_Top_Query.html)

[\$skip  Query](402_Skip_Query.html)

[Full-text Search (q) Query](408_Full_Text_Search_Query.html)

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