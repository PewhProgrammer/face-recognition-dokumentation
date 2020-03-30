Cropping Service
========================
Dieser Service, soll anhand eines Bildes ein Porträtfoto erzeugen. Ein externer Aufruf war für diesen Service nicht vorgesehen.
Dennoch ist er notwendig, da die Schwarz-Weiß-Bilder die von Gruppe 6 in ihrem UI angezeigt werden,
entsprechend zugeschnitten sein müssen. Des Weiteren muss die Auflösung des Porträtfotos adressierbar sein, da der Laser, der am Ende der Produktion
für die Eingravierung zuständig ist, die Bilder in dem Format 600x600px benötigt.

Evaluation
++++++++++++++++
Für diesen Service wurde evaluiert, wie am effizientesten ein ROI (Range of Intereset) definiert werden kann.
Der ROI für diesen Service definiert sich aus der Erkennung des Gesichts und einem Offset, da auf dem Porträtfoto
Haare und Teil vom Hals zu sehen sein soll. 

Nach einer Recherche haben sich hierfür folgende Set-ups als möglichen Lösungsansatz definieren lassen:

1.  Haar Cascade Face Detector in OpenCV
2.  Deep Learning basierter Face Detector in OpenCV
3.  HoG Face Detector in Dlib
4.  Deep Learning basierter Face Detector in Dlib

.. note::

    Für einen genauen Vergleich der Lösungsansätze siehe: https://www.learnopencv.com/face-detection-opencv-dlib-and-deep-learning-c-python/

Zunächst wurde für den Service die Variante 1) verwendet. Der Grund hierfür war, dass nur ein einfaches Descriptor-File, das von OpenCV bereitgestellt wird, verwendet werden musste.
Der Nachteil von diesem Ansatz ist jedoch, dass keine Gesichter von nicht frontal geschossenen Bilder erkannt werden. Da der Use-Case von diesem Service darauf abzielt, dass ein frontal geschossenes Foto an den Service übermittelt wird, war dies vollkommen ausreichend und hat auch funktioniert. 
Dennoch waren wir gewollt eine andere Variante zu wählen, bei der diese Probleme entfallen.

Wenn es darum geht, den performantesten Lösungsansatz zu wählen, voraussichtlich die Berechnung erfolgt auf der CPU, dann würde die Variante 4) hervorgehen. Das Problem bei dieser ist jedoch, dass Gesichter die kleiner als 70x70px sind, nicht erkannt werden. 
Aus diesem Grund wurde sich für die Variante 2) entschieden. Sie weist keinerlei Nachteile vor, außer das sie minimal langsamer ist, als Variante 4).

Cropping
++++++++++++++
Zunächst wird auf dem Originalbild der ROI mit einem Deep Learning basierten Face Detector in OpenCV definiert. Danach wird ein Offset drauf gerechnet, sodass ein Porträtfoto entsteht.
Anschließend wird das Foto auf die Zielgröße skaliert, ohne das Seitenverhältnis zu verlieren. 
Obwohl der Service derzeit nur in dem Use Case von dem Schwarz-Weiß-Service stattfindet und nicht extern aufgerufen wird, wurde die Möglichkeit geschaffen, dass dies möglich ist.
Der Grund dafür lässt sich auf die Version dieses gesamten Service reduzieren. Sämtliche Bildverarbeitungsschritte, sollen sowohl extern als intern aufgerufen und kombiniert werden können.

API
++++
.. toctree::
  :maxdepth: 3

  api