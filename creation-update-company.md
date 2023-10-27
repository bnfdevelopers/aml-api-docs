---
cover: >-
  https://images.unsplash.com/photo-1570126618953-d437176e8c79?crop=entropy&cs=tinysrgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHwxfHxjb21wYW55fGVufDB8fHx8MTY3NTk4MzQ4MA&ixlib=rb-4.0.3&q=80
coverY: -282
---

# Creation / Update of Company Clients

## Specifications

**Service URL:**

```bash
https://{ENTITY_CLIENT_URL}/company_core_customer
```

**Method:** POST

**Authentication Type:** Basic Authentication. The IT team must provide the respective access credentials to this service.

## Input Parameters

A data structure is provided, under the JSON format. Which is constituted as follows:

* **company\_info:** Main company information such as: name, identification, economic activity, entity where it was registered, etc.
* **company\_main\_address:** Company headquarters location information.
* **company\_business\_type:** Information about the type of business the company does.
* **company\_alternate\_accounts:** List of accounts in other entities owned by the company.
* **company\_branches:** List of branches owned by the company. In case it has only one branch then it will be a list of one element.
* **company\_subsidiaries:** List of the main affiliated companies or subsidiaries of the company to be created.
* **company\_suppliers:** List of the company's main suppliers.
* **company\_main\_customers:** List of the company's main customers.
* **company\_transactionality:** Information on the estimated transactionality that the company will have within the entity.
* **company\_managers:** List containing the information of senior management representatives. Here can be the information of the president, directors and managers.
* **company\_treasures:** A list containing the information of those responsible for the company's finances.
* **company\_entity\_branch:** Information about the branch of the entity where the company is registering.
* **company\_partners:** Node containing two values:
   * **persons\_members:** List of the information of the partners (individuals) of the company.
   * **company\_members:** If the company has partners that are also companies, then the information for those companies will appear (in a list) at this level. These companies can, in turn, have members, both companies and individuals, so the tree begins to expand depending on the number of partners added by the user. In the example below we will see that the main company has a single natural person partner (person\_members) and two partner companies (company\_members) that in turn, one of them has a natural person (person\_members) and the other has a company as a partner and so on.

> :warning: **IMPORTANT:** We recommend that those fields that are not marked with an asterisk (\*), first validate if that field and/or object is sent in the json or not. This is because there may be cases in which said field and/or object travels in the json if there is a value, otherwise it will not appear.

A continuación la descripción de cada campo:

| LEVEL 1                      | LEVEL 2                                          | LEVEL 3                           | TYPE    | LENGTH | DESCRIPTION                                                                                                                                                                                                                           |
| ---------------------------- | ------------------------------------------------ | --------------------------------- | ------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| company\_info                | name\*                                           |                                   | varchar | 250    | Company name                                                                                                                                                                                                                          |
|                              | alternate\_name                                  |                                   | varchar | 250    | Alternative company name                                                                                                                                                                                                              |
|                              | acronym                                          |                                   | varchar | 100    | Company acronym                                                                                                                                                                                                                       |
|                              | founding\_date\*                                 |                                   | date    | -      | Founding date in format: YYYY-MM-DD                                                                                                                                                                                                   |
|                              | customer\_number                                 |                                   | integer | -      | Customer number assigned by the core of the entity. For new clients this value is not sent in the Json                                                                                                                                |
|                              | code                                             |                                   | varchar | 6      | Company code assigned by the platform                                                                                                                                                                                                 |
|                              | main\_language                                   |                                   | varchar | 100    | Language of main use. Example: Spanish, English, etc                                                                                                                                                                                  |
|                              | main\_email                                      |                                   | varchar | 60     | company mail                                                                                                                                                                                                                          |
|                              | url                                              |                                   | varchar | 200    | Company website                                                                                                                                                                                                                       |
|                              | company\_category\_unique\_name\*                |                                   | varchar | 100    | Unique code that identifies the type of company. For example: limited\_liability\_partnership. See Annexes [here](static-data.md)                                                                                                     |
|                              | company\_category\_description\*                 |                                   | text    | -      | Description of the type of company. For example: Limited Liability Company (SRL)                                                                                                                                                      |
|                              | document\_description\*                          |                                   | varchar | 30     | Company identification number (TXID, Tax ID, ...)                                                                                                                                                                                     |
|                              | document\_issuing\_country\_code\*               |                                   | varchar | 2      | Country issuing document in Alpha-2 format.                                                                                                                                                                                           |
|                              | document\_type\_code\*                           |                                   | varchar | 20     | Document type.                                                                                                                                                                                                                        |
|                              | economic\_activity\_code\*                       |                                   | varchar | 6      | Code of the economic activity of the company. In the case of the Dominican Republic, they will be the ISIC codes. For the USA it will be the NAICs                                                                                    |
|                              | economic\_activity\_description\*                |                                   | varchar | 250    | Description of the economic activity of the company according to the previous code.                                                                                                                                                   |
|                              | registry\_id\*                                   |                                   | varchar | 50     | Registry number.                                                                                                                                                                                                                      |
|                              | registering\_entity\_name\*                      |                                   | varchar | 50     | Name of the registration entity, for example: DGII                                                                                                                                                                                    |
|                              | registering\_entity\_country\_code\*             |                                   | varchar | 2      | Country code where the registrar is located in Alpha2 format. Example: DO.                                                                                                                                                            |
|                              | tax\_residence\_country\_code\*                  |                                   | varchar | 2      | Code of the country where the company pays taxes                                                                                                                                                                                      |
|                              | total\_shares\*                                  |                                   | float   | -      | Total number of shares.                                                                                                                                                                                                               |
|                              | is\_private\_entity                              |                                   | Boolean | -      | Indicates if the entity is private or not (true: It is private, false: It is not private)                                                                                                                                             |
|                              | is\_financial\_entity                            |                                   | Boolean | -      | Indicates whether it is a financial entity or not (true: it is financial, false: it is not financial)                                                                                                                                 |
|                              | additional\_field1                               |                                   | varchar | 150    | Optional field to store what the client indicates. This field is only added to the structure if it is a valid value. That is, it will only be added to the JSON if there is a value, otherwise not.                                   |
|                              | additional\_field2                               |                                   | varchar | 150    | Optional field to store what the client indicates. This field is only added to the structure if it is a valid value. That is, it will only be added to the JSON if there is a value, otherwise it will not..                          |
|                              | additional\_field3                               |                                   | varchar | 150    | Optional field to store what the client indicates. This field is only added to the structure if it is a valid value. That is, it will only be added to the JSON if there is a value, otherwise not.                                   |
|                              | product\_request\_type                           |                                   | Object  |        | Dictionary that contains information regarding the type of request being made. See Annexes [here](static-data.md)                                                                                                                     |
|                              |                                                  | description                       | varchar | 50     | Request Type Description.                                                                                                                                                                                                             |
|                              |                                                  | code                              | varchar | 1      | Request type code.                                                                                                                                                                                                                    |
| company\_main\_address       | city\_description\*                              |                                   | varchar | 150    | City where the company's headquarters are located.                                                                                                                                                                                    |
|                              | country\_code\*                                  |                                   | varchar | 2      | Country code                                                                                                                                                                                                                          |
|                              | country\_name                                    |                                   | varchar | 50     | Country name                                                                                                                                                                                                                          |
|                              | neighbourhood\_description                       |                                   | varchar | 150    | Neighbourhood where the company's headquarters are located.                                                                                                                                                                           |
|                              | province\_code                                   |                                   | varchar | 10     | Province/state code. See Annexes [here](static-data.md)                                                                                                                                                                               |
|                              | province\_description                            |                                   | varchar | 150    | Name of the province/state.                                                                                                                                                                                                           |
|                              | street\*                                         |                                   | varchar | 150    | Company address.                                                                                                                                                                                                                      |
|                              | company\_properties\_type\*                      |                                   | varchar | 50     | Takes the values: rented (if the property is rented), owned (if it is owned)                                                                                                                                                          |
|                              | company\_properties\_owner\*                     |                                   | varchar | 100    | Full name of the owner of the property when it is rented                                                                                                                                                                              |
|                              | company\_properties\_owner\_phone\*              |                                   | varchar | 30     | Contact telephone number of the property when it is rented                                                                                                                                                                            |
|                              | company\_properties\_rent\_amount\*              |                                   | float   | -      | Rent amount when the property is rented                                                                                                                                                                                               |
|                              | postal\_code\*                                   |                                   | varchar | 10     | Postal Code                                                                                                                                                                                                                           |
|                              | locality\_code\*                                 |                                   | varchar | 15     | Town code. In the case of the USA it is the same postal code                                                                                                                                                                          |
|                              | locality\_description\*                          |                                   | varchar | 70     | Town name                                                                                                                                                                                                                             |
| company\_alternate\_accounts | bank\_name                                       |                                   | varchar | 50     | Name of the bank                                                                                                                                                                                                                      |
|                              | account                                          |                                   | varchar | 50     | Account number                                                                                                                                                                                                                        |
|                              | aba\_routing                                     |                                   | varchar | 9      | ABA routing number.                                                                                                                                                                                                                   |
| company\_business\_type      | product\_unique\_description                     |                                   | varchar | 200    | Unique description of the product or service offered by the company.                                                                                                                                                                  |
|                              | product\_description                             |                                   | varchar | 200    | Description of the product or service offered by the company.                                                                                                                                                                         |
|                              | last\_year\_sales\_amount                        |                                   | float   | -      | Amount of the total sales of the previous year.                                                                                                                                                                                       |
|                              | last\_year\_sales\_amount\_currency\_code        |                                   | varchar | 3      | Code of the currency of the amount of sales of the previous year.                                                                                                                                                                     |
|                              | current\_year\_sales\_amount                     |                                   | float   | -      | Amount of total sales for the current year.                                                                                                                                                                                           |
|                              | current\_year\_sales\_currency\_code             |                                   | varchar | 3      | Code of the currency of the amount of sales for the current year.                                                                                                                                                                     |
|                              | cash\_expected\_monthly\_revenue\_amount         |                                   | float   | -      | Approximate amount to receive in cash.                                                                                                                                                                                                |
|                              | cash\_expected\_monthly\_revenue\_currency\_code |                                   | varchar | 3      | Code of the currency of the cash to be received.                                                                                                                                                                                      |
|                              | employee\_ranges                                 |                                   | varchar | 200    | Range of the number of employees of the company. For example, 1 to 20, 20 to 100, 100 to 500, more than 1000.                                                                                                                         |
| company\_suppliers           | supplier\_description                            |                                   | varchar | 200    | Supplier's name                                                                                                                                                                                                                       |
|                              | supplier\_country\_code                          |                                   | varchar | 2      | Code of the country where the provider is located.                                                                                                                                                                                    |
|                              | supplied\_product\_description                   |                                   | varchar | 2      | Product or service offered by the supplier.                                                                                                                                                                                           |
| company\_subsidiaries        | subsidiary\_name                                 |                                   | varchar | 200    | Name of the affiliated or subsidiary company of the company being created.                                                                                                                                                            |
|                              | document\_number                                 |                                   | varchar | 20     | Identification of the affiliated or subsidiary company.                                                                                                                                                                               |
|                              | document\_type\_code                             |                                   | varchar | 20     | ID Type.                                                                                                                                                                                                                              |
|                              | issuing\_country\_code                           |                                   | varchar | 2      | Code of the country where the identification was issued.                                                                                                                                                                              |
|                              | subsidiary\_product\_name                        |                                   | varchar | 200    | Product or service offered by said affiliated company or subsidiary.                                                                                                                                                                  |
| company\_main\_customers     | main\_customer\_description                      |                                   | varchar | 200    | Customer name.                                                                                                                                                                                                                        |
|                              | main\_customer\_country\_code                    |                                   | varchar | 2      | Country code.                                                                                                                                                                                                                         |
|                              | consumed\_product\_description                   |                                   | varchar | 200    | Product offered to said customers.                                                                                                                                                                                                    |
| company\_transactionality    | inc\_transactionality\*                          |                                   | float   | -      | Approximate amount of incoming transactions in the month.                                                                                                                                                                             |
|                              | inc\_transactionality\_currency\_code\*          |                                   | varchar | 3      | Code of the currency of the incoming transactionality.                                                                                                                                                                                |
|                              | inc\_monthly\_movements\*                        |                                   | integer | -      | Number of incoming transactions.                                                                                                                                                                                                      |
|                              | out\_transactionality\*                          |                                   | float   | -      | Approximate amount of outgoing transactions in the month.                                                                                                                                                                             |
|                              | out\_transactionality\_currency\_code\*          |                                   | varchar | 3      | Code of the currency of the outgoing transactionality.                                                                                                                                                                                |
|                              | out\_monthly\_movements\*                        |                                   | integer | -      | Number of outgoing transactions.                                                                                                                                                                                                      |
|                              | funds\_country\_code\*                           |                                   | varchar | 2      | Code of the country of origin and destination of the funds in Alpha2. Example: DO, IT, CO, etc.                                                                                                                                       |
| company\_branches            | name\*                                           |                                   | varchar | 100    | Branch name.                                                                                                                                                                                                                          |
|                              | document\_type\_code\*                           |                                   | varchar | 10     | Document type code. Example: TXID.                                                                                                                                                                                                    |
|                              | document\_number\*                               |                                   | varchar | 100    | Branch identification number.                                                                                                                                                                                                         |
|                              | document\_issuing\_country\_code\*               |                                   | varchar | 2      | Code of the issuing country of the document in Aplha2.                                                                                                                                                                                |
|                              | phone\_number\*                                  |                                   | varchar | 20     | Phone number.                                                                                                                                                                                                                         |
|                              | extension                                        |                                   | integer | -      | Extension number.                                                                                                                                                                                                                     |
|                              | responsible\_name\*                              |                                   | varchar | 100    | Name of a person responsible for the branch.                                                                                                                                                                                          |
|                              | neighbourhood\_description                       |                                   | varchar | 150    | Sector where the branch is located.                                                                                                                                                                                                   |
|                              | description\*                                    |                                   | varchar | 200    | Address where the branch is located.                                                                                                                                                                                                  |
| company\_managers            | first\_name                                      |                                   | varchar | 50     | First name.                                                                                                                                                                                                                           |
|                              | middle\_name                                     |                                   | varchar | 50     | Second name in case the person has. Otherwise, this information does not come.                                                                                                                                                        |
|                              | last\_name                                       |                                   | varchar | 50     | Surname.                                                                                                                                                                                                                              |
|                              | middle\_last\_name                               |                                   | varchar | 50     | Second last name in case the person has. Otherwise, this information does not come.                                                                                                                                                   |
|                              | document\_number                                 |                                   | varchar | 20     | Document number.                                                                                                                                                                                                                      |
|                              | document\_issuing\_country\_code                 |                                   | varchar | 2      | Country issuing document in Alpha2.                                                                                                                                                                                                   |
|                              | document\_type\_code                             |                                   | varchar | 10     | Document type. Example: ide, PAOR, etc.                                                                                                                                                                                               |
|                              | gender                                           |                                   | varchar | 1      | Gender. Example: M, F, NB, NC, O.                                                                                                                                                                                                     |
|                              | birth\_date                                      |                                   | date    | -      | Birthdate. Format YYYY-MM-DD                                                                                                                                                                                                          |
|                              | email                                            |                                   | varchar | 50     | Email                                                                                                                                                                                                                                 |
| company\_treasurers          | first\_name                                      |                                   | varchar | 50     | First name.                                                                                                                                                                                                                           |
|                              | middle\_name                                     |                                   | varchar | 50     | Second name in case the person has. Otherwise, this information does not come.                                                                                                                                                        |
|                              | last\_name                                       |                                   | varchar | 50     | Surname.                                                                                                                                                                                                                              |
|                              | middle\_last\_name                               |                                   | varchar | 50     | Second last name in case the person has. Otherwise, this information does not come.                                                                                                                                                   |
|                              | document\_number                                 |                                   | varchar | 20     | Document number.                                                                                                                                                                                                                      |
|                              | document\_issuing\_country\_code                 |                                   | varchar | 2      | Country issuing document in Alpha2.                                                                                                                                                                                                   |
|                              | document\_type\_code                             |                                   | varchar | 10     | Document type. Example: IDEN, PAOR, etc.                                                                                                                                                                                              |
|                              | gender                                           |                                   | varchar | 1      | Gender. Example: M, F.                                                                                                                                                                                                                |
|                              | birth\_date                                      |                                   | date    | -      | Birthdate. Format YYYY-MM-DD                                                                                                                                                                                                          |
|                              | email                                            |                                   | varchar | 50     | Email                                                                                                                                                                                                                                 |
| company\_partners            | person\_members                                  | first\_name                       | varchar | 50     | First name.                                                                                                                                                                                                                           |
|                              |                                                  | middle\_name                      | varchar | 50     | Second name in case the person has. Otherwise, this information does not come.                                                                                                                                                        |
|                              |                                                  | last\_name                        | varchar | 50     | Last name.                                                                                                                                                                                                                            |
|                              |                                                  | middle\_last\_name                | varchar | 50     | Second last name in case the person has. Otherwise, this information does not come.                                                                                                                                                   |
|                              |                                                  | document\_number                  | varchar | 20     | Document number.                                                                                                                                                                                                                      |
|                              |                                                  | document\_issuing\_country\_code  | varchar | 2      | Country issuing document in Alpha2.                                                                                                                                                                                                   |
|                              |                                                  | document\_type\_code              | varchar | 10     | Document type. Example: IDEN, PAOR, etc.                                                                                                                                                                                              |
|                              |                                                  | gender                            | varchar | 1      | Gender. Example: M, F.                                                                                                                                                                                                                |
|                              |                                                  | birth\_date                       | date    | -      | Birthdate. Format YYYY-MM-DD                                                                                                                                                                                                          |
|                              |                                                  | email                             | varchar | 50     | Email                                                                                                                                                                                                                                 |
|                              | company\_members                                 | name                              | varchar | 250    | Name of the partner company.                                                                                                                                                                                                          |
|                              |                                                  | document\_description             | varchar | 30     | Identification number of the partner company.                                                                                                                                                                                         |
|                              |                                                  | document\_type\_code              | varchar | 20     | Code of the type of identification of the partner company.                                                                                                                                                                            |
|                              |                                                  | document\_issuning\_country\_code | varchar | 2      | Code of the issuing country of the document in Alpha2.                                                                                                                                                                                |
|                              |                                                  | address                           | varchar | 150    | Partner company address                                                                                                                                                                                                               |
|                              |                                                  | company\_members                  | json    | -      | If the company has another company as a partner, the cycle is repeated, otherwise it will be an empty list (\[ ]). The depth of the tree will depend on the number of partner companies and individuals entered by the user.          |
|                              |                                                  | person\_members                   | json    | -      | If the company has other natural persons as partners, then this information will come, otherwise it will be an empty list (\[ ]).                                                                                                     |
| company\_entity\_branch      | branch\_code\*                                   |                                   | varchar | 6      | Entity branch code.                                                                                                                                                                                                                   |
|                              | branch\_name\*                                   |                                   | varchar | 35     | Name of the branch of the entity.                                                                                                                                                                                                     |

**Note:** Those marked with an asterisk (\*) are recommended to be stored in the core. Those without this mark can arrive with the value of null or empty ("").

## Examples

<details>

<summary>Example 1</summary>

```
{
   "company_business_type":{
      "cash_expected_monthly_revenue_currency_code":"USD",
      "employees_range":"20 to 100",
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
      "main_email":"synthex@dev.aclaoverseas.com",
      "company_category_description":"Empresa Individual de Responsabilidad Limitada (EIRL)",
      "total_shares":0.0,
      "additional_field1":"Optional 1",
      "additional_field2":"Optional 2",
      "additional_field3":"Optional 3",
      "founding_date":"2012-07-12",
      "company_category_unique_name":"limited_liability_partnership",
      "name":"SYNTHEX SYSTEMS",
      "code":"2GAf13",
      "url":"synthex.com",
      "document_issuing_country_code":"US",
      "economic_activity_description":"Space Research and Technology",
      "registry_id":"111000013"
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

</details>

<details>

<summary>Example 2</summary>

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
      "employees_range":"20 to 100",
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
      "main_email":"property@example.com",
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

<summary>Example 3</summary>

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
      "economic_activity_code":"523130",
      "economic_activity_description":"Commodity Contracts Dealing",
      "founding_date":"2021-03-03",
      "main_language":"English",
      "name":"VASOLY SRL",
      "code":"35Kcd4",
      "main_email":"vasoly@example.com",
      "url": "vasoly.com",
      "registering_entity_country_code":"DO",
      "registering_entity_name":"DGII",
      "registry_id":"131683045",
      "tax_residence_country_code":"DO",
      "total_shares":70.0,
      "product_request_type":{
         "description":"Product request",
         "code":"product_request"
      }
   },
   "company_main_address":{
      "city_description":"Santo Domingo",
      "neighbourhood_description":"Sabana Centro",
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
   "company_alternate_accounts":[
      {
         "aba_routing":"3333333",
         "account":"123456789",
         "bank_name":"Bank Y"
      }
   ],
   "company_business_type":{
      "cash_expected_monthly_revenue_amount":2700000.0,
      "cash_expected_monthly_revenue_currency_code":"DOP",
      "current_year_sales_amount":3200000.0,
      "current_year_sales_currency_code":"DOP",
      "employees_range":"1 to 20",
      "last_year_sales_amount":3000000.0,
      "last_year_sales_amount_currency_code":"DOP",
      "product_description":"VEGETABLE CULTURE PRODUCTS",
      "product_unique_description":"vegetables"
   },
   "company_suppliers":[
      {
         "supplied_product_description":"VEGETABLE CULTURE PRODUCTSO",
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
         "subsidiary_product_name":"Commodity Advisor"
      }
   ],
   "company_main_customers":[
      {
         "consumed_product_description":"Commodity Company",
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
         "responsible_name":"Andres Rodriguez"
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
            "company_members":[],
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
                        "person_members":[]
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
            "person_members":[]
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
```

</details>

## Responses by the entity

The entity must return, in JSON format, the customer number with the following http status codes:

* **201** - Response successful, client created
* **400** - Error creating the client. It must return the error message to be displayed in the application.

## Sample answers

<details>

<summary>Example 1 - successful (HTTP STATUS CODE 201)</summary>

```
{
	"customer_number": 1234
}
```

</details>

<details>

<summary>Example 2 - Error case (HTTP STATUS CODE 400)</summary>

```
{
"message": "ERROR MESSAGE"
}
```
</details>
