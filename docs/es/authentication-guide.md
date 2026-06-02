# Autenticación

> ### ℹ️Antes de empezar
> 
> Este es un ejemplo ficticio de una documentación técnica creado para fines de portafolio. Simula una API de pagos y no contiene información confidencial.

La API de NexusPay utiliza autenticación mediante Bearer Token para validar las solicitudes enviadas por los comercios. Esto permite cumplir con los altos estándares de privacida y seguridad en cada transacción.

Para realizar una solicitud de la API, debes incluir tu token de acceso en el header `Authorization` de cada una.
<br></br>

## Requisitos

Antes de realizar una solicitud, asegúrate de contar con lo siguiente:

* Una cuenta activa en NexusPay.
* Acceso al ambiente sandbox o producción.
* Tu `merchant_id`.
* Tu `merchant_id`.
* Un Bearer Token válido, ya sea genérico o de tu comercio.
<br></br>

## Ambientes disponibles

DemoPay cuenta con dos ambientes:

| Ambiente   | Descripción                                                              | Base URL                             |
| ---------- | ------------------------------------------------------------------------ | ------------------------------------ |
| Sandbox    | Ambiente de pruebas para validar integraciones sin procesar dinero real. | `https://api.sandbox.nexuspay.com/v1` |
| Producción | Ambiente para procesar transacciones con fondos y datos reales. | `https://api.nexuspay.com/v1`         |


> ### ℹ️Tipos de ambientes 
>
> Utiliza siempre el ambiente sandbox para realizar pruebas de tus integraciones antes de enviar solicitudes en producción.
<br></br>

## Obtener credenciales

Para obtener tus credenciales de integración:

1. Inicia sesión en el [dashboard de NexusPay](www.nexuspay.com/dashboard/login).
2. Dirígete a la ruta **Developers > API credentials**.
3. Selecciona el ambiente que deseas utilizar: **Sandbox** o **Producción**.
4. Copia tu `merchant_id` y `merchant_secret`.
5. Guarda tus credenciales en un lugar seguro.

> ### ⚠️Importante
>
> No compartas tu `merchant_secret` ni lo expongas en documentaciones públicas, aplicaciones frontend o herramientas de colaboración.
<br></br>
## Generar un Bearer Token

Para generar un Bearer Token, envía una solicitud `POST` al endpoint de autenticación.

### Endpoint

```http
POST /auth/token
```

### Request

```bash
curl --request POST 'https://api.sandbox.demopay.com/v1/auth/token' \
  --header 'Content-Type: application/json' \
  --data-raw '{
    "client_id": "demo_client_id",
    "client_secret": "demo_client_secret"
  }'
```

### Parámetros del body

| Parámetro       | Tipo   | Requerido | Descripción                                              |
| --------------- | ------ | --------- | -------------------------------------------------------- |
| `client_id`     | string | Sí        | Identificador público de la integración.                 |
| `client_secret` | string | Sí        | Llave privada utilizada para generar el token de acceso. |

### Response exitoso

```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.demo-token",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

### Campos del response

| Campo          | Tipo    | Descripción                                          |
| -------------- | ------- | ---------------------------------------------------- |
| `access_token` | string  | Token que debes enviar en el header `Authorization`. |
| `token_type`   | string  | Tipo de token generado. En este caso, `Bearer`.      |
| `expires_in`   | integer | Tiempo de vigencia del token, expresado en segundos. |

## Usar el Bearer Token

Después de generar el token, inclúyelo en el header `Authorization` de cada solicitud a la API.

### Formato del header

```http
Authorization: Bearer {{access_token}}
```

### Ejemplo de request autenticada

```bash
curl --request GET 'https://api.sandbox.demopay.com/v1/payments/payment_123456' \
  --header 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.demo-token' \
  --header 'Content-Type: application/json'
```

## Expiración del token

Los Bearer Tokens tienen una vigencia limitada. Cuando el token expire, la API responderá con un error `401 Unauthorized`.

Para continuar enviando solicitudes, genera un nuevo token usando tus credenciales de integración.

### Ejemplo de error por token expirado

```json
{
  "error": {
    "code": "AUTH_TOKEN_EXPIRED",
    "message": "The access token has expired. Generate a new token and try again."
  }
}
```

## Errores comunes de autenticación

| Código HTTP | Código de error       | Descripción                                                             |
| ----------- | --------------------- | ----------------------------------------------------------------------- |
| `400`       | `INVALID_REQUEST`     | La solicitud no incluye todos los campos requeridos.                    |
| `401`       | `INVALID_CREDENTIALS` | El `client_id` o `client_secret` es incorrecto.                         |
| `401`       | `AUTH_TOKEN_EXPIRED`  | El Bearer Token expiró.                                                 |
| `401`       | `AUTH_TOKEN_INVALID`  | El Bearer Token es inválido o fue modificado.                           |
| `403`       | `FORBIDDEN`           | Las credenciales no tienen permisos para acceder al recurso solicitado. |

## Buenas prácticas

Para mantener segura tu integración:

* No compartas tu `client_secret`.
* No expongas credenciales en aplicaciones frontend.
* No subas tokens ni llaves privadas a repositorios públicos.
* Usa variables de entorno para guardar credenciales.
* Genera un nuevo token cuando el actual expire.
* Usa credenciales diferentes para sandbox y producción.
* Revoca y reemplaza tus credenciales si sospechas que fueron comprometidas.

## Ejemplo con variables de entorno

Puedes guardar tus credenciales como variables de entorno para evitar escribirlas directamente en el código.

```bash
DEMO_PAY_CLIENT_ID=demo_client_id
DEMO_PAY_CLIENT_SECRET=demo_client_secret
```

Luego puedes usarlas en tu aplicación para generar el token de forma segura.

## Siguiente paso

Después de configurar la autenticación, puedes enviar tu primera solicitud para crear un pago.

Consulta la guía [Crear un pago](./crear-pago.md) para continuar con la integración.
