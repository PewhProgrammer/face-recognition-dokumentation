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
            "methods": [
                {
                    "Simple": 150
                },{
                    "Adaptive_Mean": 200
                },
                {
                    "Adaptive_Gaussian": 250
                }
            ]
        }

.. note::

   Die crop-Property entscheidet, ob neben der Schwarz-Weiß-Berechnung auch ein Portraitfoto erzeugt werden soll. Die Schwarz-Weiß-Berechnung findet dann auf dem Portraitfoto statt.
   Das bedeutet es wird intern zunächst der Crop-Service aufgerufen und anschließend die Schwarz-Weiß-Berechnung durchgeführt.


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

.. note::

   Der Name der berechneten Bilder wird folgendermaßen zugesammengesetzt: <cropped>_<algorithmus>_<imageName>.<mime>. Das erste Segment (cropped) wird nicht gesetzt,
   wenn die Operation ohne cropping statt gefunden hat (im Request **crop: false** ).