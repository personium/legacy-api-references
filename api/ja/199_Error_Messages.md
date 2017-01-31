# エラーメッセージ一覧

### 認証系 API
|OAUTH エラーコード<br>|メッセージコード<br>|メッセージ<br>|
|:--|:--|:--|
|unsupported_grant_type<br>|PR400-AN-0001<br>|Unsupported grant type.<br>|<br>|
|invalid_request<br>|PR400-AN-0002<br>|Invalid dc_target.<br>|<br>|
|invalid_client<br>|PR400-AN-0003<br>|Failed to parse client secret.<br>|<br>|
|invalid_client<br>|PR400-AN-0004<br>|Client secret is expired and invalid.<br>|<br>|
|invalid_client<br>|PR400-AN-0005<br>|Client secret dsig is invalid.<br>|<br>|
|invalid_client<br>|PR400-AN-0006<br>|Client secret issuer does not match the client_id.<br>|<br>|
|invalid_client<br>|PR400-AN-0007<br>|Client secret target is wrong.<br>|<br>|
|invalid_grant<br>|PR400-AN-0008<br>|Trans-Cell access can not represent owner.<br>|<br>|
|invalid_grant<br>|PR400-AN-0009<br>|Token parse error.<br>|<br>|
|invalid_grant<br>|PR400-AN-0010<br>|Token expired or invalid.<br>|<br>|
|invalid_grant<br>|PR400-AN-0011<br>|Token dsig is invalid.<br>|<br>|
|invalid_grant<br>|PR400-AN-0012<br>|Token target is wrong. target=[{0}]<br>|<br>|
|invalid_grant<br>|PR400-AN-0013<br>|Not a refresh token.<br>|<br>|
|invalid_grant<br>|PR400-AN-0014<br>|Not allowed to represent owner.<br>|<br>|
|invalid_grant<br>|PR400-AN-0015<br>|Cell owner does not exist.<br>|<br>|
|invalid_request<br>|PR400-AN-0016<br>|Required parameter [{0}] missing.<br>|<br>|
|invalid_grant<br>|PR400-AN-0017<br>|Authentication failed.<br>|<br>|
|invalid_client<br>|PR400-AN-0018<br>|Authorization header is invalid.<br>|<br>|
|invalid_grant<br>|PR400-AN-0019<br>|Authentication failed.<br>|<br>|
|invalid_grant<br>|PR400-AN-0030<br>|Wrong IDToken Audience [{0}].<br>|<br>|
|invalid_grant<br>|PR400-AN-0031<br>|OpenID Connect Authentication failed.<br>|<br>|
|invalid_grant<br>|PR400-AN-0032<br>|OpenID Connect Invalid Token. ({0})<br>|<br>|
|invalid_grant<br>|PR400-AN-0033<br>|OpenID Connect ID Token Expired (at UnixTime: {0}).<br>|<br>|

### その他 API
|レスポンスコード<br>|メッセージコード<br>|メッセージ<br>|詳細<br>|
|:--|:--|:--|:--|
|400<br>|PR400-OD-0001<br>|JSON parse error.<br>|<br>|
|400<br>|PR400-OD-0002<br>|OData Query parse error.<br>|<br>|
|400<br>|PR400-OD-0003<br>|OData $filter Parse Error.<br>|<br>|
|400<br>|PR400-OD-0004<br>|OData EntityKey Parse error.<br>|<br>|
|400<br>|PR400-OD-0005<br>|$format value [{0}] is invalid.<br>|<br>|
|400<br>|PR400-OD-0006<br>|[{0}] field format error.<br>|<br>|
|400<br>|PR400-OD-0007<br>|[{0}]<br>|<br>|
|400<br>|PR400-OD-0008<br>|No such association.<br>|<br>|
|400<br>|PR400-OD-0009<br>|[{0}] is required.<br>|<br>|
|400<br>|PR400-OD-0010<br>|Key for Navigation Property should not be specified in $links POST.<br>|<br>|
|400<br>|PR400-OD-0011<br>|Key for Navigation Property should be specified in $links.<br>|<br>|
|400<br>|PR400-OD-0012<br>|Specifying the type of [{0}] is invalid.<br>|<br>|
|400<br>|PR400-OD-0013<br>|$inlinecount value [{0}] is invalid.<br>|<br>|
|400<br>|PR400-OD-0014<br>|Unknown property was appointed.<br>|<br>|
|400<br>|PR400-OD-0015<br>|Odata $orderby parse error.<br>|<br>|
|400<br>|PR400-OD-0016<br>|Single key should not be null.<br>|<br>|
|400<br>|PR400-OD-0017<br>|OData $select Parse error.<br>|<br>|
|400<br>|PR400-OD-0018<br>|Number of properties exceeds the limit [{0}].<br>|<br>|
|400<br>|PR400-OD-0019<br>|AssociationEnd do not put request.<br>|<br>|
|400<br>|PR400-OD-0020<br>|Schema mismatch.<br>|<br>|
|400<br>|PR400-OD-0021<br>|$batch body format is invalid. Request header format is invalid [{0}].<br>|<br>|
|400<br>|PR400-OD-0022<br>|$batch body format is invalid. Changeset should not have other changeset.<br>|<br>|
|400<br>|PR400-OD-0023<br>|$batch body parse error.<br>|<br>|
|400<br>|PR400-OD-0024<br>|[{0}] does not exist.<br>|<br>|
|400<br>|PR400-OD-0025<br>|[{0}] does not exist.<br>|<br>|
|400<br>|PR400-OD-0026<br>|OData $expand parse error.<br>|<br>|
|400<br>|PR400-OD-0027<br>|The index definition of another type already exists.<br>|<br>|
|400<br>|PR400-OD-0028<br>|OData EntityKey for $links parse error.<br>|<br>|
|400<br>|PR400-OD-0029<br>|Query value [{0}]=[{1}] is invalid.<br>|<br>|
|400<br>|PR400-OD-0030<br>|$batch body format is invalid. Too many requests. [{0}]<br>|<br>|
|400<br>|PR400-OD-0031<br>|Cannot create $links because multiplicity is [1:1].<br>|<br>|
|400<br>|PR400-OD-0032<br>|EntityType structure hierarchy exceeds the limit or number of properties exceeds the limit.<br>|<br>|
|400<br>|PR400-OD-0033<br>|EntityType count exceeds the limit.<br>|<br>|
|400<br>|PR400-OD-0034<br>|$batch body format is invalid. Request path format is invalid [{0}].<br>|<br>|
|400<br>|PR400-OD-0035<br>|$batch body format is invalid. Request Method not allowed [{0}].<br>|<br>|
|400<br>|PR400-OD-0036<br>|OData Query [{0}] parse error.<br>|<br>|
|400<br>|PR400-OD-0037<br>|Total of the specified value of $top exceeds the limit.<br>|<br>|
|400<br>|PR400-OD-0038<br>|$links count exceeds the limit.<br>|<br>|
|400<br>|PR400-OD-0039<br>|Total of the specified value of $expand exceeds the limit.<br>|<br>|
|400<br>|PR400-OD-0040<br>|Cannot specify the list type to $orderby.<br>|<br>|
|400<br>|PR400-OD-0041<br>|Request header {0} value is invalid [{1}].<br>|<br>|
|400<br>|PR400-OD-0042<br>|Operation is not supported: {0}<br>|<br>|
|400<br>|PR400-OD-0043<br>|Unsupported operator specified in query.<br>|<br>|
|400<br>|PR400-OD-0044<br>|Unsupported function specified in query.<br>|<br>|
|400<br>|PR400-OD-0045<br>|An unknown property with name ''{0}'' was found.<br>|<br>|
|400<br>|PR400-OD-0046<br>|Mismatched operand/argument is specified for ''{0}''.<br>|<br>|
|400<br>|PR400-OD-0047<br>|Operand or argument for ''{0}'' has unsupported/invalid format.<br>|<br>|
|400<br>|PR400-OD-0048<br>|Unable to parse operand or argument. ''{0}''<br>|<br>|
|400<br>|PR400-OD-0049<br>|[{0}] field format error. Cell URL should be normalized URL with http(s) scheme and trailing slash.<br>|<br>|
|400<br>|PR400-OD-0050<br>|[{0}] field format error. Schema URI should be either normalized URL with http(s) scheme and trailing slash, or URN.<br>|<br>|
|404<br>|PR404-OD-0000<br>|Not found.<br>|<br>|
|404<br>|PR404-OD-0001<br>|No such entity set.<br>|<br>|
|404<br>|PR404-OD-0002<br>|No such entity.<br>|<br>|
|404<br>|PR404-OD-0003<br>|No such Navigation Property.<br>|<br>|
|409<br>|PR409-OD-0001<br>|This entity has related data.<br>|<br>|
|409<br>|PR409-OD-0002<br>|Links exists already.<br>|<br>|
|409<br>|PR409-OD-0003<br>|The entity already exists.<br>|<br>|
|409<br>|PR409-OD-0004<br>|The entity already exists. [{0}]<br>|<br>|
|409<br>|PR409-OD-0005<br>|The entity already exists. [{0}]<br>|<br>|
|409<br>|PR409-OD-0006<br>|Relation between EntityTypes already exists.<br>|<br>|
|412<br>|PR412-OD-0001<br>|If-Match header is required.<br>|<br>|
|412<br>|PR412-OD-0002<br>|ETag does not match.<br>|<br>|
|412<br>|PR412-OD-0003<br>|Conflict.<br>|<br>|
|415<br>|PR415-OD-0001<br>|Unsupported media type:[{0}].<br>|<br>|
|500<br>|PR500-OD-0001<br>|Detected a duplicate of the property name:[{0}].<br>|<br>|
|500<br>|PR500-OD-0002<br>|Internal data inconsistency detected.<br>|<br>|
|400<br>|PR400-DV-0001<br>|XML parse error.<br>|<br>|
|400<br>|PR400-DV-0002<br>|XML content error.<br>|<br>|
|400<br>|PR400-DV-0003<br>|Invalid depth header value:[{0}].<br>|<br>|
|400<br>|PR400-DV-0004<br>|Role not found.<br>|<br>|
|400<br>|PR400-DV-0005<br>|Box not found url:[{0}].<br>|<br>|
|400<br>|PR400-DV-0006<br>|XML validate error.<br>|<br>|
|400<br>|PR400-DV-0007<br>|Cannot add any more child resources.<br>|<br>|
|400<br>|PR400-DV-0008<br>|Hierarchy of the collection is too deep.<br>|<br>|
|400<br>|PR400-DV-0009<br>|Request header {0} value is invalid [{1}].<br>|<br>|
|400<br>|PR400-DV-0010<br>|Header {0} is required.<br>|<br>|
|400<br>|PR400-DV-0011<br>|Prohibited to move service source collection.<br>|<br>|
|400<br>|PR400-DV-0012<br>|Prohibited to overwrite collection resource.<br>|<br>|
|400<br>|PR400-DV-0013<br>|Prohibited to move resource to OData collection.<br>|<br>|
|400<br>|PR400-DV-0014<br>|Prohibited to move resource to file.<br>|<br>|
|400<br>|PR400-DV-0015<br>|Prohibited to move box.<br>|<br>|
|400<br>|PR400-DV-0016<br>|Prohibited to move resource to service collection.<br>|<br>|
|400<br>|PR400-DV-0017<br>|Prohibited to overwrite service source collection.<br>|<br>|
|400<br>|PR400-DV-0018<br>|Prohibited to create collection under service source collection.<br>|<br>|
|403<br>|PR403-DV-0001<br>|Infinite depth is not supported.<br>|<br>|
|403<br>|PR403-DV-0003<br>|Cannot delete collection if it has any child resources.<br>|<br>|
|403<br>|PR403-DV-0004<br>|Resource name is invalid [{0}].<br>|<br>|
|403<br>|PR403-DV-0005<br>|Destination header value [{0}] equals request URL.<br>|<br>|
|404<br>|PR404-DV-0001<br>|Resource not found.<br>|<br>|
|404<br>|PR404-DV-0002<br>|Box not found at [{0}].<br>|<br>|
|404<br>|PR404-DV-0003<br>|Cell not found.<br>|<br>|
|405<br>|PR405-DV-0001<br>|Method not allowed. MKCOL can only be executed on a deleted/non-existent resource.<br>|<br>|
|409<br>|PR409-DV-0001<br>|intermediate collection [{0}] should be created first.<br>|<br>|
|412<br>|PR412-DV-0001<br>|ETag does not match.<br>|<br>|
|412<br>|PR412-DV-0002<br>|Overwrite header is "F" and the destination URL is already mapped to a resource.<br>|<br>|
|416<br>|PR416-DV-0001<br>|Requested range not satisfiable.<br>|<br>|
|500<br>|PR500-DV-0001<br>|File system inconsistency detected.<br>|<br>|
|500<br>|PR500-DV-0002<br>|Dav system inconsistency detected.<br>|<br>|
|503<br>|PR503-DV-0001<br>|Failed to access the target resource due to concurrent update/delete operation by another process.<br>|<br>|
|400<br>|PR400-SM-0001<br>|ToRelation [{0}] does not exist.<br>|<br>|
|400<br>|PR400-SM-0002<br>|ToRelation [{0}] does not have related ExtCell.<br>|<br>|
|400<br>|PR400-SM-0003<br>|The maximum number of destinations was exceeded.<br>|<br>|
|500<br>|PR500-SM-0001<br>|Sent Message connection error.<br>|<br>|
|500<br>|PR500-SM-0002<br>|Sent Message body parse error.<br>|<br>|
|400<br>|PR400-RM-0001<br>|Requested relation already exists.<br>|<br>|
|409<br>|PR409-RM-0001<br>|Requested relation URL parse error.<br>|<br>|
|409<br>|PR409-RM-0002<br>|Requested relation [{0}] does not exists.<br>|<br>|
|409<br>|PR409-RM-0003<br>|Requested relation target URL parse error.<br>|<br>|
|409<br>|PR409-RM-0004<br>|Request relation target [{0}] does not exist.<br>|<br>|
|409<br>|PR409-RM-0005<br>|RequestRelation and RequestRelationTarget is not related. [{0}] - [{1}].<br>|<br>|
|500<br>|PR500-SC-0001<br>|Engine connection error.<br>|<br>|
|500<br>|PR500-SC-0002<br>|File open error.<br>|<br>|
|500<br>|PR500-SC-0003<br>|File close error.<br>|<br>|
|500<br>|PR500-SC-0004<br>|IO error.<br>|<br>|
|500<br>|PR500-SC-0005<br>|Unknown error at Engine.<br>|<br>|
|500<br>|PR500-SC-0006<br>|Invalid HTTP response was returned from a service.<br>|<br>|
|400<br>|PR400-AU-0001<br>|Password format is invalid.<br>|<br>|
|400<br>|PR400-AU-0002<br>|Request parameter is invalid [{0}].<br>|<br>|
|400<br>|PR400-AU-0003<br>|Dc credential required.<br>|<br>|
|401<br>|PR401-AU-0001<br>|Authorization required.<br>|<br>|
|401<br>|PR401-AU-0002<br>|Access token expired.<br>|<br>|
|401<br>|PR401-AU-0003<br>|Invalid authentication scheme.<br>|<br>|
|401<br>|PR401-AU-0004<br>|Basic auth format error.<br>|<br>|
|401<br>|PR401-AU-0005<br>|Authentication failed.<br>|<br>|
|401<br>|PR401-AU-0006<br>|Token parse error.<br>|<br>|
|401<br>|PR401-AU-0007<br>|Can not access with refresh token.<br>|<br>|
|401<br>|PR401-AU-0008<br>|Token dsig error.<br>|<br>|
|401<br>|PR401-AU-0009<br>|Authentication failed.<br>|<br>|
|401<br>|PR401-AU-0010<br>|Authentication failed.<br>|<br>|
|401<br>|PR401-AU-0011<br>|Authentication failed.<br>|<br>|
|403<br>|PR403-AU-0001<br>|Unit user access required.<br>|<br>|
|403<br>|PR403-AU-0002<br>|Necessary privilege is lacking.<br>|<br>|
|403<br>|PR403-AU-0003<br>|This resource can not be accessed by the Unit User specified in authorization header.<br>|<br>|
|403<br>|PR403-AU-0004<br>|Schema authentication is required to access this resource.<br>|<br>|
|403<br>|PR403-AU-0005<br>|This resource can not be accessed with the schema that has been authenticated.<br>|<br>|
|403<br>|PR403-AU-0006<br>|Insufficient schema authorization level.<br>|<br>|
|400<br>|PR400-AZ-0001<br>|Unsupported response_type.<br>|<br>|
|400<br>|PR400-AZ-0002<br>|Request parameter is invalid [client_id].<br>|<br>|
|400<br>|PR400-AZ-0003<br>|Request parameter is invalid [redirect_uri].<br>|<br>|
|400<br>|PR400-AZ-0004<br>|Request parameter is invalid [response_type].<br>|<br>|
|401<br>|PR401-AZ-0001<br>|User cancel.<br>|<br>|
|401<br>|PR401-AZ-0002<br>|Token authorization error.<br>|<br>|
|400<br>|PR400-EV-0001<br>|JSON parse error.<br>|<br>|
|400<br>|PR400-EV-0002<br>|Request header is invalid [X-Dc-RequestKey].<br>|<br>|
|400<br>|PR400-EV-0003<br>|[{0}] is required.<br>|<br>|
|400<br>|PR400-EV-0004<br>|[{0}] field format error.<br>|<br>|
|500<br>|PR500-EV-0001<br>|Failed to output http response.<br>|<br>|
|500<br>|PR500-EV-0002<br>|Cannot open archive eventLog file.<br>|<br>|
|500<br>|PR500-SV-0000<br>|Unknown error.<br>|<br>|
|500<br>|PR500-SV-0001<br>|Datastore connection error.<br>|<br>|
|500<br>|PR500-SV-0002<br>|Datastore unknown error.<br>|<br>|
|500<br>|PR500-SV-0003<br>|Elasticsearch request retry over [{0}].<br>|<br>|
|500<br>|PR500-SV-0004<br>|File system access error [{0}].<br>|<br>|
|500<br>|PR500-SV-0005<br>|Failed to search from datastore.<br>|<br>|
|500<br>|PR500-SV-0006<br>|Failed to update data store, and failed to rollback.<br>|<br>|
|500<br>|PR500-SV-0007<br>|Failed to update data store, and completed to rollback.<br>|<br>|
|503<br>|PR503-SV-0001<br>|Too many concurrent requests.<br>|<br>|
|503<br>|PR503-SV-0002<br>|Server connection error.<br>|<br>|
|503<br>|PR503-SV-0003<br>|Server error. (Lock Manager) Data lock state unknown.<br>|<br>|
|503<br>|PR503-SV-0004<br>|Service is under maintenance [restoring].<br>|<br>|
|503<br>|PR503-SV-0005<br>|Operation is prohibited as one or more disks are almost full.<br>|<br>|
|503<br>|PR503-SV-0006<br>|Server connection error. (Datastore)<br>|<br>|
|405<br>|PR405-MC-0001<br>|Method not allowed.<br>|<br>|
|408<br>|PR408-MC-0001<br>|Request timeout in server.<br>|<br>|
|409<br>|PR409-MC-0001<br>|Cell to be deleted is being accessed by other requests.<br>|<br>|
|412<br>|PR412-MC-0001<br>|Precondition failed [Header: {0}].<br>|<br>|
|501<br>|PR501-MC-0001<br>|Method not implemented.<br>|<br>|
|501<br>|PR501-MC-0002<br>|Not implemented. [{0}].<br>|<br>|
|400<br>|PR400-BI-0001<br>|Request header is not defined or invalid [{0}].<br>|<br>|
|400<br>|PR400-BI-0002<br>|Bar file size too large [{0}B].<br>|<br>|
|400<br>|PR400-BI-0003<br>|Bar file entry size too large [{0}, {1}B].<br>|<br>|
|400<br>|PR400-BI-0004<br>|Install target box is already registered as Box Schema[{0}].<br>|<br>|
|400<br>|PR400-BI-0005<br>|Bar file size invalid[{0}, {1}B].<br>|<br>|
|400<br>|PR400-BI-0006<br>|[{0}] file format error.<br>|<br>|
|400<br>|PR400-BI-0007<br>|Cannot open bar file [{0}].<br>|<br>|
|400<br>|PR400-BI-0008<br>|Cannot read bar file [{0}].<br>|<br>|
|400<br>|PR400-BI-0009<br>|Invalid bar file structures [{0}].<br>|<br>|
|405<br>|PR405-BI-0001<br>|Install target box is already registered [{0}].<br>|<br>|
|500<br>|PR500-BI-0001<br>|Failed to output http response.<br>|<br>|
|500<br>|PR500-NW-0000<br>|Network error. {0}<br>|<br>|
|500<br>|PR500-NW-0000<br>|Network error. {0}<br>|<br>|
|500<br>|PR500-NW-0001<br>|HTTP {0} request to {1} failed. Response code : {2}.<br>|<br>|
|500<br>|PR500-NW-0001<br>|HTTP {0} request to {1} failed. Response code : {2}.<br>|<br>|
|500<br>|PR500-NW-0002<br>|Unexpected response from {0}, where {1} expected.<br>|<br>|
|500<br>|PR500-NW-0002<br>|Unexpected response from {0}, where {1} expected.<br>|<br>|
|500<br>|PR500-NW-0003<br>|Unexpected value from key={0}, where "{1}" value expected.<br>|<br>|
|500<br>|PR500-NW-0003<br>|Unexpected value from key={0}, where "{1}" value expected.<br>|<br>|
|500<br>|PR500-SV-9999<br>|Unknown Exception [{0}] {1}<br>| <br>|
|500<br>|PR500-AN-0001<br>|Root ca certificate setting error.<br>|<br>|
|412<br>|-<br>|["{0}","{1}","{2}"…]<br>|利用可能なpersonium.io<br>バージョン番号を返却する<br>|
<br>
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
