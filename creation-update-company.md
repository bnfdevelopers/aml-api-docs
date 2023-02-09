---
cover: >-
  https://images.unsplash.com/photo-1531973576160-7125cd663d86?crop=entropy&cs=tinysrgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHw0fHxjb21wYW55fGVufDB8fHx8MTY3NTk4MzQ4MA&ixlib=rb-4.0.3&q=80
coverY: 0
---

# CREACIÓN / ACTUALIZACIÓN DE CLIENTES JURIDICAS

## Especificaciones

**URL del servicio:**

```bash
https://{URLCLIENTE}/company_core_customer
```

**Método:** POST

**Tipo de Autenticación:** Basic Authentication (Autenticación Básica). El equipo de TI debe proveer las respectivas credenciales de acceso a este servicio.

**Parámetros de Entrada:** Se provee una estructura de datos, bajo el formato JSON. La cual esta constituida de la siguiente forma:

* **company\_info:** Información principal de la compañía tales como: nombre, identificación, actividad económica, entidad donde fue registrada, etc.
* **company\_main\_address:** Información de la ubicación de la sede principal de la empresa.
* **company\_business\_type:** Información sobre el tipo de negocio que realiza la empresa.
* **company\_alternate\_accounts:** Lista de las cuentas en otras entidades que posea la empresa.
* **company\_branches:** Listado de sucursales que posee la empresa. En caso de que tenga solo una sucursal entonces será una lista de un elemento.
* **company\_subsidiaries:** Lista de los principales empresas afiliadas o subsidiarias de la compañía a ser creada.
* **company\_suppliers:** Lista de los principales proveedores de la empresa.
* **company\_main\_customers:** Listado de los principales clientes de la empresa.
* **company\_transactionality:** Información de la transaccionalidad estimada que tendrá la compañía dentro de la entidad.
* **company\_managers:** Lista que contiene la información de los representantes de la alta gerencia. Acá puede estar la información del presidente, directores y gerentes.
* **company\_treasures:** Lista que contiene la información de los responsables de las finanzas de la empresa.
* **company\_entity\_branch:** Información de la sucursal de la entidad donde la empresa se está registrando.
*   **company\_partners:** Nodo que contiene dos valores:

    * **persons\_members:** Lista de la información de los socios (personas físicas) de la empresa.
    * **company\_members:** Si la empresa tiene socios que también son empresas, entonces aparecerá la información de dichas empresas (en una lista) en este nivel. Estas empresas pueden, a su vez, tener socias tanto empresas como personas físicas, por lo que el árbol se empieza expandir dependiendo de la cantidad de socios que agregue el usuario. En el ejemplo más adelante veremos que la compañía principal tiene un sólo socio persona física (person\_members) y dos empresas socias (company\_members) que a su vez, una de ellas tiene una persona física (person\_members) y la otra tiene una empresa como socia y así sucesivamente.

    A continuación la descripción de cada campo:

| NIVEL 1                      | NIVEL 2                                          | NIVEL 3                           | TIPO    | TAMAÑO | DESCRIPCIÓN                                                                                                                                                                                                                           |
| ---------------------------- | ------------------------------------------------ | --------------------------------- | ------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| company\_info                | name\*                                           |                                   | varchar | 250    | Nombre de la empresa                                                                                                                                                                                                                  |
|                              | alternate\_name                                  |                                   | varchar | 250    | Nombre alternativo de la empresas                                                                                                                                                                                                     |
|                              | founding\_date                                   |                                   | date    | -      | Fecha de constitución en formato: YYYY-MM-DD                                                                                                                                                                                          |
|                              | main\_language                                   |                                   | varchar | 100    | Idioma de principal uso. Ejemplo: Español, Inglés, etc                                                                                                                                                                                |
|                              | company\_category\_unique\_name                  |                                   | varchar | 100    | Código único que identifica al tipo de sociedad. Por ejemplo: limited\_liability\_partnership. Ver Anexo 1                                                                                                                            |
|                              | company\_category\_description                   |                                   | text    | -      | Descripción del tipo de sociedad. Por ejemplo: Sociedad de Responsabilidad Limitada (SRL) Ver Anexo 1                                                                                                                                 |
|                              | document\_description                            |                                   | varchar | 30     | Número de identificación de la empresa (RNC, Tax ID, ...)                                                                                                                                                                             |
|                              | document\_issuing\_country\_code                 |                                   | varchar | 2      | País emisor de documento en formato Alpha-2.                                                                                                                                                                                          |
|                              | document\_type\_code                             |                                   | varchar | 20     | Tipo de documento.                                                                                                                                                                                                                    |
|                              | economic\_activity\_code                         |                                   | varchar | 6      | Código CIIU que indica la actividad económica de la empresa.                                                                                                                                                                          |
|                              | economic\_activity\_description                  |                                   | varchar | 250    | Descripción de la actividad económica de la empresa de acuerdo al código CIIU.                                                                                                                                                        |
|                              | registry\_id                                     |                                   | varchar | 50     | Número de registro.                                                                                                                                                                                                                   |
|                              | registering\_entity\_name                        |                                   | varchar | 50     | Nombre de la entidad registradora, por ejemplo: DGII                                                                                                                                                                                  |
|                              | registering\_entity\_country\_code               |                                   | varchar | 2      | Código del país donde se encuentra la entidad registradora en formato Alpha2. Ejemplo: DO.                                                                                                                                            |
|                              | tax\_residence\_country\_code                    |                                   | varchar | 2      | Código del país donde la empresa paga impuestos                                                                                                                                                                                       |
|                              | total\_shares                                    |                                   | float   | -      | Cantidad total de acciones.                                                                                                                                                                                                           |
|                              | additional\_field1                               |                                   | varchar | 150    | Campo opcional para almacenar lo que indique el cliente. Este campo solo se agrega en la estructura si es un valor válido. Es decir, sólo se agregará en el JSON si existe valor, de lo contrario no.                                 |
|                              | additional\_field2                               |                                   | varchar | 150    | Campo opcional para almacenar lo que indique el cliente. Este campo solo se agrega en la estructura si es un valor válido. Es decir, sólo se agregará en el JSON si existe valor, de lo contrario no.                                 |
|                              | additional\_field3                               |                                   | varchar | 150    | Campo opcional para almacenar lo que indique el cliente. Este campo solo se agrega en la estructura si es un valor válido. Es decir, sólo se agregará en el JSON si existe valor, de lo contrario no.                                 |
|                              | product\_request\_type                           |                                   | Object  |        | Diccionario que contiene la información referente al tipo de solicitud que se esta realizando. Los tipos de solicitudes son especificados en la tabla 1.2                                                                             |
|                              |                                                  | description                       | varchar | 50     | Título del tipo de solicitud.                                                                                                                                                                                                         |
|                              |                                                  | code                              | varchar | 1      | Código del tipo de solicitud.                                                                                                                                                                                                         |
| company\_main\_address       | city\_description                                |                                   | varchar | 150    | Ciudad donde se encuentra la sede principal de la empresa.                                                                                                                                                                            |
|                              | neighbourhood\_description                       |                                   | varchar | 150    | Sector donde se encuentra ubicada la sede principal de la empresa.                                                                                                                                                                    |
|                              | province\_code                                   |                                   | varchar | 10     | Código de la provincia. Ver Anexo 1                                                                                                                                                                                                   |
|                              | province\_description                            |                                   | varchar | 150    | Nombre de la provincia.                                                                                                                                                                                                               |
|                              | street                                           |                                   | varchar | 150    | Dirección de la empresa.                                                                                                                                                                                                              |
| company\_alternate\_accounts | bank\_name                                       |                                   | varchar | 50     | Nombre del banco.                                                                                                                                                                                                                     |
|                              | account                                          |                                   | varchar | 50     | Número de cuenta.                                                                                                                                                                                                                     |
|                              | aba\_routing                                     |                                   | varchar | 9      | Número de enrutamiento de ABA.                                                                                                                                                                                                        |
| company\_business\_type      | product\_unique\_description                     |                                   | varchar | 200    | Descripción única del producto o servicio que ofrece la empresa.                                                                                                                                                                      |
|                              | product\_description                             |                                   | varchar | 200    | Descripción del producto o servicio que ofrece la empresa.                                                                                                                                                                            |
|                              | last\_year\_sales\_amount                        |                                   | float   | -      | Monto del total de ventas del año anterior.                                                                                                                                                                                           |
|                              | last\_year\_sales\_amount\_currency\_code        |                                   | varchar | 3      | Código de la moneda del monto de las ventas del año anterior.                                                                                                                                                                         |
|                              | current\_year\_sales\_amount                     |                                   | float   | -      | Monto del total de ventas del año en curso.                                                                                                                                                                                           |
|                              | current\_year\_sales\_currency\_code             |                                   | varchar | 3      | Código de la monedea del monto de las ventas del año en curso.                                                                                                                                                                        |
|                              | cash\_expected\_monthly\_revenue\_amount         |                                   | float   | -      | Monto aproximado a recibir en efectivo.                                                                                                                                                                                               |
|                              | cash\_expected\_monthly\_revenue\_currency\_code |                                   | varchar | 3      | Código de la moneda del efectivo a recibir.                                                                                                                                                                                           |
|                              | employee\_ranges                                 |                                   | varchar | 200    | Rango de la cantidad de empleados de la empresa. Por ejemplo, 1 a 20, 20 a 100, 100 a 500, más de 1000.                                                                                                                               |
| company\_suppliers           | supplier\_description                            |                                   | varchar | 200    | Nombre del proveedor-                                                                                                                                                                                                                 |
|                              | supplier\_country\_code                          |                                   | varchar | 2      | Código del país donde se encuentra el proveedor.                                                                                                                                                                                      |
|                              | supplied\_product\_description                   |                                   | varchar | 2      | Producto o servicio que le ofrece el proveedor.                                                                                                                                                                                       |
| company\_subsidiaries        | subsidiary\_name                                 |                                   | varchar | 200    | Nombre de la empresa afiliada o subsidiaria de la empresa que se está creando.                                                                                                                                                        |
|                              | document\_number                                 |                                   | varchar | 20     | Identificación de la empresa afiliada o subsidiaria.                                                                                                                                                                                  |
|                              | document\_type\_code                             |                                   | varchar | 20     | Tipo de identificación.                                                                                                                                                                                                               |
|                              | issuing\_country\_code                           |                                   | varchar | 2      | Código del país donde fue emitido la identificación.                                                                                                                                                                                  |
|                              | subsidiary\_product\_name                        |                                   | varchar | 200    | Producto o servicio que ofrece dicha empresa afiliada o subsidiaria.                                                                                                                                                                  |
| company\_main\_customers     | main\_customer\_description                      |                                   | varchar | 200    | Nombre del cliente.                                                                                                                                                                                                                   |
|                              | main\_customer\_country\_code                    |                                   | varchar | 2      | Código del país.                                                                                                                                                                                                                      |
|                              | consumed\_product\_description                   |                                   | varchar | 200    | Producto ofrecido a dichos clientes.                                                                                                                                                                                                  |
| company\_transaccionality    | inc\_transactionality                            |                                   | float   | -      | Monto aproximado de la transaccionalidad entrante en el mes.                                                                                                                                                                          |
|                              | inc\_transactionality\_currency\_code            |                                   | varchar | 3      | Código de la moneda de la transaccionalidad entrante.                                                                                                                                                                                 |
|                              | inc\_monthly\_movements                          |                                   | integer | -      | Cantidad de transacciones entrantes.                                                                                                                                                                                                  |
|                              | out\_transactionality                            |                                   | float   | -      | Monto aproximado de la transaccionalidad saliente en el mes.                                                                                                                                                                          |
|                              | out\_transactionality\_currency\_code            |                                   | varchar | 3      | Código de la moneda de la transaccionalidad saliente.                                                                                                                                                                                 |
|                              | out\_monthly\_movements                          |                                   | integer | -      | Cantidad de transacciones salientes.                                                                                                                                                                                                  |
|                              | funds\_country\_code                             |                                   | varchar | 2      | Código del país de origen y destino de los fondos en Alpha2. Ejemplo: DO, IT, CO, etc.                                                                                                                                                |
| company\_branches            | name                                             |                                   | varchar | 100    | Nombre de la sucursal.                                                                                                                                                                                                                |
|                              | document\_type\_code                             |                                   | varchar | 10     | Código del tipo de documento. Ejemplo: RNC.                                                                                                                                                                                           |
|                              | document\_number                                 |                                   | varchar | 100    | Número de identificación de la sucursal.                                                                                                                                                                                              |
|                              | document\_issuing\_country\_code                 |                                   | varchar | 2      | Código del país emisor del documento en Aplha2.                                                                                                                                                                                       |
|                              | phone\_number                                    |                                   | varchar | 20     | Número de teléfono.                                                                                                                                                                                                                   |
|                              | responsible\_name                                |                                   | varchar | 100    | Nombre de una persona responsable de la sucursal.                                                                                                                                                                                     |
|                              | neighbourhood\_description                       |                                   | varchar | 150    | Sector donde se encuentra ubicada la sucursal.                                                                                                                                                                                        |
|                              | description                                      |                                   | varchar | 200    | Dirección donde se encuentra ubicada la sucursal.                                                                                                                                                                                     |
| company\_managers            | first\_name                                      |                                   | varchar | 50     | Primer nombre.                                                                                                                                                                                                                        |
|                              | middle\_name                                     |                                   | varchar | 50     | Segundo nombre en caso de que la persona posea. De lo contrario, no viene esta información.                                                                                                                                           |
|                              | last\_name                                       |                                   | varchar | 50     | Primer apellido.                                                                                                                                                                                                                      |
|                              | middle\_last\_name                               |                                   | varchar | 50     | Segundo apellido en caso de que la persona posea. De lo contrario, no viene esta información.                                                                                                                                         |
|                              | document\_number                                 |                                   | varchar | 20     | Número de documento.                                                                                                                                                                                                                  |
|                              | document\_issuing\_country\_code                 |                                   | varchar | 2      | País emisor de documento en Alpha2.                                                                                                                                                                                                   |
|                              | document\_type\_code                             |                                   | varchar | 10     | Tipo de documento. Ejemplo: CEDU, PASA, etc                                                                                                                                                                                           |
|                              | gender                                           |                                   | varchar | 1      | Género. Ejemplo: M, F.                                                                                                                                                                                                                |
| company\_treasurers          | first\_name                                      |                                   | varchar | 50     | Primer nombre.                                                                                                                                                                                                                        |
|                              | middle\_name                                     |                                   | varchar | 50     | Segundo nombre en caso de que la persona posea. De lo contrario, no viene esta información.                                                                                                                                           |
|                              | last\_name                                       |                                   | varchar | 50     | Primer apellido.                                                                                                                                                                                                                      |
|                              | middle\_last\_name                               |                                   | varchar | 50     | Segundo apellido en caso de que la persona posea. De lo contrario, no viene esta información.                                                                                                                                         |
|                              | document\_number                                 |                                   | varchar | 20     | Número de documento.                                                                                                                                                                                                                  |
|                              | document\_issuing\_country\_code                 |                                   | varchar | 2      | País emisor de documento en Alpha2.                                                                                                                                                                                                   |
|                              | document\_type\_code                             |                                   | varchar | 10     | Tipo de documento. Ejemplo: CEDU, PASA, etc                                                                                                                                                                                           |
|                              | gender                                           |                                   | varchar | 1      | Género. Ejemplo: M, F.                                                                                                                                                                                                                |
| company\_partners            | person\_members                                  | first\_name                       | varchar | 50     | Segundo nombre en caso de que la persona posea. De lo contrario, no viene esta información.                                                                                                                                           |
|                              |                                                  | middle\_name                      | varchar | 50     | Primer apellido.                                                                                                                                                                                                                      |
|                              |                                                  | last\_name                        | varchar | 50     | Segundo apellido en caso de que la persona posea. De lo contrario, no viene esta información.                                                                                                                                         |
|                              |                                                  | middle\_last\_name                | varchar | 20     | Número de documento.                                                                                                                                                                                                                  |
|                              |                                                  | document\_number                  | varchar | 2      | País emisor de documento en Alpha2.                                                                                                                                                                                                   |
|                              |                                                  | document\_issuing\_country\_code  | varchar | 10     | Tipo de documento. Ejemplo: CEDU, PASA, etc                                                                                                                                                                                           |
|                              |                                                  | document\_type\_code              | varchar | 1      | Género. Ejemplo: M, F.                                                                                                                                                                                                                |
|                              |                                                  | gender                            |         |        |                                                                                                                                                                                                                                       |
|                              | company\_members                                 | name                              | varchar | 250    | Nombre de la empresa socia.                                                                                                                                                                                                           |
|                              |                                                  | document\_description             | varchar | 30     | Número de identificación de la empresa socia.                                                                                                                                                                                         |
|                              |                                                  | document\_type\_code              | varchar | 20     | Código del tipo de identificación de la empresa socia.                                                                                                                                                                                |
|                              |                                                  | document\_issuning\_country\_code | varchar | 2      | Código del país emisor del documento en Alpha2.                                                                                                                                                                                       |
|                              |                                                  | address                           | varchar | 150    | Dirección de la empresa socia                                                                                                                                                                                                         |
|                              |                                                  | company\_members                  | json    | -      | En caso de que la empresa tenga como socia otra empresa, se repite el ciclo, de lo contraria será una lista vacía (\[ ]). La profundidad del árbol dependerá de la cantidad de empresas y personas físicas socias ingrese el usuario. |
|                              |                                                  | person\_members                   | json    | -      | En caso de que la empresa tenga como socia otras personas físicas, entonces vendrá esta información, de lo contrario será una lista vacía (\[ ]).                                                                                     |
| company\_entity\_branch      | branch\_code\*                                   |                                   | varchar | 6      | Código de la sucursal de la entidad.                                                                                                                                                                                                  |
|                              | branch\_name\*                                   |                                   | varchar | 35     | Nombre de la sucursal de la entidad.                                                                                                                                                                                                  |

**Nota:** Los marcados con asterisco (\*) son los recomendados a que sean almacenados en el core. Los que no tengan esta marca pueden llegar con el valor de null o vacío (“”).

## Ejemplos

<details>

<summary>Ejemplo 1</summary>

```
{
   "company_info":{
      "additional_field1":"Value1",
      "additional_field2":"Value2",
      "additional_field3":"Value3",
      "company_category_description":"Sociedad publica",
      "company_category_unique_name":"public_society",
      "document_description":"999-99999-9",
      "document_issuing_country_code":"DO",
      "document_type_code":"RNC",
      "economic_activity_code":"552111",
      "economic_activity_description":"Servicios de restaurantes y cantinas sin espectáculos",
      "founding_date":"2021-03-03",
      "main_language":"Español",
      "name":"VASOLY SRL",
      "registering_entity_country_code":"DO",
      "registering_entity_name":"DGII",
      "registry_id":"131683045",
      "tax_residence_country_code":"DO",
      "total_shares":70.0,
      "product_request_type":{
         "description":"Solicitud de Producto",
         "code":"product_request"
      }
   },
   "company_main_address":{
      "city_description":"Santo Domingo",
      "neighbourhood_description":"Sabana Centro ",
      "province_code":"3200",
      "province_description":"Santo Domingo",
      "street":"Av. Caonabo, Calle 8"
   },
   "company_alternate_accounts":[
      {
         "aba_routing":"3333333",
         "account":"123456789",
         "bank_name":"Banco Y"
      }
   ],
   "company_business_type":{
      "cash_expected_monthly_revenue_amount":2700000.0,
      "cash_expected_monthly_revenue_currency_code":"DOP",
      "current_year_sales_amount":3200000.0,
      "current_year_sales_currency_code":"DOP",
      "employees_range":"1 a 20",
      "last_year_sales_amount":3000000.0,
      "last_year_sales_amount_currency_code":"DOP",
      "product_description":"PRODUCTOS VEGETALES DE CULTIVO",
      "product_unique_description":"vegetables"
   },
   "company_suppliers":[
      {
         "supplied_product_description":"PRODUCTOS VEGETALES DE CULTIVO",
         "supplier_country_code":"DO",
         "supplier_description":"Elisabeth Lopez"
      }
   ],
   "company_subsidiaries":[
      {
         "document_number":"888-88888-8",
         "document_type_code":"RNC",
         "issuing_country_code":"DO",
         "subsidiary_name":"VASONEC SUPLLY SRL",
         "subsidiary_product_name":"PRODUCTOS ANIMALES CARNICOS"
      }
   ],
   "company_main_customers":[
      {
         "consumed_product_description":"PRODUCTOS VEGETALES DE CULTIVO",
         "main_customer_country_code":"DO",
         "main_customer_description":"Sara Martinez"
      }
   ],
   "company_transactionality":{
      "inc_monthly_movements":22,
      "inc_transactionality":2890000.0,
      "inc_transactionality_currency_code":"DOP",
      "out_monthly_movements":60,
      "out_transactionality":1870000.0,
      "out_transactionality_currency_code":"DOP",
      "funds_country_code":"DO"
   },
   "company_branches":[
      {
         "description":"Calle los sauces",
         "document_issuing_country_code":"DO",
         "document_number":"7777777777",
         "document_type_code":"RNC",
         "name":"VASOLY SRL",
         "neighbourhood_description":"Sabana Centro ",
         "phone_number":"80972343452",
         "responsible_name":"Elisabeth Martinez"
      },
      {
         "description":"Romulo Betancourt",
         "document_issuing_country_code":"DO",
         "document_number":"666666666",
         "document_type_code":"RNC",
         "name":"Colmado Acla Ovrseas BF",
         "neighbourhood_description":"Mirador Del Este ",
         "phone_number":"948406632",
         "responsible_name":"Andres Morales"
      }
   ],
   "company_managers":[
      {
         "birth_date":"1996-03-01",
         "document_issuing_country_code":"DO",
         "document_number":"1234567890",
         "document_type_code":"CEDU",
         "first_name":"ANDREA",
         "gender":"F",
         "last_name":"ARAUJO"
      }
   ],
   "company_treasurers":[
      {
         "birth_date":"1985-03-01",
         "document_issuing_country_code":"DO",
         "document_number":"05601222411",
         "document_type_code":"CEDU",
         "first_name":"ROSMEIRYS  ",
         "gender":"F",
         "last_name":"TINEO"
      },
      {
         "birth_date":"1996-03-01",
         "document_issuing_country_code":"DO",
         "document_number":"12345678903",
         "document_type_code":"CEDU",
         "first_name":"ANDREA",
         "gender":"F",
         "last_name":"ARAUJO"
      }
   ],
   "company_partners":{
      "company_members":[
         {
            "address":"Calle los santos",
            "company_members":[

            ],
            "document_description":"111-12345-6",
            "document_issuing_country_code":"DO",
            "document_type_code":"RNC",
            "name":"VASOS DOMINICANOS CXA",
            "person_members":[
               {
                  "birth_date":"2021-04-01",
                  "customer_number":14808,
                  "document_issuing_country_code":"DO",
                  "document_number":"12312312312",
                  "document_type_code":"CEDU",
                  "first_name":"PABLO",
                  "gender":"M",
                  "last_name":"VIERA"
               }
            ]
         },
         {
            "address":"Calle 2",
            "company_members":[
               {
                  "address":"Calle las juanas",
                  "company_members":[
                     {
                        "address":"av los soles",
                        "company_members":[

                        ],
                        "document_description":"123-11111-1",
                        "document_issuing_country_code":"DO",
                        "document_type_code":"RNC",
                        "name":"VASORI MAKE UP SRL",
                        "person_members":[

                        ]
                     }
                  ],
                  "document_description":"444-33333-2",
                  "document_issuing_country_code":"DO",
                  "document_type_code":"RNC",
                  "name":"VASOS DE BARRO",
                  "person_members":[
                     {
                        "birth_date":"2021-04-01",
                        "customer_number":14808,
                        "document_issuing_country_code":"DO",
                        "document_number":"12312312312",
                        "document_type_code":"CEDU",
                        "first_name":"PABLO",
                        "gender":"M",
                        "last_name":"VIERA"
                     }
                  ]
               }
            ],
            "document_description":"321-54321-1",
            "document_issuing_country_code":"DO",
            "document_type_code":"RNC",
            "name":"VASOS Y PLATOS CXA",
            "person_members":[

            ]
         }
      ],
      "person_members":[
         {
            "birth_date":"1995-04-01",
            "document_issuing_country_code":"VE",
            "document_number":"045444444",
            "document_type_code":"PASA",
            "first_name":"ANDRES",
            "gender":"M",
            "last_name":"MORALES"
         }
      ]
   },
   "company_entity_branch":{
      "branch_code":"01",
      "branch_name":"NOMBRE SUCURSAL"
   }
}
```

</details>

<details>

<summary>Ejemplo 2</summary>

```
{
   "company_branches":[
      {
         "description":"calle prueba, #1000",
         "document_issuing_country_code":"DO",
         "document_number":"101007712",
         "document_type_code":"RNC",
         "name":"RAFAEL TAVAREZ & CO, SRL",
         "neighbourhood_description":"30 De Mayo ",
         "phone_number":"8097081365",
         "responsible_name":"NIRCIA MERCEDES DEL ROSARIO PEREZ"
      }
   ],
   "company_business_type":{
      "cash_expected_monthly_revenue_amount":500000.0,
      "cash_expected_monthly_revenue_currency_code":"DOP",
      "current_year_sales_amount":12000000.0,
      "current_year_sales_currency_code":"DOP",
      "employees_range":"1 a 20",
      "last_year_sales_amount":20000000.0,
      "last_year_sales_amount_currency_code":"DOP",
      "product_description":"PRODUCTOS VEGETALES DE CULTIVO",
      "product_unique_description":"vegetables"
   },
   "company_entity_branch":{
      "branch_code":"2",
      "branch_name":"Yamasa"
   },
   "company_info":{
      "acronym":"TRANSE",
      "company_category_description":"Sociedad de Responsabilidad Limitada (SRL)",
      "company_category_unique_name":"limited_liability_partnership",
      "document_description":"101711035",
      "document_issuing_country_code":"DO",
      "document_type_code":"RNC",
      "economic_activity_code":"602100",
      "economic_activity_description":"Servicios de transporte automotor",
      "founding_date":"1996-04-22",
      "is_financial_entity":false,
      "is_private_entity":true,
      "main_language":"Español",
      "name":"TRANSPORTE EMMANUEL SRL",
      "registering_entity_country_code":"DO",
      "registering_entity_name":"Dirección General de Impuestos Internos",
      "registry_id":"12345SD",
      "tax_residence_country_code":"DO",
      "total_shares":0.0,
      "product_request_type":{
         "description":"Solicitud de Producto",
         "code":"product_request"
      }
   },
   "company_main_address":{
      "city_description":"Santo Domingo",
      "neighbourhood_description":"Alcarrizos Viejo",
      "province_code":"3200",
      "province_description":"Santo Domingo",
      "street":"calle prueba nueva, #200"
   },
   "company_main_customers":[
      {
         "consumed_product_description":"PRODUCTOS VEGETALES DE CULTIVO",
         "main_customer_country_code":"DO",
         "main_customer_description":"JOSE MARIA ALMONTE GONZALEZ"
      }
   ],
   "company_managers":[
      {
         "birth_date":"1975-01-04",
         "document_issuing_country_code":"DO",
         "document_number":"07600125962",
         "document_type_code":"CEDU",
         "first_name":"PASIRIS",
         "gender":"F",
         "last_name":"REYES",
         "middle_last_name":"REYES"
      },
      {
         "birth_date":"1958-05-11",
         "document_issuing_country_code":"DO",
         "document_number":"00200141877",
         "document_type_code":"CEDU",
         "first_name":"OBELIO",
         "gender":"M",
         "last_name":"DE LOS SANTOS",
         "middle_last_name":"DE LOS SANTOS"
      }
   ],
   "company_partners":{
      "company_members":[

      ],
      "person_members":[
         {
            "birth_date":"1961-11-20",
            "document_issuing_country_code":"DO",
            "document_number":"00200190593",
            "document_type_code":"CEDU",
            "first_name":"SINENCIO",
            "gender":"M",
            "last_name":"RODRIGUEZ"
         }
      ]
   },
   "company_subsidiaries":[
      {
         "document_number":"101007672",
         "document_type_code":"RNC",
         "issuing_country_code":"DO",
         "subsidiary_name":"R SIRAGUSA SRL",
         "subsidiary_product_name":"PRODUCTOS VEGETALES DE CULTIVO"
      }
   ],
   "company_suppliers":[
      {
         "supplier_country_code":"DO",
         "supplier_description":"TENEDORA LIBERAL S R L"
      }
   ],
   "company_transactionality":{
      "funds_country_code":"DO",
      "inc_monthly_movements":25,
      "inc_transactionality":800000.0,
      "inc_transactionality_currency_code":"DOP",
      "out_monthly_movements":60,
      "out_transactionality":300000.0,
      "out_transactionality_currency_code":"DOP"
   },
   "company_treasurers":[
      {
         "birth_date":"1951-08-30",
         "document_issuing_country_code":"DO",
         "document_number":"00200189314",
         "document_type_code":"CEDU",
         "first_name":"LEONIDAS",
         "gender":"M",
         "last_name":"NOVA",
         "middle_last_name":"VALDEZ"
      }
   ]
}
```

</details>

<details>

<summary>Ejemplo 3</summary>

```
{
   "company_branches":[
      {
         "description":"calle azul y morado",
         "document_issuing_country_code":"DO",
         "document_number":"101563281",
         "document_type_code":"RNC",
         "name":"INMOBILIARIA PARADISO SRL",
         "neighbourhood_description":"Alcarrizos Viejo",
         "phone_number":"8097081365",
         "responsible_name":"VICTOR HIPOLITO SANCHEZ FELIZ"
      }
   ],
   "company_business_type":{
      "cash_expected_monthly_revenue_amount":500000.0,
      "cash_expected_monthly_revenue_currency_code":"DOP",
      "current_year_sales_amount":1900000.0,
      "current_year_sales_currency_code":"DOP",
      "employees_range":"1 a 20",
      "last_year_sales_amount":5000000.0,
      "last_year_sales_amount_currency_code":"DOP",
      "product_description":"PRODUCTOS VEGETALES DE CULTIVO",
      "product_unique_description":"vegetables"
   },
   "company_entity_branch":{
      "branch_code":"2",
      "branch_name":"Yamasa"
   },
   "company_info":{
      "acronym":"inmo",
      "company_category_description":"Sociedad de Responsabilidad Limitada (SRL)",
      "company_category_unique_name":"limited_liability_partnership",
      "document_description":"101563281",
      "document_issuing_country_code":"DO",
      "document_type_code":"RNC",
      "economic_activity_code":"702001",
      "economic_activity_description":"Servicios inmobiliarios realizados a cambio de una retribución o por contrata (incluye compra, venta, alquiler, reate, tasación)",
      "founding_date":"1991-02-26",
      "is_financial_entity":false,
      "is_private_entity":true,
      "main_language":"Español",
      "name":"INMOBILIARIA PARADISO SRL",
      "registering_entity_country_code":"DO",
      "registering_entity_name":"Dirección General de Impuestos Internos",
      "registry_id":"12345SD",
      "tax_residence_country_code":"DO",
      "total_shares":0.0,
      "url":"www.inmobiliaria.com",
      "product_request_type":{
         "description":"Solicitud de Producto",
         "code":"product_request"
      }
   },
   "company_main_address":{
      "city_description":"San Cristobal",
      "neighbourhood_description":"5 De Abril",
      "province_code":"2100",
      "province_description":"San Cristobal",
      "street":"calle prueba azul, #100"
   },
   "company_main_customers":[
      {
         "consumed_product_description":"PRODUCTOS VEGETALES DE CULTIVO",
         "main_customer_country_code":"DO",
         "main_customer_description":"TEOFILO ROSARIO MARTINEZ"
      }
   ],
   "company_managers":[
      {
         "birth_date":"1957-09-30",
         "document_issuing_country_code":"DO",
         "document_number":"00500432463",
         "document_type_code":"CEDU",
         "first_name":"MARIA",
         "gender":"F",
         "last_name":"TEJADA",
         "middle_last_name":"HERNANDEZ",
         "middle_name":"ALTAGRACIA"
      },
      {
         "birth_date":"1956-04-11",
         "document_issuing_country_code":"DO",
         "document_number":"00105030092",
         "document_type_code":"CEDU",
         "first_name":"LUIS",
         "gender":"M",
         "last_name":"FERRY",
         "middle_last_name":"ALMODOVAR",
         "middle_name":"ARTURO"
      }
   ],
   "company_partners":{
      "company_members":[

      ],
      "person_members":[
         {
            "birth_date":"1972-10-07",
            "document_issuing_country_code":"DO",
            "document_number":"12100101901",
            "document_type_code":"CEDU",
            "first_name":"WELINGTON",
            "gender":"F",
            "last_name":"CABRERA",
            "middle_last_name":"MERCEDES"
         }
      ]
   },
   "company_subsidiaries":[
      {
         "document_number":"101563281",
         "document_type_code":"RNC",
         "issuing_country_code":"DO",
         "subsidiary_name":"INMOBILIARIA PARADISO SRL",
         "subsidiary_product_name":"PRODUCTOS VEGETALES DE CULTIVO"
      }
   ],
   "company_suppliers":[
      {
         "supplier_country_code":"DO",
         "supplier_description":"JOWIS INTERNACIONAL C X A"
      }
   ],
   "company_transactionality":{
      "funds_country_code":"DO",
      "inc_monthly_movements":20,
      "inc_transactionality":250000.0,
      "inc_transactionality_currency_code":"DOP",
      "out_monthly_movements":26,
      "out_transactionality":175000.0,
      "out_transactionality_currency_code":"DOP"
   },
   "company_treasurers":[
      {
         "birth_date":"1961-02-12",
         "document_issuing_country_code":"DO",
         "document_number":"00102483146",
         "document_type_code":"CEDU",
         "first_name":"BOLIVAR",
         "gender":"M",
         "last_name":"MENA",
         "middle_last_name":"LOZANO",
         "middle_name":"GENARO"
      },
      {
         "birth_date":"1954-01-29",
         "document_issuing_country_code":"DO",
         "document_number":"00105089536",
         "document_type_code":"CEDU",
         "first_name":"JUAN",
         "gender":"M",
         "last_name":"ROJAS",
         "middle_last_name":"GUERRERO",
         "middle_name":"FRANCISCO"
      }
   ]
}
```

</details>

## Respuestas por parte de la entidad

La entidad deberá retornar, en formato JSON, el número de cliente con los siguientes http status codes:

* **201** - Respuesta exitosa, cliente creado
* **400** - Error creando el cliente. Deberá retornar el mensaje del error para ser mostrado en la aplicación.

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

<summary>Ejemplo 2 - Caso error (HTTP STATUS CODE 400)</summary>

```
{
	"message": "MENSAJE DEL MOTIVO DEL ERROR"
}
```

</details>
