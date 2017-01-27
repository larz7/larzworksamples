## Overview

This API automates the downloading, transcoding, and management of third party ad content.

## REST Interface

**Status**
----
A simple status call that you can use to verify the service is running.

* **URL**

  /status

* **Method:**

  `GET`

* **URL Params**

  None

* **Data Params**

  None

* **Success Response:**

  * **Code:** 200 <br />
    **Content:** `OK`

* **Error Response:**

  None

* **Notes:**

  If the service is running, this call always returns 'OK'

**Manifest**
----

Returns information about the application instance.

* **URL**

  /manifest

* **Method:**

  `GET`

* **URL Params**

  None

* **Data Params**

  None

* **Success Response:**

  * **Code:** 200 <br />
    **Content:**
    ``{"servicename":"ads-ingestion:qa:1.0","version":{"major":1,"minor":0,"micro":"100"},"gitRevision":"75841e2009f262e4a8f36b8ccf6536454bd1c1f8","buildDate":"Thu Sep 24 13:17:40 PDT 2015","apis":[],"dependencies":[{"name":"Discovery","version":{"major":1,"minor":0}},{"name":"DevAdmin","version":{"major":1,"minor":0}}],"configFeatures":{"ixxxxxx.ws.s2s.cacheWarmupEnabled":{"typ":"bool","value":false,"matcher":{},"tolerance":1},"ixxxxxx.ws.s2s.discoveryLocation":{"typ":"string","value":"s2s-imqa.svc.xxxx.com:8443","matcher":{},"tolerance":0},"xxxxx.ws.s2s.cacheWarmupPeriod":{"typ":"string","value":"1 hour","matcher":{},"tolerance":1},"xxxxx.ws.s2s.cacheWarmupStart":{"typ":"string","value":"1 second","matcher":{},"tolerance":1},"ixxxxx.ws.s2s.clientPrivateKeyKey":{"typ":"string","value":"/opt/certificates/hsm_key.pem","matcher":{},"tolerance":0},"inxxx.ws.s2s.caPublicCertificate":{"typ":"string","value":"/opt/certificates/ca_bundle_internal.pem","matcher":{},"tolerance":0},"ixxxxa.ws.s2s.defaultServiceTag":{"typ":"string","value":"vod:qa@validated","matcher":{},"tolerance":0},"xxxxxa.ws.s2s.clientPublicCertificate":{"typ":"string","value":"/opt/certificates/cert_with_chain_internal.pem","matcher":{},"tolerance":0}}}``

* **Error Response:**

  None

* **Notes:**

  None

**creative/fetch**
----

This entry point triggers the fetching and encoding of a creative
referenced by the url

* **URL** /creative/fetch/$adSystem/$product/$url

* **Method:**

  `POST`

*  **URL Params**

   **Required:**

   `adSystem=[string]`

   `$product=[string]`

   `url=[string]`

* **Data Params**

  None

* **Success Response:**

  * **Code:** 200 <br />
    **Content:** `{ result : "OK" }`

* **Error Response:**

  None

* **Sample Call:**

  curl -i -X POST "http://localhost:9101/creative/fetch/Freewheel/go90/http%3A%2F%2Fthis%2Fis%2Fmy%2Ffile2.mp4"

* **Notes:**

  None

**creative/bulk/fetch**
----

  This entry point triggers the retranscoding of all of the
  creatives that match the constraints specified by the
  parameters.

  **Please, use caution.  This is an expensive call.**

* **URL**

  /creative/bulk/fetch

* **Method:**

  `POST``

*  **URL Params**

   **Optional:**

   `transcodeState=[string]`

   `blacklisted=[boolean]`

   `search=[string]`

* **Data Params**

  None

* **Success Response:**

  * **Code:** 200 <br />
    **Content:** `{ result : "OK" }`

* **Error Response:**

  None

* **Sample Call:**

  curl -i -X POST "http://localhost:9101/creative/bulk/fetch"

  curl -i -X POST "http://localhost:9101/creative/bulk/fetch?transcodeState=transcoding&search=foo&blacklisted=true"

* **Notes:**

  None

**/creative/alternative/**
----
  Ads-router calls this entry point to retrieve transcoded alternatives for 3rd party urls.

* **URL**

  /creative/alternative/$adSystem/$product/$url

* **Method:**

  `GET`

*  **URL Params**

   **Required:**

   `adSystem=[string]`

   `product=[string]`

   `url=[string]`

* **Data Params**

  None

* **Success Response:**

  * **Code:** 200 <br />
    **Content:** `{ "result" : "OK", "transcodedUrl" : "http://some/url", "duration": 10000, "transcodeState": "Transcoded" }`

  OR

  * **Code:** 200 <br />
    **Content:** `{ result : "OK", "transcodeState": "Blacklisted" }`

  OR

  * **Code:** 200 <br />
    **Content:** `{ result : "OK", "transcodeState": "NotFound" }`

  OR

  * **Code:** 200 <br />
    **Content:** `{ result : "OK", "transcodeState": "TranscodePending" }`

* **Error Response:**

  None

* **Sample Call:**

  curl -i "http://localhost:9101/creative/alternative/Freewheel/iptv/http%3A%2F%2Fcdn.vindicosuite.com%2FRepository%2FCampaignCreative%2FCampaign_21050%2FINSTREAMAD%2FTFSS_015NG22H_Edgy_Review_Countdown_15_Rev_Now_mp4_700kpbs_640x360_16-9.mp4"

* **Notes:**

  If the ad creative referenced by the url does not exist for the given product, a background task to download and transcode the creative is started (not for spotIds).


**/creative/blacklist**
----

Use this entry point to blacklist a creative.

* **URL**

  creative/blacklist/$product/$url

* **Method:**

  <_The request type_>

  `POST`

*  **URL Params**

   **Required:**

   `product=[string]`

   `url=[string]`

* **Data Params**

  None

* **Success Response:**

  * **Code:** 200 <br />
    **Content:** `{ result : "OK" }`

* **Error Response:**

  None

* **Sample Call:**

  curl -i -X POST "http://localhost:9101/creative/blacklist/iptv/http%3A%2F%2Fc87ba34c553d24ecde3c-295b398e45d860166e9531d85c53f076.r6.cf2.rackcdn.com%2F93d68d92-5872-bb6b-938d-014c2447c8f3-M_S_HD_Beaker_OwiRE_1280x720_3500_h32.m4v"

* **Notes:**

  None

**creative/enable**
----
  This entry point enables a creative that was previously blacklisted.

* **URL**

  /creative/enable/$product/$url

* **Method:**

  `POST`

*  **URL Params**

   **Required:**

   `product=[string]`

   `url=[string]`

* **Data Params**

  None

* **Success Response:**

  * **Code:** 200 <br />
    **Content:** `{ result : "OK" }`

* **Error Response:**

None

* **Sample Call:**

  curl -i -X POST "http://localhost:9101/creative/enable/iptv/http%3A%2F%2Fc87ba34c553d24ecde3c-295b398e45d860166e9531d85c53f076.r6.cf2.rackcdn.com%2F93d68d92-5872-bb6b-938d-014c2447c8f3-M_S_HD_Beaker_OwiRE_1280x720_3500_h32.m4v"

* **Notes:**

  None


**/creative/transcoded*
----

  The transcoding system uses this entry point to let ads ingestion know when
  as transcoding job has been completed.

* **URL**

  /creative/transcoded

* **Method:**

  <_The request type_>

  `POST`

* **URL Params**

  None

* **Data Params**

   `sourcePath=[string]`

   `manifest_location_hd=[string]`

   `assetId=[string]`

   `mediaRunTime=[integer]`

* **Success Response:**

  * **Code:** 200 <br />
    **Content:** `{ result : "OK" }`

* **Error Response:**

  None

* **Sample Call:**

  curl -i -X POST  --date "{"mediaInfoAsXml":null,"assetId":"00000000000000000","manifest_location_audio_sd":null,"manifest_location_sd":"Ads/dev/dcdd5881-8de6-4012-8a35-2d18b6d0c6a7_e63f52fa-0936-b429-32a9-014e2162868b-93d68d92-5872-bb6b-938d-014c2447c8f3-M_S_HD_Beaker_OwiRE_1280x720_3500_h32/dash/e63f52fa-0936-b429-32a9-014e2162868b-93d68d92-5872-bb6b-938d-014c2447c8f3-M_S_HD_Beaker_OwiRE_1280x720_3500_h32-SD.mpd","manifest_location_hd":"Ads/dev/dcdd5881-8de6-4012-8a35-2d18b6d0c6a7_e63f52fa-0936-b429-32a9-014e2162868b-93d68d92-5872-bb6b-938d-014c2447c8f3-M_S_HD_Beaker_OwiRE_1280x720_3500_h32/dash/e63f52fa-0936-b429-32a9-014e2162868b-93d68d92-5872-bb6b-938d-014c2447c8f3-M_S_HD_Beaker_OwiRE_1280x720_3500_h32-HD.mpd","mediaRunTime":"30128","rootOutputPath":"Ads/dev","sourcePath":"s3://vodworkflow/qa/ads/ads/qa/thirdparty/e63f52fa-0936-b429-32a9-014e2162868b-93d68d92-5872-bb6b-938d-014c2447c8f3-M_S_HD_Beaker_OwiRE_1280x720_3500_h32.m4v","manifest_location_audio_hd":"Ads/dev/dcdd5881-8de6-4012-8a35-2d18b6d0c6a7_e63f52fa-0936-b429-32a9-014e2162868b-93d68d92-5872-bb6b-938d-014c2447c8f3-M_S_HD_Beaker_OwiRE_1280x720_3500_h32/dash/e63f52fa-0936-b429-32a9-014e2162868b-93d68d92-5872-bb6b-938d-014c2447c8f3-M_S_HD_Beaker_OwiRE_1280x720_3500_h32-HD.mpd","sourceCoreMediaFile_0_fps":"Ads/dev/dcdd5881-8de6-4012-8a35-2d18b6d0c6a7_e63f52fa-0936-b429-32a9-014e2162868b-93d68d92-5872-bb6b-938d-014c2447c8f3-M_S_HD_Beaker_OwiRE_1280x720_3500_h32/dash/e63f52fa-0936-b429-32a9-014e2162868b-93d68d92-5872-bb6b-938d-014c2447c8f3-M_S_HD_Beaker_OwiRE_1280x720_3500_h32-HD.mpd"}" "http://localhost:9101/creative/transcoded"

* **Notes:**

  None

**/creative/transcode/failed**
----

Given a creative's guide, this entry point marks all transcoding jobs as failed.

* **URL**

  /creative/transcode/failed/$creativeGuid

* **Method:**

  `POST`

*  **URL Params**

   **Required:**

   `$creativeGuid=[string]`

* **Data Params**

  None

* **Success Response:**

  * **Code:** 200 <br />
    **Content:** `{ result : "OK" }`

* **Error Response:**

  None

* **Sample Call:**

  curl -i -X POST "http://localhost:9101/creative/transcode/failed/ae088ce9-9173-4d3f-ac5b-374acd64cfff/1ca23a59

* **Notes:**

  None

**/transcodejob/failed**
----
  Given a transcoding job guid, mark the the transcoding job as failed.

* **URL**

  /transcodejob/failed/$transcodeJobGuid

* **Method:**

  `POST`

*  **URL Params**

   **Required:**

   `$transcodeJobGuid=[string]`

* **Data Params**

  None

* **Success Response:**

  * **Code:** 200 <br />
    **Content:** `{ result : "OK" }`

* **Error Response:**

  None

* **Sample Call:**

  curl -i -X POST "http://localhost:9101/transcodejob/failed/ae088ce9-9173-4d3f-ac5b-374acd64cfff/1ca23a59

* **Notes:**

  None

**/creatives/list**
----
Use this entry point to list creatives

* **URL**

  /creatives/list

* **Method:**

  <_The request type_>

  `GET`

*  **URL Params**

   <_If URL params exist, specify them in accordance with name mentioned in URL section. Separate into optional and required. Document data constraints._>

   **Required:**

   `pageOffset=[integer]`

   `pageSize=[integer]`

   **Optional:**

   `lastToken=[string]`

   `sortBy=[ASC or DSC]`

   `sortOrder=[any field in the result set]`

   `transcodeState=[alphanumeric]`

   `blacklisted=[true or false]`

   `search=[string]` matches against externalUrl

* **Data Params**

  None

* **Success Response:**

  * **Code:** 200 <br />
    **Content:**
```
{
          'records' => [
                         {
                           'blacklisted' => bless( do{\(my $o = 0)}, 'JSON::PP::Boolean' ),
                           'externalUrlAlias' => 'http://mockadcdn/fake-ad-creative.avi',
                           'source' => 'thirdparty',
                           'product' => 'iptv',
                           'acquisitionDate' => 1441923273,
                           'transcodedUrl' => undef,
                           'originalPath' => 'ads/dev/thirdparty/ae23d4fc-0008-bc11-0274-014fb953124c-fake-ad-creative.avi',
                           'freewheelCreativeId' => undef,
                           'guid' => 'ae23d4fc-0008-bc11-0274-014fb953124c',
                           'duration' => 0,
                           'adSystem' => 'default',
                           'externalUrl' => 'http://mockadcdn/fake-ad-creative.avi',
                           'transcodedPath' => undef,
                           'transcodeState' => 'failed'
                         }
                       ],
          'result' => 'OK',
          'totalRecordCount' => 1,
          'lastToken' => '55f200c9e4b0b7d53409973d'
        };
```

* **Error Response:**

    None

* **Sample Call:**

  http://ads-ingestion:9000/creatives/list?pageOffset=0&pageSize=100&transcodeState=fetched

* **Notes:**

  Pagination Notes:

  You can pass pageOffset or lastToken to specifify where you should start paging.
  lastToken is more efficient than pageOffset if you're scrolling through pages
  sequentially.  You get 'lastToken' from the previously fetched page set.

  Constraints:

  transcodeState, blacklisted, search are optional parameters that you can use to narrow
  the scope of the data returned.

  Sorting:

  sortBy and sortOrder are used to specify the sort order of the result set.

**/transcodejobs/list**
----

  Use this entry point to list transcode jobs

* **URL**

  /transcodejobs/list

* **Method:**

  `GET`

*  **URL Params**

   **Required:**

   `pageOffset=[integer]`

   `pageSize=[integer]`

   **Optional:**

   `lastToken=[alphanumeric]`

   `sortBy=[alphanumeric]`

   `sortOrder=[alphanumeric]`

   `transcodeState=[alphanumeric]`

   `path=[alphanumeric]` matches against sourcePath

   `creativeGuid=[alphanumeric]`

* **Data Params**

  None

* **Success Response:**

  * **Code:** 200 <br />
    **Content:**
```
{
          'result' => 'OK',
          'records' => [
                         {
                           'guid' => 'd8092d75-0008-bc11-0274-014fb9529310',
                           'jobStatus' => 'transcoding',
                           'jobEnd' => 0,
                           'graphId' => 'd8092d75-0008-bc11-0274-014fb9529310',
                           'transcodePath' => undef,
                           'creativeGuid' => '8d3ddfb4-0008-bc11-0274-014fb95292ea',
                           'jobStart' => 1441923240,
                           'sourcePath' => 'ads/dev/thirdparty/8d3ddfb4-0008-bc11-0274-014fb95292ea-file.webm'
                         },
                         {
                           'guid' => '32719204-0008-bc11-0274-014fb9526bb0',
                           'jobStatus' => 'transcoded',
                           'jobEnd' => 1441923245,
                           'graphId' => '32719204-0008-bc11-0274-014fb9526bb0',
                           'transcodePath' => 'Ads/dev/dcdd5881-8de6-4012-8a35-2d18b6d0c6a7_e63f52fa-0936-b429-32a9-014e2162868b-93d68d92-5872-bb6b-938d-014c2447c8f3-M_S_HD_Beaker_OwiRE_1280x720_3500_h32/dash/e63f52fa-0936-b429-32a9-014e2162868b-93d68d92-5872-bb6b-938d-014c2447c8f3-M_S_HD_Beaker_OwiRE_1280x720_3500_h32-HD.mpd',
                           'creativeGuid' => 'e6a54543-0008-bc11-0274-014fb9526b89',
                           'jobStart' => 1441923230,
                           'sourcePath' => 'ads/dev/thirdparty/e6a54543-0008-bc11-0274-014fb9526b89-file.webm'
                         },
                         {
                           'jobStatus' => 'transcoding',
                           'guid' => '7f90c7f6-0008-bc11-0274-014fb952ce0d',
                           'jobEnd' => 0,
                           'graphId' => '7f90c7f6-0008-bc11-0274-014fb952ce0d',
                           'transcodePath' => undef,
                           'creativeGuid' => '24d47a36-0008-bc11-0274-014fb952cde3',
                           'jobStart' => 1441923255,
                           'sourcePath' => 'ads/dev/thirdparty/24d47a36-0008-bc11-0274-014fb952cde3-file.webm'
                         }
                       ],
          'totalRecordCount' => 3,
          'lastToken' => '55f200b7e4b0b7d534099730'
        }
```
* **Error Response:**

  None

* **Sample Call:**

  http://ads-ingestion:9000/transcodejobs/list?pageOffset=0&pageSize=100

* **Notes:**

  Pagination Notes:

  You can pass pageOffset or lastToken to specifify where you should start paging.
  lastToken is more efficient than pageOffset if you're scrolling through pages
  sequentially.  You get 'lastToken' from the previously fetched page set.

  Constraints:

  transcodeState, blacklisted, path, and constraints are optional parameters that you
  can use to narrow the scope of the data returned.

  Sorting:

  sortBy and sortOrder are used to specify the sort order of the result set.
