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

- [microservice-consul-demo-catalog is the application to take care of items.
- microserivce-consul-demo-customer is responsible for customers.
- microservice-consul-demo-order does order processing. It uses
  microservice-demo-catalog and microservice-demo-customer. Ribbon is
  used for load balancing and Hystrix for resilience.


The microservices have a Java main application in `src/test/java` to
run them stand alone. `microservice-demo-order` uses a stub for the
other services then. Also there are tests that use _consumer-driven
contracts_. That is why it is ensured that the services provide the
correct interface. These CDC tests are used in microservice-demo-order
to verify the stubs. In `microservice-demo-customer` and
`microserivce-demo-catalog` they are used to verify the implemented
REST services.

Note that the code has no dependencies on Kubernetes. Only Spring
Cloud Hystrix is used to add resilience.
