---
layout: default
title: Mobileid Init Hash Signing
parent: API Reference
has_toc: true
nav_order: 4
---

# Mobileid Init Hash Signing
{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

Short description

## Endpoint

<table>
  <tbody>
    <tr>
      <td>Path (Locale: LT)</td>
      <td>/mobile/sign/hash.json</td>
    </tr>
    <tr>
      <td>Path (Locale: EN)</td>
      <td>/en/mobile/sign/hash.json</td>
    </tr>
    <tr>
      <td>Method</td>
      <td>POST</td>
    </tr>
    <tr>
      <td>Request Body Schema</td>
      <td>application/json</td>
    </tr>
  </tbody>
</table>

## Request body parameter description

| Key | Requirement | Type | Description |
| :--- | :--- | :--- | :--- |
| access_token | Mandatory | String | Description |
| hash | Mandatory | String | Description |
| phone | Mandatory | String | Description |
| hash_algorithm | Mandatory | String | Description |
| country | Mandatory | String | Description |
| message | Mandatory | String | Description |
| code | Mandatory | String | Description |



## Response body parameter description

### Successful response

| Key | Type | Description |
| :--- | :--- | :--- |
| status | String | Description |
| token | String | Description |
| control_code | String | Description |



### Failed response

| Key | Type | Description |
| :--- | :--- | :--- |
| status | String | Description |
| message | String | Description |
| error_code | Integer | Description |



## Sample request

```
POST /en/mobile/sign/hash.json HTTP/1.1
Host: app.marksign.local
Content-Type: application/json

{
  "access_token": "52900c96-3f60-5307-3719-5948f0191da6",
  "hash": "5fef901dc387c585fb470ec7887e204a23d8e16434ec94940019951bd02ffe44",
  "phone": "+37269000366",
  "hash_algorithm": "SHA256",
  "country": "EE",
  "message": "any message",
  "code": "60001017705"
}
```

## Sample response

### Sample success response

```
{
  "status": "ok",
  "token": "31c5b0b8-9534-3e0e-2126-9d25945efc4c",
  "control_code": "3555"
}
```

### Sample failed response

```
{
  "status": "error",
  "message": "Invalid parameter [country]: This value is not valid.",
  "error_code": 40001
}
```

## Implementation

### CURL

```
curl --location --request POST 'https://app.marksign.local/en/mobile/sign/hash.json' \
--header 'Content-Type: application/json' \
--data-raw '{
  "access_token": "52900c96-3f60-5307-3719-5948f0191da6",
  "hash": "5fef901dc387c585fb470ec7887e204a23d8e16434ec94940019951bd02ffe44",
  "phone": "+37269000366",
  "hash_algorithm": "SHA256",
  "country": "EE",
  "message": "any message",
  "code": "60001017705"
}'
```

### Using php-client

To use the php-client, please follow the installation and basic usage [here](/documentation/sdk-php-client.html#usage), and use [`AppBundle\GatewaySDKPhp\RequestBuilder\MobileidInitHashSigningRequestBuilder`](/documentation/class-ref/GatewaySDKPhp/RequestBuilder/MobileidInitHashSigningRequestBuilder.html) as request builder.

```
/**
 * The token in response ($hashSignInitResArray['token'] in this example),
 * might need to be saved for future purposes.
 */
$accessToken = '6102d227-0dcf-4ce3-ab8f-337385f09ee4';

$filePath = __DIR__ . '/demo.pdf';

$hashSignInitReq = (new MobileidInitHashSigningRequestBuilder)
  ->withAccessToken($accessToken)
  ->withHash(hash('sha256', file_get_contents($filePath)))
  ->withHashAlgorithm('SHA256')
  ->withCountry('EE')
  ->withPhone('+37269000366')
  ->withCode('60001017705')
  ->withMessage('Dummy')
  ->createRequest();
$hashSignInitRes = $client->postRequest($hashSignInitReq);
$hashSignInitResArray = $hashSignInitRes->toArray(false);
var_dump($hashSignInitResArray);
```