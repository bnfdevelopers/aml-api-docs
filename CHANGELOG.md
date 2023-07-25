# CHANGELOG

## Release: 2.0.1

### Date: 2023-07-14

### CREATION / UPDATE OF NATURAL PERSON CLIENTS

* Node **client_info**: 

    * Removed fields: _document_type_code, document_number, issuing_country_code, other_document_type_code, other_document_number, other_issuing_country_code_. These fields will be sent in a new node called **client_documents**, explained later.

    * Removed the _product_request_type_ object.

    * Added field _civil_status_unique_description, nickname, other_sex_description_.

* Node **client_location:**

    * The fields _province_code, neighbourhood_description_ may not appear in the JSON depending on whether the value is null or not.

* Node **client_contact:**

    * Removed field: _fax_.

* Node **client_job_info:**

    * Added the fields: __company_code, annual_gross_salary, entry_date, professional_position, work_email, company_country_code, company_country_description, country_description (in other_job_info)_.

    * Removed fields in additional commercial activity: _ciiu_.

    * Added fields in additional commercial activity: _commercial_activity_code, commercial_activity_description, origin_income, work_email_.

    * Added the object _previously_employment_data_. If it exists, it will be included in the JSON; otherwise, it will be omitted.

    * Renamed field _ciiu_ to _occupation_code_.

* Node **client_references:**

    * Added fields in bank references: _canceled, canceled_reason, aba_code, iban_code, swift_code_.

    * Added fields in personal references: _country, reference_relationship_unique_description_.

    * Added new nodes: _first_client_commercial_reference, second_client_commercial_reference, first_supplier_commercial_reference, second_supplier_commercial_reference_.

* Node **client_transactionality:**

    * Added fields _funds_country_code and funds_country_description_.

* New node **client_documents:** Used to send information about the identification documents provided by the client.


### CREATION / UPDATE OF LEGAL PERSON CLIENTS

* Node **company_info:**

    * Added fields _acronym, is_private_entity, is_financial_entity, main_email, url, code_.

* Node **company_main_address:**

    * Added fields: _postal_code, country_code, country_description, locality_code, locality_description, company_properties_type, company_properties_owner, company_properties_owner_phone, company_properties_rent_amount_. The last three fields are displayed or omitted depending on whether the property is rented or owned.


### PRODUCT CREATION

* Added new node: _related_client_info_ to indicate the data of individuals related or linked to an account, such as co-owners or beneficiaries.