MQTT API
==================
Vom Image-Processing Service wird das MQTT Protokoll benutzt, um Anfragen entgegenzunehmen bzw.
Antworten zu senden. Die MQTT Topics sind in ``actions`` und ``results`` unterteilt.

.. note::

    Die Anfragen zu der Demo-Anwendung API gilt für öffentliche und private Informationen.
    Alle ``Endpunkte`` benötigen keine weiteren Authentifizierungsschritte.

Im Folgenden sind die Topics und ihre Funktion für den Schwarz-Weiß-Service (Oreo-Service) aufgeführt.

:PUBLISH `image-processing/actions/oreo`: Hier wird eine Nachricht gepostet, um ein Schwarz-Weiß-Bild (Oreo) zu erstellen.

    **Example request**:

    .. sourcecode:: json

        {
            "requestUuid": "zf3fc95a-8697-4ff2-9fc4-50820873c5c0",
            "messageType": "createRequest",
            "bucket": "image-processing",
            "path": "originalImage/demo_image.jpg",
            "imageName": "demo_image.jpg",
            "mime": "image/jpeg",
            "crop": true,
            "targetWidthPx": 600,
            "targetHeightPx": 600,
            "methods": [{
                "algorithm": "SIMPLE_THRESH",
                "options": {
                     "thresh": 147,
                     "maxVal": 255,
                     "invertThresh": false
                }   
            },{
                "algorithm": "ADAPTIVE_MEAN",
                "options": {
                     "blockSize": 147,
                     "constantVal": 255,
                     "invertThresh": false
                } 
            }, {
                "algorithm": "ADAPTIVE_GAUSSIAN",
                "options": {
                     "blockSize": 147,
                     "constantVal": 255,
                     "invertThresh": false
                } 
            }]
        }

    **Parameter**

    requestUuid 
        Unique-Identifier für einen Request.
    messageType 
        Identifiziert den Typen der Nachricht.
    bucket 
        Name des MinIo Buckets, indem sich das Original-Bild befindet.
    path 
        Pfad innerhalb des Buckets zu dem Bild.
    imageName 
        Name des Bildes. Dieser Name wird in den erzeugten Bildern wieder verwendet.
    mime 
        Mime-Type des Bildes.
    crop 
        Identifiziert, ob ein Bild zugeschnitten werden soll.
    targetWidthPx : int
        Ziel-Breite des Bildes.
    targetHeightPx : int
        Ziel-Höhe des Bildes.
    methods 
        ``Optionales-Array`` zur Konfiguration der einzelnen Algorithmen. 
    algorithm
        Name des Algorithmus, zudem die Konfiguration gehört. Entweder ``SIMPLE_THRESH``, ``ADAPTIVE_GAUSSIAN`` oder ``ADAPTIVE_MEAN``
    thresh
        Wert für Threshold-Value. Nur für ``SIMPLE_THRESH``. Default-Value: ``147``.
    maxVal
        Wert der für Pixel angewendet wird, die über dem Threshold liegen. Nur für ``SIMPLE_THRESH``. Default-Value: ``255``
    blockSize
        Wert für blockSize. Nur für ``ADAPTIVE_MEAN`` und ``ADAPTIVE_GAUSSIAN``. Default-Value: ``3``
    constantVal
        Wer für die Konstante C. Nur für ``ADAPTIVE_MEAN`` und ``ADAPTIVE_GAUSSIAN``. Default-Value: ``2``
    invertThresh
        Gibt an, ob ein Threshold invertiert werden soll. Default-Value: ``false``
        



:SUBSCRIBE `image-processing/results/oreo`: Auf diese topic publisht der Image-Processing Service seine Nachricht, wenn ein Schwarz-Weiß-Bild erzeugt wurde.

    **Example request**:

    .. sourcecode:: json

        {
            "originalRequestUuid": "zf3fc95a-8697-4ff2-9fc4-50820873c5c0",
            "messageType": "createResponse",
            "oreos": [{
                "bucket": "image-processing",
                "path": "oreo/results/zf3fc95a-8697-4ff2-9fc4-50820873c5c0/cropped_global_thresh_oreo_demo_image.jpg",
                "mime": "image/jpeg"
            }, {
                "bucket": "image-processing",
                "path": "oreo/results/zf3fc95a-8697-4ff2-9fc4-50820873c5c0/cropped_adaptive_gaussian_thresh_oreo_demo_image.jpg",
                "mime": "image/jpeg"
            }, {
                "bucket": "image-processing",
                "path": "oreo/results/zf3fc95a-8697-4ff2-9fc4-50820873c5c0/cropped_adaptive_mean_thresh_oreo_demo_image.jpg",
                "mime": "image/jpeg"
            }]
        }

    **Parameter**

    originalRequestUuid
        Die Ursprüngliche ``requestUuid`` die dem Service beim Request mitgeteilt wurde.
    messageType
        Identifiziert den Typen der Nachricht.
    oreos
        Array mit den erstellten Bildern.
    bucket
        Name des MinIo Buckets, indem das  Bild abgelegt wurde.
    path
        Pfad innerhalb des Buckets zu dem Bild.
    mime
        Mime-Type des Bildes.
    
.. note::

   Der Name der berechneten Bilder wird folgendermaßen zugesammengesetzt: <cropped>_<algorithmus>_<imageName>.<mime>. Das erste Segment (cropped) wird nicht gesetzt,
   wenn die Operation ohne cropping statt gefunden hat (im Request **crop: false** ).