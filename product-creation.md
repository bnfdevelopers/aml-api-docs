---
description: Service available for both natural person and legal person.
cover: >-
  https://images.unsplash.com/photo-1423666639041-f56000c27a9a?crop=entropy&cs=tinysrgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHwxfHxvbmxpbmUlMjBiYW5rfGVufDB8fHx8MTY3NTk4MzYyNg&ixlib=rb-4.0.3&q=80
coverY: 187
---


# Product Creation

## Specifications

**Service URL:**

```bash
https://{URLCLIENTE}/core_product
```

**Method:** POST

**Authentication Type:**  Basic Authentication. For security purposes, the same credentials as the previous service must be used.

## **Input Parameters**

| LEVEL 1               | LEVEL 2                  | LEVEL 3                  | TYPE    | LENGTH | DESCRIPTION                                                                                                                                |
| --------------------- | ------------------------ | ------------------------ | ------- | ---- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| product\_code*        |                          |                          | varchar | 4    | Product code. Must be the same as the entity's code                                                                                        |
| customer\_number*     |                          |                          | integer | -    | Customer number returned in client creation service                                                                                        |
| open\_date*           |                          |                          | date    | -    | Opening date in YYYY-MM-DD format                                                                                                          |
| related\_client\_info |                          |                          | array   | -    | List of related accounts to be created. They can be beneficiaries or joint account holders                                                 |
|                       | customer\_number         |                          | integer | -    | Customer number of the related account in the entity's core                                                                                |
|                       | related\_wisenroll\_code |                          | integer | -    | Wisenroll code of the related account in the entity's core                                                                                 |
|                       | account\_link\_type      |                          | varchar | 2    | Type of link between the person and the account. Check the values in the [Annex](static-data.md), table **Account Link Type**              |
|                       | authorized\_signature    |                          | integer | -    | Flag indicating whether the related person is a signer of the account: 1 for True and 0 for False                                          |
|                       | main\_document           |                          | object  | -    | Object indicating information about the related person's document. Sent as additional information if required                              |
|                       |                          | document\_type\_code     | varchar | 20   | Document type of the related person                                                                                                        |
|                       |                          | document\_number         | varchar | 20   | Document number of the related person                                                                                                      |
|                       |                          | issuing\_number\_code    | varchar | 2    | Country issuing the document in Alpha-2 format                                                                                             |

**Note:** Parameters marked with an asterisk (\*) are recommended to be stored in the core. Those that do not have this mark may have a null value, an empty string ("") or may not appear in the JSON at all. It is recommended to always validate if a value that is not marked with an asterisk is present in the JSON or not in order to handle it.
## Examples

<details>

<summary>Example 1 </summary>

```
{
	"product_code": “AH01”,
	"customer_number": 1234,
	"open_date": “2021-01-02”,
}
```

</details>

<details>

<summary>Example 2 </summary>

```
{
	"product_code": “AH01”,
	"customer_number": 1234,
	"open_date": “2021-01-02”,
	"related_client_info":[
		{
			"authorized_signature":1,
			"customer_number":46,
			"related_wisenroll_code":"K9ZW56",
			"main_document":{
				"document_type_code":"SSN",
				"document_number":"111010001",
				"issuing_country_code":"US"
			},
			"account_link_type_code":"O"
		},
		...
   ],
}
```

</details>

## Responses from the entity

The entity must respond a JSON with the following http status codes:

* **201** - Successful response, account(s)/product(s) created
* **400** - Error assigning the account(s)/product(s) to the customer The error message should be returned to be displayed in the application

The JSON must follow the following format:

| LEVEL 1               | LEVEL 2                  | TYPE    | SIZE   | DESCRIPTION                                                                                                              |
| --------------------- | ------------------------ | ------- | ------ | ------------------------------------------------------------------------------------------------------------------------ |
| account\_info*        |                          | array   |   -    | List of objects that contain the information of the accounts/products that generate the customer or those that gave error|
|                       | action                   | varchar |   -    | Assign the value **CREATE** for successful creation. In case of error assign the value **ERROR**                         |
|                       | account_product          | varchar |   10   | Subproduct code generated to the account. In case of error, indicate the code in the same way                            |
|                       | account_number           | varchar |   20   | Account number                                                                                                           |
|                       | message                  | text    |   -    | In case the value of the **action** field is **ERROR**, indicate the error message in this field                         |

**Notes:** The _**account_product**_ and _**account_number**_ fields must be sent when the action field is **CREATE**. In case of **ERROR** the fields _**account_product**_ and _**message**_ are required.

## Sample answers

<details>

<summary>Example - Successful cases (HTTP STATUS CODE 201)</summary>

```
{
    "accounts_info":[
       {
          "action":"CREATE",
          "account_product":"SAV1",
          "account_number":"000201000903"
       }
    ]
}
```

```
{
    "accounts_info":[
       {
          "action":"CREATE",
          "account_product":"SAV1",
          "account_number":"000201000903"
       },
	   {
          "action":"CREATE",
          "account_product":"SAV2",
          "account_number":"000221000148"
       }
    ]
}
```

</details>

<details>

<summary>Example - Error case (HTTP STATUS CODE 400)</summary>

```
{
    "accounts_info":[
       {
          "action":"ERROR",
          "account_product":"NOW1",
          "message":"ERROR MESSAGE"
       },
	   {
          "action":"ERROR",
          "account_product":"NOW3",
          "message":"ERROR MESSAGE"
       }
    ]
}
```

</details>

***
