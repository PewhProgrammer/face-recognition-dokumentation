Allgemeines
=========================

Im Rahmen der Projektarbeit der Gruppe 7 im WS19/20 sollten folgende Funktionalitäten in Form von
serverseitigen Services bereitgestellt werden:

* Erkennen/Trainieren der Gesichter in angegebenen Bildern
* Wiedererkennung von Personen basierend auf vorhandenen Trainingsdaten
* Ermitteln von Begrenzungsboxen zu erkannten Gesichtern
* Die Verarbeitung von Bildern zur Anwendung verschiedener Effekte

Zu diesem Zweck wurden die folgenden 3 Services und eine Demo Applikation zur Nutzung dieser entwickelt:


**Bounding Box um Gesichter (face-box)**

MQTT basierter Node.js Service für Berechnung und Austausch von Gesichtsrahmenboxen.

:doc:`Getting Started <../implementation/face-services/face-box/index>`


**face-recognition**

MQTT basierter Node.js Service zur

* Berechnung und Speicherung von Gesichtsdeskriptoren
* Wiedererkennung von Personen auf Basis der Trainingsdaten

:doc:`Getting started <../implementation/face-services/face-recognition/index>`

**image-processing**
MQTT basierter python Service zur Verarbeitung von Bildern. Unterstützte Effekte:

* Black-and-White (Oreo)
* Cropping

:doc:`Getting started <../implementation/image-processing/index>`


Übersicht
+++++++++++++++
Alle Dienste wurden abgekapselt in ihren eigenen Docker Container entwickelt und für eine Microservice-Architektur bereitgestellt. Hierbei wurden in den einzelnen Diensten die 
Wahl der Frameworks und Implementierungsart frei entschieden, wodurch eine hohe Abkopplung zwischen den Diensten aufgebaut wurde. Das Verbindungsglied dieser Dienste wurde durch die 
Kommunikationsschnittstelle der MQTT Broker realisiert. Bestimmte Topic Namen wurden ausgewählt und dienten als Input/Output für die Dienste. Dadurch sind fremde Dienste in der Lage 
die Funktionalitäten dieser Projektarbeit einfach und flexible zu nutzen. Die Architektur und der konkrete Usecase wird im :doc:`nächsten Kapitel <architektur>`  näher erläutert. Am Ende wird ein kurzer Ausblick 
auf das Projekt zusammengefasst und ein Fazit gezogen.

In :doc:`Kapitel 2 <../implementation/index>` haben wir Ihnen die Dienste im Detail aufgestellt. Hier betrachten wir die Evaluation verschiedener Frameworks , verwendeten Technologien und Implementierungsdetails. Außerdem
wir ein großer Bereich die öffentliche Schnittstelle nach außen über MQTT sein und wie man die Services zum Laufen bekommt.

In :doc:`Kapitel 3 <../demo/index>` schildern wir Ihnen alles über unsere entwickelte HTML/CSS/JS basierte Demo-Applikation zur Demonstration der Funktionalitäten der obengenannten Services. Wir zeigen Ihnen Video- und Bildmaterial, während der Usecase der Anwendung erläutert wird.


In :doc:`Kapitel 4 <../tech-stack/docker>` wird Docker thematisiert und wie man dies in unserer Umgebung am Besten konfiguriert.
