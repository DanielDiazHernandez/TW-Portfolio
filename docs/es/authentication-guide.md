# Guía de autenticación API

Esta guía explica cómo autenticar solicitudes a una API usando un token Bearer.

## Requisitos previos

Antes de comenzar, asegúrate de tener:

- Credenciales de sandbox.
- Un token de acceso válido.
- La URL base del ambiente de pruebas.
- Un cliente HTTP como Postman, Insomnia o cURL.

## Encabezado de autorización

Para autenticar una solicitud, incluye el token en el encabezado `Authorization`.

```http
Authorization: Bearer TU_TOKEN