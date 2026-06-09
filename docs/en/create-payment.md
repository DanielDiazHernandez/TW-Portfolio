# Create a payment

> ### ℹ️Before you start
>
> This is a fictional technical documentation example created for portfolio purposes. It simulates a payment API and does not contain confidential information.

The NexusPay API allows you to create payment requests to process your customers’ transactions, both in a sandbox and production environment.

To create a payment, you must send a `POST` request to the payments endpoint and include your merchant’s Bearer Token in the `Authorization` header. <br></br>

## Requirements

Before creating a payment, you need:

* An active NexusPay account.
* Access to the sandbox or production environment.
* A valid Bearer Token.
* The transaction amount.
* The transaction currency.
* The customer information.
* A redirect or confirmation URL. <br></br>

## Endpoint

```http
POST /payments
```

## Headers

| Header          | Required | Description                                                           |
| --------------- | -------- | --------------------------------------------------------------------- |
| `Authorization` | Yes      | Bearer Token used to authenticate the request.                        |
| `Content-Type`  | Yes      | Indicates the format of the body sent. It must be `application/json`. |

### Headers example

```http
Authorization: Bearer {{access_token}}
Content-Type: application/json
```

<p>&nbsp;</p>

## Body Request

```bash
curl --request POST 'https://api.sandbox.nexuspay.com/v1/payments' \
  --header 'Authorization: Bearer {{access_token}}' \
  --header 'Content-Type: application/json' \
  --data-raw '{
    "amount": 1500.00,
    "currency": "MXN",
    "description": "Online store purchase",
    "customer": {
      "name": "Ana Pérez",
      "email": "ana.perez@example.com"
    },
    "payment_method": {
      "type": "card",
      "card_token": "card_token_123456"
    },
    "redirect_url": "https://www.example.com/payment/success"
  }'
```

### Request body

```json
{
  "amount": 1500.00,
  "currency": "MXN",
  "description": "Online store purchase",
  "customer": {
    "name": "Ana Pérez",
    "email": "ana.perez@example.com"
  },
  "payment_method": {
    "type": "card",
    "card_token": "card_token_123456"
  },
  "redirect_url": "https://www.example.com/payment/success"
}
```

<p>&nbsp;</p>

## Body parameters

| Parameter                   | Type   | Required | Description                                                             |            |      |           |
| --------------------------- | ------ | -------- | ----------------------------------------------------------------------- | ---------- | ---- | --------- |
| `amount`                    | number | Yes      | Total transaction amount.                                               |            |      |           |
| `currency`                  | string | Yes      | Currency ISO code: `MXN`.                                               |            |      |           |
| `description`               | string | No       | Payment description.                                                    |            |      |           |
| `customer`                  | object | Yes      | Object that contains the customer information.                          |            |      |           |
| `customer.name`             | string | Yes      | Customer’s full name.                                                   |            |      |           |
| `customer.email`            | string | Yes      | Customer’s email address.                                               |            |      |           |
| `payment_method`            | object | Yes      | Object that contains the payment method information.                    |            |      |           |
| `payment_method.type`       | string | Yes      | Payment method type: `card`                                             | `transfer` | `QR` | `wallet`. |
| `payment_method.card_token` | string | No       | Card token previously generated.                                        |            |      |           |
| `redirect_url`              | string | Yes      | URL where the customer will be redirected after completing the payment. |            |      |           |

<p>&nbsp;</p>

## Successful response example

**HTTP status:** `201 Created`

```json
{
  "payment_id": "payment_123456",
  "status": "pending",
  "amount": 1500.00,
  "currency": "MXN",
  "description": "Online store purchase",
  "created_at": "2026-06-08T18:30:00Z",
  "payment_url": "https://checkout.nexuspay.com/pay/payment_123456"
}
```

## Successful response fields

| Field         | Type   | Description                                                                           |
| ------------- | ------ | ------------------------------------------------------------------------------------- |
| `payment_id`  | string | Unique payment identifier.                                                            |
| `status`      | string | Current payment status. For example, `pending`, `approved`, `rejected`, or `expired`. |
| `amount`      | number | Total transaction amount.                                                             |
| `currency`    | string | Transaction currency.                                                                 |
| `description` | string | Payment description sent in the request.                                              |
| `created_at`  | string | Payment creation date and time in ISO format.                                         |
| `payment_url` | string | Weebhook.                                                                             |

<p>&nbsp;</p>

## Rejected response example

**HTTP status:** `400 Bad Request`

```json
{
  "error": {
    "code": "INVALID_AMOUNT",
    "message": "The transaction amount must be greater than zero."
  }
}
```

## Rejected response fields

| Field           | Type   | Description                                                                                                                                             |
| --------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `error`         | object | Object that contains the error information.                                                                                                             |
| `error.code`    | string | Internal error code.                                                                                                                                    |
| `error.message` | string | Message that indicates the reason for the rejection. Check the rejection reasons in the following [article](www.nexuspay.com/motivos-de-rechazo/pagos). |

<p>&nbsp;</p>

## Payment statuses

After creating a payment, NexusPay can return one of the following statuses:

| Status      | Description                                          |
| ----------- | ---------------------------------------------------- |
| `pending`   | The payment was created and is pending confirmation. |
| `approved`  | The payment was approved successfully.               |
| `rejected`  | The payment was rejected.                            |
| `expired`   | The payment expired before being completed.          |
| `cancelled` | The payment was cancelled.                           |

<p>&nbsp;</p>

## Rejection reasons

Below are possible rejection cases when creating a payment, along with their error code and description.

| HTTP Code | Error code               | Description                                                |
| --------- | ------------------------ | ---------------------------------------------------------- |
| `400`     | `INVALID_REQUEST`        | The request does not include all required fields.          |
| `400`     | `INVALID_AMOUNT`         | The transaction amount is invalid or less than zero.       |
| `400`     | `INVALID_CURRENCY`       | The currency sent is not valid or is not supported.        |
| `401`     | `AUTH_TOKEN_INVALID`     | The Bearer Token is invalid or has been modified.          |
| `401`     | `AUTH_TOKEN_EXPIRED`     | The Bearer Token has expired.                              |
| `403`     | `FORBIDDEN`              | The credentials do not have permission to create payments. |
| `422`     | `INVALID_PAYMENT_METHOD` | The payment method is invalid or cannot be used.           |

<p>&nbsp;</p>

## Best practices

To create payments securely and consistently:

* Validate the amount before sending the request.
* Verify that the currency is compatible with your merchant account.
* Do not send sensitive card data directly to the API.
* Use securely generated card tokens.
* Store the `payment_id` to check the payment status later.
* Use the sandbox environment to test integrations before processing real payments.
* Handle the `pending`, `approved`, `rejected`, `expired`, and `cancelled` statuses in your system.
* Implement webhooks to receive automatic updates about the payment status.
