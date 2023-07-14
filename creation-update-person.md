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
* **client\_documents:** Datos de los documentos de identificación del cliente. Es retornado en una lista de diccionarios.
* **client\_location:** Datos de vivienda del cliente.
* **client\_contact:** Datos de contacto del cliente.
* **client\_job\_info:** Datos laborales del cliente.
* **client\_references:** Referencias bancarias y personales. Las referencias bancarias son opcionales.
* **client\_transactionality:** Datos de transaccionalidad del cliente.
* **client\_entity\_branch:** Información de la sucursal de la entidad donde el cliente se está registrando.

> :warning: **IMPORTANTE:** Recomendamos que aquellos campos que no son marcados con un asterisco (\*), se valide primero si ese campo y/o objeto es enviado en el json o no. Esto porque puede haber casos en que dicho campo y/o objeto esté en el json si hay algún valor, de lo contrario no aparecerá.

A continuación la descripción de cada campo:

| NIVEL 1                  | NIVEL 2                           | NIVEL 3                       | TIPO    | TAMAÑO | DESCRIPCIÓN                                                                                                                                                                                                                                                                      |
| ------------------------ | --------------------------------- | ----------------------------- | ------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| client\_info             | first\_name\*                     |                               | varchar | 30     | Primer nombre del cliente                                                                                                                                                                                                                                                        |
|                          | middle\_name                      |                               | varchar | 30     | Segundo nombre del cliente                                                                                                                                                                                                                                                       |
|                          | last\_name\*                      |                               | varchar | 30     | Primer apellido del cliente                                                                                                                                                                                                                                                      |
|                          | middle\_last\_name                |                               | varchar | 30     | Segundo Nombre                                                                                                                                                                                                                                                                   |
|                          | nickname                          |                               | varchar | 100    | Apodo                                                                                                                                                                                                                                                                            |
|                          | sex\*                             |                               | varchar | 1      | Género: M (Male) / F (Female) / NB (No Binary) / NC (No Comments) / O (Other)                                                                                                                                                                                                    |
|                          | other\_sex\_description           |                               | varchar | 50     | Cuando sex = O (Other) se envía el género indicado por el cliente                                                                                                                                                                                                                |
|                          | birth\_date\*                     |                               | date    | -      | Fecha de Nacimiento. Formato YYYY-MM-DD                                                                                                                                                                                                                                          |
|                          | civil\_status\_id\*               |                               | integer | -      | Código de estado civil. Ver Anexos [aquí](static-data.md)                                                                                                                                                                                                                        |
|                          | civil\_status\_description        |                               | varchar | 50     | Descripción del estado civil                                                                                                                                                                                                                                                     |
|                          | civil\_status\_unique\_description|                               | varchar | 20     | Descripción única del estado civil. Ejemplo: married, single, etc.                                                                                                                                                                                                                                                     |
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
|                          | wisenroll\_code*                  |                               | varchar | 6      | Código Wisenroll                                                                                                                                                                                                                                                                 |
|                          | additional\_field1                |                               | varchar | 150    | Campo opcional para almacenar lo que indique el cliente. Este campo solo se agrega en la estructura si es un valor válido. Es decir, sólo se agregará en el JSON si existe valor, de lo contrario no.                                                                            |
|                          | additional\_field2                |                               | varchar | 150    | Campo opcional para almacenar lo que indique el cliente. Este campo solo se agrega en la estructura si es un valor válido.Es decir, sólo se agregará en el JSON si existe valor, de lo contrario no.                                                                             |
|                          | additional\_field3                |                               | varchar | 150    | Campo opcional para almacenar lo que indique el cliente. Este campo solo se agrega en la estructura si es un valor válido. Es decir, sólo se agregará en el JSON si existe valor, de lo contrario no.                                                                            |
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
|                          | province\_code                    |                               | varchar | 10     | Código de la provincia. Ver Anexos [aquí](static-data.md). Tendrá algún valor para direcciones de República Dominicana                                                                                                                                                           |
|                          | province\_description             |                               | varchar | 70     | Nombre de la provincia                                                                                                                                                                                                                                                           |
| client\_contact          | email\*                           |                               | varchar | 50     | Correo electrónico                                                                                                                                                                                                                                                               |
|                          | cell\_phone\*                     |                               | varchar | 20     | Número de teléfono celular                                                                                                                                                                                                                                                       |
|                          | home\_phone                       |                               | varchar | 20     | Número de teléfono de casa                                                                                                                                                                                                                                                       |
|                          | buss\_phone                       |                               | varchar | 20     | Número de teléfono de oficina                                                                                                                                                                                                                                                    |
|                          | social\_network\_name             |                               | varchar | 45     | Nombre de red social                                                                                                                                                                                                                                                             |
|                          | social\_network\_username         |                               | varchar | 30     | Usuario de red social                                                                                                                                                                                                                                                            |
| client\_job\_info        | salary\*                          |                               | float   | -      | Monto de salario                                                                                                                                                                                                                                                                 |
|                          | annual\_gross\_salary\*           |                               | float   | -      | Salario Bruto Anual                                                                                                                                                                                                                                                              |
|                          | net\_income\*                     |                               | float   | -      | Ingreso Neto Mensual                                                                                                                                                                                                                                                                    |
|                          | currency\_code\*                  |                               | varchar | 4      | Código de moneda. Ver Anexos [aquí](static-data.md)                                                                                                                                                                                                                              |
|                          | company\_name                     |                               | varchar | 100    | Nombre de la compañía donde labora el cliente                                                                                                                                                                                                                                    |
|                          | country*                          |                               | varchar | 50     | País donde está ubicado la compañía                                                                                                                                                                                                                                              |
|                          | work\_address\*                   |                               | varchar | 150    | Dirección de trabajo                                                                                                                                                                                                                                                             |
|                          | work\_email                       |                               | varchar | 100    | Correo electrónico del trabajo                                                                                                                                                                                                                                                             |
|                          | entry\_date\*                     |                               | date    | -      | Fecha de ingreso a la compañía. Formato YYYY-MM-DD                                                                                                                                                                                                                               |
|                          | professional\_position\*          |                               | varchar | 100    | Cargo dentro de la empresa                                                                                                                                                                                                                                                       |
|                          | occupation\_code\*                |                               | varchar | 50     | Código de la ocupación. Para el caso de Dominicana será el código CIIU                                                                                                                                                                                                                                                           |
|                          | occupation\_description\*         |                               | text    | -      | Descripción de la ocupación                                                                                                                                                                                                                                                      |
|                          | other\_job\_info                  |                               | object  | -      | Información de datos laborales adicionales del cliente                                                                                                                                                                                                                           |
|                          |                                   | commercial\_activity\_code    | varchar | 6      | Código de la actividad comercial adicional. Esta información proviene las actividades cargadas en la plataforma GIR.                                                                                                                                                             |
|                          |                                   | commercial\_activity\_description    | text | -  | Descripción de la actividad comercial adicional.                                                                                                                                                                                                                                 |
|                          |                                   | country\_code                 | varchar | 2      | País de negocio                                                                                                                                                                                                                                                                  |
|                          |                                   | quantity\_of\_employees       | integer | -      | Cantidad de empleados                                                                                                                                                                                                                                                            |
|                          |                                   | total\_assets                 | float   | -      | Monto total de los activos                                                                                                                                                                                                                                                       |
|                          |                                   | income\_in\_another\_currency | float   | -      | Ingresos en otra moneda expresado en USD. Por defecto es 0.                                                                                                                                                                                                                      |
|                          |                                   | origin\_income                | varchar | 100    | Descripción del origen de los ingresos                                                                                                                                                                                                                                           |
|                          | previously\_employment\_data      |                               | object  | -      | Datos de empleo anterior si aplica                                                                                                                                                                                                                                               |
|                          |                                   | previous\_boss\_full\_name    | varchar | 200    | Nombre completo de su jefe en el empleo anterior                                                                                                                                                                                                                                 |
|                          |                                   | previous\_company\_name       | varchar | 50     | Nombre de la compañía de su empleo anterior                                                                                                                                                                                                                                      |
|                          |                                   | previous\_seniority           | integer | -      | Antiguedad en su empleo anterior en meses                                                                                                                                                                                                                                        |
| client\_references       | first\_bank\_reference            | country\_code                 | varchar | 2      | Código de país en Alpha-2                                                                                                                                                                                                                                                        |
|                          |                                   | bank\_name                    | varchar | 50     | Nombre del banco                                                                                                                                                                                                                                                                 |
|                          |                                   | acount\_type\_description     | varchar | 50     | Tipo de cuenta                                                                                                                                                                                                                                                                   |
|                          |                                   | account\_number               | varchar | 50     | Número de cuenta                                                                                                                                                                                                                                                                 |
|                          |                                   | officer                       | varchar | 100    | Nombre de oficial responsable                                                                                                                                                                                                                                                    |
|                          |                                   | canceled                      | integer | -      | 1 si es una cuenta cancelada, 0 en caso contrario                                                                                                                                                                                                                                |
|                          |                                   | canceled\_reason              | varchar | 200    | Razón de cancelación, en caso de que canceled sea true                                                                                                                                                                                                                           |
|                          |                                   | aba\_code                     | varchar | 50     | ABA Routing Number. Para caso de bancos en USA                                                                                                                                                                                                                                   |
|                          |                                   | iban\_code                    | varchar | 50     | Internation Bank Account number                                                                                                                                                                                                                                                  |
|                          |                                   | swift\_code                   | varchar | 50     | Código SWIFT                                                                                                                                                                                                                                                                     |
|                          | second\_bank\_reference           |                               | -       | -      | Igual que **first_bank_reference**                                                                                                                                                                                                                                               |
|                          | first\_personal\_reference\*      | name                          | varchar | 100    | Nombre completo de la referencia                                                                                                                                                                                                                                                 |
|                          |                                   | relationship                  | varchar | 50     | Indica si es Familiar o No Familiar                                                                                                                                                                                                                                              |
|                          |                                   | reference\_relationship\_unique\_description  | varchar | 50     | Tipo de relación                                                                                                                                                                                                                                                 |
|                          |                                   | phone                         | varchar | 20     | Número de teléfono de la referencia                                                                                                                                                                                                                                              |
|                          |                                   | address                       | varchar | 150    | Dirección                                                                                                                                                                                                                                                                        |
|                          |                                   | document\_number              | varchar | 20     | Número de documento                                                                                                                                                                                                                                                              |
|                          |                                   | document\_type\_code          | varchar | 4      | Tipo de documento                                                                                                                                                                                                                                                                |
|                          |                                   | country                       | varchar | 2      | País del documento                                                                                                                                                                                                                                                               |
|                          |                                   | email                         | varchar | 150    | Correo electrónico                                                                                                                                                                                                                                                               |
|                          | second\_personal\_reference\*     | name                          | varchar | 100    | Igual que **first_personal_reference**                                                                                                                                                                                                                                           |
|                          | first\_supplier\_commercial\_reference |                          | -       | -      | Referencias comerciales (Proveedor). Se solicitan cualquier el cliente tiene alguna actividad económica adicional                                                                                                                                                                |
|                          |                                   | name                          | varchar | 100    | Nombre completo del proveedor                                                                                                                                                                                                                                                    |
|                          |                                   | phone                         | varchar | 20     | Número de teléfono del proveedor                                                                                                                                                                                                                                                 |
|                          |                                   | country                       | varchar | 2      | País donde se encuentra el proveedor                                                                                                                                                                                                                                             |
|                          | second\__supplier_\_commercial\_reference |                         | -       | -      | Igual que en **first_supplier_commercial_reference**                                                                                                                                                                                                                           |
|                          | first\_client_\_commercial\_reference |                           | -       | -      | Principales clientes. Se solicitan cualquier el cliente tiene alguna actividad económica adicional                                                                                                                                                                               |
|                          |                                   | name                          | varchar | 100    | Nombre del cliente. Puede ser empresa o persona física                                                                                                                                                                                                                           |
|                          |                                   | phone                         | varchar | 20     | Número de teléfono del cliente                                                                                                                                                                                                                                                   |
|                          |                                   | country                       | varchar | 2      | País donde se encuentra el cliente                                                                                                                                                                                                                                               |
|                          | second\_client_\_commercial\_reference |                          | -       | -      | Igual que en **first_client_commercial_reference**                                                                                                                                                                                                                               |
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

# Condición que valida si el key previously_employment_data viene en el json
if data.get('client_job_info').get('previously_employment_data'):
   # Write your code here
```

## Ejemplos

<details>

<summary>Ejemplo 1</summary>

```
{
   "client_info":{
      "civil_status_unique_description":"married",
      "last_name":"MARTINEZ",
      "nationality_code":"US",
      "sex":"M",
      "bonding_type":"NI",
      "first_name":"LUIS",
      "middle_name":"ALEJANDRO",
      "industry_code":"511",
      "account_officer":"JCA",
      "business_code":"L003",
      "other_nationality_code":"DO",
      "spouse_info":{
         "document_type_code":"IDEN",
         "first_name":"EMILY",
         "last_name":"TORRES",
         "document_number":"556699130",
         "issuing_country_code":"VE",
      },
      "wisenroll_code":"4MTA2C",
      "customer_number":null,
      "office_representative":"VME",
      "civil_status_description":"Casado(a)",
      "birth_date":"1991-11-14",
      "civil_status_id":2,
   },
   "client_entity_branch":{
      "branch_name":"Branch 01",
      "branch_code":"01"
   },
   "client_transactionality":{
      "inc_transactionality":8000.0,
      "out_transactionality":7550.0,
      "funds_country_description":"Estados Unidos",
      "out_monthly_movements":10,
      "funds_country_code":"US",
      "out_transactionality_currency":"USD",
      "inc_monthly_movements":2,
      "inc_transactionality_currency":"USD"
   },
   "client_location":{
      "rent_currency":"USD",
      "province_description":"Florida",
      "locality_description":"MIAMI",
      "address":"1100 Biscayne Boulevard",
      "housing_type":"Alquilada",
      "city_description":"Miami",
      "country_description":"Estados Unidos",
      "rent_amount":2000.0,
      "postal_code":"33132",
      "country_code":"US",
      "owner_phone":"7869898899",
      "owner":"JOE DOE",
      "locality_code":"33132"
   },
   "client_job_info":{
      "salary":8000.0,
      "other_job_info":{
         "quantity_of_employees":1,
         "total_assets":500.0,
         "origin_income":"Ad Campaign for some companies",
         "income_in_another_currency":10000.0,
         "commercial_activity_code":"541810",
         "country_code":"US",
         "commercial_activity_description":"Advertising Agencies"
      },
      "professional_position":"Software Developer Senior",
      "annual_gross_salary":100000.0,
      "occupation_code":"151252",
      "country":"Estados Unidos",
      "entry_date":"2020-02-21",
      "company_name":"TECH GROUP LLC",
      "work_address":"601 Brickell Key Drive, Miami, FL 33131",
      "occupation_description":"Software Developers",
      "work_email":"luis@business.com",
      "currency_code":"USD",
      "net_income":8000.0
   },
   "client_documents":[
      {
         "document_type_code":"DL",
         "document_number":"M1112333999",
         "main":0,
         "issuing_country_code":"US"
      },
      {
         "document_type_code":"DOPI",
         "document_number":"2132322355",
         "main":0,
         "issuing_country_code":"US"
      },
      {
         "document_type_code":"IDAD",
         "document_number":"9987441111",
         "main":0,
         "issuing_country_code":"DO"
      },
      {
         "document_type_code":"PAAD",
         "document_number":"99914470141",
         "main":0,
         "issuing_country_code":"DO"
      },
      {
         "document_type_code":"SSN",
         "document_number":"111110001",
         "main":1,
         "issuing_country_code":"US"
      }
   ],
   "client_references":{
      "first_personal_reference":{
         "document_type_code":"IDEN",
         "name":"JESUS RODRIGUEZ",
         "relationship":"Familiar",
         "country":"Republica Dominicana",
         "document_number":"78986888668",
         "reference_relationship_unique_description":"brother_in_law",
         "phone":"7865545454",
         "address":"465 Ocean Drive, Miami Beach, FL 33139",
         "email":"jesusr@example.com"
      },
      "first_client_commercial_reference":{
         "name":"LUMINATECH CORPORATION",
         "country":"Estados Unidos",
         "phone":"7865555454",
      },
      "first_bank_reference":{
         "bank_name":"WELLS FARGO",
         "aba_code":"063107513",
         "canceled":0,
         "officer":"Ramon Marquez",
         "country_code":"US",
         "account_type_description":"Corriente",
         "account_number":"515155155"
      },
      "second_supplier_commercial_reference":null,
      "second_personal_reference":{
         "document_type_code":"SSN",
         "name":"MARIA PEREZ",
         "relationship":"No Familiar",
         "country":"Estados Unidos",
         "document_number":"788888888",
         "reference_relationship_unique_description":"coworker",
         "phone":"7865554545",
         "address":"801 South Miami Avenue, Miami, FL 33130",
         "email":"mariap@example.com"
      },
      "first_supplier_commercial_reference":{
         "name":"NEXGEN SOLUTIONS",
         "country":"Estados Unidos",
         "phone":"7865454545",
      }
   },
   "client_contact":{
      "buss_phone":"7861111121",
      "social_network_name":"Instagram",
      "social_network_username":"luismart01",
      "cell_phone":"7862229999",
      "email":"aauser07@dev.aclaoverseas.com"
   }
}
```

</details>

<details>

<summary>Ejemplo 2</summary>

```
{
   "client_info":{
      "civil_status_unique_description":"single",
      "first_name":"DANIELA",
      "last_name":"MATOS",
      "middle_name":"ANDREINA",
      "nationality_code":"US",
      "civil_status_description":"Soltero(a)",
      "industry_code":"511",
      "account_officer":"JCA",
      "sex":"F",
      "office_representative":"VME",
      "bonding_type":"NI",
      "business_code":"L003",
      "birth_date":"1989-11-23",
      "civil_status_id":1,
      "wisenroll_code":"K9ZW56",
      "nickname":"Maria M",
      "additional_field1":"Value1",
      "additional_field2":"Value2",
      "additional_field3":"Value3",
   },
   "client_entity_branch":{
      "branch_name":"Agencia 27 de febrero",
      "branch_code":"01"
   },
   "client_transactionality":{
      "inc_transactionality":4000.0,
      "out_transactionality":3000.0,
      "funds_country_description":"Estados Unidos",
      "out_monthly_movements":6,
      "funds_country_code":"US",
      "out_transactionality_currency":"USD",
      "inc_monthly_movements":2,
      "inc_transactionality_currency":"USD"
   },
   "client_location":{
      "rent_currency":"USD",
      "locality_description":"MIAMI",
      "province_description":"Florida",
      "housing_type":"Propia",
      "city_description":"Miami",
      "country_description":"Estados Unidos",
      "locality_code":"33130",
      "country_code":"US",
      "address":"801 South Miami Avenue. Apt 123",
      "postal_code":"33130"
   },
   "client_job_info":{
      "salary":4000.0,
      "professional_position":"Ad Agent Junior",
      "annual_gross_salary":50000.0,
      "occupation_code":"413011",
      "entry_date":"2022-04-20",
      "company_name":"AdSolutions Plus",
      "company_country_code":"DO",
      "work_address":"Avenida Winston Churchill #654, Santo Domingo 10106",
      "occupation_description":"Advertising Sales Agents",
      "currency_code":"USD",
      "net_income":4000.0
   },
   "client_documents":[
      {
         "document_type_code":"DL",
         "document_number":"7775000123",
         "main":0,
         "issuing_country_code":"DO"
      },
      {
         "document_type_code":"DOPI",
         "document_number":"222221221",
         "main":0,
         "issuing_country_code":"US"
      },
      {
         "document_type_code":"SSN",
         "document_number":"111010001",
         "main":1,
         "issuing_country_code":"US"
      }
   ],
   "client_references":{
      "first_bank_reference":{
         "bank_name":"Banco BHD/Leon",
         "canceled":0,
         "officer":"Ramiro Lopez",
         "country_code":"DO",
         "account_type_description":"Ahorros",
         "account_number":"2322222232"
      },
      "first_personal_reference":{
         "document_type_code":"SSN",
         "name":"ALEXANDER ROMERO",
         "relationship":"Familiar",
         "country":"Estados Unidos",
         "document_number":"111221212",
         "reference_relationship_unique_description":"cousin",
         "phone":"7865454545",
         "address":"601 Brickell Key Drive, Miami, FL 33131",
         "email":"alex@example.com"
      },
      "second_personal_reference":{
         "document_type_code":"IDEN",
         "name":"JULIA MARTINEZ",
         "relationship":"No Familiar",
         "country":"Republica Dominicana",
         "document_number":"11111558989",
         "reference_relationship_unique_description":"friend",
         "phone":"7865454545",
         "address":"Calle El Conde #789, Santo Domingo 10205",
         "email":"julia@example.com"
      }
   },
   "client_contact":{
      "social_network_username":"mariam18",
      "cell_phone":"7891231100",
      "email":"aauser08@dev.aclaoverseas.com",
      "social_network_name":"Instagram"
   }
}
```

</details>

<details>

<summary>Ejemplo 3</summary>
 
```
{
   "client_info":{
      "civil_status_unique_description":"married",
      "first_name":"DAVID",
      "last_name":"CASTILLO",
      "middle_name":"JOSE",
      "nationality_code":"US",
      "civil_status_description":"Casado(a)",
      "sex":"M",
      "office_representative":"VME",
      "bonding_type":"NI",
      "business_code":"L003",
      "birth_date":"1988-12-27",
      "civil_status_id":2,
      "wisenroll_code":"LH0KN3",
      "middle_last_name":"TORREALBA",
      "other_nationality_code":"DO",
      "spouse_info":{
         "document_type_code":"IDEN",
         "first_name":"ANDREINA",
         "last_name":"PEREZ",
         "middle_name":"MARIA",
         "document_number":"55554555044",
         "issuing_country_code":"DO",
         "middle_last_name":"RODRIGUEZ"
      },
      "additional_field1":"Addition information 1",
      "additional_field2":"Addition information 2",
      "additional_field3":"Addition information 3",
   },
   "client_entity_branch":{
      "branch_name":"Branch US",
      "branch_code":"01"
   },
   "client_transactionality":{
      "inc_transactionality":5000.0,
      "out_transactionality":4000.0,
      "funds_country_description":"United States",
      "out_monthly_movements":7,
      "funds_country_code":"US",
      "out_transactionality_currency":"USD",
      "inc_monthly_movements":4,
      "inc_transactionality_currency":"USD"
   },
   "client_location":{
      "rent_currency":"USD",
      "province_description":"Florida",
      "locality_description":"MIAMI",
      "owner_phone":"8295454545",
      "housing_type":"Alquilada",
      "city_description":"Miami",
      "country_description":"United States",
      "rent_amount":2500.0,
      "postal_code":"33101",
      "country_code":"US",
      "address":"123 Main Street",
      "owner":"PETER JETER",
      "locality_code":"33101"
   },
   "client_job_info":{
      "salary":6000.0,
      "professional_position":"Civil Engineer",
      "occupation_code":"172051",
      "currency_code":"USD",
      "entry_date":"2019-08-01",
      "company_name":"COMPANY TEST LLC",
      "company_country_code":"DO",
      "work_address":"5678 Maple Avenue, San Francisco, CA 94101",
      "occupation_description":"Civil Engineers",
      "work_email":"david@business.com",
      "annual_gross_salary":80000.0,
      "net_income":6000.0
   },
   "client_contact":{
      "social_network_username":"david10",
      "cell_phone":"8291230010",
      "email":"aauser10@dev.aclaoverseas.com",
      "social_network_name":"Twitter"
   },
   "client_references":{
      "first_bank_reference":{
         "canceled":1,
         "account_type_description":"No Product",
         "canceled_reason":"inactivity",
         "country_code":"DO",
         "bank_name":"Banco Popular"
      },
      "first_personal_reference":{
         "document_type_code":"IDEN",
         "name":"FRANCISCO CASTILLO",
         "relationship":"Familiar",
         "country":"Dominican Republic",
         "document_number":"15555550000",
         "reference_relationship_unique_description":"brother",
         "phone":"8098877444",
         "address":"3456 Pine Drive, Sacramento, CA 95801",
         "email":"francisco@example.com"
      },
      "second_personal_reference":{
         "document_type_code":"SSN",
         "name":"MARIA PEREZ",
         "relationship":"No Familiar",
         "country":"Estados Unidos",
         "document_number":"110000155",
         "reference_relationship_unique_description":"acquaintance",
         "phone":"7865454444",
         "address":"9012 Oak Boulevard, San Diego, CA 92101",
         "email":"mariap@dev.aclaoverseas.com"
      }
   },
   "client_documents":[
      {
         "document_type_code":"DOPI",
         "document_number":"111000001",
         "main":0,
         "issuing_country_code":"US"
      },
      {
         "document_type_code":"IDAD",
         "document_number":"1000100001",
         "main":0,
         "issuing_country_code":"DO"
      },
      {
         "document_type_code":"PAAD",
         "document_number":"M11515520001",
         "main":0,
         "issuing_country_code":"DO"
      },
      {
         "document_type_code":"SSN",
         "document_number":"100000001",
         "main":1,
         "issuing_country_code":"US"
      }
   ]
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
