Fazit
=========================
Um die Anforderungen an das Projekt zu erfüllen wurden letztendlich 3 voneinander unabhängige Services entwickelt.
Diese stellen folgende Funktionalitäten bereit:

* Berechnung der Koordinaten von Begrenzungsboxen zur grafischen Anzeige des erkannten Gesichts
* Erkennung des deutlichsten Gesichtes in gesendeten Bildern und Berechnung und Speicherung der Gesichtsdeskriptoren,
  sowie die Wiederkennung von Gesichtern auf Basis gespeicherter Deskriptoren
* Verarbeitung von Bildern, d.h. die Anwendung bestimmter Effekte (Portrait / Image-Thresholding)

Die Integrationsvorgabe wurde aus Zeitgründen insofern verändert, dass bislang lediglich der Image-Processing Service mit dem UI der Gruppe 6 integriert werden musste.
Zusätzlich wurde jedoch eine Demo-Anwendung entwickelt, an die alle Services angebunden sind, sodass diese getestet werden können. Hierzu wurde außerdem ein kurzes
Demo Video bereitgestellt.

Im Rahmen des Projektes haben die Teammitglieder verschiedenste Technologien und Frameworks kennengelernt und miteinander verglichen, darunter:

Programmiersprachen:

* Node.js / Javascript / Typescript
* Python

Bibliotheken & Frameworks:

* Gesichtserkennung
  
  - facenet
  - face++
  - OpenFaceTracker
  - face-api.js
* Bildbearbeitung

  - OpenCV
* Kommunikationsprotokolle

  - HTTP (JSON Endpunkte)
  - MQTT
* Persistenz

  - MinIO
  - Postgres
* Dokumentation

  - Sphinx (ReadTheDocs Theme)

Die funktionalen Anforderungen wurden nach entsprechender Evaluation schlussendlich unter Nutzung der Kombinationen "Node.js, Typescript, MQTT, MinIO, face-api.js"
und "Python, MQTT, MinIO, OpenCV" erfüllt.

Darüber hinaus wurde Docker als Deployment-Hilfe eingesetzt. Demnach existiert zu jedem Service eine Docker-Image, das ohne weiteres in Betrieb genommen werden kann.
Um eine gemeinsame Nutzung aller Services zu ermöglichen wurde docker-compose verwendet.

Durch das zuvor beschriebene konnte zur gegebenen Aufgabenstellung eine Lösung entwickelt werden, die unabhängige Probleme in verschiedene Teilsysteme auslagert und unter minimalem
Einrichtungsaufwand ein lose gekoppeltes Zusammenspiel dieser ermöglicht.
