MQTT API
==================
Vom face-box Service wird das MQTT Protokoll benutzt, um Anfragen entgegenzunehmen bzw.
Antworten zu senden. Die MQTT Topics sind in ``actions`` und ``results`` unterteilt.

.. note::

    Die Anfragen zu der Demo-Anwendung API gilt für öffentliche und private Informationen.
    Alle ``Endpunkte`` benötigen keine weiteren Authentifizierungsschritte.

Im Folgenden sind die Topics und ihre Funktion aufgeführt.

:PUBLISH `face-box/actions/update-box`: An diese topic soll das Frontend eine Nachricht posten, um eine Box für ein gegebenes Gesicht zu bekommen. Der Facebox-Bounding Box Service subscribt darauf und nimmt die Anfrage dann entgegen, sofern der payload stimmt.

    **Example request**:

    .. sourcecode:: json

        {
            "id": 12,
            "data_base64": "djsapd90809v8c02j3ß23ßjßfd..."
        }


:SUBSCRIBE `face-box/results/update-box`: An diese topic soll das Frontend subscriben, um das Ergebnis der Facebox-Anfrage zu bekommen. Der payload ``"data": "12 32 15 23"`` sind die Koordinaten des Rechtecks für das erkannte Gesicht. Wenn kein Gesicht erkannt wird, wird auch keine Nachricht auf diese topic gepublisht.

    **Example response**:

    .. sourcecode:: json

        {
            "$id": 12,
            "type": "webcam-result",
            "data": "12 32 15 23",
        }
