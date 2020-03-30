Technologie (Face Services)
============================

Node.js
---------------------

.. meta::
   :description lang=en: Get started writing technical documentation with Sphinx and publishing to Read the Docs.


Node.js wird zur Bereitstellung von WebServern / MQTT-Listenern verwendet.
Dadurch wird die Funktionalität der Face Services nach außen zugänglich gemacht.
Node.js bietet unter anderem:

* Unterstützt zahlreiche State-of-the-Art Frameworks
* Sehr aktive Community
* Einfache Anbindung an bestehende Javascript/Typescript Anwendungen

Im Rahmen der Face Services werden außerdem folgende Bibliotheken verwendet:

* face-api.js
* MQTT.js
* MinIO

Außerdem wird an Stelle von normalem Javascript das typisierte Superset Typescript verwendet, das dann
zu Javascript kompiliert werden kann. Dies unterstützt bei der Entwicklung beispielsweise durch bessere Code Completion.

Es wird immer empfohlen die dockerisierte Anwendung zu benutzen, wenn man die Software in Produktionsbetrieb
nehmen möchte. Für Entwicklungsaufgaben sollte man die Nodejs Variante starten, da diese sich einfacher und schneller compilieren lässt.
Alle Änderungen im `src/` Ordner werden auch automatisch in den Docker-build aufgenommen, wenn man das Dockerfile baut.

Quick Start
+++++++++++++++++++++

.. warning::

   Vor der Installation der Abhängigkeiten muss python2.7 installiert sein,
   da dies eine Voraussetzung für die Nutzung von node-gyp (Kompilierung nativer Bibliotheken) ist. (siehe z.B. `bindings <https://www.npmjs.com/package/@tensorflow/tfjs-node>`_.)
   Außerdem muss drauf geachtet werden, dass eine kompatible node version installiert ist.
   Manche node versionen funktionieren nicht mit dem tensorflow module. Mehr dazu `hier <https://github.com/tensorflow/tfjs/issues/2003>`_ 

Es wird angenommen, dass ihre CLI auf den project root eines der face services zeigt.

1. Installation von Abhängigkeiten ::

    user@shell:<project-root>$ npm install

2. Kompilierung source files (TS => JS) (ggf. zusätzl. Bundeling)::

    user@shell:face-box$ npm run convert

  bzw.::

    user@shell:face-recognition$ npm run build

3. Starten des Projekts::

    user@shell:<project-root>$ npm start

Das User Interface kann man nun durch den port 4321 im localhost über den Browser sehen. Für den korrekten
Betrieb wird empfohlen die anderen Services im Hintergrund laufen zu lassen, z.B. per docker-compose.
(Hiervon ausgenommen sind per node manuell gestarteten Services)

* localhost:4321 -> UI
* localhost:7001 -> minIO storage

MQTT Broker 
+++++++++++++++++++++

Der Broker sollte nun auch laufen. Um den Nachrichten-Austausch zwischen den Diensten zu beobachten, kann man einen MQTT Client (z.b. MQTT Explorer) an 
den Broker anbinden:

* mqtt://localhost:1883
* keine TLS Terminierung
