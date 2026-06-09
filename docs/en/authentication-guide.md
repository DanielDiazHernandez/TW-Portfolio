# Authentication

> ### ℹ️Before you start
>
> This is a fictional technical documentation example created for portfolio purposes. It simulates a payment API and does not contain confidential information.

The NexusPay API uses Bearer Token authentication to validate requests sent by merchants. This helps meet high privacy and security standards in each transaction.

To make a request to the API, you must include your access token in the `Authorization` header of each request. <br></br>

## Requirements

Before making a request, make sure you have the following:

* An active NexusPay account.
* Access to the sandbox or production environment.
* Your `merchant_id`.
* Your `merchant_secret`.
* A valid Bearer Token, either generic or assigned to your merchant account. <br></br>

## Available environments

DemoPay has two environments:

| Environment | Description                                                                   | Base URL                              |
| ----------- | ----------------------------------------------------------------------------- | ------------------------------------- |
| Sandbox     | Test environment used to validate integrations without processing real money. | `https://api.sandbox.nexuspay.com/v1` |
| Production  | Environment used to process transactions with real funds and data.            | `https://api.nexuspay.com/v1`         |

> ### ℹ️Environment types
>
> Always use the sandbox environment to test your integrations before sending requests in production.

<p>&nbsp;</p>

## Get credentials

To get your integration credentials:

1. Log in to the [NexusPay dashboard](www.nexuspay.com/dashboard/login).
2. Go to **Developers > API credentials**.
3. Select the environment you want to use: **Sandbox** or **Production**.
4. Copy your `merchant_id` and `merchant_secret`.
5. Store your credentials in a secure place.

> ### ⚠️Important
>
> Do not share your `merchant_secret` or expose it in public documentation, frontend applications, or collaboration tools.

<p>&nbsp;</p>

## Generate a Bearer Token

To generate a Bearer Token, send a `POST` request to the authentication endpoint.

### Endpoint

```http
POST /auth/token
```

### Body Request

```bash
curl --request POST 'https://api.sandbox.nexuspay.com/v1/auth/token' \
  --header 'Content-Type: application/json' \
  --data-raw '{
    "merchant_id": "demo_merchant_id",
    "merchant_secret": "demo_merchant_secret"
  }'
```

### Body parameters

| Parameter         | Type   | Required | Description                                             |
| ----------------- | ------ | -------- | ------------------------------------------------------- |
| `merchant_id`     | string | Yes      | Unique merchant identifier.                             |
| `merchant_secret` | string | Yes      | Merchant private key used to generate the access token. |

### Successful generation response example

**HTTP status:** `200 OK`

```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.demo-token",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

### Successful response fields

| Field          | Type    | Description                                              |
| -------------- | ------- | -------------------------------------------------------- |
| `access_token` | string  | Token that you must place in the `Authorization` header. |
| `token_type`   | string  | Type of token generated. In this case, `Bearer`.         |
| `expires_in`   | integer | Token validity period, expressed in seconds.             |

### Rejected generation response example

**HTTP status:** `401 Unauthorized`

```json
{
  "error": {
    "code": "INVALID_CREDENTIALS",
    "message": "The merchant credentials are invalid. Verify your merchant_id and merchant_secret and try again."
  }
}
```

### Rejected response fields

| Field           | Type   | Description                                                                                                                                                   |
| --------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `error`         | object | Object that contains the authentication error information.                                                                                                    |
| `error.code`    | string | Internal error code.                                                                                                                                          |
| `error.message` | string | Message that indicates the reason for the rejection. Check the rejection reasons in the following [article](www.nexuspay.com/motivos-de-rechazo/bearer-token) |

<p>&nbsp;</p>

## Use the Bearer Token

Once your Bearer Token has been generated, you can include it in the `Authorization` header of each request to the NexusPay API.

### Header format

```http
Authorization: Bearer {{access_token}}
```

### Request example

```bash
curl --request GET 'https://api.sandbox.nexuspay.com/v1/payments/payment_123456' \
  --header 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.demo-token' \
  --header 'Content-Type: application/json'
```

<p>&nbsp;</p>

## Bearer Token expiration

Your Bearer Token has a time limit. When the token expires, the API will respond with a `401 Unauthorized` error.

To continue sending requests, you must generate a new token using your integration credentials.

### Expired token error example

**HTTP status:** `401 Unauthorized`

```json
{
  "error": {
    "code": "AUTH_TOKEN_EXPIRED",
    "message": "The access token has expired. Generate a new token and try again."
  }
}
```

### Rejected response fields

| Field           | Type   | Description                                                                                                                                                   |
| --------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `error`         | object | Object that contains the authentication error information.                                                                                                    |
| `error.code`    | string | Internal error code.                                                                                                                                          |
| `error.message` | string | Message that indicates the reason for the rejection. Check the rejection reasons in the following [article](www.nexuspay.com/motivos-de-rechazo/bearer-token) |

<p>&nbsp;</p>

## Rejection reasons

Below are possible rejection cases from NexusPay, along with their error code and description.

| HTTP Code | Error code            | Description                                                              |
| --------- | --------------------- | ------------------------------------------------------------------------ |
| `400`     | `INVALID_REQUEST`     | The request does not include all required fields.                        |
| `401`     | `INVALID_CREDENTIALS` | The `merchant_id` or `merchant_secret` is incorrect.                     |
| `401`     | `AUTH_TOKEN_EXPIRED`  | The Bearer Token has expired.                                            |
| `401`     | `AUTH_TOKEN_INVALID`  | The Bearer Token is invalid or has been modified.                        |
| `403`     | `FORBIDDEN`           | The credentials do not have permission to access the requested resource. |

<p>&nbsp;</p>

## Best practices

To keep your integration secure:

* Do not share your `merchant_secret`.
* Do not expose credentials in frontend applications.
* Do not upload tokens or private keys to public repositories.
* Use environment variables to store credentials.
* Generate a new token when the current one expires.
* Use different credentials for sandbox and production.
* Revoke and replace your credentials if you suspect they have been compromised.

<p>&nbsp;</p>

## Next steps

Once your authentication is configured, you can send your first request to create a payment.

Check the [Create a payment](./crear-pago.md) guide to continue with the integration.
