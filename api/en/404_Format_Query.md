# \$format Query

### Overview

When a \$format query is specified, the response is returned with the media type specified by the query option.<br>
Query options for \$format Valid values are shown in the table below.

### Request Query

|$format Option<br>|Format of response data<br>|
|:--|:--|
|atom<br>|application/atom+xml<br>|
|xml<br>|application/xml<br>|
|JSON<br>|application/json<br>|
|Other IANA-defined content formats<br>|IANA definition content format<br>|
|A service-specific value representing a format unique to a certain OData service<br>|IANA definition content format<br>|

### cURL Command

Example: When acquiring the cell list in JSON format:

```sh
curl "https://{UnitFQDN}/__ctl/Cell?\$format=JSON" -X GET -i -H 'Authorization: Bearer {AccessToken}'
```

<br><br><br><br><br>

###### Copyright 2017 FUJITSU LIMITED