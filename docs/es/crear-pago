# Crear un pago

> ### ℹ️Antes de empezar
>
> Este es un ejemplo ficticio de documentación técnica creado para fines de portafolio. Simula una API de pagos y no contiene información confidencial.

La API de NexusPay permite crear solicitudes de pago para procesar transacciones de tus clientes, tanto en un ambiente sandbox como de producción.

Para crear un pago, debes enviar una solicitud `POST` al endpoint de pagos e incluir el Bearer Token de tu comercio en el header `Authorization`. <br></br>

## Requisitos

Antes de crear un pago, necesitas:

* Una cuenta activa en NexusPay.
* Acceso al ambiente sandbox o producción.
* Un Bearer Token válido.
* El monto de la transacción.
* La moneda de la transacción.
* Los datos del cliente.
* Una URL de redirección o confirmación. <br></br>

## Endpoint

```http
POST /payments
```

## Headers

| Header          | Requerido | Descripción                                                      |
| --------------- | --------- | ---------------------------------------------------------------- |
| `Authorization` | Sí        | Bearer Token utilizado para autenticar la solicitud.             |
| `Content-Type`  | Sí        | Indica el formato del body enviado. Debe ser `application/json`. |

### Ejemplo de headers

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
    "description": "Compra en tienda en línea",
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
  "description": "Compra en tienda en línea",
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

## Parámetros del body

| Parámetro                   | Tipo   | Requerido | Descripción                                                               |
| --------------------------- | ------ | --------- | ------------------------------------------------------------------------- |
| `amount`                    | number | Sí        | Monto total de la transacción.                                            |
| `currency`                  | string | Sí        | Código ISO de la moneda: `MXN`. |
| `description`               | string | No        | Descripción del pago.                                                     |
| `customer`                  | object | Sí        | Objeto que contiene la información del cliente.                           |
| `customer.name`             | string | Sí        | Nombre completo del cliente.                                              |
| `customer.email`            | string | Sí        | Correo electrónico del cliente.                                           |
| `payment_method`            | object | Sí        | Objeto que contiene la información del método de pago.                    |
| `payment_method.type`       | string | Sí        | Tipo de método de pago: `card`|`transfer`|`QR`|`wallet`.                              |
| `payment_method.card_token` | string | No        | Token de la tarjeta generado previamente.                                 |
| `redirect_url`              | string | Si        | URL a la que será redirigido el cliente después de completar el pago.     |

<p>&nbsp;</p>

## Ejemplo de respuesta exitosa

**HTTP status:** `201 Created`

```json
{
  "payment_id": "payment_123456",
  "status": "pending",
  "amount": 1500.00,
  "currency": "MXN",
  "description": "Compra en tienda en línea",
  "created_at": "2026-06-08T18:30:00Z",
  "payment_url": "https://checkout.nexuspay.com/pay/payment_123456"
}
```

## Campos de respuesta exitosa

| Campo         | Tipo   | Descripción                                                                         |
| ------------- | ------ | ----------------------------------------------------------------------------------- |
| `payment_id`  | string | Identificador único del pago.                                                       |
| `status`      | string | Estado actual del pago. Por ejemplo, `pending`, `approved`, `rejected` o `expired`. |
| `amount`      | number | Monto total de la transacción.                                                      |
| `currency`    | string | Moneda de la transacción.                                                           |
| `description` | string | Descripción del pago enviada en la solicitud.                                       |
| `created_at`  | string | Fecha y hora de creación del pago en formato ISO.                              |
| `payment_url` | string | Weebhook.                           |

<p>&nbsp;</p>

## Ejemplo de respuesta rechazada

**HTTP status:** `400 Bad Request`

```json
{
  "error": {
    "code": "INVALID_AMOUNT",
    "message": "El monto de la transacción debe ser mayor a cero."
  }
}
```

## Campos de respuesta rechazada

| Campo           | Tipo   | Descripción                                                                                                                                      |
| --------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| `error`         | object | Objeto que contiene la información del error.                                                                                                    |
| `error.code`    | string | Código interno del error.                                                                                                                        |
| `error.message` | string | Mensaje que indica el motivo del rechazo. Consulta los motivos de rechazo en el siguiente [artículo](www.nexuspay.com/motivos-de-rechazo/pagos). |

<p>&nbsp;</p>

## Estados del pago

Después de crear un pago, NexusPay puede devolver uno de los siguientes estados:

| Estado      | Descripción                                          |
| ----------- | ---------------------------------------------------- |
| `pending`   | El pago fue creado y está pendiente de confirmación. |
| `approved`  | El pago fue aprobado correctamente.                  |
| `rejected`  | El pago fue rechazado.                               |
| `expired`   | El pago expiró antes de completarse.                 |
| `cancelled` | El pago fue cancelado.                               |

<p>&nbsp;</p>

## Motivos de rechazo

A continuación se muestran posibles casos de rechazo al crear un pago, junto con su código de error y descripción.

| Código HTTP | Código de error          | Descripción                                            |
| ----------- | ------------------------ | ------------------------------------------------------ |
| `400`       | `INVALID_REQUEST`        | La solicitud no incluye todos los campos requeridos.   |
| `400`       | `INVALID_AMOUNT`         | El monto de la transacción es inválido o menor a cero. |
| `400`       | `INVALID_CURRENCY`       | La moneda enviada no es válida o no está soportada.    |
| `401`       | `AUTH_TOKEN_INVALID`     | El Bearer Token es inválido o fue modificado.          |
| `401`       | `AUTH_TOKEN_EXPIRED`     | El Bearer Token expiró.                                |
| `403`       | `FORBIDDEN`              | Las credenciales no tienen permisos para crear pagos.  |
| `422`       | `INVALID_PAYMENT_METHOD` | El método de pago es inválido o no puede utilizarse.   |

<p>&nbsp;</p>

## Buenas prácticas

Para crear pagos de forma segura y consistente:

* Valida el monto antes de enviar la solicitud.
* Verifica que la moneda sea compatible con tu comercio.
* No envíes datos sensibles de tarjetas directamente a la API.
* Utiliza tokens de tarjeta generados de forma segura.
* Guarda el `payment_id` para consultar el estado del pago posteriormente.
* Usa el ambiente sandbox para probar integraciones antes de procesar pagos reales.
* Maneja los estados `pending`, `approved`, `rejected`, `expired` y `cancelled` en tu sistema.
* Implementa webhooks para recibir actualizaciones automáticas del estado del pago.
