Microservice Cloud Foundry Sample
=====================

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

Cloud Foundry
------------

To use Cloud Foundry you need to install the `cf` command line tool
first, see <https://docs.cloudfoundry.org/cf-cli/install-go-cli.html>.

Then you need to install Cloud Foundry itself. You can either do a
local installation as described at <https://pivotal.io/pcf-dev > or
get an account at a public Cloud Foundry
instance. <https://www.cloudfoundry.org/how-can-i-try-out-cloud-foundry-2016/>
is a list of public Cloud Foundry provider.

Then a simple `cf push` in the directory
`microservice-cloudfoundry-demo` will deploy the microservices and the
Hystrix dashboard.

The application assumes that the microservices are accessible with the
path `local.pcfdev.io` - that is the default for the local Cloud
Foundry installation. A website to access the microservices is
deployed at <http://microservices. local.pcfdev.io/>. The website has
links to all the microservices.


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
