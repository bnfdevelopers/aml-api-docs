# CHANGELOG

## Release: 2.0.1

### Date: 2023-07-14

### CREACIÓN / ACTUALIZACIÓN DE CLIENTES PERSONA FÍSICA

* Nodo **client_info**: 

    * Eliminados los campos: _document_type_code, document_number, issuing_country_code, other_document_type_code, other_document_number, other_issuing_country_code_. Estos campos seràn enviados en un nuevo nodo llamado **client_documents** explicado más adelante.

    * Eliminado el objeto _product_request_type_.

    * Agregado campo _civil_status_unique_description, nickname, other_sex_description, customer_number_.

    * Agregado objeto risk_data para enviar información del cálculo de riesgo de lavado de los clientes.

* Nodo **client_location:**

    * Los campos _province_code, neighbourhood_description_ pueden no aparecer en el json dependiendo si el valor es null o no.

* Nodo **client_contact:**

    * Eliminado los campos: _fax_.

* Nodo **client_job_info:**

    * Agregados los campos: _company_code, annual_gross_salary, entry_date, professional_position, work_email, company_country_code, company_country_description, country_description (en other_job_info)_.

    * Eliminado campo: _country_.

    * En la actividad comercial adicional se eliminaron los campos: _ciiu_.

    * En la actividad comercial adicional se agregaron los campos: _commercial_activity_code, commercial_activity_description, origin_income, work_email_.

    * Agregado el objeto _previously_employment_data_. En caso de existir irá en el json de lo contrario no.

    * Renombrado campo _ciiu_ por _occupation_code_.

* Nodo **client_references:**

    * En las referencias bancarias se agregaron los campos _canceled, canceled_reason, aba_code, iban_code, swift_code_.

    * En las referencias personales se agregó los campos _country, reference_relationship_unique_description, reference_relationship_description, reference_relationship_type_description_.

    * Se agregaron nuevos nodos: _first_client_commercial_reference, second_client_commercial_reference, first_supplier_commercial_reference, second_supplier_commercial_reference_

* Nodo **client_transactionality:**

    * Agregado los campos _funds_country_code y funds_country_description_.

* Nuevo nodo **client_documents:** Usado para enviar la información de los documentos de identidad ingresados por el cliente.


### CREACIÓN / ACTUALIZACIÓN DE CLIENTES PERSONA JURÍDICA

* Nodo **company_info:**

    * Agregado los campos _acronym, is_private_entity, is_financial_entity, main_email, url, code, customer_number_

* Nuevo nodo **company_contact**.

* Nodo **company_main_address:**

    * Agregados los campos: _postal_code, country_code, country_description, locality_code, locality_description, company_properties_type, company_properties_owner, company_properties_owner_phone, company_properties_rent_amount_. Los 3 últimos se muestran o no dependiendo si la propiedad es alquilada o propia.

    * Eliminado el campo _mail_email_. Movido a nuevo nodo _company_contact_.

* Node **company_partners:**

     * In person_members added the fields: _birth_date and email_.

* Node **company_treasurers:**

     * Added the fields: _birth_date and email_.

* Node **company_branches:**

     * Added the field: _extension_.
     
### CREACIÓN DE PRODUCTO

* Agregado nuevo nodo: _related_client_info_ para indicar los datos de los relacionados o vinculados a una cuenta, dígase co-titulares o beneficiarios.

* Nuevo formato de respuesta.