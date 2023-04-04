---
cover: >-
  https://images.unsplash.com/photo-1491975474562-1f4e30bc9468?crop=entropy&cs=tinysrgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHwxMHx8cGVyc29uJTIwdGVjaG5vbG9neXxlbnwwfHx8fDE2NzU5ODM0NTA&ixlib=rb-4.0.3&q=80
coverY: -196
---

# Creación / Actualización de Clientes Persona Física

Para poder crear clientes en el core de la entidad financiera, es necesario que la entidad cliente desarrolle un servicio que permita enviar toda la información tomada desde la aplicación de Clients Inspector hacia el core. El servicio que provea la entidad, deberá retornar el número de cliente que asigna el core. En caso de que el cliente ya exista en el core, deberá ser capaz de actualizar dicha información con la información enviada desde Clients Inspector.

## Especificaciones

**URL del servicio:**

```bash
https://{URLCLIENTE}/core_customer
```

**Método:** POST

**Tipo de Autenticación:** Basic Authentication (Autenticación Básica). Por seguridad, el servicio que tendrá que crear el equipo de TI de la entidad deberá ser capaz de manejar este tipo de autenticación para que nos provea unas credenciales de acceso al servicio.

## **Parámetros de Entrada**

Dividiremos la estructura del JSON (llave-valor) en tres niveles: nivel 1, nivel 2 y nivel 3, dependiendo del tipo de información que provea los nodos de nivel 2. Los nodos de nivel 1 serán los siguientes:

* **client\_info:** Indicará la información personal del cliente a ser creado: nombres, apellidos, número de documentos, etc.
* **client\documents:** Datos de los documentos de identificación del cliente. Es retornado en una lista de diccionarios.
* **client\_location:** Datos de vivienda del cliente.
* **client\_contact:** Datos de contacto del cliente.
* **client\_job\_info:** Datos laborales del cliente.
* **client\_references:** Referencias bancarias y personales. Las referencias bancarias son opcionales.
* **client\_transactionality:** Datos de transaccionalidad del cliente.
* **client\_entity\_branch:** Información de la sucursal de la entidad donde el cliente se está registrando.

> :warning: **IMPORTANTE:** Recomendamos que aquellos campos que no son marcados con un asterisco (\*), se valide primero si ese campo y/o objeto es enviado en el json o no. Esto porque puede haber casos en que dicho campo y/o objeto viaje en el json si hay algún valor, de lo contrario no aparecerá.

A continuación la descripción de cada campo:

| NIVEL 1                  | NIVEL 2                           | NIVEL 3                       | TIPO    | TAMAÑO | DESCRIPCIÓN                                                                                                                                                                                                                                                                      |
| ------------------------ | --------------------------------- | ----------------------------- | ------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| client\_info             | first\_name\*                     |                               | varchar | 30     | Primer nombre del cliente                                                                                                                                                                                                                                                        |
|                          | middle\_name                      |                               | varchar | 30     | Segundo nombre del cliente                                                                                                                                                                                                                                                       |
|                          | last\_name\*                      |                               | varchar | 30     | Primer apellido del cliente                                                                                                                                                                                                                                                      |
|                          | middle\_last\_name                |                               | varchar | 30     | Segundo Nombre                                                                                                                                                                                                                                                                   |
|                          | sex\*                             |                               | varchar | 1      | Género: M (Male) / F (Female) / NB (No Binary) / NC (No Comments) / O (Other)                                                                                                                                                                                                    |
|                          | other\_sex\_description           |                               | varchar | 50     | Cuando sex = O (Other) se envía el género indicado por el cliente                                                                                                                                                                                                                |
|                          | birth\_date\*                     |                               | date    | -      | Fecha de Nacimiento. Formato YYYY-MM-DD                                                                                                                                                                                                                                          |
|                          | civil\_status\_id\*               |                               | integer | -      | Código de estado civil. Ver Anexos [aquí](static-data.md)                                                                                                                                                                                                                        |
|                          | civil\_status\_description        |                               | varchar | 50     | Descripción del estado civil                                                                                                                                                                                                                                                     |
|                          | nationality\_code                 |                               | varchar | 2      | Código de país de la nacionalidad en Alpha-2. Ejemplo: DO, IT, CO, etc. Ver Anexo 1                                                                                                                                                                                              |
|                          | other\_nationality\_code          |                               | varchar | 2      | Código de país de segunda nacionalidad en Alpha-2                                                                                                                                                                                                                                |
|                          | person\_type\_code                |                               | varchar | 10     | Indica si la persona es PEP o RPEP. En caso de que no, el valor es vacío                                                                                                                                                                                                         |
|                          | spouse\_info                      | first\_name                   | varchar | 30     | Primer nombre del cónyuge                                                                                                                                                                                                                                                        |
|                          |                                   | middle\_name                  | varchar | 30     | Segundo nombre del cónyuge                                                                                                                                                                                                                                                       |
|                          |                                   | last\_name                    | varchar | 30     | Apellido del cónyuge                                                                                                                                                                                                                                                             |
|                          |                                   | middle\_last\_name            | varchar | 30     | Segundo apellido del cónyuge                                                                                                                                                                                                                                                     |
|                          |                                   | document\_type\_code          | varchar | 4      | Tipo de documento                                                                                                                                                                                                                                                                |
|                          |                                   | document\_number              | varchar | 15     | Número de documento                                                                                                                                                                                                                                                              |
|                          |                                   | issuing\_country\_code        | varchar | 2      | País emisor del documento                                                                                                                                                                                                                                                        |
|                          | immigration\_status               | unique\_name                  | varchar | 50     | Identificador único del estatus migratorio: work\_permit, holidays, resident. Ver Anexos [aquí](static-data.md)                                                                                                                                                                  |
|                          |                                   | description                   | varchar | 50     | Descripción del estatus migratorio: Permiso de trabajo, Vacaciones, Residente.                                                                                                                                                                                                   |
|                          | additional\_field1                |                               | varchar | 150    | Campo opcional para almacenar lo que indique el cliente. Este campo solo se agrega en la estructura si es un valor válido. Es decir, sólo se agregará en el JSON si existe valor, de lo contrario no.                                                                            |
|                          | additional\_field2                |                               | varchar | 150    | Campo opcional para almacenar lo que indique el cliente. Este campo solo se agrega en la estructura si es un valor válido.Es decir, sólo se agregará en el JSON si existe valor, de lo contrario no.                                                                             |
|                          | additional\_field3                |                               | varchar | 150    | Campo opcional para almacenar lo que indique el cliente. Este campo solo se agrega en la estructura si es un valor válido. Es decir, sólo se agregará en el JSON si existe valor, de lo contrario no.                                                                            |
|                          | related\_accounts                 |                               | Array   |        | DESACTIVADO MOMENTÁNEAMENTE ESTE OBJETO. Este campo comprende una lista de objetos. Cada objeto contiene la información de la cuenta existente en el CORE bancario a la cual se va a vincular al solicitante. Este campo puede venir como una lista vaciá, en caso de no aplicar |
|                          |                                   | account\_link\_type\_code     | varchar | 1      | Código del tipo de vinculación del cliente. Descripción de los valores en los Anexos [aquí](static-data.md)                                                                                                                                                                      |
|                          |                                   | authorized\_signature         | boolean |        | Valor booleano que indica si el cliente es firma autorizada de la cuenta a vincular.                                                                                                                                                                                             |
|                          |                                   | related\_account\_number      | varchar | 20     | Número de cuenta a vincular al cliente.                                                                                                                                                                                                                                          |
|                          | wisenroll\_code*                  |                               | varchar | 6      | Código Wisenroll                                                                                                                                                                                                                                                                 |
| client\_documents        |                                   |                               | Array   |        | Listado de diccionarios de los documentos de identificación de clientes                                                                                                                                                                                                          |
|                          | document\_number\*                |                               | varchar | 20     | Número de documento                                                                                                                                                                                                                                                              |
|                          | document\_type\_code\*            |                               | varchar | 20     | Tipo de documento. Revisar los valores en la sección de [anexos](static-data.md)                                                                                                                                                                                                 |
|                          | issuing\_country\_code\*          |                               | varchar | 2      | País emisor del documento en Alpha-2                                                                                                                                                                                                                                             |
|                          | main\*                            |                               | integer | -      | Flag para etiquetas el documento como principal (1 es principal, 0 no es principal)                                                                                                                                                                                              |
| client\_location         | city\_description\*               |                               | varchar | 30     | Nombre de la ciudad de residencia                                                                                                                                                                                                                                                |
|                          | country\_code\*                   |                               | varchar | 2      | País de residencia en Alpha-2                                                                                                                                                                                                                                                    |
|                          | country\_description\*            |                               | varchar | 20     | Nombre del país de residencia                                                                                                                                                                                                                                                    |
|                          | locality\_code\*                  |                               | varchar | 6      | Código de localidad. Para clientes de USA es el código postal de la localidad                                                                                                                                                                                                    |
|                          | locality\_description\*           |                               | varchar | 70     | Nombre de la localidad                                                                                                                                                                                                                                                           |
|                          | country\_code\*                   |                               | varchar | 2      | Código de país Alpha-2                                                                                                                                                                                                                                                           |
|                          | address\*                         |                               | varchar | 100    | Dirección de domicilio                                                                                                                                                                                                                                                           |
|                          | housing\_type                     |                               | varchar | 50     | Tipo de vivienda. Los valores que toma son: Propia y Alquilada.                                                                                                                                                                                                                  |
|                          | rent\_amount                      |                               | float   | -      | Monto de alquiler en caso de que el tipo de vivienda es alquilada. Puede ser null.                                                                                                                                                                                               |
|                          | rent\_currency                    |                               | varchar | 3      | Moneda del monto del pago de alquiler.                                                                                                                                                                                                                                           |
|                          | owner                             |                               | varchar | 80     | Nombre del propietario (caso alquilado)                                                                                                                                                                                                                                          |
|                          | owner\_phone                      |                               | varchar | 30     | Número de teléfono del propietario (caso alquilado)                                                                                                                                                                                                                              |
|                          | postal\_code                      |                               | varchar | 10     | Código postal                                                                                                                                                                                                                                                                    |
|                          | neighbourhood\_description        |                               | varchar | 150    | Sector donde se encuentra ubicada la sede principal de la empresa.                                                                                                                                                                                                               |
|                          | province\_code                    |                               | varchar | 10     | Código de la provincia. Ver Anexos [aquí](static-data.md)                                                                                                                                                                                                                        |
|                          | province\_description             |                               | varchar | 70     | Nombre de la provincia                                                                                                                                                                                                                                                           |
| client\_contact          | email\*                           |                               | varchar | 50     | Correo electrónico                                                                                                                                                                                                                                                               |
|                          | cell\_phone\*                     |                               | varchar | 20     | Número de teléfono celular                                                                                                                                                                                                                                                       |
|                          | home\_phone                       |                               | varchar | 20     | Número de teléfono de casa                                                                                                                                                                                                                                                       |
|                          | buss\_phone                       |                               | varchar | 20     | Número de teléfono de oficina                                                                                                                                                                                                                                                    |
|                          | social\_network\_name             |                               | varchar | 45     | Nombre de red social                                                                                                                                                                                                                                                             |
|                          | social\_network\_username         |                               | varchar | 30     | Usuario de red social                                                                                                                                                                                                                                                            |
| client\_job\_info        | salary\*                          |                               | float   | -      | Monto de salario                                                                                                                                                                                                                                                                 |
|                          | annual\_gross\_salary\*           |                               | float   | -      | Salario Bruto Anual                                                                                                                                                                                                                                                              |
|                          | net\_income\*                     |                               | float   | -      | Ganancia Neta                                                                                                                                                                                                                                                                    |
|                          | currency\_code\*                  |                               | varchar | 4      | Código de moneda. Ver Anexos [aquí](static-data.md)                                                                                                                                                                                                                              |
|                          | company\_name                     |                               | varchar | 100    | Nombre de la compañía donde labora el cliente                                                                                                                                                                                                                                    |
|                          | country*                          |                               | varchar | 50     | País donde está ubicado la compañía                                                                                                                                                                                                                                              |
|                          | work\_address\*                   |                               | varchar | 150    | Dirección de trabajo                                                                                                                                                                                                                                                             |
|                          | entry\_date\*                     |                               | date    | -      | Fecha de ingreso a la compañía. Formato YYYY-MM-DD                                                                                                                                                                                                                               |
|                          | professional\_position\*          |                               | varchar | 100    | Cargo dentro de la empresa                                                                                                                                                                                                                                                       |
|                          | occupation\_code\*                |                               | varchar | 50     | Código de la ocupación                                                                                                                                                                                                                                                           |
|                          | occupation\_description\*         |                               | text    | -      | Descripción de la ocupación                                                                                                                                                                                                                                                      |
|                          | other\_job\_info                  |                               | object  | -      | Información de datos laborales adicionales del cliente                                                                                                                                                                                                                           |
|                          |                                   | ciiu                          | varchar | 6      | Código de la actividad comercial adicional. Esta información proviene las actividades cargadas en la plataforma GIR.                                                                                                                                                             |
|                          |                                   | commercial\_activity\_code    | varchar | 6      | Código de la actividad comercial adicional. Esta información proviene las actividades cargadas en la plataforma GIR.                                                                                                                                                             |
|                          |                                   | commercial\_activity\_description    | text | -  | Descripción de la actividad comercial adicional.                                                                                                                                                                                                                                 |
|                          |                                   | country\_code                 | varchar | 2      | País de negocio                                                                                                                                                                                                                                                                  |
|                          |                                   | quantity\_of\_employees       | integer | -      | Cantidad de empleados                                                                                                                                                                                                                                                            |
|                          |                                   | total\_assets                 | float   | -      | Monto total de los activos                                                                                                                                                                                                                                                       |
|                          |                                   | income\_in\_another\_currency | float   | -      | Ingresos en otra moneda expresado en USD. Por defecto es 0.                                                                                                                                                                                                                      |
|                          |                                   | origin\_income                | varchar | 100    | Origen de los ingresos. Por defecto retorna null                                                                                                                                                                                                                                 |
|                          | previously\_employment\_data      |                               | object  | -      | Datos de empleo anterior si aplica                                                                                                                                                                                                                                               |
|                          |                                   | previous\_boss\_full\_name    | varchar | 200    | Nombre completo de su jefe en el empleo anterior                                                                                                                                                                                                                                 |
|                          |                                   | previous\_company\_name       | varchar | 50     | Nombre de la compañía de su empleo anterior                                                                                                                                                                                                                                      |
|                          |                                   | previous\_seniority           | integer | -      | Antiguedad en su empleo anterior en meses                                                                                                                                                                                                                                        |
| client\_references       | first\_bank\_reference            | country\_code                 | varchar | 2      | Código de país en Alpha-2                                                                                                                                                                                                                                                        |
|                          |                                   | bank\_name                    | varchar | 50     | Nombre del banco                                                                                                                                                                                                                                                                 |
|                          |                                   | acount\_type\_description     | varchar | 50     | Tipo de cuenta                                                                                                                                                                                                                                                                   |
|                          |                                   | account\_number               | varchar | 50     | Número de cuenta                                                                                                                                                                                                                                                                 |
|                          |                                   | officer                       | varchar | 100    | Nombre de oficial responsable                                                                                                                                                                                                                                                    |
|                          |                                   | canceled                      | boolean | -      | True si es una cuenta cancelada, false en caso contrario                                                                                                                                                                                                                         |
|                          |                                   | canceled\_reason              | varchar | 200    | Razón de cancelación, en caso de que canceled sea true                                                                                                                                                                                                                           |
|                          | second\_bank\_reference           | country\_code                 | varchar | 2      | Código de país en Alpha-2.                                                                                                                                                                                                                                                       |
|                          |                                   | bank\_description             | varchar | 50     | Nombre del banco                                                                                                                                                                                                                                                                 |
|                          |                                   | acount\_type\_description     | varchar | 50     | Tipo de cuenta                                                                                                                                                                                                                                                                   |
|                          |                                   | account\_number               | varchar | 50     | Número de cuenta                                                                                                                                                                                                                                                                 |
|                          |                                   | officer                       | varchar | 100    | Nombre de oficial responsable                                                                                                                                                                                                                                                    |
|                          |                                   | canceled                      | boolean | -      | True si es una cuenta cancelada, false en caso contrario                                                                                                                                                                                                                         |
|                          |                                   | canceled\_reason              | varchar | 200    | Razón de cancelación, en caso de que canceled sea true                                                                                                                                                                                                                           |
|                          | first\_personal\_reference\*      | name                          | varchar | 100    | Nombre completo de la referencia                                                                                                                                                                                                                                                 |
|                          |                                   | relationship                  | varchar | 50     | Indica si es Familiar o No Familiar                                                                                                                                                                                                                                              |
|                          |                                   | reference\_relationship\_unique\_description  | varchar | 50     | Tipo de relación                                                                                                                                                                                                                                                 |
|                          |                                   | phone                         | varchar | 20     | Número de teléfono de la referencia                                                                                                                                                                                                                                              |
|                          |                                   | address                       | varchar | 150    | Dirección                                                                                                                                                                                                                                                                        |
|                          |                                   | document\_number              | varchar | 20     | Número de documento                                                                                                                                                                                                                                                              |
|                          |                                   | document\_type\_code          | varchar | 4      | Tipo de documento                                                                                                                                                                                                                                                                |
|                          |                                   | country                       | varchar | 2      | País del documento                                                                                                                                                                                                                                                               |
|                          | second\_personal\_reference\*     | name                          | varchar | 100    | Nombre completo de la referencia                                                                                                                                                                                                                                                 |
|                          |                                   | relationship                  | varchar | 50     | Indica si es Familiar o No Familiar                                                                                                                                                                                                                                              |
|                          |                                   | reference\_relationship\_unique\_description  | varchar | 50     | Tipo de relación                                                                                                                                                                                                                                                 |
|                          |                                   | phone                         | varchar | 20     | Número de teléfono de la referencia                                                                                                                                                                                                                                              |
|                          |                                   | address                       | varchar | 150    | Dirección                                                                                                                                                                                                                                                                        |
|                          |                                   | document\_number              | varchar | 20     | Número de documento                                                                                                                                                                                                                                                              |
|                          |                                   | document\_type\_code          | varchar | 4      | Tipo de documento                                                                                                                                                                                                                                                                |
|                          |                                   | country                       | varchar | 2      | País del documento                                                                                                                                                                                                                                                               |
| client\_transactionality | inc\_transactionality\*           |                               | float   | -      | Monto de transaccionalidad entrante                                                                                                                                                                                                                                              |
|                          | inc\_transactionality\_currency\* |                               | varchar | 3      | Moneda                                                                                                                                                                                                                                                                           |
|                          | inc\_monthly\_movements\*         |                               | integer | -      | Cantidad de movimientos mensuales de entrada                                                                                                                                                                                                                                     |
|                          | out\_transactionality\*           |                               | float   | -      | Monto de transaccionalidad saliente                                                                                                                                                                                                                                              |
|                          | out\_transactionality\_currency\* |                               | varchar | 3      | Moneda                                                                                                                                                                                                                                                                           |
|                          | out\_monthly\_movements\*         |                               | integer | -      | Cantidad de movimientos mensuales de salida                                                                                                                                                                                                                                      |
|                          | funds\_country\_code\*            |                               | varchar | 2      | Código de país en Alpha-2.                                                                                                                                                                                                                                                       |
|                          | funds\_country\_description\*     |                               | varchar | 50     | País de origen y destino de los fondos                                                                                                                                                                                                                                           |
| client\_entity\_branch   | branch\_code\*                    |                               | varchar | 6      | Código de la sucursal de la entidad.                                                                                                                                                                                                                                             |
|                          | branch\_name\*                    |                               | varchar | 35     | Nombre de la sucursal de la entidad.                                                                                                                                                                                                                                             |

**Nota:** Los marcados con asterisco (\*) son los recomendados a que sean almacenados en el core. Los que no tengan esta marca pueden llegar con el valor de null, vacío (“”) o incluso no aparecer en el json.

> :warning: **Importante:** Se recomienda siempre validar que algún objeto y/o campo del json venga en el mismo. Ejemplo:

```
data = request.json

if data.get('client_job_info').get('previously_employment_data'):
   # Write your code here
```

## Ejemplos

<details>

<summary>Ejemplo 1</summary>

```
{
        "client_info": {
            "additional_field1": "Value1",
            "additional_field2": "Value2",
            "additional_field3": "Value3",
            "first_name": "PEDRO",
            "middle_name": null,
            "last_name": "LOPEZ",
            "middle_last_name": "MARTINEZ",
            "sex": "M",
            "birth_date": "1990-10-12",
            "civil_status_id": 2,
            "civil_status_description": "Casado(a)",
            "nationality_code": "CO",
            "other_nationality_code": "",
            "document_type_code": "PASA",
            "document_number": "ABC100010",
            "issuing_country_code": "CO",
            "other_document_type_code": "CEDU",
            "other_document_number": "12312232",
            "other_issuing_country_code": "CO",
            "person_type_code": null,
            "product_request_type": {
                  "description": "Solicitud de Producto",
                  "code": "product_request"
           },
           "related_accounts": [],
            "spouse_info": {
                "first_name": "CAROLINA",
                "middle_name": "",
                "last_name": "CEDEPA",
                "middle_last_name": "",
                "document_type_code": "CEDU",
                "document_number": "12345678903",
                "issuing_country_code": "DO"
            },
            "immigration_status": {
                "unique_name": "resident",
                "description": "Residente"
            }
        },
        "client_location": {
            "city_description": "Distrito Nacional",
            "province_code": "0100",
            "province_description": "Distrito Nacional",
            "country_description": "Republica Dominicana",
            "locality_code": "010101",
            "locality_description": "SANTO DOMINGO DE GUZMÁN",
            "country_code": "DO",
            "address": "C/Juan Sanchez Ramirez no. 183",
            "neighbourhood_description": "Altos de Arroyo Hondo",
            "housing_type": "Propia",
            "rent_amount": null,
            "owner": null,
            "owner_phone": null,
            "postal_code": "11111"
        },
        "client_contact": {
            "email": "example@example.com",
            "cell_phone": "8294477744",
            "home_phone": null,
            "buss_phone": "8094417785",
            "fax": null,
            "social_network_name": "Instagram",
            "social_network_username": "pedrolopez"
        },
        "client_job_info": {
            "salary": 25000.00,
            "net_income": 20000.00,
            "currency_code": "DOP",
            "work_address": "Av 27 de Febrero",
            "ciiu": "950004",
            "company_name": "COMPANY S.A.",
            "other_job_info": {
                "ciiu": "171200",
                "country_code": "DO",
                "quantity_of_employees": 10,
                "total_assets": 100000.00,
                "income_in_another_currency": 5000.00,
                "origin_income": "Venta de vehiculos" ,
            }
        },
        "client_references": {
            "first_bank_reference": {
                "country_code": "DO",
                "bank_name": "Banco NOMBRE",
                "account_type_description": "Cuenta de Ahorros",
                "account_number": "12345678",
                "officer": "Luis Perez"
            },
            "second_bank_reference": null,
            "first_personal_reference": {
                "name": "Maria Perez",
                "relationship": "Familiar",
                "phone": "8095414411",
                "address": "Av Caonabo",
                "document_number": "12345648",
                "document_type_code": "CEDU"
            },
            "second_personal_reference": {
                "name": "Pedro Perez",
                "relationship": "No Familiar",
                "phone": "8094444444",
                "address": "Av Anacaona",
                "document_number": "14444444",
                "document_type_code": "CEDU"
            }
        },
        "client_transactionality": {
            "inc_transactionality": 25000.00,
            "inc_monthly_movements": 2,
            "out_transactionality": 20000.00,
            "out_monthly_movements": 5,
        },
        "client_entity_branch": {
            "branch_code": "01",
            "branch_name": "NOMBRE SUCURSAL",
        }
    }
```

</details>

<details>

<summary>Ejemplo 2</summary>

```
{
   "client_info":{
      "additional_field1":"Value1",
      "additional_field2":"Value2",
      "additional_field3":"Value3",
      "first_name":"PEDRO",
      "middle_name":"",
      "last_name":"LOPEZ",
      "middle_last_name":"MARTINEZ",
      "sex":"M",
      "birth_date":"1990-10-12",
      "civil_status_id":1,
      "civil_status_description":"Soltero(a)",
      "nationality_code":"DO",
      "other_nationality_code":null,
      "document_type_code":"CEDU",
      "document_number":"40211101000",
      "issuing_country_code":"DO",
      "other_document_type_code":null,
      "other_document_number":null,
      "other_issuing_country_code":null,
      "person_type_code":null,
      "product_request_type":{
         "description":"Solicitud de Producto",
         "code":"product_request"
      },
      "related_accounts": [],
      "spouse_info":null,
      "immigration_status":{
         "unique_name":"work_permit",
         "description":"Permiso de Trabajo"
      }
   },
   "client_location":{
      "city_description":"Distrito Nacional",
      "province_code":"0100",
      "province_description":"Distrito Nacional",
      "country_description":"Republica Dominicana",
      "locality_code":"010101",
      "locality_description":"SANTO DOMINGO DE GUZMÁN",
      "country_code":"DO",
      "address":"C/Juan Sanchez Ramirez no. 183",
      "neighbourhood_description":"Altos de Arroyo Hondo",
      "housing_type":"Alquilada",
      "rent_amount":20000.00,
      "owner":"jose perez",
      "owner_phone":"8095511111",
      "postal_code":"11111"
   },
   "client_contact":{
      "email":"example@example.com",
      "cell_phone":"8294477744",
      "home_phone":null,
      "buss_phone":"8094417785",
      "fax":null,
      "social_network_name":"Instagram",
      "social_network_username":"pedrolopez"
   },
   "client_job_info":{
      "salary":25000.00,
      "net_income":20000.00,
      "currency_code":"DOP",
      "work_address":"Av 27 de Febrero",
      "ciiu":"950004",
      "company_name":"COMPANY S.A.",
      "other_job_info":null
   },
   "client_references":{
      "first_bank_reference": null,
      "second_bank_reference":null,
      "first_personal_reference":{
         "name":"Maria Perez",
         "relationship":"Familiar",
         "phone":"8095414411",
         "address":"Av Caonabo",
         "document_number":"12345648",
         "document_type_code":"CEDU"
      },
      "second_personal_reference":{
         "name":"Pedro Perez",
         "relationship":"No Familiar",
         "phone":"8094444444",
         "address":"Av Anacaona",
         "document_number":"14444444",
         "document_type_code":"CEDU"
      }
   },
   "client_transactionality":{
      "inc_transactionality":25000.00,
      "inc_monthly_movements":2,
      "out_transactionality":20000.00,
      "out_monthly_movements":5
   },
   "client_entity_branch":{
      "branch_code":"01",
      "branch_name":"NOMBRE SUCURSAL"
   }
}
```

</details>

<details>

<summary>Ejemplo 3</summary>

```
{
   "client_info":{
      "additional_field1":null,
      "additional_field2":null,
      "additional_field3":null,
      "birth_date":"1992-06-26",
      "civil_status_description":"Casado(a)",
      "civil_status_id":2,
      "civil_status_unique_description":"married",
      "first_name":"DAVID",
      "immigration_status":null,
      "last_name":"CASTILLO",
      "middle_last_name":"TORREALBA",
      "middle_name":"JOSE",
      "nationality_code":"US",
      "nickname":"josedav",
      "other_nationality_code":"US",
      "other_sex_description":null,
      "person_type_code":"PEP",
      "product_request_type":null,
      "related_accounts":null,
      "sex":"M",
      "spouse_info":{
         "document_number":"7896321",
         "document_type_code":"IDAD",
         "first_name":"YRENE",
         "issuing_country_code":"US",
         "last_name":"ROJAS",
         "middle_last_name":"RAMOS",
         "middle_name":"CLARA"
      },
      "wisenroll_code":"X2T2UU"
   },
   "client_contact":{
      "buss_phone":"789456002",
      "cell_phone":"321654002",
      "email":"example@example.com",
      "fax":null,
      "home_phone":null,
      "social_network_name":"Facebook",
      "social_network_username":"davidjos"
   },
   "client_documents":[
      {
         "document_number":"502073319",
         "document_type_code":"SSN",
         "issuing_country_code":"US",
         "main":1
      },
      {
         "document_number":"502-07-3319",
         "document_type_code":"DOPI",
         "issuing_country_code":"US",
         "main":0
      }
   ],
   "client_entity_branch":{
      "branch_code":"01",
      "branch_name":"Agencia 27 de febrero"
   }
   "client_job_info":{
      "annual_gross_salary":100.0,
      "company_name":"byphone",
      "country":"Estados Unidos",
      "currency_code":"USD",
      "entry_date":"2004-03-27",
      "net_income":100.0,
      "occupation_code":"432021",
      "occupation_description":"Telephone Operators                                                                                                                                             ",
      "other_job_info":{
         "commercial_activity_code":"312230",
         "commercial_activity_description":"Tobacco Manufacturing                                                                                                                                           ",
         "country_code":"US",
         "income_in_another_currency":1000.0,
         "origin_income":"sales and trade",
         "quantity_of_employees":10,
         "total_assets":10.0
      },
      "previously_employment_data":null,
      "professional_position":"Operator",
      "salary":100.0,
      "work_address":"street byphone",
      "work_email":null
   },
   "client_location":{
      "address":"street florida",
      "city_description":"Miami",
      "country_code":"US",
      "country_description":"Estados Unidos",
      "housing_type":"Alquilada",
      "locality_code":"33101",
      "locality_description":"MIAMI",
      "neighbourhood_description":null,
      "owner":"JUAN",
      "owner_phone":"456987002",
      "postal_code":"33101",
      "province_code":null,
      "province_description":"Florida",
      "rent_amount":10000.0,
      "rent_currency":"USD"
   },
   "client_references":{
      "first_bank_reference":null,
      "first_personal_reference":{
         "address":"main street florida",
         "country":"Estados Unidos",
         "document_number":"1213456789",
         "document_type_code":"IDAD",
         "email":"juliet002@dev.aclaoverseas.com",
         "name":"JULIET DEL CARMEN ROJAS",
         "phone":"789456002",
         "reference_relationship_unique_description":"uncle",
         "relationship":"Familiar"
      },
      "second_bank_reference":null,
      "second_personal_reference":{
         "address":"main street florida",
         "country":"Estados Unidos",
         "document_number":"741852963",
         "document_type_code":"IDAD",
         "email":"gina002@dev.aclaoverseas.com",
         "name":"GINA NIEVES MORA",
         "phone":"369258002",
         "reference_relationship_unique_description":"neighbour",
         "relationship":"No Familiar"
      }
   },
   "client_transactionality":{
      "funds_country_code":"US",
      "funds_country_description":"Estados Unidos",
      "inc_monthly_movements":100,
      "inc_transactionality":100.0,
      "inc_transactionality_currency":"USD",
      "out_monthly_movements":100,
      "out_transactionality":100.0,
      "out_transactionality_currency":"USD"
   }
}
```

</details>

## Respuestas por parte de la entidad

La entidad deberá retornar, en formato JSON, el número de cliente con los siguientes http status codes:

* **201** - Respuesta exitosa, cliente creado
* **4XX** - Error creando el cliente. Deberá retornar el mensaje del error para ser mostrado en la aplicación.

## Ejemplos de respuestas

<details>

<summary>Ejemplo 1 - Caso exitoso (HTTP STATUS CODE 201)</summary>

```
{
	"customer_number": 1234
}
```

</details>

<details>

<summary>Ejemplo 2 - Caso error (HTTP STATUS CODE 4XX)</summary>

```
{
	"message": "MENSAJE DEL MOTIVO DEL ERROR"
}
```

</details>
