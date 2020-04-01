Docker
=======================================

.. meta::
   :description lang=en: Get started writing technical documentation with Sphinx and publishing to Read the Docs.


Docker ist ein sehr ausgeprägtes und industrie-angewendetes Virtualisierungstool, um Anwendungen zu containerisieren und diese leicht protierbar zu machen:

* Benötigt keine Dependencies
* Besitzt seine eigene hybride Netzwerkstruktur und DNS-Auflösung
* Microservice konform
* Portabilität und Skalierbarkeit

Alle Änderungen im `src/` Ordner werden auch automatisch in den Docker-build aufgenommen, wenn man das Dockerfile baut. Installieren Sie zunächst Docker.

Quick Start
-----------

Sobald Docker installiert wurde, kann man die Funktion testen durch den folgenden einfachen Befehl::

    user@shell:<project-root>$ docker --version

Benutzen Sie unser Docker-compose.yml File, um die benötigten Services schnell und einfach zu starten::

    user@shell:<project-root>$ docker-compose up -d --build

Alle Dienste sollten jetzt laufen. Überprüfen Sie dies mit dem folgendem Befehl::

    user@shell:<project-root>$ docker ps

Configuration: docker-compose
----------------------------------------

Wir benutzen ein `docker-compose.yml` File, um die Microservices zu starten. In diesem Abschnitt
sollen besondere Konfigurationsmöglichkeiten gezeigt werden.

Standard-Konfiguration
+++++++++++++++++++++++++++++++

    .. sourcecode:: yaml

        version: "3.7"

        x-logging:
        &default-logging
        options:
            max-size: "200k"
            max-file: "10"

        services:
        mqtt_broker:
            image: hivemq/hivemq-ce
            container_name: gaia_mqtt_broker
            restart: always
            ports:
            - 9001:9001
            - 1883:1883
            logging: *default-logging

        minio:
            image: minio/minio
            container_name: gaia_minio
            restart: always
            ports:
            - 7001:9000
            environment:
            MINIO_ACCESS_KEY: xQC2dV7ohZmUaz1Fxz0bKLzDeaGS1WXF
            MINIO_SECRET_KEY: gioVfeVp4Qus4wWBk0ITvKrzebu8xcMB
            command: server /data
            logging: *default-logging

        face-box:
            image: faceapibox:latest
            build: .
            container_name: face_box
            restart: always
            depends_on: 
            - "mqtt_broker"
            - "minio"
            environment: 
                MQTT_HOST: gaia_mqtt_broker
                MQTT_PORT: 1883
                MINIO_HOST: gaia_minio
                MINIO_PORT: 9000
                DB_HOST: faceapi-database
                DB_PORT: 5432
            ports:
            - 4321:4321
            logging: *default-logging

        db-persons:
            image: faceapi-database:latest
            build: 
            context: .
            dockerfile: database/Dockerfile
            container_name: faceapi-database
            restart: always
            # network_mode: gaia
            ports:
            - 5432:5432
            logging: *default-logging

        face-recognition:
            image: jcl94/erl-face-recognition:latest
            container_name: face-recognition
            restart: always
            depends_on: 
            - "mqtt_broker"
            - "minio"
            environment: 
                MQTT_URL: mqtt://gaia_mqtt_broker
                MQTT_PORT: 1883
                MINIO_HOST: gaia_minio
                MINIO_PORT: 9000
            logging: *default-logging

        image-processing:
            image: image-processing:latest
            build: .
            container_name: gaia_image_processing
            restart: always
            depends_on: 
            - "mqtt_broker"
            - "minio"
            environment:
                MINIO_HOST: minio:9000
                MINIO_SECURE: false
                MINIO_ACCESS_KEY: xQC2dV7ohZmUaz1Fxz0bKLzDeaGS1WXF
                MINIO_SECRET_KEY: gioVfeVp4Qus4wWBk0ITvKrzebu8xcMB
                MINIO_BUCKET: image-processing

                MQTT_BROKER_ADDRESS: mqtt_broker
                MQTT_CLIENT_ID: image-processing-client
                MQTT_MAIN_TOPIC: image-processing/actions/#
                MQTT_OREO_SUB_TOPIC: image-processing/actions/oreo
                MQTT_CROP_SUB_TOPIC: image-processing/actions/crop
                MQTT_OREO_RESULT_TOPIC: image-processing/results/oreo
                MQTT_CROP_RESULT_TOPIC: image-processing/results/crop
            logging: *default-logging
        
        networks:
        gaia:
            name: gaia-network_mode
            external: false


Environment Variables
+++++++++++++++++++++++++++++

Alle bereitgestellten Services können über Environment Variables konfiguriert werden. Zum Beispiel kann der Port vom MQTT Broker beim Start 
des Containers über die Konfiguration eingestellt werden:

    .. sourcecode:: yaml

        face-box:
            environment: 
                MQTT_HOST: gaia_mqtt_broker
                MQTT_PORT: 1883
                MINIO_HOST: gaia_minio
                MINIO_PORT: 9000
                DB_HOST: faceapi-database
                DB_PORT: 5432


Docker Build 
+++++++++++++++++++++++++++++

Beim Start der docker-compose.yml wird ein docker image für die entsprechende Anwendung generiert.
Die jeweils anderen Services liegen in anderen repositories, weswegen sie stattdessen vom docker hub gepullt werden.

Bsp.:
    
    .. sourcecode:: yaml

        face-box: # sucht im root das Dockerfile und baut diesen
            image: faceapibox:latest
            build: .

        face-recognition: # pullt einen vorgefertigten Docker Image
            image: jcl94/erl-face-recognition:latest

