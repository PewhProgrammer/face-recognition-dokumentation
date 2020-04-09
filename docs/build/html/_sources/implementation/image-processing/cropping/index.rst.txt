Cropping Service
========================
Dieser Service, soll anhand eines Bildes ein Porträtfoto erzeugen. Ein externer Aufruf war für diesen Service nicht vorgesehen.
Dennoch ist er notwendig, da die Schwarz-Weiß-Bilder die von Gruppe 6 in ihrem UI angezeigt werden,
entsprechend zugeschnitten sein müssen. Des Weiteren muss die Auflösung des Porträtfotos adressierbar sein, da der Laser, der am Ende der Produktion
für die Eingravierung zuständig ist, die Bilder in dem Format 600x600px benötigt.

Evaluation
++++++++++++++++
Für die Bildung des Porträtfotos muss zunächst ein ROI definiert werden. Dieser definiert sich in folgendem Anwendungsszenario aus dem Gesicht,
sowie einem Offset, da auf dem Porträtfoto Haare und Teil vom Hals zu sehen sein sollen.
Im Folgenden wird aufgeführt, für welchen Lösungsansatz wir uns entschieden haben, um eine zuverlässige Gesichtserkennung durchzuführen.

Folgende Set-ups haben sich als möglichen Lösungsansatz definieren lassen:

1.  Haar Cascade Face Detector in OpenCV
2.  Deep Learning basierter Face Detector in OpenCV
3.  HoG Face Detector in Dlib
4.  Deep Learning basierter Face Detector in Dlib

Da bei Variante 3) und Variante 4) nur Gesichter mit einer Größe von >= 80x80 erkannt werden können. Haben wir uns für explizit gegen diese entschieden.

Die Varianten 1) und 2) werden nun mithilfe des folgenden Beispielbildes verglichen:

.. image:: ../../../_static/images/girl.jpg
   :width: 600


Haar Cascade Face Detector in OpenCV
----------------------------------------

Die Haar Casecade Gesichtserkennung war seit seiner Vorstellung im Jahr 2001 für viele Jahre das State of the Art Verfahren, um eine Gesichtserkennung durchzuführen. 
Über die Jahre hinweg, wurde dieses Verfahren stetig verbessert. In OpenCv kann solch eine Erkennung sehr einfach durchgeführt werden, indem der hierfür entsprechenden Methode,
ein Descriptor File als Parameter mitgegeben wird. Diese stellt Opencv ebenfalls zur Verfügung.

Dabei konnten wir die besten Ergebnisse mit folgendem Descriptor File erzielen: ``haarcascade_frontalface_alt.xml``

Mit diesem Verfahren konnte das Gesicht aus unserem Beispiel erfolgreich erkannt werden:

.. image:: ../../../_static/images/haarcascade_girl.jpg
   :width: 600


Der Nachteil von diesem Ansatz ist jedoch, dass bei nicht frontal geschossenen Bilder, keine Gesichter erkannt werden. 

Deep Learning basierter Face Detector in OpenCV
----------------------------------------------------------
Dieses Model wurde mit der Version 3.3 von OpenCv veröffentlicht. Es handelt sich um Single-Shot-Mutlibox detector, der eine ResNet-10 Architektur als Backbone verwendet.
Das Model wurde basierend auf Bilder aus dem Internet trainiert. Leider wurde nicht bekannt gegeben, welche Bilder genau zum Trainieren verwendet wurden.

Dieser Face Detector, kann mit zwei Models ausgeführt werden:

1. FP16 version of the original caffe implementation
2. 8 bit Quantized version using Tensorflow

Wir haben uns aus Performancegründen für die 8bit Quantized Version entschieden.

Mit diesem Verfahren konnte das Gesicht aus unserem Beispiel erfolgreich erkannt werden:

.. image:: ../../../_static/images/dnn_girl.jpg
   :width: 600

Des Weiteren können mit diesem Lösungsansatz auch Gesichter erkannt werden, die seitlich fotografiert wurden:

.. image:: ../../../_static/images/dnn_side.jpg
   :width: 600

.. note::

   Für einen weiteren Vergleich aller Lösungsansätze siehe: https://www.learnopencv.com/face-detection-opencv-dlib-and-deep-learning-c-python/

Cropping
++++++++++++++
Zunächst wird auf dem Originalbild das Gesicht mit einem Deep Learning basierten Face Detector in OpenCV erkannt.

.. image:: ../../../_static/images/dnn_flori.jpg
   :width: 600

Danach wird der ROI definiert, indem auf die Bounding-Box ein Offset drauf gerechnet wird. Daraufhin wird das Porträtfoto erzeugt.

.. image:: ../../../_static/images/flori_cropped.jpg
   :width: 600


Anschließend wird das Foto auf die Zielgröße skaliert, ohne das Seitenverhältnis zu verlieren. 




API
++++
Obwohl der Service derzeit nur in dem Use Case von dem Schwarz-Weiß-Service stattfindet und nicht extern aufgerufen wird, wurde die Möglichkeit geschaffen, dass dies möglich ist.
Für weitere Informationen siehe API-Referenz

.. toctree::
  :maxdepth: 3

  api