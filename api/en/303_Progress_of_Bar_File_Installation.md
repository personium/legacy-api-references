# Acquire Box Meta Data

### Overview

Get the metadata of Box. The metadata includes the following information.

* Box status<br><br>
    Box installation status (installation result, progress rate, error message, etc.)
* There are the following three types of states of Box

    * Box available
    * Box installation process in progress
    * Box installation process terminated abnormally

* Box Schema URL
* Box creation date and time

### Required Privileges

read

### Restrictions

* The expiration date that Box installation status can be confirmed is until the deadline set in Unit after completion of Box installation (including abnormal termination)
* If Box installation fails, you need to refer to the log output to EventBus

<br>

### Request

#### Request URL

```
/{CellName}/{BoxName}
```

#### Request Method

GET

#### Request Query

| Query Name<br>    | Overview<br>                    | Effective Value<br>                                                                | Required<br> | Notes<br>                                                                                                                |
|:-- |:-- |:-- |:-- |:-- |
| p_cookie_peer<br> | Cookie Authentication Value<br> | The cookie authentication value returned from the server during authentication<br> | No<br>       | Valid only if no Authorization header specified<br>Specify this when cookie authentication information is to be used<br> |

#### Request Header

| Header Name<br>            | Overview<br>                                                     | Effective Value<br>                                                                                        | Required<br> | Notes<br>                                                                                                         |
|:-- |:-- |:-- |:-- |:-- |
| X-HTTP-Method-Override<br> | Method override function<br>                                     | User-defined<br>                                                                                           | No<br>       | If you specify this value when requesting with the POST method, the specified value will be used as a method.<br> |
| X-Override<br>             | Header override function<br>                                     | ${OverwrittenHeaderName}:${Value} override} $: $ {value}<br>                                               | No<br>       | Overwrite normal HTTP header value. To overwrite multiple headers, specify multiple X-Override headers.<br>       |
| X-Personium-RequestKey<br> | RequestKey field value output in the event log<br>               | Single-byte alphanumeric characters, hyphens ("-"), and underscores ("_")<br>Maximum of 128 characters<br> | No<br>       | Supported in V 1.1.7 and later<br>                                                                                |
| Authorization<br>          | Specifies authentication information in the OAuth 2.0 format<br> | Bearer {AccessToken}<br>                                                                                   | No<br>       | * Authentication tokens are the tokens acquired using the Authentication Token Acquisition API<br>                |

#### Request Body

None

#### Request Sample

None

<br>

### Response

#### Response Code

| Code<br> | Message<br> | Overview<br>   | Notes<br>                                                            |
|:-- |:-- |:-- |:-- |
| 200<br>  | OK<br>      | On success<br> | Refer to response body for success / failure of Box installation<br> |

#### Response Header

| Header Name<br>                 | Overview<br>                                     | Notes<br>                                          |
|:-- |:-- |:-- |
| Access-Control-Allow-Origin<br> | Cross domain communication permission header<br> | Return value fixed to "*"<br>                      |
| X-Personium-Version<br>         | API version that the request is processed<br>    | Version of the API used to process the request<br> |

#### Response Body

Response is JSON format, defined in object (subobject).<br>
The correspondence between key (name) and type, and value are as follows.

| Object<br> | Name(Key)<br>    | Type<br>   | Value<br>                                                                                                  | Notes<br>                                                                                                                                                    |
|:-- |:-- |:-- |:-- |:-- |
| Root<br>   | schema<br>       | string<br> | The URL of the schema to which the Box is attached<br>                                                     | Null for no schema<br>                                                                                                                                       |
| Root<br>   | installed_at<br> | string<br> | Start time (ISO 8610 UTC format)<br>                                                                       | Do not output when status is one of the following.<br>- "Installation in Progress"<br>- "installation failed"<br>                                            |
| Root<br>   | started_at<br>   | string<br> | Start time (ISO 8610 UTC format)<br>                                                                       | Do not output when status is below.<br>- "Ready"<br>                                                                                                         |
| Root<br>   | progress<br>     | string<br> | Progress rate (for example, "30%")<br>                                                                     | Do not output when status is below.<br>- "Ready"<br>                                                                                                         |
| Root<br>   | message<br>      | object<br> | Object (message format)<br>                                                                                | Output only when status is below.<br>- "Installation failed"<br>For details, see the [error message list](004_Error_Messages.html)<br>                       |
| Root<br>   | status<br>       | string<br> | One of the following strings: <br>- "ready"<br>- "installation in progress"<br>- "installation failed"<br> | Box shows usable state<br>Box indicating that the installation process is in progress<br>Box indicates completion of installation (abnormal termination)<br> |

#### Error Messages

Refer to [Error Message List](004_Error_Messages.html)

#### Response Sample

After creating Box (including when Box installation is completed)

```JSON
{
  "schema": "https://example.com/app1/",
  "installed_at": "2017-02-13T09:00:00.000Z",
  "status": "ready"
}
```

During Box installation process

```JSON
{
  "schema": "https://example.com/app1/",
  "started_at": "2017-02-13T09:00:00.000Z",
  "progress": "81%",
  "status": "installation in progress"
}
```

When Box installation is completed (abnormal termination)<br>
(After expiration of Box installation, within the expiration date)

```JSON
{
  "schema": "https://example.com/app1/",
  "started_at": "2017-02-13T09:00:00.000Z",
  "progress": "81%",
  "message": {
      "code" : "PR409-OD-0003",
      "message" : {
          "lang" : "en",
          "value" : "The entity already exists."
      }
  },
  "status": "installation failed"

}
```

<br>

### cURL Command

```sh
curl "https://{UnitFQDN}/{CellName}/{BoxName}" -X GET -i -H 'Authorization: Bearer {AccessToken}' -H 'Accept: application/json'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED