# CHANGELOG

## Release: 2.0.1

### Date: YYYY-MM-DD

### CREACIÓN / ACTUALIZACIÓN DE CLIENTES PERSONA FÍSICA

* Nodo **client_info**: 

    * Eliminados los campos: _document_type_code, document_number, issuing_country_code, other_document_type_code, other_document_number, other_issuing_country_code_. Estos campos seràn enviados en un nuevo nodo llamado **client_documents** explicado más adelante.

    * Eliminado el objeto _product_request_type_.

* Nodo **client_location:**

    * Los campos _province_code, neighbourhood_description_ pueden no aparecer en el json dependiendo si el valor es null o no.

* Nodo **client_contact:**

    * Eliminado los campos: _fax_.

* Nodo **client_job_info:**

    * Agregados los campos: _annual_gross_salary, entry_date, professional_position_.

    * Agregado el objeto _previously_employment_data_. En caso de existir irá en el json de lo contrario no.

    * Renombrado campo _ciiu_ por _occupation_code_.

* Nodo **client_references:**

    * En las referencias bancarias se agregaron los campos _canceled y canceled_reason_.

    * En las referencias personales se agregó el campo _country_.

* Nodo **client_transactionality:**

    * Agregado los campos _funds_country_.

* Nuevo nodo **client_documents:** Usado para enviar la información de los documentos de identidad ingresados por el cliente.


### CREACIÓN / ACTUALIZACIÓN DE CLIENTES PERSONA JURÍDICA

* Nodo **company_main_address:**

    * Agregados los campos: _company_properties_type, company_properties_owner, company_properties_owner_phone, company_properties_rent_amount_. Los 3 últimos se muestran o no dependiendo si la propiedad es alquilada o propia.