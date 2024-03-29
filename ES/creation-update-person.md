
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