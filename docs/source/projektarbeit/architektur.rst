Architektur
====================

Im Folgenden soll ein kurzer Einblick über die Architektur gegeben werden.
Die erste Schnittstelle ist der MQTT Broker. An bestimmten topics lauschen die Dienste auf eingehende Anfragen. MinIO wird als File Storage für unsere Bilder benutzt. Diese sind 
Service-übergreifend, sodass jeder Service auf diese Dateien zugreifen kann. Alle Dienste werden über Docker Compose orchestriert, welches aber auch
durch Docker Swarm einfach ersetzt werden kann. Unser Technologie Stack besteht aus:

* Mqttjs
* Nodejs
* MinIO
* Docker
* face-api.js
* openCV
* Python

.. image:: ../_static/images/architecture.png
   :width: 600

Workflow
++++++++++++++++++++++

Das Projekt wurde so aufgesetzt, dass alle Services unabhängig voneinander benutzt werden kann. Der Usecase aus Sicht der Nutzer wird in dem Kapitel :doc:`Demo - Usecase <../demo/webdemo>` erläutert.


Face-Recognition
---------------------
1.
    Das Frontend schickt eine Recognition-Anfrage an den MQTT Broker. Darin ist eine Request-ID und der Name der zu erkennenden Bilddatei enthalten.

2.
    Die Backend-Services reagieren auf Nachrichten in einem :doc:`bestimmten Topic <../demo/api>` und verarbeiten diese. Die genauen Topics sind
    in der Beschreibung der Services enthalten.

.. note::

      Falls nötig, werden Bilder aus dem MinIO Storage genommen und zur weiteren Bearbeitung verwendet.

3. 
    Das Backend schickt sein Recognition-Ergebnis zurück über den MQTT Broker.

4. 
    Das Frontend empfängt das Ergebnis und visualiert dieses auf dem Dashboard.

.. note::

   Falls der Nutzer erkannt wird, wird an dieser Stelle eine Anfrage an die Datenbank gemacht, um die personenbezogenen Daten zu extrahieren.

5. 
   Sofern der Nutzer nicht erkannt wird, wird ein Formular bereitgestellt, worin der Nutzer sich registrieren lassen kann. Dabei werden zum einen die Nutzerinformationen in der Datenbank
   abgespeichert, zum anderen werden jedoch auch Bilder der Person zum Training an den face-recognition-service geschickt, der anhand dessen Gesichtsdeskriptoren errechnet und diese per MinIO speichert.

.. note::

  Der Nutzer ist nun in der Datenbank gespeichert und das Model wurde auf das Gesicht des Nutzers trainiert.

Image-Processing
----------------------

1.
    Das Frontend erzeugt mit der Webcam des Benutzers einen Snapshot und legt diesen in einem MinIO-Bucket ab.

2.
    Daraufhin wird eine MQTT-Nachricht für die gewünschte Bildoperation (derzeit :doc:`Crop <../implementation/image-processing/cropping/index>`/:doc:`Oreo <../implementation/image-processing/black-and-white/index>`) mit der entsprechenden Payload in das dazugehörige Topic gepublisht.

.. note::
    Für die jeweilige API-Referenz siehe: :doc:`Crop-API <../implementation/image-processing/cropping/api>` und :doc:`Oreo-API <../implementation/image-processing/black-and-white/api>`

3.
    Der Service validiert zunächst die Payload und führt die jeweilige Operation mit den mitgelieferten Parameter aus. 

4.
    Danach wird das bearbeite Bild in ein MinIO-Bucket geschrieben

5.
    Zum Schluss wird das Frontend darüber informiert, indem eine entsprechende Response-Payload in das hierfür vorgesehene Topic gepublisht wird.




MQTT
+++++++++++++++++

MQTT wird benutzt, um die Kommunikationsschnittstelle in unserer Microservice-Architektur zu realisieren. Hierbei besitzt jeder service seinen eigenen namespace für Anfragen und Ergebnis.
Das Format der Topics sieht folgendermaßen aus:
<service-name>/actions/<feature> oder <service-name>/results/<feature>.

../**actions**/.. ist die Schnittstelle für Anfragen an den Service.

../**results**/.. ist die Schnittstelle für das Ergebnis der Anfrage.

Hier ist eine Abbildung eines MQTT Clients (MQTT Explorer), welches den Nachrichtenaustausch und die verwendeten Topics der Frontend- und Backend Services 
übersichtlich darstellen kann.

.. image:: ../_static/images/mqtt.png
   :width: 600