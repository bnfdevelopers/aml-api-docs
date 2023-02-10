---
cover: >-
  https://images.unsplash.com/photo-1526628953301-3e589a6a8b74?crop=entropy&cs=tinysrgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHw0fHx0cmFuc2FjdGlvbnN8ZW58MHx8fHwxNjc1OTg4MDc4&ixlib=rb-4.0.3&q=80
coverY: 0
---

<style scoped>
table {
  font-size: 10px;
}
</style>

# Monitoreo de Transacciones

Este es un servicio que provee la empresa Better and Faster para la carga asíncrona de transacciones y No clientes que servirá de fuente de datos para el despliegue de la suite del monitoreo de transacciones.

**No clientes:** Se refiere a la información de terceras personas que no pertenecen a la cartera de la entidad pero que participan en las transacciones. También lo denominamos **clientes ocasionales**.

## Especificaciones

**URL del servicio:**

```bash
http://{BNF_API_URL}/process_request_transactions
```

**Método:** POST

**Tipo de Autenticación:** Basic Authentication. Por seguridad se les proveerá un usuario y contraseña para poder realizar la petición al servicio.

**Códigos de Error:**

**5XX** - Errores de servidor, intente nuevamente. En caso de persistir el error, comunicarse directamente con nosotros.

**4XX** - Errores de formato, dependencias, validaciones, entre otros. Por favor leer el mensaje de retorno. La plataforma devolverá una lista con la o las transacciones que arrojaron error. En cada elemento de esa lista estará una llave llamada **errors** (puede tener o un mensaje de error o una lista con los campos de la transacción que dieron error ) y otra llave llamada **transaction\_data** (información de la transacción que dió error). Ver ejemplos más adelante.

**Rate limit:** Ninguno, más allá de los límites impuestos por el motor del servidor donde se despliega el API.

**Observaciones Adicionales:** Existe la posibilidad de realizar una carga síncrona suministrando el flag: execute = True. No obstante, no se recomienda su uso para cargas masivas dado que se trata de un proceso lento y costoso.

**Preliminares:** Acciones que deben ser ejecutadas antes de invocar los servicios provistos por esta API.

* **Códigos de Transacciones:** Se necesita que nos sea provisto un listado de todos los posibles códigos de transacciones manejados por el core de la entidad de manera que puedan ser mapeados con los manejados por el API.

## **Parámetros de Entrada**

Dividiremos la estructura del JSON (llave-valor) en tres niveles: nivel 1, nivel 2 y nivel 3, dependiendo del tipo de información que provea los nodos de nivel 2. Los nodos de nivel 1 serán los siguientes:

* **transactions:** Se trata de una lista de transacciones a registrar que pueden incluir o no a personas que no son clientes de la entidad para el resguardo de datos asociados a operaciones como: depósitos por taquilla, cobro de cheques, etc.
* **execute:** Se trata de un flag que al habilitarse se ejecuta la creación de registros de manera síncrona. Por defecto este valor es false cuando no se incluye en el cuerpo del JSON. Revisar Observaciones Adicionales para más información.

| **NIVEL 1**             | **NIVEL 2**                       | **NIVEL 3**                  | **TIPO** | **TAM** | **DESCRIPCIÓN**                                                                                                                                                                                             |
| ----------------------- | --------------------------------- | ---------------------------- | -------- | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><br>transactions</p> | transaction\_id\*                 |                              | varchar  | 50      | <p>Identificador único de la transacción dado por la entidad.<br>Ejemplo: TR33457741</p>                                                                                                                    |
|                         | date                              |                              | datetime | -       | <p>Fecha en la que se ejecutó la transacción.<br>Debe estar expresada en ISO 8601, formato extendido de fechas y horas conjuntas, como sigue:<br>YYYY-MM-DDTHH:MM:SS.<br>Ejemplo: (2020-11-03T24:12:03)</p> |
|                         | amount\*                          |                              | float    | -       | <p>Monto de la transacción.<br>Ejemplo: 478.56</p>                                                                                                                                                          |
|                         | origin\_bank\_id                  |                              | varchar  | 25      | <p>Identificador único del banco de origen. Este código puede ser el BIC de la entidad en caso de que sea una entidad Swift o el código de la UAF.<br>Ejemplo: BELNDOSD</p>                                 |
|                         | origin\_account                   |                              | varchar  | 25      | <p>Identificador único de la cuenta bancaria de origen. También conocido como número de cuenta.<br>Ejemplo: 000221001794</p>                                                                                |
|                         | origin\_identification            |                              | varchar  | 30      | <p>Identificador único del documento de identidad de la persona de origen en la transacción.<br>Ejemplo: 12345678903</p>                                                                                    |
|                         | origin\_identification\_type      |                              | varchar  | 10      | <p>Tipo del documento de identidad de la persona de origen en la transacción. Ver Anexo 1 Document Types<br>Ejemplo: CEDU</p>                                                                               |
|                         | origin\_country\_code             |                              | varchar  | 10      | <p>Código del país del documento de identidad de la persona de origen en la transacción.<br>Debe estar expresado en Alpha-2.<br>Ejemplo: DO</p>                                                             |
|                         | destination\_bank\_id             |                              | varchar  | 20      | Identificador único del banco de destino. Este código puede ser el BIC de la entidad en caso de que sea una entidad Swift o el código de la UAF. Ejemplo: BELNDOSD                                          |
|                         | destination\_account              |                              | varchar  | 25      | Identificador único de la cuenta bancaria de destino. También conocido como número de cuenta del destinatario. Ejemplo: 221002898                                                                           |
|                         | destination\_identification       |                              | varchar  | 30      | Identificador único del documento de identidad de la persona de destino en la transacción. Ejemplo: 002114011014                                                                                            |
|                         | destination\_identification\_type |                              | varchar  | 10      | Tipo del documento de identidad de la persona de destino en la transacción. Ver Anexo 1, Document TypesEjemplo: CEDU                                                                                        |
|                         | destination\_country\_code        |                              | varchar  | 10      | <p>País del documento de identidad de la persona de destino en la transacción.Debe estar expresado en Alpha-2.<br>Ejemplo: DO</p>                                                                           |
|                         | note\*\*                          |                              | text     | -       | Nota o comentario incluído en la transacción. Ejemplo: Pago de renta                                                                                                                                        |
|                         | currency\*                        |                              | varchar  | 10      | <p>Tipo de moneda de la transacción.Debe estar expresado en ISO 4217.<br>Ejemplo: DOP</p>                                                                                                                   |
|                         | branch\*                          |                              | varchar  | 20      | Identificador, en su CORE bancario, de la sucursal de la entidad en la cual se realizó la transacción o, en su defecto, en la cual el cliente está suscrito. Ejemplo: 1                                     |
|                         | transaction\_code\*               |                              | varchar  | 25      | Tipo de transacción. IMPORTANTE: Estos valores deben estar en la lista provista en el ítem de Códigos de Transacciones en la sección de Preliminales. Ejemplo: DE                                           |
|                         | reversed                          |                              | boolean  | -       | Booleano para determinar si se trata o no de una transacción reversada. Por defecto enviar en False. Ejemplo: False                                                                                         |
|                         | user\*                            |                              | varchar  | 10      | Identificador único del operador que registró la transacción en el CORE. Ejemplo: COREUSER1                                                                                                                 |
|                         | origin\_non\_customer             | first\_name\*                | varchar  | 50      | Nombre del no cliente de origen.Ejemplo: Juan                                                                                                                                                               |
|                         |                                   | last\_name\*                 | varchar  | 50      | Apellido del no cliente de origen.Ejemplo: Pérez                                                                                                                                                            |
|                         |                                   | email\*\*                    | varchar  | 50      | Dirección de correo electrónico del no cliente de origen.Ejemplo: juanperez@ejemplo.com                                                                                                                     |
|                         |                                   | gender\*\*                   | varchar  | 50      | Género del no cliente de origen: F o M.Ejemplo: M                                                                                                                                                           |
|                         |                                   | phone\_number\*\*            | varchar  | 50      | Número de teléfono del no cliente de origen.Ejemplo: +18297320014                                                                                                                                           |
|                         |                                   | role\*                       | varchar  | 50      | Rol del no cliente de destino. Toma los valores de deposit para depósitos, withdrawal para retiros y beneficiary para transferencia a cuenta externa.                                                       |
|                         |                                   | citizenship\*\*              | varchar  | 50      | <p>Código del pais de ciudadanía del no cliente de origen.Debe estar expresado en Alpha-2.<br>Ejemplo: DO</p>                                                                                               |
|                         |                                   | birth\_date\*\*              | date     | -       | <p>Fecha de nacimiento del no cliente de origen.<br>Debe estar expresada en ISO 8601, formato extendido de fechas, como sigue:<br>YYYY-MM-DD<br>Ejemplo: (2020-11-03)</p>                                   |
|                         |                                   | address\*\*                  | varchar  | 50      | Dirección del no cliente de origen.Ejemplo: Avenida Romulo Betancourt 2154                                                                                                                                  |
|                         |                                   | residence\_city\*\*          | varchar  | 50      | Ciudad de residencia del no cliente de origen.Ejemplo: SANTO DOMINGO DE GUZMAN                                                                                                                              |
|                         |                                   | residence\_country\_code\*\* | varchar  | 50      | <p>Código del país de residencia del no cliente de origen.Debe estar expresado en Alpha-2.<br>Ejemplo: DO</p>                                                                                               |
|                         | destination\_non\_customer        | first\_name\*                | varchar  | 50      | Nombre del no cliente de destino.Ejemplo: Juan                                                                                                                                                              |
|                         |                                   | last\_name\*                 | varchar  | 50      | Apellido del no cliente de destino.Ejemplo: Pérez                                                                                                                                                           |
|                         |                                   | email\*\*                    | varchar  | 50      | Dirección de correo electrónico del no cliente de destino.Ejemplo: juanperez@ejemplo.com                                                                                                                    |
|                         |                                   | gender\*\*                   | varchar  | 50      | Género del no cliente de destino. Debe suministrar la etiqueta que utiliza para diferenciar másculino y femenino en su CORE bancario.Ejemplo: M                                                             |
|                         |                                   | phone\_number\*\*            | varchar  | 50      | Número de teléfono del no cliente de destino.Ejemplo: +18297320014                                                                                                                                          |
|                         |                                   | role\*                       | varchar  | 50      | Rol del no cliente de destino. Toma los valores de deposit para depósitos, withdrawal para retiros y beneficiary para transferencia a cuenta externa.                                                       |
|                         |                                   | citizenship\*\*              | varchar  | 50      | <p>Código del pais de ciudadanía del no cliente de destino.Debe estar expresado en Alpha-2.<br><br>Ejemplo: DO</p>                                                                                          |
|                         |                                   | birth\_date\*\*              | date     | -       | <p>Fecha de nacimiento del no cliente de destino.<br>Debe estar expresada en ISO 8601, formato extendido de fechas, como sigue:<br>YYYY-MM-DD<br>Ejemplo: (2020-11-03)</p>                                  |
|                         |                                   | address\*\*                  | varchar  | 50      | Dirección del no cliente de destino.Ejemplo: Avenida Romulo Betancourt 2154                                                                                                                                 |
|                         |                                   | residence\_city\*\*          | varchar  | 50      | <p>Ciudad de residencia del no cliente de destino.Ejemplo: SANTO DOMINGO DE GUZMAN<br></p>                                                                                                                  |
|                         |                                   | residence\_country\_code\*\* | varchar  | 50      | <p>Código del país de residencia del no cliente de destino.Debe estar expresado en Alpha-2.<br>Ejemplo: DO, IT, CO, etc</p>                                                                                 |
|                         | origin\_account\_info             | customer\_number\*           | varchar  | 25      | Número de cliente del dueño de la cuenta origen                                                                                                                                                             |
|                         |                                   | currency\_code\*             | varchar  | 25      | Código de la moneda base de la cuenta origen. Ver tabla de currency en los anexos.                                                                                                                          |
|                         |                                   | product\_code\*              | varchar  | 20      | Código del producto asociado a la cuenta origen. Este código debe ser los indicados por la entidad en el proceso de implementación.                                                                         |
|                         |                                   | subproduct\_code\*           | varchar  | 20      | Código del subproducto asociado a la cuenta origen. Este código debe ser los indicados por la entidad en el proceso de implementación.                                                                      |
|                         |                                   | branch\_code\*               | varchar  | 6       | Código de la sucursal a la que está asociada la cuenta origen.                                                                                                                                              |
|                         |                                   | opening\_date                | date     | -       | Fecha de apertura de la cuenta origen.                                                                                                                                                                      |
|                         |                                   | gross\_balance               | float    | -       | Saldo bruto de la cuenta origen.                                                                                                                                                                            |
|                         |                                   | net\_balance                 | float    | -       | Saldo neto de la cuenta origen.                                                                                                                                                                             |
|                         | destination\_account\_info        | customer\_number\*           | varchar  |         | Número de cliente del dueño de la cuenta destino.                                                                                                                                                           |
|                         |                                   | currency\_code\*             | varchar  |         | Código de la moneda base de la cuenta destino. Ver tabla de currency en los anexos.                                                                                                                         |
|                         |                                   | product\_code\*              | varchar  |         | Código del producto asociado a la cuenta destino. Este código debe ser los indicados por la entidad en el proceso de implementación.                                                                        |
|                         |                                   | branch\_code\*               | varchar  |         | Código de la sucursal a la que está asociada la cuenta destino.                                                                                                                                             |
|                         |                                   | subproduct\_code\*           | varchar  | 20      | Código del subproducto asociado a la cuenta destino. Este código debe ser los indicados por la entidad en el proceso de implementación.                                                                     |
|                         |                                   | opening\_date                | date     | -       | Fecha de apertura de la cuenta destino.                                                                                                                                                                     |
|                         |                                   | gross\_balance               | float    | -       | Saldo bruto de la cuenta destino.                                                                                                                                                                           |
|                         |                                   | net\_balance                 | float    | -       | Saldo neto de la cuenta destino.                                                                                                                                                                            |
| execute                 |                                   |                              | boolean  | -       | Booleano para determinar si la ejecución será síncrona. True/False. Default: false. (Revisar observaciones adicionales).                                                                                    |

<mark style="color:red;">\*Obligatorio / \*\*Recomendado</mark>

En la Tabla anterior se aprecia las etiquetas **origin\_non\_customer** y **destination\_non\_customer**. Estos campos sirven para indicar la información de aquellas personas involucradas en un tipo de transacciones (por ejemplo depósitos, retiros y destinatario de una transferencia a una cuenta externa) pero que no son clientes de la entidad.

## Aspectos importantes a tomar en cuenta

1. Los campos:

* origin\_identification
* origin\_identification\_type
* origin\_country\_code

corresponden a los datos de identidad del originador. La información del destinatario viene dado por los siguientes campos:

* destination\_identification
* destination\_identification\_type
* destination\_country\_code

Estos campos a pesar de no ser requeridos dependiendo del tipo de operación, se deben enviar cuando no se tenga una cuenta de origen o destino respectivamente. Cada transacción deberá tener, al menos, o bien los datos de identidad del cliente, o bien los datos de la cuenta de origen (origin\_account, origin\_bank\_id).

2. Los campos origin\_bank\_id y destination\_bank\_id corresponden a los identificadores únicos de los bancos dueños de las cuentas de origen y destino, estos códigos pueden ser, o bien el código SWIFT, o bien el código otorgado por la UAF.
3. El campo origin\_account\_info corresponde a la información de la cuenta origen indicado en el campo origin\_account. Entre esa información se encuentra el dueño de la misma. Esto para almacenar los datos en nuestra plataforma en caso de que esa cuenta haya sido asignada al cliente directamente en el core de la entidad. Es importante indicar que el cliente indicado en el campo customer\_number deberá existir previamente en nuestra base de datos.
4. El campo destination\_account\_info corresponde a la información de la cuenta destino indicado en el campo destination\_account. Entre esa información se encuentra el dueño de la misma. Esto para almacenar los datos en nuestra plataforma en caso de que esa cuenta haya sido asignada al cliente directamente en el core de la entidad. Es importante indicar que el cliente indicado en el campo customer\_number deberá existir previamente en nuestra base de datos.
5. El sistema recibe un listado de transacciones y aquellas que arrojen algún error serán devueltas en el cuerpo de la respuesta del servicio con el código http status code 400 con el o los mensajes de error. Ver ejemplos de respuestas de error más abajo.

A continuación explicaremos qué información se debe enviar para las transacciones de depósitos, retiros y transferencia cuenta a cuenta.

### Caso depósito

Cuando se hace un depósito, el depositante (que puede ser o no cliente de la entidad) no posee una cuenta de origen, ya que entrega los fondos en efectivo, en ese caso, se envía únicamente los campos de identificación del cliente de origen, mientras que para el destinatario, se envía el número de cuenta e identificación del banco (que sería el banco que recibe los fondos): destination\_account y destination\_bank\_id (Ver Tabla 2.0).

### Caso retiro

Cuando se hace un retiro, la persona que retira (que puede ser o no cliente de la entidad) no posee una cuenta de destino, ya que recibe los fondos en efectivo, en ese caso, se envían únicamente los campos de identificación del cliente de destino, mientras que para el origen, se envía los campos de número de cuenta e identificación del banco (que sería el banco que otorga los fondos): origin\_account y origin\_bank\_id. Es importante notar que el origen es la cuenta de donde salen los fondos y el destino es el cliente que retira, ya que la dirección de las transacciones queda definida siempre desde el origen hacia el destino.

### Caso transferencia cuenta a cuenta

Cuando se hace una transferencia entre cuentas dentro de la misma entidad (interna), no es necesario enviar los datos de identificación de las personas de origen y destino, basta con enviar los datos de sus cuentas bancarias junto al identificador de los bancos respectivos: origin\_bank\_id y origin\_account para el originador, destination\_bank\_id y destination\_account para el destinatario. Para el caso de transferencias entre una cuenta interna a una externa es necesario proveer los datos de destination\_non\_customer (cuando es una transferencia saliente). Y para el caso de transferencia entre una cuenta externa a una interna es necesario proveer los datos de origin\_non\_customer (cuando es una transferencia entrante).

## Ejemplos del cuerpo de la petición:

<details>

<summary>Ejemplo 1</summary>

Caso depositante no es cliente de la entidad, por lo tanto se envía información en origin\_non\_customer:

```
{
    "transactions": [
        {
            "transaction_id": "TR33457741",
            "date": "2020-10-16T21:27:59.469957Z",
            "amount": "478.56",
            "origin_identification": "12345678903",
            "origin_identification_type": "IDEN",
            "origin_country_code": "DO",
            "destination_bank_id": "BELNDOSD",
            "destination_account": "221002898",
            "note": "Deposito pago de servicio",
            "currency": "DOP",
            "branch": "0",
            "transaction_code": "DE",
            "user": "AGD0088",
            "origin_non_customer": {
                "first_name": "adiel",
                "last_name": "dominguez",
                "email": "adieldominguez@dev.aclaoverseas.com",
                "gender": "M",
                "phone_number": "+18297320014",
                "role": "deposit",
                "citizenship": "DO",
                "birth_date": "2020-05-14",
                "address": "Avenida Romulo Betancourt 2154",
                "residence_city": "SANTO DOMINGO DE GUZMAN",
                "residence_country_code": "DO"
            }
        }
    ]
}
```

</details>

<details>

<summary>Ejemplo 2</summary>

Caso depósito donde el depositante es cliente de la entidad. No se envía datos de origin\_non\_customer:

```
{
    "transactions": [
        {
            "transaction_id": "TR33457742",
            "date": "2020-10-16T21:27:59.469957Z",
            "amount": "478.56",
            "origin_identification": "12345678903",
            "origin_identification_type": "IDEN",
            "origin_country_code": "DO",
            "destination_bank_id": "BELNDOSD",
            "destination_account": "221002898",
            "currency": "RD$",
            "branch": "1",
            "transaction_code": "DE",
            "reversed": false,
            "user": "AGD0088"
        }
    ]
}
```

</details>

<details>

<summary>Ejemplo 3</summary>

Caso retiro donde el que recibe el dinero no es cliente de la entidad por lo que se envía la información en destination\_non\_customer:

```
{
    "transactions": [
        {
            "transaction_id": "TR33457742",
            "date": "2020-10-16T21:27:59.469957Z",
            "amount": "478.56",
            "origin_bank_id": "BELNDOSD",
            "origin_account": "000214111112",
            "destination_identification": "12345678903",
            "destination_identification_type": "IDEN",
            "destination_country_code": "DO",
            "note": "Nota del retiro",
            "currency": "USD",
            "branch": "0",
            "transaction_code": "RT",
            "reversed": "False",
            "user": "AGD0088",
            "destination_non_customer": {
                "first_name": "adiel",
                "last_name": "dominguez",
                "email": "adieldominguez@dev.bellbank.com",
                "gender": "M",
                "phone_number": "+18297320014",
                "role": "withdrawal",
                "citizenship": "DO",
                "birth_date": "2020-05-14",
                "address": "Avenida Romulo Betancourt 2154",
                "residence_city": "SANTO DOMINGO DE GUZMAN",
                "residence_country_code": "DO"
            }
        }, ...
    ]
}
```

</details>

<details>

<summary>Ejemplo 4</summary>

Caso retiro donde el que recibe el dinero es cliente de la entidad por lo que no se envía la información en destination\_non\_customer:

```
{
    "transactions": [
        {
            "transaction_id": "TR33457742",
            "date": "2020-10-16T21:27:59.469957Z",
            "amount": "478.56",
            "origin_bank_id": "BELNDOSD",
            "origin_account": "000221001794",
            "destination_identification": "12345678903",
            "destination_identification_type": "IDEN",
            "destination_country_code": "DO",
            "note": "Nota del retiro",
            "currency": "RD$",
            "branch": "0",
            "transaction_code": "RT",
            "reversed": false,
            "user": "AGD0088",
        },
…
    ]
}
```

</details>

<details>

<summary>Ejemplo 5</summary>

Caso transferencia cuenta a cuenta donde el origen y destino son cuentas dentro de la entidad (transferencias internas):

```
{
    "transactions": [
        {
            "transaction_id": "TR33457742",
            "date": "2020-10-16T21:27:59.469957Z",
            "amount": "478.56",
            "origin_bank_id": "BELNDOSD",
            "origin_account": "000221001794",
            "destination_bank_id": "BELNDOSD",
            "destination_account": "221002898",
            "note": "Transferencia interna",
            "currency": "RD$",
            "branch": "0",
            "transaction_code": "TRANSINT",
            "reversed": false,
            "user": "AGD0088",
        }, ...
    ]
}
```

</details>

<details>

<summary>Ejemplo 6</summary>

Caso transferencia donde el origen es una cuenta de la entidad y el destinatario es cuenta en otra entidad (beneficiario). Transferencias externas:

```
{
    "transactions": [
        {
            "transaction_id": "TR33457742",
            "date": "2020-10-16T21:27:59.469957Z",
            "amount": "478.56",
            "origin_bank_id": "BELNDOSD",
            "origin_account": "000221001794",
            "destination_bank_id": "BELNDOSD",
            "destination_account": "221001111",
            "destination_identification": "12345678903",
            "destination_identification_type": "IDEN",
            "destination_country_code": "DO",
            "note": "Transferencia a otra entidad",
            "currency": "RD$",
            "branch": "0",
            "transaction_code": "TRANSEXT",
            "reversed": false,
            "user": "AGD0088",
            "destination_non_customer": {
                "first_name": "adiel",
                "last_name": "dominguez",
                "email": "adieldominguez@dev.bellbank.com",
                "gender": "M",
                "phone_number": "+18297320014",
                "role": "beneficiary",
                "citizenship": "DO",
                "birth_date": "2020-05-14",
                "address": "Avenida Romulo Betancourt 2154",
                "residence_city": "SANTO DOMINGO DE GUZMAN",
                "residence_country_code": "DO"
            }
        }
    ]
}
```

</details>

<details>

<summary>Ejemplo 7</summary>

Caso transferencia donde el origen es una cuenta de otra entidad y el destinatario es una cuenta de un cliente de la entidad. Transferencias externas:

```
{
    "transactions": [
        {
            "transaction_id": "TR33457742",
            "date": "2020-10-16T21:27:59.469957Z",
            "amount": "478.56",
            "origin_bank_id": "BELNDOSD",
            "origin_account": "000221001794",
            "origin_identification": "12345678903",
            "origin_identification_type": "IDEN",
            "origin_country_code": "DO",
            "destination_bank_id": "BELNDOSD",
            "destination_account": "221002898",
            "note": "Transferencia externa entrante",
            "currency": "RD$",
            "branch": "0",
            "transaction_code": "TRANEXT",
            "reversed": false,
            "user": "AGD0088",
            "origin_non_customer": {
                "first_name": "adiel",
                "last_name": "dominguez",
                "email": "adieldominguez@dev.bellbank.com",
                "gender": "M",
                "phone_number": "+18297320014",
                "role": "beneficiary",
                "citizenship": "DO",
                "birth_date": "2020-05-14",
                "address": "Avenida Romulo Betancourt 2154",
                "residence_city": "SANTO DOMINGO DE GUZMAN",
                "residence_country_code": "DO"
            }
        }, ...
    ]
}
```

</details>

<details>

<summary>Ejemplo 8</summary>

Caso depósito donde el depositante es cliente de la entidad y la cuenta destino no existe en nuestra plataforma. No se envía datos de origin\_non\_customer:

```
{
    "transactions":
    [
        {
            "transaction_id": "TR33457746",
            "date": "2022-08-04T21:27:59.469957Z",
            "amount": "500000.0",
            "origin_identification": "12345678903",
            "origin_identification_type": "IDEN",
            "origin_country_code": "DO",
            "destination_bank_id": "BELNDOSD",
            "destination_account": "221002898",
            "currency": "RD$",
            "branch": "1",
            "transaction_code": "TRANEXT",
            "reversed": false,
            "note": "DEPOSITO",
            "user": "AGD0088",
            "destination_account_info": {
                "customer_number": "8408",
                "currency_code": "RD$",
                "product_code": "1",
                "subproduct_code": "2",
                "branch_code": "01",
                "opening_date": "2022-12-20"
            }
        }
    ]
}
```

</details>

<details>

<summary>Ejemplo 9</summary>

Caso retiro donde el que recibe el dinero es cliente de la entidad por lo que no se envía la información en destination\_non\_customer. La información de la cuenta origen es enviada:

```
{
    "transactions": [
        {
            "transaction_id": "TR33457742",
            "date": "2020-10-16T21:27:59.469957Z",
            "amount": "478.56",
            "origin_bank_id": "BELNDOSD",
            "origin_account": "000221001794",
            "destination_identification": "12345678903",
            "destination_identification_type": "IDEN",
            "destination_country_code": "DO",
            "note": "Nota del retiro",
            "currency": "RD$",
            "branch": "0",
            "transaction_code": "RT",
            "reversed": false,
            "user": "AGD0088",
            "origin_account_info": {
                "customer_number": "8999",
                "currency_code": "RD$",
                "product_code": "1",
                "subproduct_code": "2",
                "branch_code": "01",
                "opening_date": "2021-12-20"
            }
        },
…
    ]
}
```

</details>

<details>

<summary>Ejemplo 10</summary>

Caso transferencia cuenta a cuenta donde el origen y destino son cuentas dentro de la entidad (transferencias internas). En caso de no existir ambas cuentas se envían las dos. Si solo existe una, se puede enviar una de las dos o incluso ninguna:

```
{
    "transactions": [
        {
            "transaction_id": "TR33457742",
            "date": "2020-10-16T21:27:59.469957Z",
            "amount": "478.56",
            "origin_bank_id": "BELNDOSD",
            "origin_account": "000221001794",
            "destination_bank_id": "BELNDOSD",
            "destination_account": "221002898",
            "note": "Transferencia interna",
            "currency": "RD$",
            "branch": "0",
            "transaction_code": "TRANSINT",
            "reversed": false,
            "user": "AGD0088",
            "origin_account_info": {
                "customer_number": "8999",
                "currency_code": "RD$",
                "product_code": "2",
                "subproduct_code": "2",
                "branch_code": "01",
                "opening_date": "2021-12-20"
            },
            "destination_account_info": {
                "customer_number": "8408",
                "currency_code": "RD$",
                "product_code": "1",
                "subproduct_code": "3",
                "branch_code": "01",
                "opening_date": "2021-12-20"
            }
        }, ...
    ]
}
```

</details>

## Ejemplos de respuestas

<details>

<summary>Ejemplo 1 - Caso exitoso (HTTP STATUS CODE 204)</summary>

```
No retorna ningún body de respuesta
```

</details>

<details>

<summary>Ejemplo 2 - Caso error (HTTP STATUS CODE 400). Campo monto no enviado</summary>

```
[
    {
        "errors": {
            "amount": [
                "This field is required."
            ]
        },
        "transaction_data": {
            "transaction_id": "TR33457746",
            "date": "2022-08-04T21:27:59.469957Z",
            "origin_identification": "123456678909",
            "origin_identification_type": "IDEN",
            "origin_country_code": "DO",
            "destination_bank_id": "BELNOSD",
            "destination_account": "00100000101",
            "currency": "RD$",
            "branch": "1",
            "transaction_code": "DE",
            "reversed": "False",
            "note": "DEPOSITO",
            "user": "AGDUSER"
        }
    }
]
```

</details>

<details>

<summary>Ejemplo 3 - Caso error (HTTP STATUS CODE 400). Transaction id repetido</summary>

```
[
    {
        "errors": {
            "transaction_id": [
                "transaction with this transaction id already exists."
            ]
        },
        "transaction_data": {
            "transaction_id": "TR33457746",
            "date": "2022-08-04T21:27:59.469957Z",
            "origin_identification": "123456678909",
            "origin_identification_type": "IDEN",
            "origin_country_code": "DO",
            "destination_bank_id": "BELNDOSD",
            "destination_account": "00100000101",
            "currency": "RD$",
            "branch": "1",
            "transaction_code": "DE",
            "reversed": "False",
            "note": "DEPOSITO",
            "user": "AGDUSER"
        }
    }
]
```

</details>

<details>

<summary>Ejemplo 4</summary>

Caso error (HTTP STATUS CODE 400). Caso cuando el customer number la información de la cuenta suministrada no existe. Este ejemplo muestra error de dos transacciones enviadas simultáneamente en el servicio:

```
[
    {
        "errors": "Customer does not exist with the customer number provided.",
        "transaction_data": {
            "transaction_id": "TR33457746",
            "date": "2022-08-04T21:27:59.469957Z",
            "amount": "500000.0",
            "origin_identification": "1234567990",
            "origin_identification_type": "IDEN",
            "origin_country_code": "DO",
            "destination_bank_id": "BELNDOSD",
            "destination_account": "001284848",
            "currency": "RD$",
            "branch": "1",
            "transaction_code": "DE",
            "reversed": "False",
            "note": "DEPOSITO",
            "user": "JANDUJAR",
            "destination_account_info": {
                "customer_number": "8408",
                "currency_code": "RD$",
                "product_code": "1",
                "branch_code": "01",
                "opening_date": "2022-12-20"
            }
        }
    },
    {
        "errors": "Customer does not exist with the customer number provided.",
        "transaction_data": {
            "transaction_id": "TR33457747",
            "date": "2022-08-04T21:27:59.469957Z",
            "amount": "500000.0",
            "origin_identification": "12345678909",
            "origin_identification_type": "PAOR",
            "origin_country_code": "DO",
            "destination_bank_id": "BELNDOSD",
            "destination_account": "0012122444",
            "currency": "RD$",
            "branch": "1",
            "transaction_code": "DE",
            "reversed": "False",
            "note": "DEPOSITO",
            "user": "JANDUJAR",
            "origin_account_info": {
                "customer_number": "83361",
                "currency_code": "RD$",
                "product_code": "1",
                "branch_code": "01",
                "opening_date": "2021-12-20"
            },
            "destination_account_info": {
                "customer_number": "8410",
                "currency_code": "RD$",
                "product_code": "1",
                "branch_code": "01",
                "opening_date": "2021-12-20"
            }
        }
    }
]
```

</details>

***
