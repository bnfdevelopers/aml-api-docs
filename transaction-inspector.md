---
cover: >-
  https://images.unsplash.com/photo-1526628953301-3e589a6a8b74?crop=entropy&cs=tinysrgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHw0fHx0cmFuc2FjdGlvbnN8ZW58MHx8fHwxNjc1OTg4MDc4&ixlib=rb-4.0.3&q=80
coverY: 0
---

<style scoped>
table {
  font-size: 10px;
}
</style>

# Transaction Monitoring

This is a service provided by the Better and Faster company for the asynchronous loading of transactions and non-customers that will serve as a data source for the deployment of the transaction monitoring suite.

**Non-clients:** Refers to the information of third parties who do not belong to the entity's portfolio but who participate in the transactions. We also call it **occasional customers**.

## Specifications

**Service URL:**

```bash
http://{BNF_API_URL}/process_request_transactions
```

**Method:** POST

**Authentication Type:** Basic Authentication. For security, a username and password will be provided to be able to make the request to the service.

**Error Codes:**

**5XX** - Server errors, please try again. If the error persists, contact us directly.

**4XX** - Format errors, dependencies, validations, among others. Please read the return message. The platform will return a list with the transaction(s) that returned an error. In each element of that list there will be a key called **errors** (it can have either an error message or a list with the fields of the transaction that gave error) and another key called **transaction\_data** (information of the transaction that failed). See examples later.

**Rate limit:** None, beyond the limits imposed by the engine of the server where the API is deployed.

**Additional Observations:** There is the possibility of performing a synchronous load supplying the flag: execute = True. However, its use is not recommended for bulk uploads as it is a slow and expensive process.

**Preliminaries:** Actions that must be executed before invoking the services provided by this API.

* **Transaction Codes:** A list of all the possible transaction codes managed by the core of the entity must be provided so that they can be mapped with those managed by the API.

## **Input Parameters**

We will divide the JSON structure (key-value) into three levels: level 1, level 2 and level 3, depending on the type of information provided by the level 2 nodes. The level 1 nodes will be the following:

* **transactions:** This is a list of transactions to be recorded that may or may not include people who are not clients of the entity for the protection of data associated with operations such as: box office deposits, check cashing, etc.
* **execute:** This is a flag that, when enabled, creates the records synchronously. By default this value is false when it is not included in the JSON body. Review Additional Observations for more information.

| **LEVEL 1**             | **LEVEL 2**                       | **LEVEL 3**                  | **TYPE** | **SIZE**| **DESCRIPTION**                                                                                                                                                                                             |
| ----------------------- | --------------------------------- | ---------------------------- | -------- | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><br>transactions</p> | transaction\_id\*                 |                              | varchar  | 50      | <p>Unique identifier of the transaction given by the entity.<br>Example: TR33457741</p>                                                                                                                     |
|                         | date                              |                              | datetime | -       | <p>Date on which the transaction was executed.<br>It must be expressed in ISO 8601, extended format of joint dates and times, as follows:<br>YYYY-MM-DDTHH:MM:SS.<br>Example : (2020-11-03T24:12:03)</p>    |
|                         | amount\*                          |                              | float    | -       | <p>Transaction amount.<br>Example: 478.56</p>                                                                                                                                                               |
|                         | origin\_bank\_id                  |                              | varchar  | 25      | <p>Unique identifier of the bank of origin. This code can be the BIC of the entity in case it is a Swift entity or the UAF code.<br>Example: BELNDOSD</p>                                                   |
|                         | origin\_account                   |                              | varchar  | 25      | <p>Origin bank account number.<br>Example: 000221001794</p>                                                                                                                                                 |
|                         | origin\_identification            |                              | varchar  | 30      | <p>Originator document number.<br>Example: 12345678903</p>                                                                                                                                                  |
|                         | origin\_identification\_type      |                              | varchar  | 10      | <p>Type of the identity document of the person of origin in the transaction. See [annexes](static-data.md) <br>Example: SSN</p>                                                                             |
|                         | origin\_country\_code             |                              | varchar  | 10      | <p>Country code of the identity document of the person of origin in the transaction.<br>It must be expressed in Alpha-2.<br>Example: DO</p>                                                                 |
|                         | destination\_bank\_id             |                              | varchar  | 20      | Unique identifier of the destination bank. This code can be the BIC of the entity in case it is a Swift entity or the UAF code. Example: BELNDOSD                                                           |
|                         | destination\_account              |                              | varchar  | 25      | Destination account number. Example: 221002898                                                                                                                                                              |
|                         | destination\_identification       |                              | varchar  | 30      | Unique identifier of the identity document of the destination person in the transaction. Example: 002114011014                                                                                              |
|                         | destination\_identification\_type |                              | varchar  | 10      | Type of identity document of the destination person in the transaction. See [annexes](static-data.md) <br>Example: IDEN</p>                                                                                 |
|                         | destination\_country\_code        |                              | varchar  | 10      | <p>Country of the identity document of the destination person in the transaction. It must be expressed in Alpha-2.<br>Example: DO</p>                                                                       |
|                         | note\*\*                          |                              | text     | -       | Note or comment included in the transaction. Example: Rent payment                                                                                                                                          |
|                         | currency\*                        |                              | varchar  | 10      | <p>Type of currency of the transaction. It must be expressed in ISO 4217.<br>Example: DOP</p>                                                                                                               |
|                         | branch\*                          |                              | varchar  | 20      | Identifier, in your bank CORE, of the branch of the entity in which the transaction was carried out or, by default, in which the client is subscribed. Example 01                                           |
|                         | transaction\_code\*               |                              | varchar  | 25      | Transaction Type. IMPORTANT: These values must be in the list provided in the Transaction Codes item in the Preliminaries section. Example: DE                                                              |
|                         | reversed                          |                              | boolean  | -       | Boolean to determine whether or not it is a reversed transaction. By default send in False. Ejemplo: False                                                                                                  |
|                         | user\*                            |                              | varchar  | 10      | Unique identifier of the operator that registered the transaction in the CORE. Example: COREUSER1                                                                                                           |
|                         | origin\_non\_customer             | first\_name\*                | varchar  | 50      | Name of the non-customer of origin. Example: Juan                                                                                                                                                           |
|                         |                                   | last\_name\*                 | varchar  | 50      | Last name of the non-customer of origin. Example: Pérez                                                                                                                                                     |
|                         |                                   | email\*\*                    | varchar  | 50      | Email address of the originating non-customer. Example: johnson@example.com                                                                                                                                 |
|                         |                                   | gender\*\*                   | varchar  | 50      | Gender of non-customer of origin: F or M. Example: M                                                                                                                                                        |
|                         |                                   | phone\_number\*\*            | varchar  | 50      | Telephone number of the non-customer of origin. Example: +18297320014                                                                                                                                       |
|                         |                                   | role\*                       | varchar  | 50      | Role of the target non-customer. Takes the values of deposit for deposits, withdrawal for withdrawals and beneficiary for transfer to external account.                                                     |
|                         |                                   | citizenship\*\*              | varchar  | 50      | <p>Code of the country of citizenship of the non-customer of origin. It must be expressed in Alpha-2.<br>Example: DO</p>                                                                                    |
|                         |                                   | birth\_date\*\*              | date     | -       | <p>Date of birth of the non-customer of origin.<br>It must be expressed in ISO 8601, extended date format, as follows:<br>YYYY-MM-DD<br>Example: (2020-11-03) </p>                                          |
|                         |                                   | address\*\*                  | varchar  | 50      | Address of the non-customer of origin. Example: Avenida Romulo Betancourt 2154                                                                                                                              |
|                         |                                   | residence\_city\*\*          | varchar  | 50      | City of residence of the non-customer of origin. Example: MIAMI                                                                                                                                             |
|                         |                                   | residence\_country\_code\*\* | varchar  | 50      | <p>Code of the country of residence of the non-customer of origin. It must be expressed in Alpha-2.<br>Example: DO</p>                                                                                      |
|                         | destination\_non\_customer        | first\_name\*                | varchar  | 50      | Name of the destination non-customer. Example: Juan                                                                                                                                                         |
|                         |                                   | last\_name\*                 | varchar  | 50      | Last name of the destination non-client. Example: Pérez                                                                                                                                                     |
|                         |                                   | email\*\*                    | varchar  | 50      | Email address of the target non-customer. Example: johnson@example.com                                                                                                                                      |
|                         |                                   | gender\*\*                   | varchar  | 50      | Gender of the target non-customer. You must provide the label you use to differentiate between male and female in your banking CORE. Example: M                                                             |
|                         |                                   | phone\_number\*\*            | varchar  | 50      | Telephone number of the destination non-customer. Example: +18297320014                                                                                                                                     |
|                         |                                   | role\*                       | varchar  | 50      | Role of the target non-customer. It takes the values of deposit for deposits, withdrawal for withdrawals and beneficiary for transfer to an external account                                                |
|                         |                                   | citizenship\*\*              | varchar  | 50      | <p>Code of the country of citizenship of the destination non-customer. It must be expressed in Alpha-2.<br><br>Example: DO</p>                                                                              |
|                         |                                   | birth\_date\*\*              | date     | -       | <p>Date of birth of the destination non-customer.<br>It must be expressed in ISO 8601, extended date format, as follows:<br>YYYY-MM-DD<br>Example: (2020-11-03) </p>                                        |
|                         |                                   | address\*\*                  | varchar  | 50      | Address of the destination non-customer. Example: 175 SW 7TH ST                                                                                                                                             |
|                         |                                   | residence\_city\*\*          | varchar  | 50      | <p>City of residence of the destination non-customer. Example: NEW YORK<br></p>                                                                                                                             |
|                         |                                   | residence\_country\_code\*\* | varchar  | 50      | <p>Code of the country of residence of the destination non-customer. It must be expressed in Alpha-2.<br>Example: DO, IT, CO, etc</p>                                                                       |
|                         | origin\_account\_info             | customer\_number\*           | varchar  | 25      | Customer number of the owner of the source account                                                                                                                                                          |
|                         |                                   | currency\_code\*             | varchar  | 25      | Source account base currency code. See currency table in the annexes.                                                                                                                                       |
|                         |                                   | product\_code\*              | varchar  | 20      | Product code associated with the source account. This code must be those indicated by the entity in the implementation process.                                                                             |
|                         |                                   | subproduct\_code\*           | varchar  | 20      | Sub-product code associated with the source account. This code must be those indicated by the entity in the implementation process                                                                          |
|                         |                                   | branch\_code\*               | varchar  | 6       | Code of the branch to which the source account is associated                                                                                                                                                |
|                         |                                   | opening\_date                | date     | -       | Origin account opening date                                                                                                                                                                                 |
|                         |                                   | gross\_balance               | float    | -       | Gross balance of the source account                                                                                                                                                                         |
|                         |                                   | net\_balance                 | float    | -       | Net balance of the source account                                                                                                                                                                           |
|                         | destination\_account\_info        | customer\_number\*           | varchar  |         | Customer number of the destination account owner                                                                                                                                                            |
|                         |                                   | currency\_code\*             | varchar  |         | Code of the base currency of the destination account. See currency table in the annexes                                                                                                                     |
|                         |                                   | product\_code\*              | varchar  |         | Product code associated with the destination account. This code must be those indicated by the entity in the implementation process                                                                         |
|                         |                                   | branch\_code\*               | varchar  |         | Code of the branch to which the destination account is associated                                                                                                                                           |
|                         |                                   | subproduct\_code\*           | varchar  | 20      | Sub-product code associated with the destination account. This code must be those indicated by the entity in the implementation process                                                                     |
|                         |                                   | opening\_date                | date     | -       | Destination account opening date                                                                                                                                                                            |
|                         |                                   | gross\_balance               | float    | -       | Gross balance of destination account                                                                                                                                                                        |
|                         |                                   | net\_balance                 | float    | -       | Net balance of destination account                                                                                                                                                                          |
| execute                 |                                   |                              | boolean  | -       | Boolean to determine if the execution will be synchronous. True/False. Default: false. (Check additional observations)                                                                                      |

<mark style="color:red;">\*Mandatory / \*\*Recommended</mark>

The previous Table shows the labels **origin\_non\_customer** and **destination\_non\_customer**. These fields serve to indicate the information of those people involved in a type of transaction (for example, deposits, withdrawals and recipient of a transfer to an external account) but who are not clients of the entity.

## Important aspects to take into account

1. The fields:

* origin\_identification
* origin\_identification\_type
* origin\_country\_code

correspond to the identity data of the originator. The recipient information is given by the following fields:

* destination\_identification
* destination\_identification\_type
* destination\_country\_code

These fields, despite not being required depending on the type of operation, must be sent when there is no source or destination account respectively. Each transaction must have, at least, either the customer's identity data, or the origin account data (origin\_account, origin\_bank\_id).

2. The origin\_bank\_id and destination\_bank\_id fields correspond to the unique identifiers of the banks that own the accounts of origin and destination, these codes can be either the SWIFT code, or the code granted by the UAF.
3. The origin\_account\_info field corresponds to the origin account information indicated in the origin\_account field. Among that information is the owner of it. This to store the data in our platform in case that account has been assigned to the client directly in the core of the entity. It is important to indicate that the customer indicated in the customer\_number field must previously exist in our database.
4. The destination\_account\_info field corresponds to the destination account information indicated in the destination\_account field. Among that information is the owner of it. This to store the data in our platform in case that account has been assigned to the client directly in the core of the entity. It is important to indicate that the customer indicated in the customer\_number field must previously exist in our database.
5. The system receives a list of transactions and those that throw an error will be returned in the body of the service response with the http status code 400 with the error message(s). See sample error responses below.

Below we will explain what information must be sent for deposit, withdrawal and account-to-account transfer transactions.

### Case deposit

When a deposit is made, the depositor (who may or may not be a client of the entity) does not have an origin account, since he delivers the funds in cash, in this case, only the identification fields of the origin client are sent, while for the recipient, the account number and identification of the bank (which would be the bank that receives the funds) is sent: destination\_account and destination\_bank\_id (See Table 2.0).

### Case withdrawal

When a withdrawal is made, the person making the withdrawal (who may or may not be a client of the entity) does not have a destination account, since they receive the funds in cash; in this case, only the identification fields of the client of destination, while for the origin, the account number and bank identification fields (which would be the bank that grants the funds) are sent: origin\_account and origin\_bank\_id. It is important to note that the origin is the account where the funds come from and the destination is the client who withdraws, since the direction of the transactions is always defined from the origin to the destination.

### Case transfer account to account

When a transfer is made between accounts within the same (internal) entity, it is not necessary to send the identification data of the people of origin and destination, it is enough to send the data of their bank accounts together with the identifier of the respective banks: origin \_bank\_id and origin\_account for the originator, destination\_bank\_id and destination\_account for the recipient. In the case of transfers between an internal account and an external one, it is necessary to provide the destination\_non\_customer data (when it is an outgoing transfer). And in the case of a transfer between an external account and an internal one, it is necessary to provide the origin\_non\_customer data (when it is an incoming transfer).

## Examples of the request body:

<details>

<summary>Example 1</summary>

Depositor is not a customer of the entity, therefore information is sent in origin\_non\_customer:

```
{
    "transactions": [
        {
            "transaction_id": "TR33457741",
            "date": "2020-10-16T21:27:59.469957Z",
            "amount": "478.56",
            "origin_identification": "12345678903",
            "origin_identification_type": "IDEN",
            "origin_country_code": "DO",
            "destination_bank_id": "BELNDOSD",
            "destination_account": "221002898",
            "note": "Service payment deposit",
            "currency": "USD",
            "branch": "0",
            "transaction_code": "DE",
            "user": "AGD0088",
            "origin_non_customer": {
                "first_name": "adiel",
                "last_name": "dominguez",
                "email": "adieldominguez@dev.aclaoverseas.com",
                "gender": "M",
                "phone_number": "+18297320014",
                "role": "deposit",
                "citizenship": "DO",
                "birth_date": "2020-05-14",
                "address": "Avenida Romulo Betancourt 2154",
                "residence_city": "SANTO DOMINGO DE GUZMAN",
                "residence_country_code": "DO"
            }
        }
    ]
}
```

</details>

<details>

<summary>Example 2</summary>

Deposit case where the depositor is a client of the entity. No origin\_non\_customer data is sent:

```
{
    "transactions": [
        {
            "transaction_id": "TR33457742",
            "date": "2020-10-16T21:27:59.469957Z",
            "amount": "478.56",
            "origin_identification": "12345678903",
            "origin_identification_type": "IDEN",
            "origin_country_code": "DO",
            "destination_bank_id": "BELNDOSD",
            "destination_account": "221002898",
            "note": "Service payment deposit",
            "currency": "RD$",
            "branch": "1",
            "transaction_code": "DE",
            "reversed": false,
            "user": "AGD0088"
        }
    ]
}
```

</details>

<details>

<summary>Example 3</summary>

Withdrawal case where the person who receives the money is not a customer of the entity, so the information is sent in destination\_non\_customer:

```
{
    "transactions": [
        {
            "transaction_id": "TR33457742",
            "date": "2020-10-16T21:27:59.469957Z",
            "amount": "478.56",
            "origin_bank_id": "BELNDOSD",
            "origin_account": "000214111112",
            "destination_identification": "12345678903",
            "destination_identification_type": "IDEN",
            "destination_country_code": "DO",
            "note": "Withdrawal note",
            "currency": "USD",
            "branch": "0",
            "transaction_code": "RT",
            "reversed": "False",
            "user": "AGD0088",
            "destination_non_customer": {
                "first_name": "adiel",
                "last_name": "dominguez",
                "email": "adieldominguez@dev.bellbank.com",
                "gender": "M",
                "phone_number": "+18297320014",
                "role": "withdrawal",
                "citizenship": "DO",
                "birth_date": "2020-05-14",
                "address": "Avenida Romulo Betancourt 2154",
                "residence_city": "SANTO DOMINGO DE GUZMAN",
                "residence_country_code": "DO"
            }
        }, ...
    ]
}
```

</details>

<details>

<summary>Example 4</summary>

Withdrawal case where the recipient of the money is a customer of the entity, therefore the information in destination\_non\_customer is not sent:

```
{
    "transactions": [
        {
            "transaction_id": "TR33457742",
            "date": "2020-10-16T21:27:59.469957Z",
            "amount": "200.00",
            "origin_bank_id": "BELNDOSD",
            "origin_account": "000221001794",
            "destination_identification": "12345678903",
            "destination_identification_type": "IDEN",
            "destination_country_code": "DO",
            "note": "Withdrawal note",
            "currency": "usd",
            "branch": "0",
            "transaction_code": "RT",
            "reversed": false,
            "user": "AGD0088",
        },
…
    ]
}
```

</details>

<details>

<summary>Example 5</summary>

Case transfer account to account where the origin and destination are accounts within the entity (internal transfers):

```
{
    "transactions": [
        {
            "transaction_id": "TR33457742",
            "date": "2020-10-16T21:27:59.469957Z",
            "amount": "478.56",
            "origin_bank_id": "BELNDOSD",
            "origin_account": "000221001794",
            "destination_bank_id": "BELNDOSD",
            "destination_account": "221002898",
            "note": "Internal transfer",
            "currency": "USD",
            "branch": "0",
            "transaction_code": "TRANSINT",
            "reversed": false,
            "user": "AGD0088",
        }, ...
    ]
}
```

</details>

<details>

<summary>Example 6</summary>

Transfer case where the origin is an account of the entity and the recipient is an account in another entity (beneficiary). External transfers:

```
{
    "transactions": [
        {
            "transaction_id": "TR33457742",
            "date": "2020-10-16T21:27:59.469957Z",
            "amount": "478.56",
            "origin_bank_id": "BELNDOSD",
            "origin_account": "000221001794",
            "destination_bank_id": "BELNDOSD",
            "destination_account": "221001111",
            "destination_identification": "12345678903",
            "destination_identification_type": "IDEN",
            "destination_country_code": "DO",
            "note": "Transfer to another entity",
            "currency": "USD",
            "branch": "0",
            "transaction_code": "TRANSEXT",
            "reversed": false,
            "user": "AGD0088",
            "destination_non_customer": {
                "first_name": "adiel",
                "last_name": "dominguez",
                "email": "adieldominguez@dev.bellbank.com",
                "gender": "M",
                "phone_number": "+18297320014",
                "role": "beneficiary",
                "citizenship": "DO",
                "birth_date": "2020-05-14",
                "address": "Avenida Romulo Betancourt 2154",
                "residence_city": "SANTO DOMINGO DE GUZMAN",
                "residence_country_code": "DO"
            }
        }
    ]
}
```

</details>

<details>

<summary>Example 7</summary>

Transfer case where the origin is an account of another entity and the recipient is an account of a client of the entity. External transfers:

```
{
    "transactions": [
        {
            "transaction_id": "TR33457742",
            "date": "2020-10-16T21:27:59.469957Z",
            "amount": "350.50",
            "origin_bank_id": "BELNDOSD",
            "origin_account": "000221001794",
            "origin_identification": "12345678903",
            "origin_identification_type": "IDEN",
            "origin_country_code": "DO",
            "destination_bank_id": "BELNDOSD",
            "destination_account": "221002898",
            "note": "Incoming External Transfer",
            "currency": "USD",
            "branch": "0",
            "transaction_code": "TRANEXT",
            "reversed": false,
            "user": "AGD0088",
            "origin_non_customer": {
                "first_name": "adiel",
                "last_name": "dominguez",
                "email": "adieldominguez@dev.bellbank.com",
                "gender": "M",
                "phone_number": "+18297320014",
                "role": "beneficiary",
                "citizenship": "DO",
                "birth_date": "2020-05-14",
                "address": "Avenida Romulo Betancourt 2154",
                "residence_city": "SANTO DOMINGO DE GUZMAN",
                "residence_country_code": "DO"
            }
        }, ...
    ]
}
```

</details>

<details>

<summary>Example 8</summary>

Deposit case where the depositor is a client of the entity and the destination account does not exist in our platform. No origin\_non\_customer data is sent:

```
{
    "transactions":
    [
        {
            "transaction_id": "TR33457746",
            "date": "2022-08-04T21:27:59.469957Z",
            "amount": "500000.0",
            "origin_identification": "12345678903",
            "origin_identification_type": "IDEN",
            "origin_country_code": "DO",
            "destination_bank_id": "BELNDOSD",
            "destination_account": "221002898",
            "currency": "USD",
            "branch": "1",
            "transaction_code": "TRANEXT",
            "reversed": false,
            "note": "DEPOSIT",
            "user": "AGD0088",
            "destination_account_info": {
                "customer_number": "8408",
                "currency_code": "USD",
                "product_code": "1",
                "subproduct_code": "2",
                "branch_code": "01",
                "opening_date": "2022-12-20"
            }
        }
    ]
}
```

</details>

<details>

<summary>Example 9</summary>

Withdrawal case where the recipient of the money is a client of the entity, therefore the information in destination\_non\_customer is not sent. The source account information is sent:

```
{
    "transactions": [
        {
            "transaction_id": "TR33457742",
            "date": "2020-10-16T21:27:59.469957Z",
            "amount": "478.56",
            "origin_bank_id": "BELNDOSD",
            "origin_account": "000221001794",
            "destination_identification": "12345678903",
            "destination_identification_type": "IDEN",
            "destination_country_code": "DO",
            "note": "Withdrawal note",
            "currency": "RD$",
            "branch": "0",
            "transaction_code": "RT",
            "reversed": false,
            "user": "AGD0088",
            "origin_account_info": {
                "customer_number": "8999",
                "currency_code": "RD$",
                "product_code": "1",
                "subproduct_code": "2",
                "branch_code": "01",
                "opening_date": "2021-12-20"
            }
        },
…
    ]
}
```

</details>

<details>

<summary>Example 10</summary>

Case transfer account to account where the origin and destination are accounts within the entity (internal transfers). If both accounts do not exist, both are sent. If only one exists, one of the two or even neither can be sent:

```
{
    "transactions": [
        {
            "transaction_id": "TR33457742",
            "date": "2020-10-16T21:27:59.469957Z",
            "amount": "478.56",
            "origin_bank_id": "BELNDOSD",
            "origin_account": "000221001794",
            "destination_bank_id": "BELNDOSD",
            "destination_account": "221002898",
            "note": "Internal transfer",
            "currency": "USD",
            "branch": "0",
            "transaction_code": "TRANSINT",
            "reversed": false,
            "user": "AGD0088",
            "origin_account_info": {
                "customer_number": "8999",
                "currency_code": "USD",
                "product_code": "2",
                "subproduct_code": "2",
                "branch_code": "01",
                "opening_date": "2021-12-20"
            },
            "destination_account_info": {
                "customer_number": "8408",
                "currency_code": "USD",
                "product_code": "1",
                "subproduct_code": "3",
                "branch_code": "01",
                "opening_date": "2021-12-20"
            }
        }, ...
    ]
}
```

</details>

## Sample answers

<details>

<summary>Example 1 - Success (HTTP STATUS CODE 204)</summary>

```
Does not return any response body
```

</details>

<details>

<summary>Example 2 - Error case (HTTP STATUS CODE 400). Unsent amount field</summary>

```
[
    {
        "errors": {
            "amount": [
                "This field is required."
            ]
        },
        "transaction_data": {
            "transaction_id": "TR33457746",
            "date": "2022-08-04T21:27:59.469957Z",
            "origin_identification": "123456678909",
            "origin_identification_type": "IDEN",
            "origin_country_code": "DO",
            "destination_bank_id": "BELNOSD",
            "destination_account": "00100000101",
            "currency": "RD$",
            "branch": "1",
            "transaction_code": "DE",
            "reversed": "False",
            "note": "DEPOSIT",
            "user": "AGDUSER"
        }
    }
]
```

</details>

<details>

<summary>Example 3 - Error case (HTTP STATUS CODE 400). Repeated transaction id</summary>

```
[
    {
        "errors": {
            "transaction_id": [
                "transaction with this transaction id already exists."
            ]
        },
        "transaction_data": {
            "transaction_id": "TR33457746",
            "date": "2022-08-04T21:27:59.469957Z",
            "origin_identification": "123456678909",
            "origin_identification_type": "IDEN",
            "origin_country_code": "DO",
            "destination_bank_id": "BELNDOSD",
            "destination_account": "00100000101",
            "currency": "RD$",
            "branch": "1",
            "transaction_code": "DE",
            "reversed": "False",
            "note": "DEPOSIT",
            "user": "AGDUSER"
        }
    }
]
```

</details>

<details>

<summary>Example 4</summary>

Error case (HTTP STATUS CODE 400). Case when the customer number provided account information does not exist. This example shows the error of two transactions sent simultaneously in the service:

```
[
    {
        "errors": "Customer does not exist with the customer number provided.",
        "transaction_data": {
            "transaction_id": "TR33457746",
            "date": "2022-08-04T21:27:59.469957Z",
            "amount": "500000.0",
            "origin_identification": "1234567990",
            "origin_identification_type": "IDEN",
            "origin_country_code": "DO",
            "destination_bank_id": "BELNDOSD",
            "destination_account": "001284848",
            "currency": "RD$",
            "branch": "1",
            "transaction_code": "DE",
            "reversed": "False",
            "note": "DEPOSIT",
            "user": "JANDUJAR",
            "destination_account_info": {
                "customer_number": "8408",
                "currency_code": "RD$",
                "product_code": "1",
                "branch_code": "01",
                "opening_date": "2022-12-20"
            }
        }
    },
    {
        "errors": "Customer does not exist with the customer number provided.",
        "transaction_data": {
            "transaction_id": "TR33457747",
            "date": "2022-08-04T21:27:59.469957Z",
            "amount": "500000.0",
            "origin_identification": "12345678909",
            "origin_identification_type": "PAOR",
            "origin_country_code": "DO",
            "destination_bank_id": "BELNDOSD",
            "destination_account": "0012122444",
            "currency": "USD",
            "branch": "1",
            "transaction_code": "DE",
            "reversed": "False",
            "note": "DEPOSIT",
            "user": "JANDUJAR",
            "origin_account_info": {
                "customer_number": "83361",
                "currency_code": "USD",
                "product_code": "1",
                "branch_code": "01",
                "opening_date": "2021-12-20"
            },
            "destination_account_info": {
                "customer_number": "8410",
                "currency_code": "USD",
                "product_code": "1",
                "branch_code": "01",
                "opening_date": "2021-12-20"
            }
        }
    }
]
```

</details>

***
