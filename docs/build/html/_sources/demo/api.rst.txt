API
=============

Die Demo-Anwendung API benutzt :abbr:`REST (Representational State Transfer)`. JSON Packete 
werden von jeder API Response zurückgegeben inklusive Fehler und HTTP Reponse Status Codes, 
die weitere Auskünfte über Erfolg oder Miserfolg einer Anfrage beinhaltet.

.. note::

    Die Anfragen zu der Demo-Anwendung API gilt für öffentliche und private Informationen.
    Alle ``Endpunkte`` benötigen keine weiteren Authentifizierungsschritte.

.. contents:: Table of contents
    :local:
    :backlinks: none
    :depth: 3


REST API
---------------------------------

.. note::

   Die Endpunkte werden von dem Frontend benutzt, um weitere Informationen von der Datenbank
   oder den Backend-Services zu bekommen.

:GET `/`: Jede default route führt zu der Startseite der Web-Anwendung.

:POST /send-mqtt/: Sendet eine MQTT Nachricht zu einer gewünschten Topic.

    **Example request**:

    .. sourcecode:: json

        {
            "$id": 12,
            "message": "send me",
            "topic": "/data/in/test"
        }

    **Example response**:

    .. sourcecode:: json

        {
            "data": "Following message sent to hivemq: send me",
            "code": 200
        }

Users
+++++++++++++

:POST /create-person/: Erstellt einen neuen User und legt diese in die Datenbank ab.

    **Example request**:

    .. sourcecode:: json

        {
            "id": 12,
            "first": "Thomas",
            "last": "Müller",
            "email": "thomas.müller@gmail.com",
            "address": "Saarland, Saarbrücken, 66111"
        }

    **Example response**:

    .. sourcecode:: json

        {
            "data": "Task 'create-person' successfull",
            "code": 200
        }

:POST /train-person/: Ladet die Bilddateien auf minIO hoch und schickt eine MQTT Nachricht zum Recognition Service für das Training.

    **Example request**:

    .. sourcecode:: json

        {
            "id": 12,
            "user": "Thomas Müller",
            "label": 2,
            "data_base64_multiple": "[djsapd90809v8c02j3ß23ßjßfd..., .., ]"
        }

    **Example response**:

    .. sourcecode:: json

        {
            "data": "Task 'train-person' successfull",
            "code": 200
        }

:POST /recognize-person/: Ladet die Bilddatei auf minIO und schickt eine Anfrage über MQTT an das Recognition Service zum Erkennen.

    **Example request**:

    .. sourcecode:: json

        {
            "id": 12,
            "data_base64": "djsapd90809v8c02j3ß23ßjßfd..."
        }

    **Example response**:

    .. sourcecode:: json

        {
            "data": "Request to recognition service has been made",
            "code": 200
        }
