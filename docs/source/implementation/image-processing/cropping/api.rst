MQTT API
==================
Vom Image-Processing Service wird das MQTT Protokoll benutzt, um Anfragen entgegenzunehmen bzw.
Antworten zu senden. Die MQTT Topics sind in ``actions`` und ``results`` unterteilt.

.. note::

    Die Anfragen zu der Demo-Anwendung API gilt für öffentliche und private Informationen.
    Alle ``Endpunkte`` benötigen keine weiteren Authentifizierungsschritte.

Im Folgenden sind die Topics und ihre Funktion für den Cropping-Service aufgeführt.

:PUBLISH `image-processing/actions/crop`: Hier wird eine Nachricht gepostet, wenn ein Portraitfoto erzeugt werden soll.

    **Example request**:

    .. sourcecode:: json

        {
            "requestUuid": "zf3fc95a-8697-4ff2-9fc4-50820873c5c0",
            "messageType": "createRequest",
            "bucket": "image-processing",
            "path": "originalImage/flori.jpg",
            "imageName": "flori.jpg",
            "mime": "image/jpeg",
            "targetWidthPx": 600,
            "targetHeightPx": 600,
        }
    
    **Parameter**

    requestUuid 
        Unique-Identifier für einen Request.
    messageType 
        Identifiziert den Typen der Nachricht.
    bucket 
        Name des MinIO Buckets, indem sich das Original-Bild befindet.
    path 
        Pfad innerhalb des Buckets zu dem Bild.
    imageName 
        Name des Bildes. Dieser Name wird in den erzeugten Bildern wieder verwendet.
    mime 
        Mime-Type des Bildes.
    targetWidthPx : int
        Ziel-Breite des Bildes.
    targetHeightPx : int
        Ziel-Höhe des Bildes.
   



:SUBSCRIBE `image-processing/results/crop`: Auf diese topic publisht der Image-Processing Service seine Nachricht, wenn ein Portraitfoto erzeugt wurde.

    **Example request**:

    .. sourcecode:: json

        {
            "originalRequestUuid": "zf3fc95a-8697-4ff2-9fc4-50820873c5c0",
            "messageType": "createResponse",
            "croppedImages": [{
                "bucket": "image-processing",
                "path": "crop/results/zf3fc95a-8697-4ff2-9fc4-50820873c5c0/not_demo_image.jpg",
                "mime": "image/jpeg"
            }, {
                "bucket": "image-processing",
                "path": "crop/results/zf3fc95a-8697-4ff2-9fc4-50820873c5c0/resized_demo_image.jpg",
                "mime": "image/jpeg"
            }]
        }
    
    **Parameter**

    originalRequestUuid
        Die Ursprüngliche ``requestUuid`` die dem Service beim Request mitgeteilt wurde.
    messageType
        Identifiziert den Typen der Nachricht.
    croppedImages
        Array mit den erstellten Bildern
    bucket
        Name des MinIO Buckets, indem das  Bild abgelegt wurde.
    path
        Pfad innerhalb des Buckets zu dem Bild.
    mime
        Mime-Type des Bildes.
    
  
    **Example for a error response**:

    .. sourcecode:: json

        {
            "originalRequestUuid": "zf3fc95a-8697-4ff2-9fc4-50820873c5c0",
            "errorMessage": "Couldn't crop the image!. Exception: AttributeError,  Arguments:(\"'str' object has no attribute 'value'\",)"
        }

    **Parameter**

    originalRequestUuid
        Die Ursprüngliche ``requestUuid`` die dem Service beim Request mitgeteilt wurde.
    errorMessage
        Nachricht die darüber informiert, weswegen die Operation fehlgeschlagen ist.
  

.. note::

   Der Service erzeugt immer zwei Portraitfotos. Das Foto mit dem Präfix **resized** ist auf die gewünschte Pixelgröße skaliert.
   Das Zweite Bild ist das Portraitfoto ohne Skalierung.