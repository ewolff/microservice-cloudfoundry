# Beispiel starten

Die ist eine Schritt-für-Schritt-Anleitung zum Starten der
Beispiele. Das Beispiel nutzt Cloud Foundry als Ablaufumgebung.

## Installation

* Die Beispiele sind in Java implementiert. Daher muss Java
  installiert werden. Die Anleitung findet sich unter
  https://www.java.com/en/download/help/download_options.xml . Da die
  Beispiele kompiliert werden müssen, muss ein JDK (Java Development
  Kit) installiert werden. Das JRE (Java Runtime Environment) reicht
  nicht aus. Nach der Installation sollte sowohl `java` und `javac` in
  der Eingabeaufforderung möglich sein.

* Die Projekte baut Maven. Zur Installation siehe
  https://maven.apache.org/download.cgi>. Nun sollte `mvn` in der
  Eingabeaufforderung eingegeben werden können.

* Um Cloud Foundry nutzen zu können, musst du das `cf`
  Kommandozeilenwerkzeug installieren, siehe
  https://docs.cloudfoundry.org/cf-cli/install-go-cli.html .

* Dann musst du Cloud Foundry installieren. Dazu kannst du eine lokale
  Installation auf deinem Rechner vornehmen, siehe
  https://pivotal.io/pcf-dev oder du registrierst dich bei einem
  öffentlichen
  Cloud-Foundry-Anbieter. https://www.cloudfoundry.org/how-can-i-try-out-cloud-foundry-2016/
  ist eine List von Cloud-Foundry-Cloud-Anbietern.

* Die Beispiele benötigen relativ viel Speicher. Daher kann solltest
  du eine lokale Cloud-Foundry-Instanz mit 8 GB RAM `cf dev start -m
  8086`. Das muss beim ersten Start der Umgebung erfolgen. Wenn die
  RAM-Einstellungen später geändert werden sollen, muss die vorhandene
  Instanz zunächst mit `cf dev destroy` gelöscht werden.

```
[~/microservice-cloudfoundry/microservice-cloudfoundry-demo]cf dev start
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

Wechsel in das Verzeichnis `microservice-cloudfoundry-demo` und starte `mvn clean
package`. Das wird einige Zeit dauern:

```
[~/microservice-cloudfoundry/microservice-cloudfoundry-demo]mvn clean package
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

Falls es dabei zu Fehlern kommt:

* Stelle sicher, dass die Datei `settings.xml` im Verzeichnis  `.m2`
in deinem Heimatverzeichnis keine Konfiguration für ein spezielles
Maven Repository enthalten. Im Zweifelsfall kannst du die Datei
einfach löschen.

* Die Tests nutzen einige Ports auf dem Rechner. Stelle sicher, dass
  im Hintergrund keine Server laufen.

* Führe die Tests beim Build nicht aus: `mvn clean package package
  -Dmaven.test.skip=true`.

* In einigen selten Fällen kann es vorkommen, dass die Abhängigkeiten
  nicht korrekt heruntergeladen werden. Wenn du das Verzeichnis
  `repository` im Verzeichnis `.m2` löscht, werden alle Abhängigkeiten
  erneut heruntergeladen.

## Microservices starten

* Authentifiziere Dich mit den beim Start der Cloud-Foundry-Instanz
ausgegebenen Benutzerkonto (Email `user` und Passwort `pass`. Der `admin`-Acoount hat das Passwort `admin`.)

```
[~/microservice-cloudfoundry/microservice-cloudfoundry-demo]cf login -a https://api.local.pcfdev.io --skip-ssl-validation
API-Endpunkt: https://api.local.pcfdev.io

Email> user

Password> 
Authentifizieren...
OK

Adressierte Organisation pcfdev-org

Adressierter Bereich pcfdev-space


                
API-Endpunkt:   https://api.local.pcfdev.io (API-Version: 2.75.0)
Benutzer:       user
Organisation:   pcfdev-org
Bereich:        pcfdev-space
```

* `cf push` im Verzeichnis `microservice-cloudfoundry-demo` startet
die Microservices und das Hystrix Dashboard.

```
[~/microservice-cloudfoundry/microservice-cloudfoundry-demo]cf push
...
angeforderter Zustand: started
Instanzen: 1/1
Verwendung: 128M x 1 Instanzen
URLs: microservices.local.pcfdev.io
Letztes Hochladen: Fri Sep 8 11:15:03 UTC 2017
Stack: cflinuxfs2
Buildpack: staticfile 1.3.17

     Zustand   seit                     CPU    Speicher     Platte       Details
#0   aktiv     2017-09-08 01:15:16 PM   0.0%   0 von 128M   0 von 512M
```

Die Microservices nehmen an, dass die Microservices unter dem Pfad 
`local.pcfdev.io` angesprochen werden können. Das ist die
Standardeinstellung einer lokalen Cloud-Foundry-Installation. Unter
http://microservices.local.pcfdev.io/ ist eine Webseite mit Links zu
allen Microservices verfügbar.
