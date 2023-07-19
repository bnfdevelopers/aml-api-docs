---
cover: >-
   https://images.unsplash.com/photo-1491975474562-1f4e30bc9468?crop=entropy&cs=tinysrgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHwxMHx8cGVyc29uJTIwdGVjaG5vbG9neXxl bnwwfHx8fDE2NzU5ODM0NTA&ixlib=rb-4.0.3&q=80
coverY: -196
---

# Creation / Update of Individual Clients

In order to create clients in the core of the financial institution, it is necessary for the client entity to develop a service that allows sending all the information taken from the Clients Inspector application to the core. The service provided by the entity must return the customer number assigned by the core. In case the client already exists in the core, you should be able to update that information with the information sent from Clients Inspector.

## Specifications

**Service URL:**

```bash
https://{ENTITY_CLIENT_URL}/core_customer
```

**Method:** POST

**Authentication Type:** Basic Authentication. For security reasons, the service that the entity's IT team will have to create must be able to handle this type of authentication in order to provide us with access credentials to the service.

## Input Parameters

We will divide the JSON structure (key-value) into three levels: level 1, level 2 and level 3, depending on the type of information provided by the level 2 nodes. The level 1 nodes will be the following:

* **client\_info:** It will indicate the personal information of the client to be created: names, surnames, number of documents, etc.
* **client\_documents:** Data of the client's identification documents. It is returned in a list of dictionaries.
* **client\_location:** Client's home data.
* **client\_contact:** Contact details of the client.
* **client\_job\_info:** Job data of the client.
* **client\_references:** Bank and personal references. Bank references are optional.
* **client\_transactionality:** Client transactionality data.
* **client\_entity\_branch:** Information of the branch of the entity where the client is registering.

> :warning: **IMPORTANT:** We recommend that those fields that are not marked with an asterisk (\*), first validate if that field and/or object is sent in the json or not. This is because there may be cases in which said field and/or object is in the json if there is a value, otherwise it will not appear.

A continuación la descripción de cada campo:

| LEVEL 1                  | LEVEL 2                           | LEVEL 3                       | TYPE    | LENGTH | DESCRIPTION                                                                                                                                                                                                                                                                      |
| ------------------------ | --------------------------------- | ----------------------------- | ------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| client\_info             | first\_name\*                     |                               | varchar | 30     | Customer first name                                                                                                                                                                                                                                                              |
|                          | middle\_name                      |                               | varchar | 30     | Second name of the customer                                                                                                                                                                                                                                                      |
|                          | last\_name\*                      |                               | varchar | 30     | Customer's first last name                                                                                                                                                                                                                                                       |
|                          | middle\_last\_name                |                               | varchar | 30     | Customer's second last name                                                                                                                                                                                                                                                      |
|                          | nickname                          |                               | varchar | 100    | Nickname                                                                                                                                                                                                                                                                         |
|                          | sex\*                             |                               | varchar | 1      | Gender: M (Male) / F (Female) / NB (No Binary) / NC (No Comments) / O (Other)                                                                                                                                                                                                    |
|                          | other\_sex\_description           |                               | varchar | 50     | When sex = O (Other) the gender indicated by the client is sent                                                                                                                                                                                                                  |
|                          | birth\_date\*                     |                               | date    | -      | Birth date. Format YYYY-MM-DD                                                                                                                                                                                                                                                    |
|                          | civil\_status\_id\*               |                               | integer | -      | Civil status code. See Annexes [here](static-data.md)                                                                                                                                                                                                                            |
|                          | civil\_status\_description        |                               | varchar | 50     | Description of marital status                                                                                                                                                                                                                                                    |
|                          | civil\_status\_unique\_description|                               | varchar | 20     | Unique description of marital status. Example: married, single, etc.                                                                                                                                                                                                             |
|                          | nationality\_code                 |                               | varchar | 2      | Country code of nationality in Alpha-2. Example: DO, IT, CO, etc. Ver Anexo 1                                                                                                                                                                                                    |
|                          | other\_nationality\_code          |                               | varchar | 2      | Country code of second nationality in Alpha-2                                                                                                                                                                                                                                    |
|                          | person\_type\_code                |                               | varchar | 10     | Indicates if the person is PEP or RPEP. If not, the value is empty                                                                                                                                                                                                               |
|                          | spouse\_info                      | first\_name                   | varchar | 30     | Spouse's first name                                                                                                                                                                                                                                                              |
|                          |                                   | middle\_name                  | varchar | 30     | Spouse's middle name                                                                                                                                                                                                                                                             |
|                          |                                   | last\_name                    | varchar | 30     | Spouse's last name                                                                                                                                                                                                                                                               |
|                          |                                   | middle\_last\_name            | varchar | 30     | Spouse's second last name                                                                                                                                                                                                                                                        |
|                          |                                   | document\_type\_code          | varchar | 4      | Document type. Review the values in the [annexes](static-data.md) section                                                                                                                                                                                                        |
|                          |                                   | document\_number              | varchar | 15     | Document number                                                                                                                                                                                                                                                                  |
|                          |                                   | issuing\_country\_code        | varchar | 2      | Country issuing the document in Alpha-2                                                                                                                                                                                                                                          |
|                          | immigration\_status               | unique\_name                  | varchar | 50     | Unique immigration status identifier: work\_permit, holidays, resident. See Annexes [here](static-data.md)                                                                                                                                                                       |
|                          |                                   | description                   | varchar | 50     | Immigration status description: Work permit, Vacation, Resident.                                                                                                                                                                                                                 |
|                          | wisenroll\_code*                  |                               | varchar | 6      | Wisenroll code                                                                                                                                                                                                                                                                   |
|                          | additional\_field1                |                               | varchar | 150    | Optional field to store what the client indicates. This field is only added to the structure if it is a valid value. That is, it will only be added to the JSON if there is a value, otherwise not                                                                               |
|                          | additional\_field2                |                               | varchar | 150    | Optional field to store what the client indicates. This field is only added to the structure if it is a valid value. That is, it will only be added to the JSON if there is a value, otherwise not                                                                               |
|                          | additional\_field3                |                               | varchar | 150    | Optional field to store what the client indicates. This field is only added to the structure if it is a valid value. That is, it will only be added to the JSON if there is a value, otherwise not                                                                               |
| client\_documents        |                                   |                               | Array   |        | List of dictionaries of customer identification documents                                                                                                                                                                                                                        |
|                          | document\_number\*                |                               | varchar | 20     | Document number                                                                                                                                                                                                                                                                  |
|                          | document\_type\_code\*            |                               | varchar | 20     | Document type. Review the values in the [annexes](static-data.md) section                                                                                                                                                                                                        |
|                          | issuing\_country\_code\*          |                               | varchar | 2      | Country issuing the document in Alpha-2                                                                                                                                                                                                                                          |
|                          | main\*                            |                               | integer | -      | Flag to label the document as main (1 is main, 0 is not main)                                                                                                                                                                                                                    |
| client\_location         | city\_description\*               |                               | varchar | 30     | Name of the city of residence                                                                                                                                                                                                                                                    |
|                          | country\_code\*                   |                               | varchar | 2      | Country of residence in Alpha-2                                                                                                                                                                                                                                                  |
|                          | country\_description\*            |                               | varchar | 20     | Name of country of residence                                                                                                                                                                                                                                                     |
|                          | locality\_code\*                  |                               | varchar | 6      | Locality code. For USA customers it is the postal code of the locality                                                                                                                                                                                                           |
|                          | locality\_description\*           |                               | varchar | 70     | Locality name                                                                                                                                                                                                                                                                    |
|                          | country\_code\*                   |                               | varchar | 2      | Country code Alpha-2                                                                                                                                                                                                                                                             |
|                          | address\*                         |                               | varchar | 100    | Home address                                                                                                                                                                                                                                                                     |
|                          | housing\_type                     |                               | varchar | 50     | Type of housing. The values it takes are: Propia (Owned) and Alquilada (Rented).                                                                                                                                                                                                 |
|                          | rent\_amount                      |                               | float   | -      | Rental amount in case the type of housing is rented. It can be null.                                                                                                                                                                                                             |
|                          | rent\_currency                    |                               | varchar | 3      | Currency of the rental payment amount.                                                                                                                                                                                                                                           |
|                          | owner                             |                               | varchar | 80     | Owner's name (rented case)                                                                                                                                                                                                                                                       |
|                          | owner\_phone                      |                               | varchar | 30     | Owner's phone number (rented case)                                                                                                                                                                                                                                               |
|                          | postal\_code                      |                               | varchar | 10     | Postal Code / Zip Code                                                                                                                                                                                                                                                           |
|                          | neighbourhood\_description        |                               | varchar | 150    | Neighbourhood                                                                                                                                                                                                                                                                    |
|                          | province\_code                    |                               | varchar | 10     | Province code. See Annexes [here](static-data.md). Will have some value for Dominican Republic addresses                                                                                                                                                                         |
|                          | province\_description             |                               | varchar | 70     | Province name                                                                                                                                                                                                                                                                    |
| client\_contact          | email\*                           |                               | varchar | 50     | Email                                                                                                                                                                                                                                                                            |
|                          | cell\_phone\*                     |                               | varchar | 20     | Cellphone number                                                                                                                                                                                                                                                                 |
|                          | home\_phone                       |                               | varchar | 20     | Home phone number                                                                                                                                                                                                                                                                |
|                          | buss\_phone                       |                               | varchar | 20     | Office phone number                                                                                                                                                                                                                                                              |
|                          | social\_network\_name             |                               | varchar | 45     | Social network name                                                                                                                                                                                                                                                              |
|                          | social\_network\_username         |                               | varchar | 30     | Social network username                                                                                                                                                                                                                                                          |
| client\_job\_info        | salary\*                          |                               | float   | -      | Salary                                                                                                                                                                                                                                                                           |
|                          | annual\_gross\_salary\*           |                               | float   | -      | Annual gross salary                                                                                                                                                                                                                                                              |
|                          | net\_income\*                     |                               | float   | -      | Net Monthly Income                                                                                                                                                                                                                                                               |
|                          | currency\_code\*                  |                               | varchar | 4      | Currency code. See Annexes [here](static-data.md)                                                                                                                                                                                                                                |
|                          | company\_name                     |                               | varchar | 100    | Name of the company where the client works                                                                                                                                                                                                                                       |
|                          | company\_country\_code*           |                               | varchar | 2      | Country code in Alpha-2 where the company is located                                                                                                                                                                                                                             |
|                          | company\_country\_description*    |                               | varchar | 20     | Name of the country where the company is located                                                                                                                                                                                                                                 |
|                          | work\_address\*                   |                               | varchar | 150    | Work address                                                                                                                                                                                                                                                                     |
|                          | work\_email                       |                               | varchar | 100    | Work email                                                                                                                                                                                                                                                                       |
|                          | entry\_date\*                     |                               | date    | -      | Date of entry into the company. Format YYYY-MM-DD                                                                                                                                                                                                                                |
|                          | professional\_position\*          |                               | varchar | 100    | Position within the company                                                                                                                                                                                                                                                      |
|                          | occupation\_code\*                |                               | varchar | 50     | Occupation code. In the case of the Dominican Republic, it will be the CIIU code. For United States will be SOC (Standard Occupation Classification)                                                                                                                             |
|                          | occupation\_description\*         |                               | text    | -      | Occupation description                                                                                                                                                                                                                                                           |
|                          | other\_job\_info                  |                               | object  | -      | Information of additional labor data of the client                                                                                                                                                                                                                               |
|                          |                                   | commercial\_activity\_code    | varchar | 6      | Additional commercial activity code. This information comes from the activities uploaded to the GIR platform.                                                                                                                                                                    |
|                          |                                   | commercial\_activity\_description    | text | -  | Description of the additional commercial activity.                                                                                                                                                                                                                               |
|                          |                                   | country\_code                 | varchar | 2      | Country of business                                                                                                                                                                                                                                                              |
|                          |                                   | country\_description          | varchar | 20     | Business Country Name                                                                                                                                                                                                                                                            |
|                          |                                   | quantity\_of\_employees       | integer | -      | Quantity of employees                                                                                                                                                                                                                                                            |
|                          |                                   | total\_assets                 | float   | -      | Total amount of assets                                                                                                                                                                                                                                                           |
|                          |                                   | income\_in\_another\_currency | float   | -      | Income in another currency expressed in USD. Default is 0                                                                                                                                                                                                                        |
|                          |                                   | origin\_income                | varchar | 100    | Description of the source of income                                                                                                                                                                                                                                              |
|                          | previously\_employment\_data      |                               | object  | -      | Previous employment data if applicable                                                                                                                                                                                                                                           |
|                          |                                   | previous\_boss\_full\_name    | varchar | 200    | Full name of your boss at previous job                                                                                                                                                                                                                                           |
|                          |                                   | previous\_company\_name       | varchar | 50     | Name of the company of your previous employment                                                                                                                                                                                                                                  |
|                          |                                   | previous\_seniority           | integer | -      | Seniority in your previous job in months                                                                                                                                                                                                                                         |
| client\_references       | first\_bank\_reference            | country\_code                 | varchar | 2      | Country code in Alpha-2                                                                                                                                                                                                                                                          |
|                          |                                   | bank\_name                    | varchar | 50     | Bank's name                                                                                                                                                                                                                                                                      |
|                          |                                   | acount\_type\_description     | varchar | 50     | Account type                                                                                                                                                                                                                                                                     |
|                          |                                   | account\_number               | varchar | 50     | Account number                                                                                                                                                                                                                                                                   |
|                          |                                   | officer                       | varchar | 100    | Name of responsible officer                                                                                                                                                                                                                                                      |
|                          |                                   | canceled                      | integer | -      | 1 if it is a canceled account, 0 otherwise                                                                                                                                                                                                                                       |
|                          |                                   | canceled\_reason              | varchar | 200    | Reason for cancellation, in case canceled is true                                                                                                                                                                                                                                |
|                          |                                   | aba\_code                     | varchar | 50     | ABA Routing Number. In the case of banks in the USA                                                                                                                                                                                                                              |
|                          |                                   | iban\_code                    | varchar | 50     | Internation Bank Account number                                                                                                                                                                                                                                                  |
|                          |                                   | swift\_code                   | varchar | 50     | SWIFT code                                                                                                                                                                                                                                                                       |
|                          | second\_bank\_reference           |                               | -       | -      | Same as **first_bank_reference**                                                                                                                                                                                                                                                 |
|                          | first\_personal\_reference\*      | name                          | varchar | 100    | Full name of the reference                                                                                                                                                                                                                                                       |
|                          |                                   | relationship                  | varchar | 50     | Indicates if it is Family (Familiar) or Not Family (No Familiar)                                                                                                                                                                                                                 |
|                          |                                   | reference\_relationship\_unique\_description  | varchar | 50     | Relationship type                                                                                                                                                                                                                                                |
|                          |                                   | phone                         | varchar | 20     | Reference phone number                                                                                                                                                                                                                                                           |
|                          |                                   | address                       | varchar | 150    | Home address                                                                                                                                                                                                                                                                     |
|                          |                                   | document\_number              | varchar | 20     | Document number                                                                                                                                                                                                                                                                  |
|                          |                                   | document\_type\_code          | varchar | 4      | Document type                                                                                                                                                                                                                                                                    |
|                          |                                   | country                       | varchar | 2      | Document issuing country                                                                                                                                                                                                                                                         |
|                          |                                   | email                         | varchar | 150    | Email                                                                                                                                                                                                                                                                            |
|                          | second\_personal\_reference\*     | name                          | varchar | 100    | Same as **first_personal_reference**                                                                                                                                                                                                                                             |
|                          | first\_supplier\_commercial\_reference |                          | -       | -      | Commercial references (Supplier). They are requested when the client has some additional economic activity                                                                                                                                                                       |
|                          |                                   | name                          | varchar | 100    | Supplier full name                                                                                                                                                                                                                                                               |
|                          |                                   | phone                         | varchar | 20     | Supplier phone number                                                                                                                                                                                                                                                            |
|                          |                                   | country                       | varchar | 2      | Country where the supplier is located                                                                                                                                                                                                                                            |
|                          | second\__supplier_\_commercial\_reference |                       | -       | -      | Same as **first_supplier_commercial_reference**                                                                                                                                                                                                                                  |
|                          | first\_client_\_commercial\_reference |                           | -       | -      | Main customers. They are requested when the client has some additional economic activity                                                                                                                                                                                         |
|                          |                                   | name                          | varchar | 100    | Customer name. It can be a company or a natural person                                                                                                                                                                                                                           |
|                          |                                   | phone                         | varchar | 20     | Customer phone number                                                                                                                                                                                                                                                            |
|                          |                                   | country                       | varchar | 2      | Country where the customer is located                                                                                                                                                                                                                                            |
|                          | second\_client_\_commercial\_reference |                          | -       | -      | Same as **first_client_commercial_reference**                                                                                                                                                                                                                                    |
| client\_transactionality | inc\_transactionality\*           |                               | float   | -      | Incoming transaction amount                                                                                                                                                                                                                                                      |
|                          | inc\_transactionality\_currency\* |                               | varchar | 3      | Currency                                                                                                                                                                                                                                                                         |
|                          | inc\_monthly\_movements\*         |                               | integer | -      | Amount of monthly entry movements                                                                                                                                                                                                                                                |
|                          | out\_transactionality\*           |                               | float   | -      | Outgoing transaction amount                                                                                                                                                                                                                                                      |
|                          | out\_transactionality\_currency\* |                               | varchar | 3      | Currency                                                                                                                                                                                                                                                                         |
|                          | out\_monthly\_movements\*         |                               | integer | -      | Number of monthly outgoing movements                                                                                                                                                                                                                                             |
|                          | funds\_country\_code\*            |                               | varchar | 2      | Country code in Alpha-2                                                                                                                                                                                                                                                          |
|                          | funds\_country\_description\*     |                               | varchar | 50     | Country of origin and destination of funds                                                                                                                                                                                                                                       |
| client\_entity\_branch   | branch\_code\*                    |                               | varchar | 6      | Entity branch code                                                                                                                                                                                                                                                               |
|                          | branch\_name\*                    |                               | varchar | 35     | Name of the branch of the entity                                                                                                                                                                                                                                                 |

**Note:** Those marked with an asterisk (\*) are recommended to be stored in the core. Those that do not have this mark can arrive with the value of null, empty (“”) or even not appear in the json.

> :warning: **Important:** It is always recommended to validate that some object and/or field of the json comes in it. Example:

```
data = request.json

# Condition that validates if the key previously_employment_data comes in the json
if data.get('client_job_info').get('previously_employment_data'):
   # Write your code here
```

## Examples

<details>

<summary>Example 1</summary>

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
         "country_description":"United States",
         "commercial_activity_description":"Advertising Agencies"
      },
      "professional_position":"Software Developer Senior",
      "annual_gross_salary":100000.0,
      "occupation_code":"151252",
      "entry_date":"2020-02-21",
      "company_name":"TECH GROUP LLC",
      "company_country_code":"US",
      "company_country_description":"United States",
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

<summary>Example 2</summary>

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
      "company_country_description":"Dominican Republic",
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

<summary>Example 3</summary>
 
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
      "company_country_code":"US",
      "company_country_description":"United States",
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

## Responses by the entity

The entity must return, in JSON format, the customer number with the following http status codes:

* **201** - Response successful, client created
* **4XX** - Error creating client. It must return the error message to be displayed in the application.

## Sample answers

<details>

<summary>Example 1 - Success (HTTP STATUS CODE 201)</summary>

```
{
	"customer_number": 1234
}
```

</details>

<details>

<summary>Example 2 - Error (HTTP STATUS CODE 4XX)</summary>

```
{
	"message": "ERROR MESSAGE"
}
```

</details>
