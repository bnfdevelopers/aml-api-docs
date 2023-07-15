---
cover: .gitbook/assets/Untitled design (1).png
coverY: 0
---

# AML API DOCUMENTATION


## Contents

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th></th></tr></thead><tbody><tr><td></td><td><a href="creation-update-person.md">Creation / Update of Natural Person Clients</a></td><td></td></tr><tr><td></td><td><a href="creation-update-company.md">Creation / Update of Legal Person Clients</a></td><td></td></tr><tr><td></td><td><a href="product-creation.md">Product Creation</a></td><td></td></tr><tr><td><p></p><p><a href="transaction-inspector.md">Transaction Monitoring</a></p></td><td></td><td></td></tr><tr><td></td><td><a href="https://github.com/bnfdevelopers/aml-api-docs/tree/englissh">Github URL</a></td><td></td></tr><tr><td><p></p><p><a href="static-data.md">Attachments</a></p></td><td></td><td></td></tr></tbody></table>

## Introduction

The Anti-Money Laundering (AML) and Counter Financing of Terrorism (CFT) Platform provided by Better and Faster Tech Inc offers mechanisms for integration with management systems, customer administration, and transactions (e.g. Core Banking) to achieve process automation and comprehensive control. The available functions include:

* **Creation of clients from Clients Inspector to the institution's systems**. Once clients are screened and authorized by the Manager role for their association with the institution, Clients Inspector will invoke the functions described in this document to create the client in the institution's systems. There are two types of clients that can be created from Clients Inspector: Natural Person Clients and Legal Person Clients (Companies).
* **Creation of products from Clients Inspector to the institution's systems**. The product creation is done after the client creation, provided that it was successful. The Manager role will see a product creation request in their inbox, which they must approve. Clients Inspector will invoke the functions described in this document to create the product in the institution's systems.
* **Sending transactions and loading occasional client data**. The Transaction Inspector application needs to be fed with the transactions that the institution decides to monitor. For this purpose, the institution's systems must send the transaction data, including the data of Occasional Clients, such as the depositor. This step is necessary to perform the corresponding simplified due diligence on these third parties, as indicated by the regulations. Transaction sending is a service enabled by Transaction Inspector to be invoked by the institution's systems.

To see the updates and releases, click [here](CHANGELOG.md).&#x20;

To see the attachments containing information about static data, click [here](static-data.md).

The following sections describe the different services that institutions must implement.