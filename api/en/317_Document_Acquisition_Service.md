# Acquire service document

### Overview

Acquire a service document for schema definition of user data

### Required Privileges

read

### Restrictions

None

<br>

### Request

#### Request URL

```
/{CellName}/{BoxName}/{ODataCollecitonName}/$metadata
```

#### Request Method

GET

#### Request Query

##### Common Request Query

| Query Name<br>    | Overview<br>                    | Effective Value<br>                                                                | Required<br> | Notes<br>                                                                                                                |
|:-- |:-- |:-- |:-- |:-- |
| p_cookie_peer<br> | Cookie Authentication Value<br> | The cookie authentication value returned from the server during authentication<br> | No<br>       | Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br> |

#### Request Header

##### Common Request Header

| Header Name<br>            | Overview<br>                                       | Effective Value<br>                                                                                        | Required<br> | Notes<br>                                                                                                                    |
|:-- |:-- |:-- |:-- |:-- |
| X-HTTP-Method-Override<br> | Method override function<br>                       | User-defined<br>                                                                                           | No<br>       | Specifying this value in a request with the POST method indicates that the specified value is used as the method<br>         |
| X-Override<br>             | Header override function<br>                       | ${OverwrittenHeaderName}:${Value}<br>                                                                      | No<br>       | The normal HTTP header value is overwritten. Specify multiple X-Override headers for the overwriting of multiple headers<br> |
| X-Personium-RequestKey<br> | RequestKey field value output in the event log<br> | Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br> | No<br>       | Supported in V 1.1.7 and later<br>                                                                                           |

##### Individual Request Header

| Header Name<br> | Overview<br>                      | Effective Value<br>             | Required<br> | Notes<br>                                                                             |
|:-- |:-- |:-- |:-- |:-- |
| Accept<br>      | Format of data to be returned<br> | application / atomsvc + xml<br> | Yes<br>      | If not specified, the schema information of the user data schema will be acquired<br> |

#### Request Body

None

#### Request Sample

None

<br>

### Response

#### Response Code

200

#### Response Header

| Header Name<br>                 | Overview<br>                                     | Notes<br>                                          |
|:-- |:-- |:-- |
| Content-Type<br>                | Format of data to be returned<br>                | <br>                                               |
| DataServiceVersion<br>          | OData version information<br>                    | <br>                                               |
| Access-Control-Allow-Origin<br> | Cross domain communication permission header<br> | Return value fixed to "*"<br>                      |
| X-Personium-Version<br>         | API version that the request is processed<br>    | Version of the API used to process the request<br> |

#### Response Body

Response sample reference

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

```xml
<?xml version='1.0' encoding='utf-8'?>
<service xmlns="http://www.w3.org/2007/app" xml:base="https://{UnitFQDN}/{CellName}/{BoxName}/{OdataCollecitonName}/$metadata/" xmlns:atom="http://www.w3.org/2005/Atom" xmlns:app="http://www.w3.org/2007/app">
  <workspace>
    <atom:title>Default </atom:title>
    <collection href="EntityType">
      <atom:title>EntityType </atom:title>
    </collection>
    <collection href="AssociationEnd">
      <atom:title>AssociationEnd </atom:title>
    </collection>
    <collection href="ComplexTypeProperty">
      <atom:title>ComplexTypeProperty </atom:title>
    </collection>
    <collection href="Property">
      <atom:title>Property </atom:title>
    </collection>
    <collection href="ComplexType">
      <atom:title>ComplexType </atom:title>
    </collection>
  </workspace>
</service>
```

<br>

### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}/{OdataCollecitonName}/\$metadata' -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/atomsvc+xml'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED