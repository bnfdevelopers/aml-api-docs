
## CREACIÓN DE PRODUCTOS PARA PERSONAS FÍSICAS Y JURÍDICAS

**URL del servicio:**
```bash 
https://{URLCLIENTE}/core_product
```

**Método:** POST

**Tipo de Autenticación:** Basic Authentication (Autenticación básica). Por seguridad, tienen que ser las mismas credenciales que en el servicio anterior.

**Parámetros de Entrada:**

| NIVEL 1         | TIPO    | TAMAÑO | DESCRIPCIÓN                                                    |
|-----------------|---------|--------|----------------------------------------------------------------|
| product_code    | varchar |   4    | Código del producto. Deben ser los mismos que el de la entidad |
| customer_number | integer |   -    | Número de cliente devuelto en servicio anterior                |
| open_date       | date    |   -    | Fecha de apertura en formato YYYY-MM-DD                        |

### Ejemplo

<details><summary>Ejemplo</summary>
<p>

```
{
	"product_code": “AH01”,
	"customer_number": 1234,
	"open_date": “2021-01-02”,
}
```

</details>

### Respuestas por parte de la entidad

La entidad deberá retornar, en formato JSON, el número de cuenta que le ha asignado a la persona física o jurídica con los siguientes http status codes:

* **201** - Respuesta exitosa, cliente creado

* **400** -  Error asignando el producto al cliente. Deberá retornar el mensaje del error para ser mostrado en la aplicación.

### Ejemplos de respuestas

<details><summary>Ejemplo 1 - Caso exitoso (HTTP STATUS CODE 201) </summary>
<p>

```
{
	"account_number": “00100002202”
}
```

</details>

<details><summary>Ejemplo 2 - Caso error (HTTP STATUS CODE 400)</summary>
<p>

```
{
	"message": "MENSAJE DEL MOTIVO DEL ERROR"
}
```

</details>

---

