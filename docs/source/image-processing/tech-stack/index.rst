Technologie (Image-Processing Services)
=====================

Python
---------------------

.. meta::
   :description lang=en: Get started writing technical documentation with Sphinx and publishing to Read the Docs.


Der gesamte Service wurde in Python 3.8.1 geschrieben.
Die verwendeten Bibliotheken, die zur Inbetriebnahme notwendig sind, befinden sich in der requirements.txt.

Quick Start
+++++++++++++++++++++

Es wird angenommen, dass ihre CLI auf den project root zeigt und die notwendigen Services im Hintergrund laufen. Dafür kann einfacher in der docker-compose.yml der image-processing-service auskommentiert werden und anschließend die Container gestartet werden.
Der Dienst kann auskommentiert werden, da dieser per Python nativ betrieben wird.

.. warning::

    Es wird davon ausgegangen, dass Python 3 installiert ist und die Umgebungsvariable **python** auch Python 3 verwendet.

Zunächst müssen wir die notwendigen Bibliotheken installieren.
Diese werden aus der requirements.txt ausgelesen ::

    user@shell:image-processing$ pip3 install --no-cache-dir -r .

Danach muss nur noch die app.py gestartet werden::

    user@shell:image-processing$ python app.py


* localhost:7001 -> minIO storage

MQTT Broker 
+++++++++++++++++++++

Der Broker sollte nun auch laufen. Um den Nachrichten-Austausch zwischen den Diensten zu beobachten, kann man einen MQTT Client (z.b. MQTT Explorer) an 
den Broker anbinden:

* mqtt://localhost:1883
* keine TLS Terminierung
