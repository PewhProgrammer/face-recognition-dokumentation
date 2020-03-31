.. Faceapi-BoundingBox documentation master file, created by
   sphinx-quickstart on Tue Mar 10 19:24:04 2020.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

:Authors: - Me
          - Myself
          - I

Willkommen zur technischen Dokumentation des Gesichtserkennungsprojekts der Gruppe 7 im WS19/20
==============================================================================================================

Allgemeines:

.. toctree::
   :maxdepth: 3
   :hidden:
   :caption: Projektarbeit

   projektarbeit/index
   projektarbeit/architektur
   projektarbeit/fazit


Im Rahmen der Projektarbeit der Gruppe 7 im WS19/20 sollten folgende Funktionalitäten in Form von
serverseitigen Services bereitgestellt werden:

* Erkennen/Trainieren der Gesichter in angegebenen Bildern
* Wiedererkennung von Personen basierend auf vorhandenen Trainingsdaten
* Ermitteln von Begrenzungsboxen zu erkannten Gesichtern
* Die Verarbeitung von Bildern zur Anwendung verschiedener Effekte

Zu diesem Zweck wurden die folgenden 3 Services und eine Demo Applikation zur Nutzung dieser entwickelt:

face-box
----------------
MQTT basierter Node.js Service für Berechnung und Austausch von Gesichtsrahmenboxen.

:doc:`Getting Started <implementation/face-services/face-box/index>`

.. toctree::
   :maxdepth: 3
   :hidden:
   :caption: Implementierung

   implementation/face-services/index
   implementation/image-processing/index


face-recognition
------------------------
MQTT basierter Node.js Service zur

* Berechnung und Speicherung von Gesichtsdeskriptoren
* Wiedererkennung von Personen auf Basis der Trainingsdaten

:doc:`Getting started <implementation/face-services/face-recognition/index>`

image-processing
------------------------------
MQTT basierter python Service zur Verarbeitung von Bildern. Unterstützte Effekte:

* Black-and-White (Oreo)
* Cropping

:doc:`Getting started <implementation/image-processing/index>`

Demo
------
HTML/CSS/JS basierte Demo-Applikation zur Demonstration der Funktionalitäten der 3 Services

:doc:`Getting started <demo/webdemo>`

.. toctree::
   :maxdepth: 2
   :hidden:
   :caption: Demo

   demo/webdemo
   demo/api

.. toctree::
   :maxdepth: 2
   :hidden:
   :caption: Technologie Stack

   tech-stack/docker


Nützliche Links
==================

.. toctree::
   :maxdepth: 2
   :hidden:
   :caption: Nützliche Links

   help

* :doc:`Kontakt <help>`