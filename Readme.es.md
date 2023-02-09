
# DOCUMENTACIÓN API AML

## Contenido

* [INTRODUCCIÓN](#INTRODUCCIÓN)
* [CREACIÓN / ACTUALIZACIÓN DE CLIENTES PERSONA FÍSICA](#CREACIÓN--ACTUALIZACIÓN-DE-CLIENTES-PERSONA-FÍSICA)
* [CREACIÓN / ACTUALIZACIÓN DE CLIENTES JURIDICAS](#CREACIÓN--ACTUALIZACIÓN-DE-CLIENTES-JURIDICAS)
* [CREACIÓN DE PRODUCTOS PARA PERSONAS FÍSICAS Y JURÍDICAS](#CREACIÓN-DE-PRODUCTOS-PARA-PERSONAS-FÍSICAS-Y-JURÍDICAS)


## INTRODUCCIÓN

La Plataforma para Prevención de Lavado de Activos y Financiamiento al Terrorismo de Better and Faster Tech Inc dispone de mecanismos para la integración con los sistemas de gestión, administración de clientes y transacciones (p.e. Core Bancario) para lograr la automatización de los procesos y el control de forma integral. Dentro de las funciones disponibles se incluyen:

* **Creación de clientes  desde Clients Inspector hacia los sistemas de la entidad**.  Una vez que los clientes son depurados y el Rol de Gerente autoriza su vinculación con la institución, Clients Inspector invocará las funciones descritas en este documento para que el cliente sea creado en los  sistemas de la institución.  Existen dos tipos de clientes que pueden ser creados desde Clients Inspector, Clientes Persona Física y Clientes Jurídicos o Compañías.

* **Creación de productos desde Clients Inspector hacia los sistemas de la entidad**. La creación del producto se realiza después de la creación del cliente, siempre que la misma haya sido exitosa.  El Rol Gerente verá una solicitud de creación de producto en su bandeja la cual debe aprobar.  Clients inspector invocará las funciones descritas en este documento para que el cliente sea creado en los  sistemas de la institución.

* **Envío de transacciones y carga de datos de clientes ocasionales.** La aplicación Transaction Inspector requiere ser alimentada con las transacciones que la institución decida monitorear.  Para esto los sistemas de la institución deberán enviar los datos de la transacción donde se deben incluir los datos de los Clientes Ocasionales, por ejemplo el depositante.  Este paso es necesario para ejecutar la correspondiente debida diligencia simplificada a estos terceros tal como lo indican las regulaciones. El envío de transacciones son servicios que Transaction Inspector habilita para ser invocados por los sistemas de la Institución.

A continuación se describen los diferentes servicios que deben ser implementados por las instituciones.

---

## CREACIÓN / ACTUALIZACIÓN DE CLIENTES PERSONA FÍSICA

Para poder crear clientes en el core de la entidad financiera, es necesario que la entidad cliente desarrolle un servicio que permita enviar toda la información tomada desde la aplicación de Clients Inspector hacia el core. El servicio que provea la entidad, deberá retornar el número de cliente que asigna el core. En caso de que el cliente ya exista en el core, deberá ser capaz de actualizar dicha información con la información enviada desde Clients Inspector.

**URL del servicio:**
```bash 
https://{URLCLIENTE}/core_customer
```

**Método:** POST

**Tipo de Autenticación:** Basic Authentication (Autenticación Básica). Por seguridad, ell servicio que tendrá que crear el equipo de TI de la entidad deberá ser capaz de manejar este tipo de autenticación para que nos provea unas credenciales de acceso al servicio.

**Parámetros de Entrada:** Dividiremos la estructura del JSON (llave-valor) en tres  niveles: nivel 1, nivel 2 y nivel 3, dependiendo del tipo de información que provea los nodos de nivel 2. Los nodos de nivel 1 serán los siguientes:

* **client_info:** Indicará la información personal del cliente a ser creado: nombres, apellidos, número de documentos, etc.

* **client_location:** Datos de vivienda del cliente.

* **client_contact:** Datos de contacto del cliente.

* **client_job_info:** Datos laborales del cliente.

* **client_references:** Referencias bancarias y personales. Las referencias bancarias son opcionales.

* **client_transactionality:** Datos de transaccionalidad del cliente.

* **client_entity_branch:** Información de la sucursal de la entidad donde el cliente se está registrando.

A continuación la descripción de cada campo:

|NIVEL 1|NIVEL 2|NIVEL 3|TIPO|TAMAÑO|DESCRIPCIÓN|
|:----|:----|:----|:----|:----|:----|
|client_info      |first_name*               |                            |varchar  | 30 |Primer nombre del cliente|
|                 |middle_name               |                            |varchar  | 30 |Segundo nombre del cliente|
|                 |last_name*                |                            |varchar  | 30 |Primer apellido del cliente|
|                 |middle_last_name          |                            |varchar  | 30 |Segundo Nombre|
|                 |sex*                      |                            |varchar  | 1  |Género: M / F|
|                 |birth_date*               |                            |Date     | -  |Fecha de Nacimiento. Formato YYYY-MM-DD|
|                 |civil_status_id*          |                            |Integer  | -  |Código de estado civil. Ver Anexo 1|
|                 |civil_status_description  |                            |varchar  | 50 |Descripción del estado civil|
|                 |nationality_code          |                            |varchar  | 2  |Código de país de la nacionalidad en Alpha-2. Ejemplo: DO, IT, CO, etc. Ver Anexo 1|
|                 |other_nationality_code    |                            |varchar  | 2  |Código de país de segunda nacionalidad en Alpha-2|
|                 |document_type_code*       |                            |varchar  | 4  |Tipo de documento: CEDU, PASA, RNC, RNCE, LCD, NSS. Anexo 1|
|                 |document_number*          |                            |varchar  | 15 |Número de documento principal|
|                 |issuing_country_code*     |                            |varchar  | 2  |Código del país emisor del documento principal|
|                 |other_document_type_code  |                            |varchar  | 4  |Tipo del documento secundario|
|                 |other_document_number     |                            |varchar  | 15 |Número de documento secundario|
|                 |other_issuing_country_code|                            |varchar  | 2  |País emisor del documento secundario|
|                 |person_type_code          |                            |varchar  | 10 |Indica si la persona es PEP o RPEP. En caso de que no, el valor es vacío|
|                 |spouse_info               |first_name                  |varchar  | 30 |Primer nombre del cónyuge|
|                 |                          |middle_name                 |varchar  | 30 |Segundo nombre del cónyuge|
|                 |                          |last_name                   |varchar  | 30 |Apellido del cónyuge|
|                 |                          |middle_last_name            |varchar  | 30 |Segundo apellido del cónyuge|
|                 |                          |document_type_code          |varchar  | 4  |Tipo de documento|
|                 |                          |document_number             |varchar  | 15 |Número de documento|
|                 |                          |issuing_country_code        |varchar  | 2  | País emisor del documento|
|                 |immigration_status        |unique_name                 |varchar  | 50 |Identificador único del estatus migratorio: work_permit, holidays, resident. Ver Anexo 1|
|                 |                          |description                 |varchar  | 50 |Descripción del estatus migratorio: Permiso de trabajo, Vacaciones, Residente.|
|                 |additional_field1         |                            |varchar  | 150|Campo opcional para almacenar lo que indique el cliente. Este campo solo se agrega en la estructura si es un valor válido. Es decir, sólo se agregará en el JSON si existe valor, de lo contrario no.|
|                 |additional_field2         |                            |varchar  | 150|Campo opcional para almacenar lo que indique el cliente. Este campo solo se agrega en la estructura si es un valor válido.Es decir, sólo se agregará en el JSON si existe valor, de lo contrario no.|
|                 |additional_field3         |                            |varchar  | 150|Campo opcional para almacenar lo que indique el cliente. Este campo solo se agrega en la estructura si es un valor válido. Es decir, sólo se agregará en el JSON si existe valor, de lo contrario no.|
|                 |related_accounts          |                            |Array    | |Este campo comprende una lista de objetos. Cada objeto contiene la información de la cuenta existente en el CORE bancario a la cual se va a vincular al solicitante.   Este campo puede venir como una lista vaciá, en caso de no aplicar|
|                 |                          |account_link_type_code      |varchar  | 1  |Código del tipo de vinculación del cliente.   Descripción de los valores en la tabla 1.1|
|                 |                          |authorized_signature        |boolean  |    |Valor booleano que indica si el cliente es firma autorizada de la cuenta a  vincular.|
|                 |                          |related_account_number      |varchar  | 20 |Número de cuenta a vincular al cliente.|
|                 |product_request_type      |                            |Object   |    |Diccionario que contiene la información referente al tipo de solicitud que se esta realizando. Los tipos de solicitudes son especificados en la tabla 1.2|
|                 |                          |description                 |varchar  | 50 |Título del tipo de solicitud.|
|                 |                          |code                        |varchar  | 1  |Código del tipo de solicitud.|
|client_location  |city_description*         |                            |varchar  | 30 |Nombre de la ciudad de residencia|
|                 |province_code*            |                            |varchar  | 4  |Código de provincia. Ver Anexo 1|
|                 |province_description*     |                            |varchar  | 70 |Nombre de la provincia|
|                 |country_description*      |                            |varchar  | 20 |Nombre del país de residencia|
|                 |locality_code*            |                            |varchar  | 6  |Código de localidad.|
|                 |locality_description*     |                            |varchar  | 70 |Nombre de la localidad|
|                 |country_code*             |                            |varchar  | 2  |Código de país Alpha-2|
|                 |address*                  |                            |varchar  | 100|Dirección de domicilio|
|                 |neighbourhood_description |                            |varchar  | 100|Nombre del Sector|
|                 |housing_type              |                            |varchar  | 50 |Tipo de vivienda. Los valores que toma son: Propia y Alquilada.|
|                 |rent_amount               |                            |float    | -  |Monto de alquiler en caso de que el tipo de vivienda es alquilada. Puede ser null.|
|                 |owner                     |                            |varchar  | 80 |Nombre del propietario (caso alquilado)|
|                 |owner_phone               |                            |varchar  | 30 |Número de teléfono del propietario (caso alquilado)|
|                 |postal_code               |                            |varchar  | 10 |Código postal|
|client_contact   |email*                    |                            |varchar  | 50 |Correo electrónico|
|                 |cell_phone*               |                            |varchar  | 20 |Número de teléfono celular|
|                 |home_phone                |                            |varchar  | 20 |Número de teléfono de casa|
|                 |buss_phone                |                            |varchar  | 20 |Número de teléfono de oficina|
|                 |fax                       |                            |varchar  | 20 |Fax|
|                 |social_network_name       |                            |varchar  | 45 |Nombre de red social|
|                 |social_network_username   |                            |varchar  | 30 |Usuario de red social|
|client_job_info  |salary*                   |                            |float    | -  |Monto de salario|
|                 |net_income*               |                            |float    | -  |Ganancia Neta|
|                 |currency_code*            |                            |varchar  | 4  |Código de moneda. Ver Anexo 1|
|                 |work_address*             |                            |varchar  | 150|Dirección de trabajo|
|                 |ciiu*                     |                            |varchar  | 6  |Código de la ocupación|
|                 |company_name              |                            |varchar  |100 |Nombre de la compañía donde labora el cliente|
|                 |other_job_info            |ciiu                        |varchar  | 6  |Código de la actividad comercial adicional. Esta información proviene las actividades cargadas en la herramienta GIR.|
|                 |                          |country_code                |varchar  | 2  |País de negocio|
|                 |                          |quantity_of_employees       |integer  | -  |Cantidad de empleados|
|                 |                          |total_assets                |Float    | -  |Monto total de los activos|
|                 |                          |income_in_another_currency  |Float    | -  |Ingresos en otra moneda expresado en USD. Por defecto es 0.|
|                 |                          |origin_income               |varchar  | 100|Origen de los ingresos. Por defecto retorna null.|
|client_references|first_bank_reference      |country_code                |varchar  | 2  |Código de país en Alpha-2.|
|                 |                          |bank_name                   |varchar  | 50 |Nombre del banco|
|                 |                          |acount_type_description     |varchar  | 50 |Tipo de cuenta|
|                 |                          |account_number              |varchar  | 50 |Número de cuenta|
|                 |                          |officer                     |varchar  | 100|Nombre de oficial responsable|
|                 |second_bank_reference     |country_code                |varchar  | 2  |Código de país en Alpha-2.|
|                 |                          |bank_description            |varchar  | 50 |Nombre del banco|
|                 |                          |acount_type_description     |varchar  | 50 |Tipo de cuenta|
|                 |                          |account_number              |varchar  | 50 |Número de cuenta|
|                 |                          |officer                     |varchar  | 100|Nombre de oficial responsable|
|                 |first_personal_reference* |name                        |varchar  | 100|Nombre completo de la referencia|
|                 |                          |relationship                |varchar  | 50 |Indica si es Familiar o No Familiar|
|                 |                          |phone                       |varchar  | 20 |Número de teléfono de la referencia|
|                 |                          |address                     |varchar  | 150|Dirección|
|                 |                          |document_number             |varchar  | 20 |Número de documento|
|                 |                          |document_type_code          |varchar  | 4  |Tipo de documento|
|                 |second_personal_reference*|name                        |varchar  | 100|Nombre completo de la referencia|
|                 |                          |relationship                |varchar  | 50 |Indica si es Familiar o No Familiar|
|                 |                          |phone                       |varchar  | 20 |Número de teléfono de la referencia|
|                 |                          |address                     |varchar  | 150|Dirección|
|                 |                          |document_number             |varchar  | 20 |Número de documento|
|                 |                          |document_type_code          |varchar  | 4  |Tipo de documento|
|client_transactionality|inc_transactionality*|                           |float    | -  | Monto de transaccionalidad entrante|
|                 |inc_monthly_movements*    |                            |integer  | -  | Cantidad de movimientos mensuales de entrada|
|                 |out_transactionality*     |                            |float    | -  | Monto de transaccionalidad saliente|
|                 |out_monthly_movements*    |                            |integer  | -  | Cantidad de movimientos mensuales de salida|
|client_entity_branch|branch_code*           |                            |varchar  | 6  |Código de la sucursal de la entidad.|
|                 |branch_name*              |                            |varchar  | 35 |Nombre de la sucursal de la entidad.|

**Nota:** Los marcados con asterisco (*) son los recomendados a que sean almacenados en el core. Los que no tengan esta marca pueden llegar con el valor de null o vacío (“”).


### Ejemplos

<details><summary>Ejemplo 1</summary>
<p>

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

<details><summary>Ejemplo 2</summary>
<p>

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

<details><summary>Ejemplo 3</summary>
<p>

```
{
   "client_contact":{
      "buss_phone":"8494065577",
      "cell_phone":"8494065577",
      "email":"ejemplo16062022@dev.alaoverseas.com",
      "fax":null,
      "home_phone":null,
      "social_network_name":"Facebook",
      "social_network_username":"ejemplo16062022"
   },
   "client_entity_branch":{
      "branch_code":null,
      "branch_name":"Agencia 27 de febrero"
   },
   "client_info":{
      "birth_date":"1974-03-04",
      "civil_status_description":"Soltero(a)",
      "civil_status_id":1,
      "document_number":"05502356982",
      "document_type_code":"CEDU",
      "first_name":"PABLO",
      "immigration_status":null,
      "issuing_country_code":"DO",
      "last_name":"VIERA",
      "middle_last_name":"BARBIERI",
      "middle_name":"RAUL",
      "nationality_code":"DO",
      "other_document_number":null,
      "other_document_type_code":null,
      "other_issuing_country_code":null,
      "other_nationality_code":null,
      "person_type_code":null,
      "sex":"M",
      "spouse_info":null,
      "product_request_type":{
         "code":"product_request",
         "description":"Solicitud de Producto"
      },
      "related_accounts":[
         {
            "account_link_type_code":"O",
            "authorized_signature":true,
            "related_account_number":"010005967"
         }
      ]
   },
   "client_job_info":{
      "ciiu":"950004",
      "company_name":"Acla Overseas",
      "currency_code":"DOP",
      "net_income":100.0,
      "other_job_info":null,
      "salary":10000.0,
      "work_address":"Av.Enriquillo"
   },
   "client_location":{
      "address":"Av. Enriquillo",
      "city_description":"Santo Domingo",
      "country_code":"DO",
      "country_description":"República Dominicana",
      "housing_type":"Propia",
      "locality_code":"320201",
      "locality_description":"SANTO DOMINGO OESTE",
      "neighbourhood_description":"Alameda ",
      "owner":null,
      "owner_phone":null,
      "postal_code":"10902",
      "province_code":"3200",
      "province_description":"Santo Domingo",
      "rent_amount":null
   },
   "client_references":{
      "first_bank_reference":{
         "account_number":"11111111111111",
         "account_type_description":"Ahorros",
         "bank_name":"Banco Popular",
         "country_code":"DO",
         "officer":"Camila Matos"
      },
      "first_personal_reference":{
         "address":"Av. Enriquillo",
         "document_number":"60588052658",
         "document_type_code":"CEDU",
         "name":"PEPE GONZALES",
         "phone":"8494065577",
         "relationship":"Familiar"
      },
      "second_bank_reference":{
         "account_number":"11111111111111",
         "account_type_description":"Ahorros",
         "bank_name":"Banco BHD/León",
         "country_code":"DO",
         "officer":"Camila Matos"
      },
      "second_personal_reference":{
         "address":"Av. Enriquillo",
         "document_number":"02784032605",
         "document_type_code":"CEDU",
         "name":"LUIS GONZALES",
         "phone":"8494065577",
         "relationship":"No Familiar"
      }
   },
   "client_transactionality":{
      "inc_monthly_movements":1,
      "inc_transactionality":10.0,
      "out_monthly_movements":1,
      "out_transactionality":10.0
   }
}
```

</details>

### Respuestas por parte de la entidad

La entidad deberá retornar, en formato JSON, el número de cliente con los siguientes http status codes:

* **201** - Respuesta exitosa, cliente creado

* **4XX** - Error creando el cliente. Deberá retornar el mensaje del error para ser mostrado en la aplicación.

### Ejemplos de respuestas

<details><summary>Ejemplo 1 - Caso exitoso (HTTP STATUS CODE 201) </summary>
<p>

```
{
	"customer_number": 1234
}
```

</details>

<details><summary>Ejemplo 2 - Caso error (HTTP STATUS CODE 4XX) </summary>
<p>

```
{
	"message": "MENSAJE DEL MOTIVO DEL ERROR"
}
```

</details>

---

## CREACIÓN / ACTUALIZACIÓN DE CLIENTES JURIDICAS

**URL del servicio:**
```bash 
https://{URLCLIENTE}/company_core_customer
```

**Método:** POST

**Tipo de Autenticación:** Basic Authentication (Autenticación Básica). El equipo de TI debe proveer las respectivas  credenciales de acceso a este servicio.

**Parámetros de Entrada:** Se provee una estructura de datos, bajo el formato JSON. La cual esta constituida de la  siguiente forma:

* **company_info:** Información principal de la compañía tales como: nombre, identificación, actividad económica, entidad donde fue registrada, etc.

* **company_main_address:** Información de la ubicación de la sede principal de la empresa.

* **company_business_type:** Información sobre el tipo de negocio que realiza la empresa.

* **company_alternate_accounts:** Lista de las cuentas en otras entidades que posea la empresa.

* **company_branches:** Listado de sucursales que posee la empresa. En caso de que tenga solo una sucursal entonces será una lista de un elemento.

* **company_subsidiaries:** Lista de los principales empresas afiliadas o subsidiarias de la compañía a ser creada.

* **company_suppliers:** Lista de los principales proveedores de la empresa.

* **company_main_customers:** Listado de los principales clientes de la empresa.

* **company_transactionality:** Información de la transaccionalidad estimada que tendrá la compañía dentro de la entidad.

* **company_managers:** Lista que contiene la información de los representantes de la alta gerencia. Acá puede estar la información del presidente, directores y gerentes.

* **company_treasures:** Lista que contiene la información de los responsables de las finanzas de la empresa.

* **company_entity_branch:** Información de la sucursal de la entidad donde la empresa se está registrando.

* **company_partners:** Nodo que contiene dos valores: 
   * **persons_members:** Lista de la información de los socios (personas físicas) de la empresa.

   * **company_members:** Si la empresa tiene socios que también son empresas, entonces aparecerá la información de dichas empresas (en una lista) en este nivel. Estas empresas pueden, a su vez, tener socias tanto empresas como personas físicas, por lo que el árbol se empieza expandir dependiendo de la cantidad de socios que agregue el usuario. En el ejemplo más adelante veremos que la compañía principal tiene un sólo socio persona física (person_members) y dos empresas socias (company_members) que a su vez, una de ellas tiene una persona física (person_members) y la otra tiene una empresa como socia y así sucesivamente.

   A continuación la descripción de cada campo:

| NIVEL 1                    | NIVEL 2                                     | NIVEL 3                        | TIPO    | TAMAÑO | DESCRIPCIÓN                                                                                                                                                                                                                          |
|----------------------------|---------------------------------------------|--------------------------------|---------|-----|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| company_info               | name*                                       |                                | varchar | 250 | Nombre de la empresa                                                                                                                                                                                                                 |
|                            | alternate_name                              |                                | varchar | 250 | Nombre alternativo de la empresas                                                                                                                                                                                                    |
|                            | founding_date                               |                                | date    | -   | Fecha de constitución en formato: YYYY-MM-DD                                                                                                                                                                                         |
|                            | main_language                               |                                | varchar | 100 | Idioma de principal uso. Ejemplo: Español, Inglés, etc                                                                                                                                                                               |
|                            | company_category_unique_name                |                                | varchar | 100 | Código único que identifica al tipo de sociedad. Por ejemplo: limited_liability_partnership. Ver Anexo 1                                                                                                                             |
|                            | company_category_description                |                                | text    | -   | Descripción del tipo de sociedad. Por ejemplo: Sociedad de Responsabilidad Limitada (SRL) Ver Anexo 1                                                                                                                                |
|                            | document_description                        |                                | varchar | 30  | Número de identificación de la empresa (RNC, Tax ID, ...)                                                                                                                                                                            |
|                            | document_issuing_country_code               |                                | varchar | 2   | País emisor de documento en formato Alpha-2.                                                                                                                                                                                         |
|                            | document_type_code                          |                                | varchar | 20  | Tipo de documento.                                                                                                                                                                                                                   |
|                            | economic_activity_code                      |                                | varchar | 6   | Código CIIU que indica la actividad económica de la empresa.                                                                                                                                                                         |
|                            | economic_activity_description               |                                | varchar | 250 | Descripción de la actividad económica de la empresa de acuerdo al código CIIU.                                                                                                                                                       |
|                            | registry_id                                 |                                | varchar | 50  | Número de registro.                                                                                                                                                                                                                  |
|                            | registering_entity_name                     |                                | varchar | 50  | Nombre de la entidad registradora, por ejemplo: DGII                                                                                                                                                                                 |
|                            | registering_entity_country_code             |                                | varchar | 2   | Código del país donde se encuentra la entidad registradora en formato Alpha2. Ejemplo: DO.                                                                                                                                           |
|                            | tax_residence_country_code                  |                                | varchar | 2   | Código del país donde la empresa paga impuestos                                                                                                                                                                                      |
|                            | total_shares                                |                                | float   | -   | Cantidad total de acciones.                                                                                                                                                                                                          |
|                            | additional_field1                           |                                | varchar | 150 | Campo opcional para almacenar lo que indique el cliente. Este campo solo se agrega en la estructura si es un valor válido. Es decir, sólo se agregará en el JSON si existe valor, de lo contrario no.                                |
|                            | additional_field2                           |                                | varchar | 150 | Campo opcional para almacenar lo que indique el cliente. Este campo solo se agrega en la estructura si es un valor válido. Es decir, sólo se agregará en el JSON si existe valor, de lo contrario no.                                |
|                            | additional_field3                           |                                | varchar | 150 | Campo opcional para almacenar lo que indique el cliente. Este campo solo se agrega en la estructura si es un valor válido. Es decir, sólo se agregará en el JSON si existe valor, de lo contrario no.                                |
|                            | product_request_type                        |                                | Object  |     | Diccionario que contiene la información referente al tipo de solicitud que se esta realizando. Los tipos de solicitudes son especificados en la tabla 1.2                                                                            |
|                            |                                             | description                    | varchar | 50  | Título del tipo de solicitud.                                                                                                                                                                                                        |
|                            |                                             | code                           | varchar | 1   | Código del tipo de solicitud.                                                                                                                                                                                                        |
| company_main_address       | city_description                            |                                | varchar | 150 | Ciudad donde se encuentra la sede principal de la empresa.                                                                                                                                                                           |
|                            | neighbourhood_description                   |                                | varchar | 150 | Sector donde se encuentra ubicada la sede principal de la empresa.                                                                                                                                                                   |
|                            | province_code                               |                                | varchar | 10  | Código de la provincia. Ver Anexo 1                                                                                                                                                                                                  |
|                            | province_description                        |                                | varchar | 150 | Nombre de la provincia.                                                                                                                                                                                                              |
|                            | street                                      |                                | varchar | 150 | Dirección de la empresa.                                                                                                                                                                                                             |
| company_alternate_accounts | bank_name                                   |                                | varchar | 50  | Nombre del banco.                                                                                                                                                                                                                    |
|                            | account                                     |                                | varchar | 50  | Número de cuenta.                                                                                                                                                                                                                    |
|                            | aba_routing                                 |                                | varchar | 9   | Número de enrutamiento de ABA.                                                                                                                                                                                                       |
| company_business_type      | product_unique_description                  |                                | varchar | 200 | Descripción única del producto o servicio que ofrece la empresa.                                                                                                                                                                     |
|                            | product_description                         |                                | varchar | 200 | Descripción del producto o servicio que ofrece la empresa.                                                                                                                                                                           |
|                            | last_year_sales_amount                      |                                | float   | -   | Monto del total de ventas del año anterior.                                                                                                                                                                                          |
|                            | last_year_sales_amount_currency_code        |                                | varchar | 3   | Código de la moneda del monto de las ventas del año anterior.                                                                                                                                                                        |
|                            | current_year_sales_amount                   |                                | float   | -   | Monto del total de ventas del año en curso.                                                                                                                                                                                          |
|                            | current_year_sales_currency_code            |                                | varchar | 3   | Código de la monedea del monto de las ventas del año en curso.                                                                                                                                                                       |
|                            | cash_expected_monthly_revenue_amount        |                                | float   | -   | Monto aproximado a recibir en efectivo.                                                                                                                                                                                              |
|                            | cash_expected_monthly_revenue_currency_code |                                | varchar | 3   | Código de la moneda del efectivo a recibir.                                                                                                                                                                                          |
|                            | employee_ranges                             |                                | varchar | 200 | Rango de la cantidad de empleados de la empresa. Por ejemplo, 1 a 20, 20 a 100, 100 a 500, más de 1000.                                                                                                                              |
| company_suppliers          | supplier_description                        |                                | varchar | 200 | Nombre del proveedor-                                                                                                                                                                                                                |
|                            | supplier_country_code                       |                                | varchar | 2   | Código del país donde se encuentra el proveedor.                                                                                                                                                                                     |
|                            | supplied_product_description                |                                | varchar | 2   | Producto o servicio que le ofrece el proveedor.                                                                                                                                                                                      |
| company_subsidiaries       | subsidiary_name                             |                                | varchar | 200 | Nombre de la empresa afiliada o subsidiaria de la empresa que se está creando.                                                                                                                                                       |
|                            | document_number                             |                                | varchar | 20  | Identificación de la empresa afiliada o subsidiaria.                                                                                                                                                                                 |
|                            | document_type_code                          |                                | varchar | 20  | Tipo de identificación.                                                                                                                                                                                                              |
|                            | issuing_country_code                        |                                | varchar | 2   | Código del país donde fue emitido la identificación.                                                                                                                                                                                 |
|                            | subsidiary_product_name                     |                                | varchar | 200 | Producto o servicio que ofrece dicha empresa afiliada o subsidiaria.                                                                                                                                                                 |
| company_main_customers     | main_customer_description                   |                                | varchar | 200 | Nombre del cliente.                                                                                                                                                                                                                  |
|                            | main_customer_country_code                  |                                | varchar | 2   | Código del país.                                                                                                                                                                                                                     |
|                            | consumed_product_description                |                                | varchar | 200 | Producto ofrecido a dichos clientes.                                                                                                                                                                                                 |
| company_transaccionality   | inc_transactionality                        |                                | float   | -   | Monto aproximado de la transaccionalidad entrante en el mes.                                                                                                                                                                         |
|                            | inc_transactionality_currency_code          |                                | varchar | 3   | Código de la moneda de la transaccionalidad entrante.                                                                                                                                                                                |
|                            | inc_monthly_movements                       |                                | integer | -   | Cantidad de transacciones entrantes.                                                                                                                                                                                                 |
|                            | out_transactionality                        |                                | float   | -   | Monto aproximado de la transaccionalidad saliente en el mes.                                                                                                                                                                         |
|                            | out_transactionality_currency_code          |                                | varchar | 3   | Código de la moneda de la transaccionalidad saliente.                                                                                                                                                                                |
|                            | out_monthly_movements                       |                                | integer | -   | Cantidad de transacciones salientes.                                                                                                                                                                                                 |
|                            | funds_country_code                          |                                | varchar | 2   | Código del país de origen y destino de los fondos en Alpha2. Ejemplo: DO, IT, CO, etc.                                                                                                                                               |
| company_branches           | name                                        |                                | varchar | 100 | Nombre de la sucursal.                                                                                                                                                                                                               |
|                            | document_type_code                          |                                | varchar | 10  | Código del tipo de documento. Ejemplo: RNC.                                                                                                                                                                                          |
|                            | document_number                             |                                | varchar | 100 | Número de identificación de la sucursal.                                                                                                                                                                                             |
|                            | document_issuing_country_code               |                                | varchar | 2   | Código del país emisor del documento en Aplha2.                                                                                                                                                                                      |
|                            | phone_number                                |                                | varchar | 20  | Número de teléfono.                                                                                                                                                                                                                  |
|                            | responsible_name                            |                                | varchar | 100 | Nombre de una persona responsable de la sucursal.                                                                                                                                                                                    |
|                            | neighbourhood_description                   |                                | varchar | 150 | Sector donde se encuentra ubicada la sucursal.                                                                                                                                                                                       |
|                            | description                                 |                                | varchar | 200 | Dirección donde se encuentra ubicada la sucursal.                                                                                                                                                                                    |
| company_managers           | first_name                                  |                                | varchar | 50  | Primer nombre.                                                                                                                                                                                                                       |
|                            | middle_name                                 |                                | varchar | 50  | Segundo nombre en caso de que la persona posea. De lo contrario, no viene esta información.                                                                                                                                          |
|                            | last_name                                   |                                | varchar | 50  | Primer apellido.                                                                                                                                                                                                                     |
|                            | middle_last_name                            |                                | varchar | 50  | Segundo apellido en caso de que la persona posea. De lo contrario, no viene esta información.                                                                                                                                        |
|                            | document_number                             |                                | varchar | 20  | Número de documento.                                                                                                                                                                                                                 |
|                            | document_issuing_country_code               |                                | varchar | 2   | País emisor de documento en Alpha2.                                                                                                                                                                                                  |
|                            | document_type_code                          |                                | varchar | 10  | Tipo de documento. Ejemplo: CEDU, PASA, etc                                                                                                                                                                                          |
|                            | gender                                      |                                | varchar | 1   | Género. Ejemplo: M, F.                                                                                                                                                                                                               |
| company_treasurers         | first_name                                  |                                | varchar | 50  | Primer nombre.                                                                                                                                                                                                                       |
|                            | middle_name                                 |                                | varchar | 50  | Segundo nombre en caso de que la persona posea. De lo contrario, no viene esta información.                                                                                                                                          |
|                            | last_name                                   |                                | varchar | 50  | Primer apellido.                                                                                                                                                                                                                     |
|                            | middle_last_name                            |                                | varchar | 50  | Segundo apellido en caso de que la persona posea. De lo contrario, no viene esta información.                                                                                                                                        |
|                            | document_number                             |                                | varchar | 20  | Número de documento.                                                                                                                                                                                                                 |
|                            | document_issuing_country_code               |                                | varchar | 2   | País emisor de documento en Alpha2.                                                                                                                                                                                                  |
|                            | document_type_code                          |                                | varchar | 10  | Tipo de documento. Ejemplo: CEDU, PASA, etc                                                                                                                                                                                          |
|                            | gender                                      |                                | varchar | 1   | Género. Ejemplo: M, F.                                                                                                                                                                                                               |
| company_partners           | person_members                              | first_name                     | varchar | 50  | Segundo nombre en caso de que la persona posea. De lo contrario, no viene esta información.                                                                                                                                          |
|                            |                                             | middle_name                    | varchar | 50  | Primer apellido.                                                                                                                                                                                                                     |
|                            |                                             | last_name                      | varchar | 50  | Segundo apellido en caso de que la persona posea. De lo contrario, no viene esta información.                                                                                                                                        |
|                            |                                             | middle_last_name               | varchar | 20  | Número de documento.                                                                                                                                                                                                                 |
|                            |                                             | document_number                | varchar | 2   | País emisor de documento en Alpha2.                                                                                                                                                                                                  |
|                            |                                             | document_issuing_country_code  | varchar | 10  | Tipo de documento. Ejemplo: CEDU, PASA, etc                                                                                                                                                                                          |
|                            |                                             | document_type_code             | varchar | 1   | Género. Ejemplo: M, F.                                                                                                                                                                                                               |
|                            |                                             | gender                         |         |     |                                                                                                                                                                                                                                      |
|                            | company_members                             | name                           | varchar | 250 | Nombre de la empresa socia.                                                                                                                                                                                                          |
|                            |                                             | document_description           | varchar | 30  | Número de identificación de la empresa socia.                                                                                                                                                                                        |
|                            |                                             | document_type_code             | varchar | 20  | Código del tipo de identificación de la empresa socia.                                                                                                                                                                               |
|                            |                                             | document_issuning_country_code | varchar | 2   | Código del país emisor del documento en Alpha2.                                                                                                                                                                                      |
|                            |                                             | address                        | varchar | 150 | Dirección de la empresa socia                                                                                                                                                                                                        |
|                            |                                             | company_members                | json    | -   | En caso de que la empresa tenga como socia otra empresa, se repite el ciclo, de lo contraria será una lista vacía ([ ]). La profundidad del árbol dependerá de la cantidad de empresas y personas físicas socias ingrese el usuario. |
|                            |                                             | person_members                 | json    | -   | En caso de que la empresa tenga como socia otras personas físicas, entonces vendrá esta información, de lo contrario será una lista vacía ([ ]).                                                                                     |
| company_entity_branch      | branch_code*                                |                                | varchar | 6   | Código de la sucursal de la entidad.                                                                                                                                                                                                 |
|                            | branch_name*                                |                                | varchar | 35  | Nombre de la sucursal de la entidad.                                                                                                                                                                                                 |

**Nota:** Los marcados con asterisco (*) son los recomendados a que sean almacenados en el core. Los que no tengan esta marca pueden llegar con el valor de null o vacío (“”).

### Ejemplos

<details><summary>Ejemplo 1</summary>
<p>

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

<details><summary>Ejemplo 2</summary>
<p>

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

<details><summary>Ejemplo 3</summary>
<p>

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

### Respuestas por parte de la entidad

La entidad deberá retornar, en formato JSON, el número de cliente con los siguientes http status codes:

* **201** - Respuesta exitosa, cliente creado

* **400** - Error creando el cliente. Deberá retornar el mensaje del error para ser mostrado en la aplicación.

### Ejemplos de respuestas

<details><summary>Ejemplo 1 - Caso exitoso (HTTP STATUS CODE 201) </summary>
<p>

```
{
	"customer_number": 1234
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

