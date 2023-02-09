---
cover: .gitbook/assets/Untitled design (1).png
coverY: 0
---

# DOCUMENTACIÓN API AML

## Contenido

<table data-view="cards"><thead><tr><th></th><th></th><th></th></tr></thead><tbody><tr><td><a href="creation-update-person.md">CREACIÓN / ACTUALIZACIÓN DE CLIENTES PERSONA FÍSICA</a></td><td></td><td></td></tr><tr><td><a href="creation-update-company.md">CREACIÓN / ACTUALIZACIÓN DE CLIENTES JURIDICAS</a></td><td></td><td></td></tr><tr><td><a href="product-creation.md">CREACIÓN DE PRODUCTOS PARA PERSONAS FÍSICAS Y JURÍDICAS</a></td><td></td><td></td></tr></tbody></table>

## Introducción

La Plataforma para Prevención de Lavado de Activos y Financiamiento al Terrorismo de Better and Faster Tech Inc dispone de mecanismos para la integración con los sistemas de gestión, administración de clientes y transacciones (p.e. Core Bancario) para lograr la automatización de los procesos y el control de forma integral. Dentro de las funciones disponibles se incluyen:

* **Creación de clientes desde Clients Inspector hacia los sistemas de la entidad**. Una vez que los clientes son depurados y el Rol de Gerente autoriza su vinculación con la institución, Clients Inspector invocará las funciones descritas en este documento para que el cliente sea creado en los sistemas de la institución. Existen dos tipos de clientes que pueden ser creados desde Clients Inspector, Clientes Persona Física y Clientes Jurídicos o Compañías.
* **Creación de productos desde Clients Inspector hacia los sistemas de la entidad**. La creación del producto se realiza después de la creación del cliente, siempre que la misma haya sido exitosa. El Rol Gerente verá una solicitud de creación de producto en su bandeja la cual debe aprobar. Clients inspector invocará las funciones descritas en este documento para que el cliente sea creado en los sistemas de la institución.
* **Envío de transacciones y carga de datos de clientes ocasionales.** La aplicación Transaction Inspector requiere ser alimentada con las transacciones que la institución decida monitorear. Para esto los sistemas de la institución deberán enviar los datos de la transacción donde se deben incluir los datos de los Clientes Ocasionales, por ejemplo el depositante. Este paso es necesario para ejecutar la correspondiente debida diligencia simplificada a estos terceros tal como lo indican las regulaciones. El envío de transacciones son servicios que Transaction Inspector habilita para ser invocados por los sistemas de la Institución.

A continuación se describen los diferentes servicios que deben ser implementados por las instituciones.
