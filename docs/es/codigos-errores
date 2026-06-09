# Códigos de error

> ### ℹ️Antes de empezar
>
> Este es un ejemplo ficticio de documentación técnica creado para fines de portafolio. Simula una API de pagos y no contiene información confidencial.

La API de NexusPay utiliza códigos de error para indicar cuando una solicitud no pudo procesarse correctamente.

Cada error incluye un código HTTP, un código interno y un mensaje descriptivo para ayudarte a identificar el problema y aplicar la acción correspondiente. <br></br>

## Estructura de un error

Cuando una solicitud es rechazada, la API responde con un objeto `error`.

### Ejemplo de respuesta de error

**HTTP status:** `400 Bad Request`

```json
{
  "error": {
    "code": "INVALID_REQUEST",
    "message": "La solicitud no incluye todos los campos requeridos."
  }
}
```

### Campos de respuesta

| Campo           | Tipo   | Descripción                                     |
| --------------- | ------ | ----------------------------------------------- |
| `error`         | object | Objeto que contiene la información del error.   |
| `error.code`    | string | Código interno que identifica el tipo de error. |
| `error.message` | string | Mensaje que describe el motivo del rechazo.     |

<p>&nbsp;</p>

## Códigos HTTP

Los códigos HTTP indican el resultado general de la solicitud.

| Código HTTP | Nombre                  | Descripción                                                                           |
| ----------- | ----------------------- | ------------------------------------------------------------------------------------- |
| `200`       | `OK`                    | La solicitud fue procesada correctamente.                                             |
| `201`       | `Created`               | El recurso fue creado correctamente.                                                  |
| `400`       | `Bad Request`           | La solicitud contiene datos inválidos o incompletos.                                  |
| `401`       | `Unauthorized`          | La solicitud no pudo autenticarse.                                                    |
| `403`       | `Forbidden`             | Las credenciales no tienen permisos para acceder al recurso solicitado.               |
| `404`       | `Not Found`             | El recurso solicitado no fue encontrado.                                              |
| `409`       | `Conflict`              | La solicitud no pudo completarse por un conflicto con el estado actual del recurso.   |
| `422`       | `Unprocessable Entity`  | La solicitud tiene un formato válido, pero no puede procesarse por reglas de negocio. |
| `429`       | `Too Many Requests`     | Se excedió el límite de solicitudes permitido.                                        |
| `500`       | `Internal Server Error` | Ocurrió un error inesperado en el servidor.                                           |

<p>&nbsp;</p>

## Errores de autenticación

Los errores de autenticación ocurren cuando las credenciales o el Bearer Token no son válidos.

| Código HTTP | Código de error       | Descripción                                                             | Acción recomendada                                                                      |
| ----------- | --------------------- | ----------------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| `401`       | `INVALID_CREDENTIALS` | El `merchant_id` o `merchant_secret` es incorrecto.                     | Verifica tus credenciales y vuelve a generar el Bearer Token.                           |
| `401`       | `AUTH_TOKEN_EXPIRED`  | El Bearer Token expiró.                                                 | Genera un nuevo Bearer Token usando tus credenciales de integración.                    |
| `401`       | `AUTH_TOKEN_INVALID`  | El Bearer Token es inválido o fue modificado.                           | Revisa que el token esté completo y correctamente enviado en el header `Authorization`. |
| `403`       | `FORBIDDEN`           | Las credenciales no tienen permisos para acceder al recurso solicitado. | Verifica los permisos asignados a tu comercio o contacta al equipo de soporte.          |

<p>&nbsp;</p>

## Errores de pagos

Los errores de pagos ocurren cuando la solicitud para crear o procesar un pago contiene información inválida o no cumple con las reglas de negocio.

| Código HTTP | Código de error             | Descripción                                                     | Acción recomendada                                                        |
| ----------- | --------------------------- | --------------------------------------------------------------- | ------------------------------------------------------------------------- |
| `400`       | `INVALID_REQUEST`           | La solicitud no incluye todos los campos requeridos.            | Revisa los parámetros obligatorios antes de enviar la solicitud.          |
| `400`       | `INVALID_AMOUNT`            | El monto de la transacción es inválido o menor a cero.          | Envía un monto válido y mayor a cero.                                     |
| `400`       | `INVALID_CURRENCY`          | La moneda enviada no es válida o no está soportada.             | Verifica que la moneda esté habilitada para tu comercio.                  |
| `422`       | `INVALID_PAYMENT_METHOD`    | El método de pago es inválido o no puede utilizarse.            | Verifica que el método de pago esté activo y correctamente configurado.   |
| `422`       | `PAYMENT_DECLINED`          | El pago fue rechazado por el emisor, banco o procesador.        | Solicita al cliente usar otro método de pago o contactar a su banco.      |
| `409`       | `PAYMENT_ALREADY_PROCESSED` | El pago ya fue procesado y no puede modificarse.                | Consulta el estado actual del pago antes de intentar una nueva operación. |
| `404`       | `PAYMENT_NOT_FOUND`         | El `payment_id` enviado no existe o no pertenece a tu comercio. | Verifica el identificador del pago y vuelve a consultar.                  |

<p>&nbsp;</p>

## Errores de payouts

Los errores de payouts ocurren cuando una solicitud de dispersión, retiro o transferencia no puede procesarse correctamente.

| Código HTTP | Código de error         | Descripción                                                          | Acción recomendada                                                      |
| ----------- | ----------------------- | -------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| `400`       | `INVALID_BENEFICIARY`   | Los datos del beneficiario son inválidos o están incompletos.        | Verifica el nombre, cuenta, banco y datos requeridos del beneficiario.  |
| `400`       | `INVALID_BANK_ACCOUNT`  | La cuenta bancaria enviada no tiene un formato válido.               | Revisa que la cuenta bancaria cumpla con el formato requerido.          |
| `400`       | `INVALID_PAYOUT_AMOUNT` | El monto del payout es inválido o menor a cero.                      | Envía un monto válido y mayor a cero.                                   |
| `403`       | `PAYOUTS_NOT_ENABLED`   | El comercio no tiene habilitada la funcionalidad de payouts.         | Solicita la activación de payouts para tu comercio.                     |
| `422`       | `INSUFFICIENT_FUNDS`    | El comercio no cuenta con saldo suficiente para completar el payout. | Verifica tu balance antes de enviar una nueva solicitud.                |
| `422`       | `PAYOUT_REJECTED`       | El payout fue rechazado por el banco o procesador.                   | Verifica los datos del beneficiario o intenta con otra cuenta bancaria. |
| `404`       | `PAYOUT_NOT_FOUND`      | El `payout_id` enviado no existe o no pertenece a tu comercio.       | Verifica el identificador del payout y vuelve a consultar.              |

<p>&nbsp;</p>

## Errores de webhooks

Los errores de webhooks ocurren cuando NexusPay no puede entregar correctamente una notificación a la URL configurada por tu comercio.

| Código HTTP | Código de error              | Descripción                                                       | Acción recomendada                                                              |
| ----------- | ---------------------------- | ----------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| `400`       | `INVALID_WEBHOOK_URL`        | La URL configurada para recibir webhooks no es válida.            | Verifica que la URL use HTTPS y tenga un formato correcto.                      |
| `401`       | `WEBHOOK_SIGNATURE_INVALID`  | La firma del webhook no es válida.                                | Revisa que estés validando la firma con el secreto correcto.                    |
| `404`       | `WEBHOOK_ENDPOINT_NOT_FOUND` | La URL configurada no responde o no existe.                       | Verifica que el endpoint esté disponible públicamente.                          |
| `408`       | `WEBHOOK_TIMEOUT`            | El endpoint del comercio no respondió dentro del tiempo esperado. | Optimiza el tiempo de respuesta de tu endpoint.                                 |
| `500`       | `WEBHOOK_DELIVERY_FAILED`    | No fue posible entregar el webhook después de varios intentos.    | Revisa los logs de tu servidor y vuelve a habilitar la entrega si es necesario. |

<p>&nbsp;</p>

## Errores de límites y disponibilidad

Estos errores ocurren cuando se exceden límites de uso o cuando la API no está disponible temporalmente.

| Código HTTP | Código de error         | Descripción                                    | Acción recomendada                                                                 |
| ----------- | ----------------------- | ---------------------------------------------- | ---------------------------------------------------------------------------------- |
| `429`       | `RATE_LIMIT_EXCEEDED`   | Se excedió el límite de solicitudes permitido. | Reduce la frecuencia de solicitudes e intenta nuevamente después de unos segundos. |
| `500`       | `INTERNAL_SERVER_ERROR` | Ocurrió un error inesperado en el servidor.    | Intenta nuevamente más tarde. Si el problema persiste, contacta a soporte.         |
| `503`       | `SERVICE_UNAVAILABLE`   | El servicio no está disponible temporalmente.  | Intenta nuevamente más tarde.                                                      |
| `504`       | `GATEWAY_TIMEOUT`       | El servidor no recibió respuesta a tiempo.     | Intenta nuevamente y verifica si la operación fue procesada antes de repetirla.    |

<p>&nbsp;</p>

## Ejemplo de manejo de errores

Cuando recibas un error, revisa primero el código HTTP y después el código interno del error.

### Ejemplo

```json
{
  "error": {
    "code": "INSUFFICIENT_FUNDS",
    "message": "El comercio no cuenta con saldo suficiente para completar el payout."
  }
}
```

En este caso, el código HTTP esperado sería `422 Unprocessable Entity`, ya que la solicitud tiene un formato válido, pero no puede procesarse por falta de saldo.

La acción recomendada es verificar el balance del comercio antes de intentar crear un nuevo payout.

<p>&nbsp;</p>

## Buenas prácticas

Para manejar errores de forma segura y consistente:

* Valida los campos requeridos antes de enviar una solicitud.
* Guarda el código HTTP y el código interno del error en tus logs.
* No muestres mensajes técnicos directamente al usuario final.
* Usa mensajes claros y amigables en tu interfaz.
* Reintenta únicamente errores temporales, como `RATE_LIMIT_EXCEEDED`, `SERVICE_UNAVAILABLE` o `GATEWAY_TIMEOUT`.
* No reintentes pagos o payouts sin verificar antes el estado de la operación.
* Contacta a soporte si recibes errores repetidos con el mismo identificador de operación.

