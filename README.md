# Illuminate Mobile REST API

The illuminate-mobile-rest-api repository contains information and examples 
for using the Illuminate Mobile REST API to read metadata for a digital twin
or protected image on the Illuminate Platform.

The metadata is specific to digital watermarks; digital twins created in an
account with a Engage Premium subscription must have a digital watermark. 
Twins with only a QR code don't have a CPM path, which is required to get the
metadata.

* [Getting Started](#getting-started)
* [Request](#request)
* [Examples](#examples)
* [Help](#help)


## Getting Started

An Account Administrator can use the Illuminate UI to get the account ID and 
create Mobile API keys for the account you want to work with. The Mobile API 
keys have read-only (`MOBILE_READ_ONLY`) permissions.

Include the API key in the authorization header for every request. See the 
[Examples](#examples).

The base API URL is:

```
https://api.dmrc.app/api/v1
```

For the [request](#requests) detailed below, append the path and any 
parameters to the base URL.

### Request 

Use the following endpoint:

```
GET /metadata/{accountId}/{cpmPath}
Authorization: ApiKey $API_KEY
```

**Path Parameters**

| Name | Type | Required? | Description |
|------|------|-----------|-------------|
| `accountId` | String | yes | ID of your account on the Illuminate Platform |
| `cpmPath` | String | yes | The encrypted payload from the watermark, passed from the Digimarc Mobile SDK |

**Query Parameters**

| Name | Type | Required? | Description |
|------|------|-----------|-------------|
| `fields` | String | yes | A comma-separated list of the metadata you want to receive |

Using the `fields` parameter, you can retrieve the following metadata from the 
Digimarc Mobile REST API:

| Name | Type | Required? | Description |
|------|------|-----------|-------------|
| `redirectUrl` | String | no | The redirect URL for the digital watermark (for Engage Premium subscribers) |
| `digitalTwinId` | String | no | The ID of the digital twin associated with the watermark (for Engage Premium subscribers) |
| `dataCarrierId` | String | no | The ID of the digital watermark (for Engage Premium subscribers) |
| `attribute` | String | no | Use this keyword followed by a colon (:) and specify the attribute name whose value you want |

> The redirectUrl is similar to a QR code's Short URL or GS1 Digital Link, 
but it's specific to digital watermarks in an Engage Premium account. If the
digital twin has an engagement behavior, Engage Premium forwards the consumer
who scanned the watermark to the appropriate destination URL for the engagement. 
If the digital twin has no engagement behavior, the consumer might see a 
4xx HTTP error.

## Examples

The following examples will help you understand what you can do with the 
Illuminate Mobile REST API. The input parameters are provided by the digital 
twin API or Validate Media API and the DM SDK after the consumer scans the 
watermarked image.

* [Get the Digital Twin ID](#get-the-digital-twin-id)
* [Get the Redirect URL](#get-the-redirect-url)
* [Get the Watermark ID](#get-the-watermark-id)
* [Get the Custom Attribute Data](#get-the-custom-attribute-data)
* [Get All Metadata](#get-all-metadata)

### Get the Digital Twin ID

This example gets the ID of the digital twin associated with the watermark. 
It's applicable only for Engage Premium subscribers.

```shell
curl -H "Authorization: ApiKey $API_KEY" \
  -X GET '$API_URL/metadata/$ACCOUNTID/$CPM_PATH?fields=digitalTwinId'
```

### Get the Redirect URL

This example gets the digital twin's redirect URL. It's applicable only for 
Engage Premium subscribers.

```shell
curl -H "Authorization: ApiKey $API_KEY" \
  -X GET '$API_URL/metadata/$ACCOUNTID/$CPM_PATH?fields=redirectUrl'
```

### Get the Watermark ID

This example gets the ID of the digital watermark. It's applicable only for 
Engage Premium subscribers.

```shell
curl -H "Authorization: ApiKey $API_KEY" \
  -X GET '$API_URL/metadata/$ACCOUNTID/$CPM_PATH?fields=dataCarrierId'
```

### Get the Custom Attribute Data

This example gets values for custom attributes called attr1 and attr2 in a 
digital twin or protected image. 

```shell
curl -H "Authorization: ApiKey $API_KEY" \
  -X GET '$API_URL/metadata/$ACCOUNTID/$CPM_PATH?fields=attribute:attr1,attribute:attr2'
```

### Get All Metadata

This example gets all available metadata from the watermark, including custom 
attribute data. It's specific to Engage Premium; protected images don't have 
the redirectUrl, dataCarrierId, or digitalTwinId fields.

```shell
curl -H "Authorization: ApiKey $API_KEY" \
  -X GET '$API_URL/metadata/$ACCOUNTID/$CPM_PATH?fields=redirectUrl,dataCarrierId,digitalTwinId,attribute:attr1,attribute:attr2'
```

## Help

You can request assistance by creating a support ticket in the Illuminate UI. 
Log into Illuminate and click the Help icon to get started.
