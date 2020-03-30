Usage
=============

Die Demo-Anwendung wurde im Rahmen der Projektarbeit als visueller Prototyp entwickelt, um die Features der
Backend-Services darzustellen. Die visuelle Darstellung des Interfaces ist frei wählbar und flexibel. Das Augenmerk
soll auf der Schnittstelle und den Services liegen. Die Demo-Anwendung kann benutzt werden, um die Interaktion mit dem 
Backend besser nachzuvollziehen und eine Anwendung für den Produktionsbetrieb zu entwickeln.

.. note::

    Zur Zeit der Abnahme wurde nur der Image-Processing Service in das Frontend des Projektes der Gruppe 6 integriert.

Um die Anwendung zu starten, muss man die Docker-Services mit unserer ``docker-compose.yml``
starten und das Interface über ``localhost:4321`` via Browser öffnen.

Use Case
++++++++++++++++++++++

1.
    Der Nutzer drückt auf Start Recognition.

    Das Frontend öffnet einen ``Webcam-Stream`` und schickt eine ``Facebox-Anfrage`` an den Facebox Service.

2.
    Der Nutzer drückt auf Snapshot.

    Das Frontend schickt eine ``Recognition-Anfrage`` und ein Bild vom Nutzer an den Recognition Service.


3. 
    Das Backend schickt ein ``Recognition Ergebnis`` zurück. 
    
    Der Nutzer wird nicht wiedererkannt.

.. note::

    Sofern der Nutzer wiedererkannt wird, werden die Persondaten 
    in einem Formular angezeigt und der Workflow ist abgeschlossen.

4. 
    Das Frontend zeigt ein Formular an. 

    Der Nutzer kann nun seine Daten eingeben und sich für die Anwendung `registrieren` lassen.


5. 
    Der Nutzer sendet das Formular ab.

    Das Frontend schickt eine ``REST Anfrage`` mit den Personendaten in das Backend 
    und nimmt parallel Bilder für das Training der Face-recognition Service.


6.
    Das Backend speichert die Daten in die Datenbank und in das minIO Storage.


Der Nutzer kann nun die Anwendung auf einem anderen Computer verwenden, um sein Gesicht zu erkennen
und seine Personendaten abzurufen beziehungsweise ausdrucken zu lassen.

Demo Video
++++++++++++++++++
.. raw:: html

    <video width="700" height="610" controls>
        <source src="../_static/videos/Projektarbeit-demo-2020.mp4" type="video/mp4">
    </video>
