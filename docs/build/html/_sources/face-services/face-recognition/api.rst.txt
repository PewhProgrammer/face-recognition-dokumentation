MQTT API
==================
Vom face-recognition Service wird das MQTT Protokoll benutzt, um Anfragen entgegenzunehmen bzw.
Antworten zu senden. Die MQTT Topics sind in ``actions`` und ``results`` unterteilt.

.. note::

    Die Anfragen zu der Demo-Anwendung API gilt für öffentliche und private Informationen.
    Alle ``Endpunkte`` benötigen keine weiteren Authentifizierungsschritte.

Im Folgenden sind die Topics und ihre Funktion aufgeführt.

:PUBLISH `face-recognition/actions/recognize`: Hier wird eine Nachricht gepostet, um ein Gesicht wiederzuerkennen.

    **Example request**:

    .. sourcecode:: json

        {
            "$id": "user1",
            "queryImage": "queryImg.jpg"
        }

.. note::

   queryImage ist der Name des Portraits in der minIO Ablage. Alle Bilder werden in dem Pfad ./query-images/ gesucht.

:PUBLISH `face-recognition/actions/train`: Hier wird eine Nachricht gepostet, damit das Face-recognition Service die Bilder zum Trainieren werden kann.

    **Example request**:

    .. sourcecode:: json

        {
                "$id": "task_1",
                "label": "1",
                "image": "image.jpg"
        }

.. note::

   label ist die userid in der Datenbank. image ist der Name des Bildes im minIO Storage.


:SUBSCRIBE `face-recognition/results/recognize`: Auf diese topic publisht der Face-Recognition Service seine Nachricht, wenn der Dienst eine Person wiedererkannt hat.

    **Example request**:

    .. sourcecode:: json

        {
            "$id": 12,
            "error": "Person has not been recognized",
            "label": "unknown"
        }