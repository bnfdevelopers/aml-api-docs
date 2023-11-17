---
cover: >-
  https://images.unsplash.com/photo-1570126618953-d437176e8c79?crop=entropy&cs=tinysrgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHwxfHxjb21wYW55fGVufDB8fHx8MTY3NTk4MzQ4MA&ixlib=rb-4.0.3&q=80
coverY: -282
---

# Creación / Actualización de Clientes Jurídicos

## Especificaciones

**URL del servicio:**

```bash
https://{URLCLIENTE}/company_core_customer
```

**Método:** POST

**Tipo de Autenticación:** Basic Authentication (Autenticación Básica). El equipo de TI debe proveer las respectivas credenciales de acceso a este servicio.

## **Parámetros de Entrada**

Se provee una estructura de datos, bajo el formato JSON. La cual esta constituida de la siguiente forma:

* **company\_info:** Información principal de la compañía tales como: nombre, identificación, actividad económica, entidad donde fue registrada, etc.
* **company\_contact:** Información de datos de contacto de la empresa, tales como: teléfono y correo electrónico.
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
* **company\_partners:** Nodo que contiene dos valores:
  * **persons\_members:** Lista de la información de los socios (personas físicas) de la empresa.
  * **company\_members:** Si la empresa tiene socios que también son empresas, entonces aparecerá la información de dichas empresas (en una lista) en este nivel. Estas empresas pueden, a su vez, tener socias tanto empresas como personas físicas, por lo que el árbol se empieza expandir dependiendo de la cantidad de socios que agregue el usuario. En el ejemplo más adelante veremos que la compañía principal tiene un sólo socio persona física (person\_members) y dos empresas socias (company\_members) que a su vez, una de ellas tiene una persona física (person\_members) y la otra tiene una empresa como socia y así sucesivamente.

> :warning: **IMPORTANTE:**  Recomendamos que aquellos campos que no son marcados con un asterisco (\*), se valide primero si ese campo y/o objeto es enviado en el json o no. Esto porque puede haber casos en que dicho campo y/o objeto viaje en el json si hay algún valor, de lo contrario no aparecerá.

A continuación la descripción de cada campo:

| NIVEL 1                      | NIVEL 2                                          | NIVEL 3                           | TIPO    | TAMAÑO | DESCRIPCIÓN                                                                                                                                                                                                                           |
| ---------------------------- | ------------------------------------------------ | --------------------------------- | ------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| company\_info                | name\*                                           |                                   | varchar | 250    | Nombre de la empresa                                                                                                                                                                                                                  |
|                              | alternate\_name                                  |                                   | varchar | 250    | Nombre alternativo de la empresas                                                                                                                                                                                                     |
|                              | acronym                                          |                                   | varchar | 100    | Acrónimo de la empresa                                                                                                                                                                                                                |
|                              | founding\_date\*                                 |                                   | date    | -      | Fecha de constitución en formato: YYYY-MM-DD                                                                                                                                                                                          |
|                              | customer\_number                                 |                                   | integer | -      | Número de cliente asignado por el core de la entidad. Para clientes nuevos este valor no es enviada en el Json                                                                                                                                                                   |
|                              | code                                             |                                   | varchar | 6      | Código de la compañía asignado por la plataforma                                                                                                                                                                                      |
|                              | main\_language                                   |                                   | varchar | 100    | Idioma de principal uso. Ejemplo: Español, Inglés, etc                                                                                                                                                                                |
|                              | url                                              |                                   | varchar | 200    | Página web de la empresa                                                                                                                                                                                                              |
|                              | company\_category\_unique\_name\*                |                                   | varchar | 100    | Código único que identifica al tipo de sociedad. Por ejemplo: limited\_liability\_partnership. Ver Anexos [aquí](static-data.md)                                                                                                      |
|                              | company\_category\_description\*                 |                                   | text    | -      | Descripción del tipo de sociedad. Por ejemplo: Sociedad de Responsabilidad Limitada (SRL)                                                                                                                                             |
|                              | document\_description\*                          |                                   | varchar | 30     | Número de identificación de la empresa (TXID, Tax ID, ...)                                                                                                                                                                            |
|                              | document\_issuing\_country\_code\*               |                                   | varchar | 2      | País emisor de documento en formato Alpha-2.                                                                                                                                                                                          |
|                              | document\_type\_code\*                           |                                   | varchar | 20     | Tipo de documento.                                                                                                                                                                                                                    |
|                              | economic\_activity\_code\*                       |                                   | varchar | 6      | Código de la actividad económica de la empresa. Para el caso de República Dominicana serán los códigos CIIU. Para USA serán los NAICs                                                                                                 |
|                              | economic\_activity\_description\*                |                                   | varchar | 250    | Descripción de la actividad económica de la empresa de acuerdo al código anterio.                                                                                                                                                     |
|                              | registry\_id\*                                   |                                   | varchar | 50     | Número de registro.                                                                                                                                                                                                                   |
|                              | registering\_entity\_name\*                      |                                   | varchar | 50     | Nombre de la entidad registradora, por ejemplo: DGII                                                                                                                                                                                  |
|                              | registering\_entity\_country\_code\*             |                                   | varchar | 2      | Código del país donde se encuentra la entidad registradora en formato Alpha2. Ejemplo: DO.                                                                                                                                            |
|                              | tax\_residence\_country\_code\*                  |                                   | varchar | 2      | Código del país donde la empresa paga impuestos                                                                                                                                                                                       |
|                              | total\_shares\*                                  |                                   | float   | -      | Cantidad total de acciones.                                                                                                                                                                                                           |
|                              | is\_private\_entity                              |                                   | Boolean | -      | Indica si la entidad es privada o no (true: Es privada, false: No es privada)                                                                                                                                                         |
|                              | is\_financial\_entity                            |                                   | Boolean | -      | Indica si es una entidad financiera o no (true: Es financiera, false: No es financiera)                                                                                                                                               |
|                              | additional\_field1                               |                                   | varchar | 150    | Campo opcional para almacenar lo que indique el cliente. Este campo solo se agrega en la estructura si es un valor válido. Es decir, sólo se agregará en el JSON si existe valor, de lo contrario no.                                 |
|                              | additional\_field2                               |                                   | varchar | 150    | Campo opcional para almacenar lo que indique el cliente. Este campo solo se agrega en la estructura si es un valor válido. Es decir, sólo se agregará en el JSON si existe valor, de lo contrario no.                                 |
|                              | additional\_field3                               |                                   | varchar | 150    | Campo opcional para almacenar lo que indique el cliente. Este campo solo se agrega en la estructura si es un valor válido. Es decir, sólo se agregará en el JSON si existe valor, de lo contrario no.                                 |
|                              | product\_request\_type                           |                                   | Object  |        | Diccionario que contiene la información referente al tipo de solicitud que se esta realizando. Ver Anexos [aquí](static-data.md)                                                                                                      |
|                              |                                                  | description                       | varchar | 50     | Descripción del tipo de solicitud.                                                                                                                                                                                                    |
|                              |                                                  | code                              | varchar | 1      | Código del tipo de solicitud.                                                                                                                                                                                                         |
| company\_contact             | contact_cell_phone                               |                                   | varchar | 30     | Número de teléfono de contacto celular de la empresa.                                                                                                                                                                                 |
|                              | phone\_country\_code                             |                                   | varchar | 2      | País del número de teléfono de contacto celular.                                                                                                                                                                                      |
|                              | phone\_international\_code                       |                                   | varchar | 5      | Código de llamada internacional, ejemplo: +1.                                                                                                                                                                                         |
|                              | main\_email                                      |                                   | varchar | 60     | Correo de la empresa                                                                                                                                                                                                                  |
| company\_main\_address       | city\_description\*                              |                                   | varchar | 150    | Ciudad donde se encuentra la sede principal de la empresa.                                                                                                                                                                            |
|                              | country\_code\*                                  |                                   | varchar | 2      | Código del país                                                                                                                                                                                                                       |
|                              | country\_name                                    |                                   | varchar | 50     | Nombre del país                                                                                                                                                                                                                       |
|                              | neighbourhood\_description                       |                                   | varchar | 150    | Sector donde se encuentra ubicada la sede principal de la empresa.                                                                                                                                                                    |
|                              | province\_code                                   |                                   | varchar | 10     | Código de la provincia/estado. Ver Anexos [aquí](static-data.md)                                                                                                                                                                      |
|                              | province\_description                            |                                   | varchar | 150    | Nombre de la provincia/estado.                                                                                                                                                                                                        |
|                              | street\*                                         |                                   | varchar | 150    | Dirección de la empresa.                                                                                                                                                                                                              |
|                              | company\_properties\_type\*                      |                                   | varchar | 50     | Toma los valores: rented (si la propiedad es alquilada), owned (si es propia)                                                                                                                                                         |
|                              | company\_properties\_owner\*                     |                                   | varchar | 100    | Nombre completo del dueño de la propiedad cuando la misma es alquilada                                                                                                                                                                |
|                              | company\_properties\_owner\_phone\*              |                                   | varchar | 30     | Teléfono de contacto de la propiedad cuando la misma es alquilada                                                                                                                                                                     |
|                              | company\_properties\_rent\_amount\*              |                                   | float   | -      | Monto del alquiler cuando la propiedad es alquilada                                                                                                                                                                                   |
|                              | postal\_code\*                                   |                                   | varchar | 10     | Código postal                                                                                                                                                                                                                         |
|                              | locality\_code\*                                 |                                   | varchar | 15     | Código de la localidad. Para el caso de USA es el mismo postal code                                                                                                                                                                   |
|                              | locality\_description\*                          |                                   | varchar | 70     | Nombre de la localidad                                                                                                                                                                                                                |
| company\_alternate\_accounts | bank\_name                                       |                                   | varchar | 50     | Nombre del banco                                                                                                                                                                                                                      |
|                              | account                                          |                                   | varchar | 50     | Número de cuenta                                                                                                                                                                                                                      |
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
| company\_suppliers           | supplier\_description                            |                                   | varchar | 200    | Nombre del proveedor                                                                                                                                                                                                                  |
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
| company\_transactionality    | inc\_transactionality\*                          |                                   | float   | -      | Monto aproximado de la transaccionalidad entrante en el mes.                                                                                                                                                                          |
|                              | inc\_transactionality\_currency\_code\*          |                                   | varchar | 3      | Código de la moneda de la transaccionalidad entrante.                                                                                                                                                                                 |
|                              | inc\_monthly\_movements\*                        |                                   | integer | -      | Cantidad de transacciones entrantes.                                                                                                                                                                                                  |
|                              | out\_transactionality\*                          |                                   | float   | -      | Monto aproximado de la transaccionalidad saliente en el mes.                                                                                                                                                                          |
|                              | out\_transactionality\_currency\_code\*          |                                   | varchar | 3      | Código de la moneda de la transaccionalidad saliente.                                                                                                                                                                                 |
|                              | out\_monthly\_movements\*                        |                                   | integer | -      | Cantidad de transacciones salientes.                                                                                                                                                                                                  |
|                              | funds\_country\_code\*                           |                                   | varchar | 2      | Código del país de origen y destino de los fondos en Alpha2. Ejemplo: DO, IT, CO, etc.                                                                                                                                                |
| company\_branches            | name\*                                           |                                   | varchar | 100    | Nombre de la sucursal.                                                                                                                                                                                                                |
|                              | document\_type\_code\*                           |                                   | varchar | 10     | Código del tipo de documento. Ejemplo: TXID.                                                                                                                                                                                          |
|                              | document\_number\*                               |                                   | varchar | 100    | Número de identificación de la sucursal.                                                                                                                                                                                              |
|                              | document\_issuing\_country\_code\*               |                                   | varchar | 2      | Código del país emisor del documento en Aplha2.                                                                                                                                                                                       |
|                              | phone\_number\*                                  |                                   | varchar | 20     | Número de teléfono.                                                                                                                                                                                                                   |
|                              | extension                                        |                                   | integer | -      | Número de extensión.                                                                                                                                                                                                                  |
|                              | responsible\_name\*                              |                                   | varchar | 100    | Nombre de una persona responsable de la sucursal.                                                                                                                                                                                     |
|                              | neighbourhood\_description                       |                                   | varchar | 150    | Sector donde se encuentra ubicada la sucursal.                                                                                                                                                                                        |
|                              | description\*                                    |                                   | varchar | 200    | Dirección donde se encuentra ubicada la sucursal.                                                                                                                                                                                     |
| company\_managers            | first\_name                                      |                                   | varchar | 50     | Primer nombre.                                                                                                                                                                                                                        |
|                              | middle\_name                                     |                                   | varchar | 50     | Segundo nombre en caso de que la persona posea. De lo contrario, no viene esta información.                                                                                                                                           |
|                              | last\_name                                       |                                   | varchar | 50     | Primer apellido.                                                                                                                                                                                                                      |
|                              | middle\_last\_name                               |                                   | varchar | 50     | Segundo apellido en caso de que la persona posea. De lo contrario, no viene esta información.                                                                                                                                         |
|                              | document\_number                                 |                                   | varchar | 20     | Número de documento.                                                                                                                                                                                                                  |
|                              | document\_issuing\_country\_code                 |                                   | varchar | 2      | País emisor de documento en Alpha2.                                                                                                                                                                                                   |
|                              | document\_type\_code                             |                                   | varchar | 10     | Tipo de documento. Ejemplo: ide, PAOR, etc                                                                                                                                                                                            |
|                              | gender                                           |                                   | varchar | 1      | Género. Ejemplo: M, F, NB, NC, O.                                                                                                                                                                                                     |
|                              | birth\_date                                      |                                   | date    | -      | Fecha de Nacimiento. Formato YYYY-MM-DD                                                                                                                                                                                               |
|                              | email                                            |                                   | varchar | 50     | Correo electrónico                                                                                                                                                                                                                    |
| company\_treasurers          | first\_name                                      |                                   | varchar | 50     | Primer nombre.                                                                                                                                                                                                                        |
|                              | middle\_name                                     |                                   | varchar | 50     | Segundo nombre en caso de que la persona posea. De lo contrario, no viene esta información.                                                                                                                                           |
|                              | last\_name                                       |                                   | varchar | 50     | Primer apellido.                                                                                                                                                                                                                      |
|                              | middle\_last\_name                               |                                   | varchar | 50     | Segundo apellido en caso de que la persona posea. De lo contrario, no viene esta información.                                                                                                                                         |
|                              | document\_number                                 |                                   | varchar | 20     | Número de documento.                                                                                                                                                                                                                  |
|                              | document\_issuing\_country\_code                 |                                   | varchar | 2      | País emisor de documento en Alpha2.                                                                                                                                                                                                   |
|                              | document\_type\_code                             |                                   | varchar | 10     | Tipo de documento. Ejemplo: IDEN, PAOR, etc                                                                                                                                                                                           |
|                              | gender                                           |                                   | varchar | 1      | Género. Ejemplo: M, F.                                                                                                                                                                                                                |
|                              | birth\_date                                      |                                   | date    | -      | Fecha de Nacimiento. Formato YYYY-MM-DD                                                                                                                                                                                               |
|                              | email                                            |                                   | varchar | 50     | Correo electrónico                                                                                                                                                                                                                    |
| company\_partners            | person\_members                                  | first\_name                       | varchar | 50     | Primer nombre.                                                                                                                                                                                                                        |
|                              |                                                  | middle\_name                      | varchar | 50     | Segundo nombre en caso de que la persona posea. De lo contrario, no viene esta información.                                                                                                                                           |
|                              |                                                  | last\_name                        | varchar | 50     | Primer apellido.                                                                                                                                                                                                                      |
|                              |                                                  | middle\_last\_name                | varchar | 50     | Segundo apellido en caso de que la persona posea. De lo contrario, no viene esta información.                                                                                                                                         |
|                              |                                                  | document\_number                  | varchar | 20     | Número de documento.                                                                                                                                                                                                                  |
|                              |                                                  | document\_issuing\_country\_code  | varchar | 2      | País emisor de documento en Alpha2.                                                                                                                                                                                                   |
|                              |                                                  | document\_type\_code              | varchar | 10     | Tipo de documento. Ejemplo: IDEN, PAOR, etc                                                                                                                                                                                           |
|                              |                                                  | gender                            | varchar | 1      | Género. Ejemplo: M, F.                                                                                                                                                                                                                |
|                              |                                                  | birth\_date                       | date    | -      | Fecha de Nacimiento. Formato YYYY-MM-DD                                                                                                                                                                                               |
|                              |                                                  | email                             | varchar | 50     | Correo electrónico                                                                                                                                                                                                                    |
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
      "document_type_code":"TXID",
      "economic_activity_code":"552111",
      "economic_activity_description":"Servicios de restaurantes y cantinas sin espectáculos",
      "founding_date":"2021-03-03",
      "main_language":"Español",
      "name":"VASOLY SRL",
      "code":"2GAf13",
      "url": "vasoly.com",
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
      "locality_description":"010101",
      "locality_code":"Santo Domingo de Guzman",
      "street":"Av. Caonabo, Calle 8",
      "country_code":"DO",
      "postal_code":"11001",
      "company_properties_owner": "REGINALD SCOTT",
      "company_properties_owner_phone": "13697741025",
      "company_properties_rent_amount": "3000.00",
      "company_properties_type": "rented",
   },
   "company_contact":{
      "contact_cell_phone":"7861234567",
      "phone_country_code":"US",
      "phone_international_code":"+1",
      "main_email":"vasoly@example.com"
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
         "document_type_code":"TXID",
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
         "document_type_code":"TXID",
         "name":"VASOLY SRL",
         "neighbourhood_description":"Sabana Centro ",
         "phone_number":"80972343452",
         "extension":123456,
         "responsible_name":"Elisabeth Martinez"
      },
      {
         "description":"Romulo Betancourt",
         "document_issuing_country_code":"DO",
         "document_number":"666666666",
         "document_type_code":"TXID",
         "name":"Colmado Acla Ovrseas BF",
         "neighbourhood_description":"Mirador Del Este ",
         "phone_number":"948406632",
         "extension":789852,
         "responsible_name":"Andres Morales"
      }
   ],
   "company_managers":[
      {
         "birth_date":"1996-03-01",
         "document_issuing_country_code":"DO",
         "document_number":"1234567890",
         "document_type_code":"IDEN",
         "first_name":"ANDREA",
         "gender":"F",
         "last_name":"ARAUJO",
         "email":"migromero@example.com"
      }
   ],
   "company_treasurers":[
      {
         "birth_date":"1985-03-01",
         "document_issuing_country_code":"DO",
         "document_number":"05601222411",
         "document_type_code":"IDEN",
         "first_name":"ROSMEIRYS  ",
         "gender":"F",
         "last_name":"TINEO",
         "email":"rosmeirys@example.com"
      },
      {
         "birth_date":"1996-03-01",
         "document_issuing_country_code":"DO",
         "document_number":"12345678903",
         "document_type_code":"IDEN",
         "first_name":"ANDREA",
         "gender":"F",
         "last_name":"ARAUJO",
         "email":"andrea@example.com"
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
            "document_type_code":"TXID",
            "name":"VASOS DOMINICANOS CXA",
            "person_members":[
               {
                  "birth_date":"2021-04-01",
                  "customer_number":14808,
                  "document_issuing_country_code":"DO",
                  "document_number":"12312312312",
                  "document_type_code":"IDEN",
                  "first_name":"PABLO",
                  "gender":"M",
                  "last_name":"VIERA",
                  "email":"andrea@example.com"
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
                        "document_type_code":"TXID",
                        "name":"VASORI MAKE UP SRL",
                        "person_members":[

                        ]
                     }
                  ],
                  "document_description":"444-33333-2",
                  "document_issuing_country_code":"DO",
                  "document_type_code":"TXID",
                  "name":"VASOS DE BARRO",
                  "person_members":[
                     {
                        "birth_date":"2021-04-01",
                        "customer_number":14808,
                        "document_issuing_country_code":"DO",
                        "document_number":"12312312312",
                        "document_type_code":"IDEN",
                        "first_name":"PABLO",
                        "gender":"M",
                        "last_name":"VIERA"
                     }
                  ]
               }
            ],
            "document_description":"321-54321-1",
            "document_issuing_country_code":"DO",
            "document_type_code":"TXID",
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
            "document_type_code":"PAOR",
            "first_name":"ANDRES",
            "gender":"M",
            "last_name":"MORALES",
            "email":"andres@example.com"
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
         "description":"175 sw miami ave",
         "document_issuing_country_code":"US",
         "document_number":"1115555",
         "document_type_code":"TXID",
         "name":"Property Company",
         "phone_number":"7869987744",
         "extension":123456,
         "responsible_name":"Ramon Marquez"
      }
   ],
   "company_business_type":{
      "cash_expected_monthly_revenue_amount":500000.0,
      "cash_expected_monthly_revenue_currency_code":"USD",
      "current_year_sales_amount":1000000.0,
      "current_year_sales_currency_code":"USD",
      "employees_range":"20 a 100",
      "last_year_sales_amount":2500000.0,
      "last_year_sales_amount_currency_code":"USD",
      "product_description":"Real Estate and Rental and Leasing",
      "product_unique_description":"real_estate"
   },
   "company_entity_branch":{
      "branch_code":"01",
      "branch_name":"Miami"
   },
   "company_info":{
      "account_officer":"15",
      "company_category_description":"Sociedad de Responsabilidad Limitada (SRL)",
      "company_category_unique_name":"limited_liability_partnership",
      "document_description":"1115555",
      "document_issuing_country_code":"US",
      "document_type_code":"TXID",
      "economic_activity_code":"531390",
      "economic_activity_description":"Other Activities Related to Real Estate                                                                                                                         ",
      "founding_date":"2009-03-18",
      "main_language":"Ingles",
      "url": "propertycorp.com",
      "name":"Property Company",
      "code":"BG3h43",
      "is_financial_entity":false,
      "is_private_entity":true,
      "product_request_type":{
         "code":"product_request",
         "description":"Solicitud de Producto"
      },
      "registering_entity_country_code":"DO",
      "registering_entity_name":"Dirección General de Impuestos Internos",
      "registry_id":"122121121",
      "tax_residence_country_code":"US",
      "total_shares":0.0,
      "url":"propertycomp.com"
   },
   "company_main_address":{
      "city_description":"Miami",
      "company_properties_type":"owned",
      "province_description":"Florida",
      "street":"175 sw miami ave",
      "locality_description":"MIAMI",
      "locality_code":"33130",
      "postal_code":"33130",
   },
   "company_contact":{
      "contact_cell_phone":"8491234455",
      "phone_country_code":"DO",
      "phone_international_code":"+1",
      "main_email":"property@example.com"
   },
   "company_main_customers":[
      {
         "consumed_product_description":"Real Estate and Rental and Leasing",
         "main_customer_country_code":"US",
         "main_customer_description":"ACB Company SRL"
      }
   ],
   "company_managers":[
      {
         "birth_date":"1971-03-11",
         "document_issuing_country_code":"US",
         "document_number":"111110011",
         "document_type_code":"SSN",
         "email":"luisma@dev.com",
         "first_name":"LUIS",
         "gender":"M",
         "last_name":"ROMERO",
         "email":"luis@example.com"
      }
   ],
   "company_partners":{
      "company_members":[
         
      ],
      "person_members":[
         {
            "birth_date":"1977-11-10",
            "document_issuing_country_code":"US",
            "document_number":"11112200",
            "document_type_code":"SSN",
            "email":"danielal@dev.com",
            "first_name":"DANIEL",
            "gender":"M",
            "last_name":"PEREZ",
            "middle_name":"ALEJANDRO",
            "email":"daniel@example.com"
         }
      ]
   },
   "company_suppliers":[
      {
         "supplied_product_description":"Real Estate and Rental and Leasing",
         "supplier_country_code":"US",
         "supplier_description":"Provider Demo"
      }
   ],
   "company_transactionality":{
      "funds_country_code":"US",
      "inc_monthly_movements":2,
      "inc_transactionality":1000000.0,
      "inc_transactionality_currency_code":"USD",
      "out_monthly_movements":3,
      "out_transactionality":800000.0,
      "out_transactionality_currency_code":"USD"
   },
   "company_treasurers":[
      {
         "birth_date":"2011-03-23",
         "document_issuing_country_code":"US",
         "document_number":"111555000",
         "document_type_code":"SSN",
         "email":"samira@dev.com",
         "first_name":"SAMIRA",
         "gender":"F",
         "last_name":"GONZALEZ",
         "email":"samira@example.com"
      }
   ]
}
```

</details>

<details>

<summary>Ejemplo 3</summary>

```
{
   "company_business_type":{
      "cash_expected_monthly_revenue_currency_code":"USD",
      "employees_range":"20 a 100",
      "current_year_sales_amount":30000.0,
      "current_year_sales_currency_code":"USD",
      "product_unique_description":"technical_services",
      "product_description":"Professional, Scientific, and Technical Services",
      "last_year_sales_amount":100000.0,
      "cash_expected_monthly_revenue_amount":50000.0,
      "last_year_sales_amount_currency_code":"USD"
   },
   "company_info":{
      "document_description":"1000000123",
      "alternate_name":"Synthex",
      "registering_entity_name":"Direccion General de Impuestos Internos",
      "economic_activity_code":"927110",
      "tax_residence_country_code":"US",
      "registering_entity_country_code":"DO",
      "is_private_entity":1,
      "document_type_code":"TXID",
      "main_language":"Ingles",
      "company_category_description":"Empresa Individual de Responsabilidad Limitada (EIRL)",
      "total_shares":0.0,
      "additional_field1":"Optional 1",
      "additional_field2":"Optional 2",
      "additional_field3":"Optional 3",
      "founding_date":"2012-07-12",
      "company_category_unique_name":"limited_liability_partnership",
      "name":"SYNTHEX SYSTEMS",
      "code":"35Kcd4",
      "url":"synthex.com",
      "document_issuing_country_code":"US",
      "economic_activity_description":"Space Research and Technology",
      "registry_id":"111000013"
   },
   "company_contact":{
      "contact_cell_phone":"7895521111",
      "phone_country_code":"US",
      "phone_international_code":"+1",
      "main_email":"synthex@dev.aclaoverseas.com"
   },
   "company_main_customers":[
      {
         "main_customer_country_code":"US",
         "main_customer_description":"AstraCore Enterprises",
         "consumed_product_description":"Professional, Scientific, and Technical Services"
      }
   ],
   "company_partners":{
      "person_members":[
         {
            "document_type_code":"SSN",
            "first_name":"JULIA",
            "last_name":"ALVARADO",
            "middle_name":"ALEJANDRA",
            "gender":"F",
            "document_number":"550000010",
            "person":1040,
            "document_issuing_country_code":"US",
            "birth_date":"1987-10-15",
            "email":"juliaalv@example.com",
         },
         {
            "document_type_code":"SSN",
            "first_name":"MARIO",
            "last_name":"OLIVARES",
            "gender":"M",
            "document_number":"660010009",
            "company_principal":204,
            "person":1039,
            "document_issuing_country_code":"US",
            "birth_date":"1979-09-06",
            "email":"mariol@dexample.com",
         }
      ]
   },
   "company_treasurers":[
      {
         "document_type_code":"IDEN",
         "first_name":"CAROLINA",
         "last_name":"MATOS",
         "middle_name":"ANDREINA",
         "gender":"F",
         "document_number":"123000710",
         "document_issuing_country_code":"CO",
         "birth_date":"1977-02-04",
         "middle_last_name":"CARDONA",
         "email":"carandre@example.com"
      }
   ],
   "company_subsidiaries":[
      {
         "document_type_code":"TXID",
         "subsidiary_product_name":"Management of Companies and Enterprises",
         "document_number":"1001000010",
         "subsidiary_name":"Vortex Global",
         "issuing_country_code":"US"
      }
   ],
   "company_alternate_accounts":[
      {
         "bank_name":"WELLS FARGO BANK",
         "aba_routing":"063107513",
         "account":"11121212112"
      }
   ],
   "company_entity_branch":{
      "branch_name":"OFC PRINCIPAL",
      "branch_code":"01"
   },
   "company_branches":[
      {
         "phone_number":"7869991100",
         "document_type_code":"TXID",
         "name":"SYNTHEX SYSTEMS",
         "extension":123456,
         "document_number":"1000000123",
         "document_issuing_country_code":"US",
         "responsible_name":"Joe Doe",
         "description":"175 sw 7th street"
      }
   ],
   "company_main_address":{
      "company_properties_rent_amount":"4000.00",
      "company_properties_owner":"RL GROUP LLC",
      "locality_description":"MIAMI",
      "city_description":"Miami",
      "company_properties_owner_phone":"7865440011",
      "street":"175 sw 7th street",
      "postal_code":"33130",
      "country_code":"US",
      "province_description":"Florida",
      "country_name":"Estados Unidos",
      "locality_code":"33130",
      "company_properties_type":"rented"
   },
   "company_transactionality":{
      "inc_transactionality":80000.0,
      "out_transactionality":70000.0,
      "inc_transactionality_currency_code":"USD",
      "out_monthly_movements":7,
      "out_transactionality_currency_code":"USD",
      "funds_country_code":"US",
      "inc_monthly_movements":2
   },
   "company_suppliers":[
      {
         "supplier_country_code":"US",
         "supplied_product_description":"Professional, Scientific, and Technical Services",
         "supplier_description":"Innovix Technologies"
      }
   ],
   "company_managers":[
      {
         "document_type_code":"SSN",
         "first_name":"MIGUEL",
         "last_name":"ROMERO",
         "middle_name":"ALEJANDRO",
         "gender":"M",
         "document_number":"333301121",
         "document_issuing_country_code":"US",
         "birth_date":"1983-11-17",
         "email":"migromero@example.com"
      }
   ]
}
```


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
