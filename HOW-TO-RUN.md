# How to Run

This is a step-by-step guide how to run the example:

## Installation

* The example is implemented in Java. See
   https://www.java.com/en/download/help/download_options.xml . The
   examples need to be compiled so you need to install a JDK (Java
   Development Kit). A JRE (Java Runtime Environment) is not
   sufficient. After the installation you should be able to execute
   `java` and `javac` on the command line.

* To use Cloud Foundry you need to install the `cf` command line tool, see https://docs.cloudfoundry.org/cf-cli/install-go-cli.html .

* Then you need to install Cloud Foundry itself. You can either do a
local installation as described at https://pivotal.io/pcf-dev or
get an account at a public Cloud Foundry
instance. https://www.cloudfoundry.org/how-can-i-try-out-cloud-foundry-2016/
is a list of public Cloud Foundry provider.

* Note that the example need quite some RAM. It might be good idea to
assign e.g. 8 GB `cf dev start -m 8086`. This has to be done on the
very first start of the local Cloud Foundry instance. If you want to
change it later on you need to destroy the current Cloud Foundry
installation first: `cf dev destroy`.

```
[~/microservice-kubernetes/microservice-kubernetes-demo]cf dev start
Using existing image.
Starting VM...
Provisioning VM...
Waiting for services to start...
7 out of 56 running
7 out of 56 running
7 out of 56 running
7 out of 56 running
38 out of 56 running
56 out of 56 running
 _______  _______  _______    ______   _______  __   __
|       ||       ||       |  |      | |       ||  | |  |
|    _  ||       ||    ___|  |  _    ||    ___||  |_|  |
|   |_| ||       ||   |___   | | |   ||   |___ |       |
|    ___||      _||    ___|  | |_|   ||    ___||       |
|   |    |     |_ |   |      |       ||   |___  |     |
|___|    |_______||___|      |______| |_______|  |___|
is now running.
To begin using PCF Dev, please run:
   cf login -a https://api.local.pcfdev.io --skip-ssl-validation
Apps Manager URL: https://local.pcfdev.io
Admin user => Email: admin / Password: admin
Regular user => Email: user / Password: pass
```


## Build

Change to the directory `microservice-cloudfoundry-demo` and run
`./mvnw clean package` or `mvnw.cmd clean package` (Windows). This
will take a while:

```
[~/microservice-cloudfoundry/microservice-cloudfoundry-demo]./mvnw clean package
...
[INFO] 
[INFO] --- maven-jar-plugin:2.6:jar (default-jar) @ microservice-cloudfoundry-demo-order ---
[INFO] Building jar: /Users/wolff/microservice-cloudfoundry/microservice-cloudfoundry-demo/microservice-cloudfoundry-demo-order/target/microservice-cloudfoundry-demo-order-0.0.1-SNAPSHOT.jar
[INFO] 
[INFO] --- spring-boot-maven-plugin:1.4.5.RELEASE:repackage (default) @ microservice-cloudfoundry-demo-order ---
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary:
[INFO] 
[INFO] microservice-cloudfoundry-demo ..................... SUCCESS [  1.327 s]
[INFO] microservice-cloudfoundry-demo-hystrix-dashboard ... SUCCESS [  3.602 s]
[INFO] microservice-cloudfoundry-demo-customer ............ SUCCESS [ 22.158 s]
[INFO] microservice-cloudfoundry-demo-catalog ............. SUCCESS [ 21.070 s]
[INFO] microservice-cloudfoundry-demo-order ............... SUCCESS [ 21.115 s]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 01:10 min
[INFO] Finished at: 2017-09-08T12:43:56+02:00
[INFO] Final Memory: 55M/424M
[INFO] ------------------------------------------------------------------------
```

If this does not work:

* Ensure that `settings.xml` in the directory `.m2` in your home
directory contains no configuration for a specific Maven repo. If in
doubt: delete the file.

* The tests use some ports on the local machine. Make sure that no
server runs in the background.

* Skip the tests: `./mvnw clean package -Dmaven.test.skip=true` or
  `mvnw.cmd clean package -Dmaven.test.skip=true` (Windows).

* In rare cases dependencies might not be downloaded correctly. In
  that case: Remove the directory `repository` in the directory `.m2`
  in your home directory. Note that this means all dependencies will
  be downloaded again.

## Run the microservices

* Log in to the local Cloud Foundry instance. The default Email is `user` and the password is `pass`. The `admin` account has the password `admin`.

```
[~/microservice-cloudfoundry/microservice-cloudfoundry-demo]cf login -a https://api.local.pcfdev.io --skip-ssl-validation
API-Endpunkt: https://api.local.pcfdev.io

Email> user

Password> 
Authenticating...
OK

Targeted org pcfdev-org

Targeted space pcfdev-space


                
API Endpoint:   https://api.local.pcfdev.io (API Version: 2.75.0)
User:       user
Organization:   pcfdev-org
Space:        pcfdev-space
```


* A simple `cf push` in the directory
`microservice-cloudfoundry-demo` will deploy the microservices and the
Hystrix dashboard.

```
[~/microservice-cloudfoundry/microservice-cloudfoundry-demo]cf push
...
requested state: started
instances: 1/1
usage: 128M x 1 instance
urls: microservices.local.pcfdev.io
Last uploaded: Fri Sep 8 11:15:03 UTC 2017
Stack: cflinuxfs2
Buildpack: staticfile 1.3.17

     state   since                     cpu    memory     disk       details
#0   active     2017-09-08 01:15:16 PM   0.0%   0 of 128M   0 of 512M

```

The microervices assumes that the microservices are accessible with the
path `local.pcfdev.io`. That is the default for the local Cloud
Foundry installation. A website to access the microservices is
deployed at http://microservices.local.pcfdev.io/ . The website has
links to all the microservices.

*  `cf dev stop` stops the Cloud Foundry Environment:
                                                                                                                                                                      
```                                                                                                                                                                    
[~/microservice-cloudfoundry/microservice-cloudfoundry-demo]cf dev stop
Stopping VM...
PCF Dev is now stopped.
```
