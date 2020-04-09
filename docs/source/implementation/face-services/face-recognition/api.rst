MQTT API
==================
Vom face-recognition Service wird das MQTT Protokoll benutzt, um Anfragen entgegenzunehmen bzw.
Antworten zu senden. Die MQTT Topics sind in ``actions`` und ``results`` unterteilt.

.. note::

    Die Anfragen zu der Demo-Anwendung API gilt für öffentliche und private Informationen.
    Alle ``Endpunkte`` benötigen keine weiteren Authentifizierungsschritte.

Im Folgenden sind die Topics und ihre Funktion aufgeführt.

:PUBLISH `face-recognition/actions/train`: Trainieren neuer Gesichter

    **Example payload**:

    .. sourcecode:: json

        {
            "$id": "ae3e9b3b-6a23-404c-a200-6be8e3e7d7f0",
            "label": "user001",
            "image": "image001.jpg"
        }

.. note::

   Label kann z.B. der userId in der Datenbank entsprechen.
   Image ist der Name des Bildes im minIO Storage. Die Bilder werden im Pfad Bilder werden im Pfad /train-images/[label]/ gesucht.


:SUBSCRIBE `face-recognition/results/train`: Veröffentlichung des Ergebnisses der Trainingsoperation

    **Example payloads**:

    Success:  

    .. sourcecode:: json

        {
        "$id": "ae3e9b3b-6a23-404c-a200-6be8e3e7d7f0", 
        "score": 0.9176841396134666
        }

    Error:  

    .. sourcecode:: json

        {
            "$id": "ae3e9b3b-6a23-404c-a200-6be8e3e7d7f0",
            "error": {
                "message": "No face could be detected"
            }
        }

:PUBLISH `face-recognition/actions/recognize`: Erkennung bekannter Personen

    **Example payload**:

    .. sourcecode:: json

        {
            "$id": "90c2db56-32a7-4160-a218-58466fe25cbc",
            "queryImage": "image002.jpg"
        }

        
:SUBSCRIBE `face-recognition/results/recognize`: Veröffentlichung des Ergebnisses der Wiedererkennungsoperation

    **Example payloads**:

    Success:  

    .. sourcecode:: json

        {
            "$id": "90c2db56-32a7-4160-a218-58466fe25cbc",
            "label": "user001",
            "distance": 0.49761874818404894
        }

    Error:  

    .. sourcecode:: json

        {
            "$id": "90c2db56-32a7-4160-a218-58466fe25cbc",
            "error": {
                "message": "No person could be recognized."
            }
        }

.. note::

   queryImage ist der Name des Portraits in der minIO Ablage. Alle Bilder werden in dem Pfad ./query-images/ gesucht.