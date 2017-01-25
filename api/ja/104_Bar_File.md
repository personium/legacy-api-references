# barファイル
barファイルはBoxインストールAPIのリクエストボディとして指定するファイル形式である。
barファイルは、Box配下に定義するOdata/WebDAV/Serviceの各データを格納しており、ZIPファイル形式でアーカイブされる。
通常は、BoxエクスポートAPI（未実装）にて、personium.ioからアーカイブされたBox配下のデータ定義がエクスポートされる。

### Specifications
ファイルフォーマットはZIP形式とし、ZIP64形式も許容する。
また、ZIPファイルの暗号化は対応しない。

#### Directory Structure
barファイル内のディレクトリ構成を以下に示す。
Boxインストール時に「★必須」となっているディレクトリ、及びファイルが存在しない場合は、必須のディレクトリ/ファイルがない旨のエラー（400 Bad Request）を返却する。
また、barファイルの構造が下記の順序で作成されていない場合はエラーとなる。
```
bar/
 |
 +-- 00_meta/  ★ 必須
 |    |
 |    +-- 00_manifest.json  ★ 必須
 |    +-- 10_relations.json
 |    +-- 20_roles.json
 |    +-- 30_extroles.json
 |    +-- 70_$links.json
 |    +-- 90_rootprops.xml  ★ 必須
 |
 +-- 90_contents/     ★ 配下のディレクトリ名はコレクション名と同じ
      |
      +-- {OData}/    ★ rootprops.xmlでODataコレクションの場合、必須
      |    |
      |    +-- 00_$metadata.xml    ★ 必須
      |    +-- 10_odatarelations.json
      |    |
      |    +-- 90_data/
      |         |
      |         +-- {EntityType}/
      |              |
      |              +-- {1.json}
      |
      +-- {Service}/
      |    |
      |    +-- {src.js}
      |
      +-- {dir1}/
           |
           +-- {dir1-1}/
                |
                +-- {userdata1-2.jpg}
                |
                +-- {dir2}/
                     |
                     +-- [userdata1-2.jpg}
```



#### Bar File Version Control
エンハンス等によってデータ構造が変更となり、後方互換が確保できなくなった場合にbarファイルのバージョンアップを行う。
* ファイルの追加レベルではバージョンアップしない
* ファイルのフォーマットやファイル名などを変更・削除した場合にバージョンアップする

### File List
#### 00_manifest.json
インストールする対象となるBoxの情報を記述したファイル

|項目名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|bar_version<br>|barファイルのバージョン<br>|有効なバージョン<br>barファイル形式の変更ごとにバージョンが変わる<br>|○<br>|現状は"1"<br>|
|box_version<br>|Boxのバージョン<br>|有効なバージョン<br>Box形式の変更ごとにバージョンが変わる<br>|○<br>|任意の文字列で良いが"1"を推奨（Box改版機能提供に向けて）<br>|
|DefaultPath<br>|barファイル内でのBox名<br>|桁数：1&#65374;128<br>文字種:半角英数字と-(半角ハイフン)と_(半角アンダーバー)<br>ただし、先頭文字に-(半角ハイフン)と_(半角アンダーバー)は指定不可<br>nullは不可<br>|○<br>| <br>|
|schema<br>|Schema名<br>|桁数：1&#65374;1024<br>URIの形式に従う（scheme：http / https / urn）<br>nullは不可<br>|○<br>| <br>|

##### サンプル
```json
{
  "bar_version": "1",
  "box_version": "1",
  "DefaultPath": "{BoxName}",
  "schema": "http://app1.example.com"
}
```
#### 10_relations.json
インストール対象とするRelationの情報を記述したファイル
※「有効値」欄が『&#65293;』となっている項目項目は、Relationのリクエストボディを参照

|項目名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Relations  <br>|Relationのリスト<br>| <br>|○<br>| <br>
|Relations/Name<br>|Relation名<br>|-<br>|○<br>| <br>|


##### サンプル
```json
{
  "Relations": [
    {
      "Name": "relation1"
    }
  ]
}
```
#### 20_roles.json
インストール対象とするRoleの情報を記述したファイル
※「有効値」欄が『&#65293;』となっている項目は、、Roleのリクエストボディを参照

|項目名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Roles   <br>|Relationのリスト<br>| <br>|○<br>| <br>
|Roles/Name<br>|Relation名<br>|-<br>|○<br>| <br>|

##### サンプル
```json
{
  "Roles": [
    {
      "Name": "role1"
    }
  ]
}
```
#### 30_extroles.json
インストール対象とするExtRoleの情報を記述したファイル
※「有効値」欄が『&#65293;』となっている項目は、ExtRoleのリクエストボディを参照

|項目名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|ExtRoles<br>|ExtRoleのリスト<br>| <br>|○<br>| <br>
|ExtRoles/ExtRole<br>|参照先RoleのURI<br>|&#65293;<br>|○<br>|nullは不可 ※1<br>|
|ExtRoles/_Relation.Name<br>|Relation名<br>|&#65293;<br>|○<br>|nullは不可<br>|

(※ 1) エクスポート時にロールクラスURLへ変換
https://{UnitFQDN}/cell1/__role/box/staff → https://{UnitFQDN}/cell1/__role/__/staff

##### サンプル
```json
{
  "ExtRoles": [
    {
      "ExtRole": "https://{UnitFQDN}/{CellName}/__role/__/role2",
      "_Relation.Name": "Relation1"
    }
  ]
}
```
#### 70_$links.json
インストール対象とする$linksのデータ関連情報を記述したファイル

|項目名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Links<br>|$linksのリスト<br>| <br>|○<br>| <br>
|Links/FromType<br>|参照元データの種類<br>|"Relation"<br>"Role"<br>&quot;ExtRole&quot;  <br>|○<br>|nullは不可<br>|
|Links/FromName<br>|参照元データの名前<br>|&#65293;(配列形式 ※1)<br>|○<br>|nullは不可<br> （例:{&quot;Name&quot;:&quot;relation1&quot;}]）<br>|
|Links/ToType<br>|参照先データの種類<br>|"Relation"<br>"Role"<br>"ExtRole"<br>|○<br> <br>|nullは不可<br> <br>|
|Links/ToName<br>|参照先データの名前<br>|&#65293;(配列形式 ※1)<br>|○<br> <br>|nullは不可<br>（例:{"Name":"role"}]）<br>|
※1 ExtRoleではRelation情報も必要のため、リスト形式とする。ただし、指定するJSONデータのキー名は、&quot;Name&quot; 固定とする。（制限）

##### サンプル
```json
{
  "Links": [
    {
      "FromType": "Relation",
      "FromName":
        {
          "Name": "relation1"
        },
      "ToType": "Role",
      "ToName":
        {
          "Name": "role1"
        }
    },
    {
      "FromType": "Role",
      "FromName":
        {
          "Name": "role1"
        },
      "ToType": "ExtRole",
      "ToName": {
          "ExtRole": "https://{UnitFQDN}/{CellName}/__role/__/role2",
          "_Relation.Name": "Relation1"
        }
    }
  ]
}
```
#### 90_rootprops.xml
barファイルにエクスポートする対象のBox配下の全階層に対して、PROPFINDメソッドで取得したXMLデータを示す。
XMLデータの詳細は、を参照。
インストール対象BoxのURLは、「dcbox:/」と記述する。
barファイルのインストール時には、下記サンプルの<prop>配下にある creationdate及び、getlastmodifiedを除いた全てのデータをインストール対象とする。
* resourcetype: コレクションの種類を設定
* acl: 権限を設定
* Other: PROPPATCHで設定

##### サンプル
```xml
<multistatus xmlns="DAV:">
    <response>
        <href>dcbox:/</href>
        <propstat>
           <prop>
              <resourcetype>
                  <collection/>
              </resourcetype>
              <acl xml:base="https://{UnitFQDN}/cell/__role/__/" xmlns:dc="urn:x-dc1:xmlns">
                  <ace>
                      <principal>
                          <href>admin</href>
                      </principal>
                      <grant>
                          <privilege>
                              <all/>
                          </privilege>
                      </grant>
                  </ace>
              </acl>
          </prop>
      </propstat>
  </response>
  <response>
      <href>dcbox:/odata</href>
      <propstat>
          <prop>
              <resourcetype>
                  <collection/>
                  <dc:service xmlns:dc="urn:x-dc1:xmlns"/>
              </resourcetype>
              <acl xml:base="https://{UnitFQDN}/cell/__role/__/" xmlns:dc="urn:x-dc1:xmlns">
                  <ace>
                      <principal>
                          <href>user</href>
                      </principal>
                      <grant>
                          <privilege>
                              <read/>
                          </privilege>
                          <privilege>
                              <write/>
                          </privilege>
                          <privilege>
                              <read-properties/>
                          </privilege>
                      </grant>
                  </ace>
              </acl>
          </prop>
      </propstat>
  </response>
  <response>
      <href>dcbox:/dav</href>
      <propstat>
          <prop>
              <resourcetype>
                  <collection/>
              </resourcetype>
              <acl xml:base="https://{UnitFQDN}/cell/__role/__/" xmlns:dc="urn:x-dc1:xmlns">
                  <ace>
                      <principal>
                          <href>user</href>
                      </principal>
                      <grant>
                          <privilege>
                              <read/>
                          </privilege>
                          <privilege>
                              <write/>
                          </privilege>
                          <privilege>
                              <read-properties/>
                          </privilege>
                      </grant>
                  </ace>
              </acl>
          </prop>
      </propstat>
  </response>
  <response>
      <href>dcbox:/dav/testdavfile.txt</href>
      <propstat>
          <prop>
              <getcontenttype>text/plain</getcontenttype>
          </prop>
      </propstat>
  </response>
  <response>
      <href>dcbox:/service</href>
      <propstat>
          <prop>
              <resourcetype>
                  <collection/>
                  <dc:service xmlns:dc="urn:x-dc1:xmlns"/>
              </resourcetype>
             <acl xml:base="https://{UnitFQDN}/cell/__role/__/" xmlns:dc="urn:x-dc1:xmlns"/>
              <dc:service language="JavaScript" xmlns:D="DAV:" xmlns:dc="urn:x-dc1:xmlns" \
              xmlns:Z="http://www.w3.com/standards/z39.50/">
                  <dc:path name="ehr" src="ehr.js"/>
                  <dc:path name="ehr_connector" src="ehr_connector.js"/>
              </dc:service>
            </prop>
        </propstat>
    </response>
    <response>
        <href>dcbox:/service/__src</href>
        <propstat>
            <prop>
                <resourcetype>
                    <collection/>
                </resourcetype>
                <acl xml:base="https://{UnitFQDN}/cell/__role/__/"\
                 xmlns:dc="urn:x-dc1:xmlns"/>
            </prop>
        </propstat>
    </response>
    <response>
        <href>dcbox:/service/__src/ehr.js</href>
        <propstat>
            <prop>
                <getcontenttype>text/javascript</getcontenttype>
            </prop>
        </propstat>
    </response>
    <response>
        <href>dcbox:/service/__src/ehr_connector.js</href>
        <propstat>
            <prop>
                <getcontenttype>text/javascript</getcontenttype>
            </prop>
        </propstat>
    </response>
</multistatus>
```
#### contents/{OData}/
以下のファイルについては、詳細未稿
* 90_data / {EntityType}/1.json

##### 00_$metadata.xml
ユーザODataのスキーマ定義を示す。barファイルにエクスポートする時に、Odata用コレクションに対して$metadataにて取得したXMLデータ。
XMLデータの詳細は、スキーマ取得（$metadata）を参照。
Boxインストール時には、Schemaタグの配下をインストール対象とする。
ユーザODataのスキーマ定義が存在しない場合でもファイル自体は存在する。

スキーマ定義がない場合のサンプル
```xml
<Edmx: Edmx Version = '1 .0 'xmlns: edmx =' http://schemas.microsoft.com/ado/2007/06/edmx \
'xmlns: d =' http://schemas.microsoft.com/ado/2007 / 08/dataservices' xmlns:\
 m = 'http://schemas.microsoft.com/ado/2007/08/dataservices/metadata' xmlns: dc = 'urn: x-dc1: xmlns'>
  <edmx:DataServices m:DataServiceVersion='1.0'>
    <Schema Xmlns='http://schemas.microsoft.com/ado/2006/04/edm' Namespace='UserData'>
      <EntityContainer Name='UserData' m:IsDefaultEntityContainer='true'/>
    </ Schema>
  </ Edmx: DataServices>
</ Edmx: Edmx>
```

##### 10_odatarelations.json
インストール対象ユーザデータの$linksのデータ関連情報を記述したファイル
ユーザODataのスキーマレベルでは、00_$metadata.xml にてAssociationEndの関連を定義しているが、本ファイルではユーザデータの実体に対する関連を定義する。

|項目名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Links<br>|$linksのリスト<br>| <br>|○<br>| <br>
|Links/FromType<br>|参照元データの種類<br>|&#65293;  <br>|○<br>|nullは不可<br>|
|Links/FromId<br>|参照元ユーザデータのID<br>|&#65293;(配列形式 ※1)<br>|○<br>|nullは不可<br> （例:{&quot;FromId&quot;:&quot;fujitsu_taro&quot;} ）<br>|
|Links/ToType<br>|参照先データの種類<br>|&#65293;<br>|○<br> <br>|nullは不可<br> <br>|
|Links/ToId  <br>|参照先ユーザデータのID  <br>|&#65293;(配列形式 ※1)<br>|○<br> <br>|nullは不可<br>（例:{"ToId":"fujitsu_hanako"} ）<br>|
※1 将来的に複合主キーへ対応した場合の対応を考慮して配列形式とする。

##### サンプル
```json
{
  "Links": [
    {
      "FromType": "Keeper",
      "FromId": {
        "usercode": "001",
        "Name": "Peru_Taro"
      },
      "ToType": "Animal",
      "ToId": {
        "shopcode": "001",
        "Name": "pochi"
      }
    },
    {
      "FromType": "Keeper",
      "FromId": {
        "usercode": "002",
        "Name": "Peru_Hanako"
      },
      "ToType": "Animal",
      "ToId": {
        "shopcode": "002",
        "Name": "tama"
      }
    }
  ]
}
```
#### 90_data / {EntityType} / {1.json}
ユーザデータを1件ずつJSON形式で格納する。

|File name<br>|Valid values: {Byte numbers} json<br>|
|:--|:--|

```json
{
    "__id": "{EntityName}",
    "name": "pochi",
    "address": {
        "country": "japan",
        "city": "tokyo"
    }
}
```

#### contents / {webdavbcol} /
※未稿

#### contents/Service/{src.js}
bar/90_contents/{Service}/{src.js}に格納されたソースファイルを、インストール先Box配下の{Service}/\__src/{src.js}に登録する。
* bar/00_meta/90_rootprops.xmlにコレクション{Service}の定義（PROPPATCH）が無い場合は、サービスとして実行できない
* bar/00_meta/90_rootprops.xmlに{Service}/\__srcの定義が無い場合は、{src.js}は登録できない
* bar/00_meta/90_rootprops.xmlの{Service}/\__src/{src.js}の定義に従い、{src.js}のContent-Typeを設定する

サービス登録用90_rootprops.xmlのサンプル
```xml
<multistatus xmlns="DAV:">
    <response>
        <href>dcbox:/service</href>
        <propstat>
            <prop>
                <resourcetype>
                    <collection/>
                    <dc:service xmlns:dc="urn:x-dc1:xmlns"/>
                </resourcetype>
                <acl xml:base="https://{UnitFQDN}/cell/__role/__/"\
                xmlns:dc="urn:x-dc1:xmlns"/>
                <dc:service language="JavaScript" xmlns:D="DAV:" xmlns:dc="urn:x-dc1:xmlns" \
                xmlns:Z="http://www.w3.com/standards/z39.50/">
                    <dc:path name="ehr" src="ehr.js"/>
                    <dc:path name="ehr_connector" src="ehr_connector.js"/>
                </dc:service>
            </prop>
        </propstat>
    </response>
    <response>
        <href>dcbox:/service/__src</href>
        <propstat>
            <prop>
                <resourcetype>
                    <collection/>
                </resourcetype>
                <acl xml:base="https://{UnitFQDN}/cell/__role/__/"\
                 xmlns:dc="urn:x-dc1:xmlns"/>
            </prop>
        </propstat>
    </response>
    <response>
        <href>dcbox:/service/__src/ehr.js</href>
        <propstat>
            <prop>
                <getcontenttype>text/javascript</getcontenttype>
            </prop>
        </propstat>
    </response>
    <response>
        <href>dcbox:/service/__src/ehr_connector.js</href>
        <propstat>
            <prop>
                <getcontenttype>text/javascript</getcontenttype>
            </prop>
        </propstat>
    </response>
</multistatus>
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED
