Microservice Cloud Foundry Sample
=====================

[Deutsche Anleitung zum Starten des Beispiels](WIE-LAUFEN.md)

This sample is like the sample for my Microservices Book
 ([English](http://microservices-book.com/) /
 [German](http://microservices-buch.de/)) that you can find at
 https://github.com/ewolff/microservice .

However, this demo uses [Cloud Foundry](https://www.cloudfoundry.org/)
to run the microservices. As a PaaS it requires very little
configuration to make the microservices work on this environment.

This project creates a complete micro service demo system with Cloud
 Foundry.  The services are implemented in Java using Spring and
 Spring Cloud.

It uses three microservices:
- `Order` to process orders.
- `Customer` to handle customer data.
- `Catalog` to handle the items in the catalog.

How to run
------------

See [How to run](HOW-TO-RUN.md).

Remarks on the Code
-------------------

The microservices are:

- [microservice-cloudfoundry-demo-catalog](microservice-cloudfoundry-demo/microservice-cloudfoundry-demo-catalog)  is the application to take care of items.
  
- [microservice-cloudfoundry-demo-customer](microservice-cloudfoundry-demo/microservice-cloudfoundry-demo-customer) is responsible for customers.

-
  [microservice-cloudfoundry-demo-order does](microservice-cloudfoundry-demo/microservice-cloudfoundry-demo-order) implements
  order processing. It uses microservice-cloudfoundry-demo-catalog and
  microservice-cloudfoundry-demo-customer. Hystrix is used for resilience.

Note that the code has no dependencies on Cloud Foundry. Only Spring
Cloud Hystrix is used to add resilience.
