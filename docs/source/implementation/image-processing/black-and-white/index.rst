Black-and-White Service (Oreo service)
================================================
Dieser Service, soll anhand eines (Farb)-Bildes, verschiedene Schwarz-Weiß-Bilder erzeugen. Der Service sollte dann später von Gruppe 6 durch
das von ihr entwickelte UI verwendet werden, sodass der Benutzer von 3 möglichen Entwürfen sich ein Bild aussuchen kann, dass mit der Auftragserfassung gespeichert werden soll.


Evaluation
++++++++++++++++

Da der Image-Processing-Service hauptsächlich mit dem Frontend interagiert, wurde evaluiert, 
ob eine Implementierung des Service direkt im UI nicht von Vorteil wäre.

Einer der Gründe war die Auswahl des Berechnungsalgorithmus und deren Parametrisierung. Diese haben einen erheblichen Einfluss auf das resultierende Schwarz-Weiß-Foto.
Diesem Problem, wurde versucht entgegenzuwirken, indem der Micro-Service drei verschiedene Berechnungen mit jeweils unterschiedlichen Algorithmen erzeugt.
Dennoch besteht das Risiko, dass aufgrund der Lichtverhältnisse die Ergebnisse nicht zufriedenstellen sind.

Aus diesem Grund wurde eine Alternative entwickelt, die direkt im Browser läuft, und der Backend-Lösung gegenüber gestellt.

Dabei kamen wir zum Ergebnis, dass die Realisierung im Frontend durchaus vorstellbar ist. Jedoch verfügt die JavaScript-Implementierung von OpenCV weniger Funktionalitäten, als die Python-Implementierung.
Zudem ist die Vision, dass sämtliche Bildbearbeitungsprozesse die zukünftig im EmRoLab notwendig sein werden, dem Service hinzugefügt werden können. 

.. note::
    
    Weitere Informationen können aus diesem Artikel entnommen werden https://blog.theodo.com/2019/02/computer-vision-web-opencv-js/

Schwarz-Weiß-Berechnung
+++++++++++++++++++++++++++++
Bei der Schwarz-Weiß-Berechnung wird zunächst das Original-Bild aus einem MinIo-Bucket eingelesen. Anschließend wird aus dem Originalfoto ein Grayscale Foto erzeugt, beidem das Farbspektrum auf eine Pixel-Range von 0 bis 255 verteilt wird (0 Schwarz, 255 Weiß).

Daraufhin erfolgt die eigentliche Schwarz-Weiß-Berechnung mittels Threshold. Hierfür bietet OpenCV verschiedene Operationen:

1. (Simple)-Threshold
2. Adaptive-Threshold

Hinzu kommt noch, dass der Adaptive-Threshold über zwei unterschiedliche Algorithmen verfügt, die zu unterschiedlichen Resultaten führen:

1. ADAPTIVE_THRESH_MEAN_C 
2. ADAPTIVE_THRESH_GAUSSIAN_C 

.. note::

    Es gibt noch eine weitere Berechnungsmethode, bei der mittels Otsu’s Binarization, oder auch Otsu’s method genannt, ein Schwellwert definiert werden kann. Dies erfolgt über eine bimodale Verteilung. Wenn ein Bild über keine solche Verteilung verfügt, kann diese Operation nicht ausgeführt werden.
    Aus diesem Grund wurde sich mit der Frontend-Gruppe darauf geeinigt, dass der Service nur mit den o. g. Methoden die Berechnung ausführt.

Für jedes Bild werden insgesamt 3 verschiedene Berechnung durchgeführt:

1. (Simple)-Threshold
2. Adaptive-Threshold mit ADAPTIVE_THRESH_MEAN_C
3. Adaptive-Threshold mit ADAPTIVE_THRESH_GAUSSIAN_C

API
++++
.. toctree::
  :maxdepth: 3

  api