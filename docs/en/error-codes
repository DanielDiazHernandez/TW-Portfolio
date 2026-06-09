# Error codes

> ### ℹ️Before you start
>
> This is a fictional technical documentation example created for portfolio purposes. It simulates a payment API and does not contain confidential information.

The NexusPay API uses error codes to indicate when a request could not be processed correctly.

Each error includes an HTTP code, an internal code, and a descriptive message to help you identify the issue and apply the corresponding action. <br></br>

## Error structure

When a request is rejected, the API responds with an `error` object.

### Error response example

**HTTP status:** `400 Bad Request`

```json
{
  "error": {
    "code": "INVALID_REQUEST",
    "message": "The request does not include all required fields."
  }
}
```

### Response fields

| Field           | Type   | Description                                      |
| --------------- | ------ | ------------------------------------------------ |
| `error`         | object | Object that contains the error information.      |
| `error.code`    | string | Internal code that identifies the error type.    |
| `error.message` | string | Message that describes the reason for rejection. |

<p>&nbsp;</p>

## HTTP codes

HTTP codes indicate the general result of the request.

| HTTP Code | Name                    | Description                                                                                  |
| --------- | ----------------------- | -------------------------------------------------------------------------------------------- |
| `200`     | `OK`                    | The request was processed successfully.                                                      |
| `201`     | `Created`               | The resource was created successfully.                                                       |
| `400`     | `Bad Request`           | The request contains invalid or incomplete data.                                             |
| `401`     | `Unauthorized`          | The request could not be authenticated.                                                      |
| `403`     | `Forbidden`             | The credentials do not have permission to access the requested resource.                     |
| `404`     | `Not Found`             | The requested resource was not found.                                                        |
| `409`     | `Conflict`              | The request could not be completed due to a conflict with the current state of the resource. |
| `422`     | `Unprocessable Entity`  | The request has a valid format, but cannot be processed due to business rules.               |
| `429`     | `Too Many Requests`     | The allowed request limit was exceeded.                                                      |
| `500`     | `Internal Server Error` | An unexpected server error occurred.                                                         |

<p>&nbsp;</p>

## Authentication errors

Authentication errors occur when the credentials or Bearer Token are not valid.

| HTTP Code | Error code            | Description                                                              | Recommended action                                                                    |
| --------- | --------------------- | ------------------------------------------------------------------------ | ------------------------------------------------------------------------------------- |
| `401`     | `INVALID_CREDENTIALS` | The `merchant_id` or `merchant_secret` is incorrect.                     | Verify your credentials and generate the Bearer Token again.                          |
| `401`     | `AUTH_TOKEN_EXPIRED`  | The Bearer Token has expired.                                            | Generate a new Bearer Token using your integration credentials.                       |
| `401`     | `AUTH_TOKEN_INVALID`  | The Bearer Token is invalid or has been modified.                        | Check that the token is complete and correctly sent in the `Authorization` header.    |
| `403`     | `FORBIDDEN`           | The credentials do not have permission to access the requested resource. | Verify the permissions assigned to your merchant account or contact the support team. |

<p>&nbsp;</p>

## Payment errors

Payment errors occur when the request to create or process a payment contains invalid information or does not meet business rules.

| HTTP Code | Error code                  | Description                                                                       | Recommended action                                                    |
| --------- | --------------------------- | --------------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| `400`     | `INVALID_REQUEST`           | The request does not include all required fields.                                 | Review the required parameters before sending the request.            |
| `400`     | `INVALID_AMOUNT`            | The transaction amount is invalid or less than zero.                              | Send a valid amount greater than zero.                                |
| `400`     | `INVALID_CURRENCY`          | The currency sent is not valid or is not supported.                               | Verify that the currency is enabled for your merchant account.        |
| `422`     | `INVALID_PAYMENT_METHOD`    | The payment method is invalid or cannot be used.                                  | Verify that the payment method is active and correctly configured.    |
| `422`     | `PAYMENT_DECLINED`          | The payment was rejected by the issuer, bank, or processor.                       | Ask the customer to use another payment method or contact their bank. |
| `409`     | `PAYMENT_ALREADY_PROCESSED` | The payment has already been processed and cannot be modified.                    | Check the current payment status before attempting a new operation.   |
| `404`     | `PAYMENT_NOT_FOUND`         | The `payment_id` sent does not exist or does not belong to your merchant account. | Verify the payment identifier and query it again.                     |

<p>&nbsp;</p>

## Payout errors

Payout errors occur when a disbursement, withdrawal, or transfer request cannot be processed correctly.

| HTTP Code | Error code              | Description                                                                      | Recommended action                                                    |
| --------- | ----------------------- | -------------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| `400`     | `INVALID_BENEFICIARY`   | The beneficiary information is invalid or incomplete.                            | Verify the beneficiary name, account, bank, and required information. |
| `400`     | `INVALID_BANK_ACCOUNT`  | The bank account sent does not have a valid format.                              | Check that the bank account meets the required format.                |
| `400`     | `INVALID_PAYOUT_AMOUNT` | The payout amount is invalid or less than zero.                                  | Send a valid amount greater than zero.                                |
| `403`     | `PAYOUTS_NOT_ENABLED`   | The merchant account does not have the payouts feature enabled.                  | Request payout activation for your merchant account.                  |
| `422`     | `INSUFFICIENT_FUNDS`    | The merchant account does not have enough balance to complete the payout.        | Check your balance before sending a new request.                      |
| `422`     | `PAYOUT_REJECTED`       | The payout was rejected by the bank or processor.                                | Verify the beneficiary information or try another bank account.       |
| `404`     | `PAYOUT_NOT_FOUND`      | The `payout_id` sent does not exist or does not belong to your merchant account. | Verify the payout identifier and query it again.                      |

<p>&nbsp;</p>

## Webhook errors

Webhook errors occur when NexusPay cannot correctly deliver a notification to the URL configured by your merchant account.

| HTTP Code | Error code                   | Description                                                     | Recommended action                                                   |
| --------- | ---------------------------- | --------------------------------------------------------------- | -------------------------------------------------------------------- |
| `400`     | `INVALID_WEBHOOK_URL`        | The URL configured to receive webhooks is not valid.            | Verify that the URL uses HTTPS and has the correct format.           |
| `401`     | `WEBHOOK_SIGNATURE_INVALID`  | The webhook signature is not valid.                             | Check that you are validating the signature with the correct secret. |
| `404`     | `WEBHOOK_ENDPOINT_NOT_FOUND` | The configured URL does not respond or does not exist.          | Verify that the endpoint is publicly available.                      |
| `408`     | `WEBHOOK_TIMEOUT`            | The merchant endpoint did not respond within the expected time. | Optimize your endpoint response time.                                |
| `500`     | `WEBHOOK_DELIVERY_FAILED`    | The webhook could not be delivered after several attempts.      | Review your server logs and enable delivery again if necessary.      |

<p>&nbsp;</p>

## Limit and availability errors

These errors occur when usage limits are exceeded or when the API is temporarily unavailable.

| HTTP Code | Error code              | Description                                    | Recommended action                                                            |
| --------- | ----------------------- | ---------------------------------------------- | ----------------------------------------------------------------------------- |
| `429`     | `RATE_LIMIT_EXCEEDED`   | The allowed request limit was exceeded.        | Reduce the request frequency and try again after a few seconds.               |
| `500`     | `INTERNAL_SERVER_ERROR` | An unexpected server error occurred.           | Try again later. If the issue persists, contact support.                      |
| `503`     | `SERVICE_UNAVAILABLE`   | The service is temporarily unavailable.        | Try again later.                                                              |
| `504`     | `GATEWAY_TIMEOUT`       | The server did not receive a response in time. | Try again and verify whether the operation was processed before repeating it. |

<p>&nbsp;</p>

## Error handling example

When you receive an error, first review the HTTP code and then the internal error code.

### Example

```json
{
  "error": {
    "code": "INSUFFICIENT_FUNDS",
    "message": "The merchant account does not have enough balance to complete the payout."
  }
}
```

In this case, the expected HTTP code would be `422 Unprocessable Entity`, since the request has a valid format but cannot be processed due to insufficient balance.

The recommended action is to verify the merchant account balance before attempting to create a new payout.

<p>&nbsp;</p>

## Best practices

To handle errors securely and consistently:

* Validate required fields before sending a request.
* Store the HTTP code and internal error code in your logs.
* Do not show technical messages directly to the end user.
* Use clear and friendly messages in your interface.
* Retry only temporary errors, such as `RATE_LIMIT_EXCEEDED`, `SERVICE_UNAVAILABLE`, or `GATEWAY_TIMEOUT`.
* Do not retry payments or payouts without first checking the operation status.
* Contact support if you receive repeated errors with the same operation identifier.
