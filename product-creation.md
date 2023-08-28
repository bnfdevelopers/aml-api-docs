---
description: Servicio  tanto para persona física como persona jurídica.
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

| NIVEL 1               | NIVEL 2                  | NIVEL 3                  | TIPO    | TAMAÑO | DESCRIPCIÓN                                                                                                                                |
| --------------------- | ------------------------ | ------------------------ | ------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------ |
| product\_code*        |                          |                          | varchar | 4      | Código del producto. Deben ser los mismos que el de la entidad                                                                             |
| customer\_number*     |                          |                          | integer | -      | Número de cliente devuelto en servicio anterior                                                                                            |
| open\_date*           |                          |                          | date    | -      | Fecha de apertura en formato YYYY-MM-DD                                                                                                    |
| related\_client\_info |                          |                          | array   | -      | Listado de relacionados a la cuenta que se está creando.Pueden ser beneficiarios o cotitulares                                             |
|                       | customer\_number         |                          | integer | -      | Número de cliente del relacionado o vinculado a la cuenta que posee en el core de la entidad                                               |
|                       | related\_wisenroll\_code |                          | integer | -      | Código Wisenroll del relacionado o vinculado a la cuenta que posee en el core de la entidad                                                |
|                       | account\_link\_type      |                          | varchar | 2      | Tipo de vinculación de esa persona a la cuenta. Revisar los valores en la sección de [anexos](static-data.md), tabla **Account Link Type** |
|                       | authorized\_signature    |                          | integer | -      | Flag que indica si el vinculado es firmante o no de la cuenta: 1 en caso de ser True y 0 en caso de False                                  |
|                       | main\_document           |                          | object  | -      | Objeto donde se indica información del documento del vinculado a la cuenta. Se envío como información adicional en caso de ser requerida   |
|                       |                          | document\_type\_code     | varchar | 20     | Tipo de documento del vinculado                                                                                                            |
|                       |                          | document\_number         | varchar | 20     | Número de documento del vinculado                                                                                                          |
|                       |                          | issuing\_number\_code    | varchar | 2      | País emisor del documento en Alpha-2                                                                                                       |

**Nota:** Los marcados con asterisco (\*) son los recomendados a que sean almacenados en el core. Los que no tengan esta marca pueden llegar con el valor de null, vacío (“”) o incluso no aparecer en el json. Se recomienda validar siempre si algún valor que no es marcado con asterisco está en el json o no para trabajarlo. 

## Ejemplo

<details>

<summary>Ejemplo 1 </summary>

```
{
	"product_code": “AH01”,
	"customer_number": 1234,
	"open_date": “2021-01-02”,
}
```

</details>

<details>

<summary>Ejemplo 2 </summary>

```
{
	"product_code": “AH01”,
	"customer_number": 1234,
	"open_date": “2021-01-02”,
	"related_client_info":[
		{
			"authorized_signature":1,
			"customer_number":46,
			"related_wisenroll_code":"K9ZW56",
			"main_document":{
				"document_type_code":"SSN",
				"document_number":"111010001",
				"issuing_country_code":"US"
			},
			"account_link_type_code":"O"
		},
		...
   ],
}
```

</details>

## Respuestas por parte de la entidad

La entidad deberá responder un JSON con los siguientes http status codes:

* **201** - Respuesta exitosa, cuenta(s)/producto(s) creadas
* **400** - Error asignando el cuenta(s)/producto(s) al cliente. Deberá retornar el mensaje del error para ser mostrado en la aplicación.

El JSON deberá seguir el siguiente formato:

| NIVEL 1               | NIVEL 2                  | TIPO    | TAMAÑO | DESCRIPCIÓN                                                                                                            | 
| --------------------- | ------------------------ | ------- | ------ | ---------------------------------------------------------------------------------------------------------------------- |
| account\_info*        |                          | array   |   -    | Lista de objectos que contienen la información de las cuentas/productos generaros al cliente o los que dieron error    |
|                       | action*                  | varchar |   -    | Asignar el valor **CREATE** para creación exitosa. En caso de error asignar el valor **ERROR**                         |
|                       | account_product*         | varchar |   10   | Código de subproducto de la cuenta/producto generado. En caso de de error, indicar de igual manera dicho código        |
|                       | account_number           | varchar |   20   | Número de cuenta                                                                                                       |
|                       | message*                 | text    |   -    | En caso en que el valor del campo **action** sea **ERROR**, indicar el mensaje de error en este campo                  |

**Notas:** Los campos _**account_product**_ y _**account_number**_ deben ser enviados cuando el campo de action sea **CREATE**. En caso de **ERROR** se requieren los campos _**account_product**_ y _**message**_.

## Ejemplos de respuestas

<details>

<summary>Ejemplo - Casos exitosos (HTTP STATUS CODE 201)</summary>

```
{
   "accounts_info":[
      {
         "action":"CREATE",
         "account_product":"SAV1",
         "account_number":"000201000903"
      }
   ]
}
```

```
{
    "accounts_info":[
       {
          "action":"CREATE",
          "account_product":"SAV1",
          "account_number":"000201000903"
       },
	   {
          "action":"CREATE",
          "account_product":"SAV2",
          "account_number":"000221000148"
       }
    ]
}
```

</details>

<details>

<summary>Ejemplo - Caso error (HTTP STATUS CODE 400)</summary>

```
{
    "accounts_info":[
       {
          "action":"ERROR",
          "account_product":"NOW1",
          "message":"MENSAJE DE ERROR"
       },
	   {
          "action":"ERROR",
          "account_product":"NOW3",
          "message":"MENSAJE DE ERROR"
       }
    ]
}
```

</details>

***
