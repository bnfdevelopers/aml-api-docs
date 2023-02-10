---
description: Servicio aplica tanto para persona física como persona jurídica.
cover: >-
  https://images.unsplash.com/photo-1423666639041-f56000c27a9a?crop=entropy&cs=tinysrgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHwxfHxvbmxpbmUlMjBiYW5rfGVufDB8fHx8MTY3NTk4MzYyNg&ixlib=rb-4.0.3&q=80
coverY: 187
---

# Creación de Productos

## Especificaciones

**URL del servicio:**

```bash
https://{URLCLIENTE}/core_product
```

**Método:** POST

**Tipo de Autenticación:** Basic Authentication (Autenticación básica). Por seguridad, tienen que ser las mismas credenciales que en el servicio anterior.

## **Parámetros de Entrada**

| NIVEL 1          | TIPO    | TAMAÑO | DESCRIPCIÓN                                                    |
| ---------------- | ------- | ------ | -------------------------------------------------------------- |
| product\_code    | varchar | 4      | Código del producto. Deben ser los mismos que el de la entidad |
| customer\_number | integer | -      | Número de cliente devuelto en servicio anterior                |
| open\_date       | date    | -      | Fecha de apertura en formato YYYY-MM-DD                        |

## Ejemplo

<details>

<summary>Ejemplo</summary>

```
{
	"product_code": “AH01”,
	"customer_number": 1234,
	"open_date": “2021-01-02”,
}
```

</details>

## Respuestas por parte de la entidad

La entidad deberá retornar, en formato JSON, el número de cuenta que le ha asignado a la persona física o jurídica con los siguientes http status codes:

* **201** - Respuesta exitosa, cliente creado
* **400** - Error asignando el producto al cliente. Deberá retornar el mensaje del error para ser mostrado en la aplicación.

## Ejemplos de respuestas

<details>

<summary>Ejemplo 1 - Caso exitoso (HTTP STATUS CODE 201)</summary>

```
{
	"account_number": “00100002202”
}
```

</details>

<details>

<summary>Ejemplo 2 - Caso error (HTTP STATUS CODE 400)</summary>

```
{
	"message": "MENSAJE DEL MOTIVO DEL ERROR"
}
```

</details>

***
