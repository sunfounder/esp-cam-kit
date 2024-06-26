.. note::

    Hallo, willkommen in der SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasten-Gemeinschaft auf Facebook! Vertiefen Sie sich mit anderen Enthusiasten in die Welt von Raspberry Pi, Arduino und ESP32.

    **Warum beitreten?**

    - **Expertenunterstützung**: Lösen Sie Nachverkaufsprobleme und technische Herausforderungen mit Hilfe unserer Gemeinschaft und unseres Teams.
    - **Lernen & Teilen**: Tauschen Sie Tipps und Tutorials aus, um Ihre Fähigkeiten zu verbessern.
    - **Exklusive Vorschauen**: Erhalten Sie frühzeitigen Zugang zu neuen Produktankündigungen und exklusiven Einblicken.
    - **Sonderangebote**: Genießen Sie exklusive Rabatte auf unsere neuesten Produkte.
    - **Festliche Aktionen und Gewinnspiele**: Nehmen Sie an Gewinnspielen und Feiertagsaktionen teil.

    👉 Bereit, mit uns zu erkunden und zu kreieren? Klicken Sie auf [|link_sf_facebook|] und treten Sie heute bei!

.. _add_libraries_ar:

1.4 Bibliotheken installieren (Wichtig)
==============================================

Eine Bibliothek ist eine Sammlung von vorgeschriebenem Code oder Funktionen, die die Möglichkeiten der Arduino IDE erweitern. Bibliotheken bieten fertigen Code für verschiedene Funktionalitäten, sodass Sie Zeit und Mühe beim Programmieren komplexer Features sparen können.

Es gibt zwei Hauptmethoden zur Installation von Bibliotheken:

Installation über den Bibliotheksmanager
-------------------------------------------------

Viele Bibliotheken sind direkt über den Arduino-Bibliotheksmanager verfügbar. Sie können den Bibliotheksmanager mit folgenden Schritten aufrufen:

#. Im **Bibliotheksmanager** können Sie nach dem gewünschten Bibliotheksnamen suchen oder durch verschiedene Kategorien stöbern.

   .. note::

      In Projekten, in denen eine Bibliotheksinstallation erforderlich ist, gibt es Hinweise, welche Bibliotheken installiert werden müssen. Befolgen Sie die angegebenen Anweisungen, wie zum Beispiel: "Die DHT-Sensorbibliothek wird hier verwendet, Sie können sie aus dem Bibliotheksmanager installieren." Installieren Sie einfach die empfohlenen Bibliotheken wie angegeben.

   .. image:: img/install_lib3.png

#. Wenn Sie die gewünschte Bibliothek gefunden haben, klicken Sie darauf und dann auf die Schaltfläche **Installieren**.

   .. image:: img/install_lib2.png

#. Die Arduino IDE wird automatisch die Bibliothek für Sie herunterladen und installieren.

.. _install_lib_man:

Manuelle Installation
--------------------------

Einige Bibliotheken sind nicht über den **Bibliotheksmanager** verfügbar und müssen manuell installiert werden. Um diese Bibliotheken zu installieren, folgen Sie diesen Schritten:

#. Laden Sie die Bibliotheken herunter.

   * :download:`ESP32-A2DP </_static/zip/ESP32-A2DP.zip>`
   * :download:`ESP8266Audio </_static/zip/ESP8266Audio.zip>`

#. Öffnen Sie die Arduino IDE und gehen Sie zu **Sketch** -> **Bibliothek einbinden** -> **.ZIP-Bibliothek hinzufügen**.

   .. image:: img/a2dp_add_zip.png

#. Navigieren Sie zum Verzeichnis, in dem sich die Bibliotheksdateien befinden, und wählen Sie die gewünschte Bibliotheksdatei, wie z.B. ``ESP32-A2DP.zip`` aus. Dann klicken Sie auf **Öffnen**.


   .. image:: img/a2dp_choose.png

#. Nach kurzer Zeit erhalten Sie eine Benachrichtigung über eine erfolgreiche Installation.

   .. image:: img/a2dp_success.png

#. Wiederholen Sie denselben Prozess, um die Bibliothek ``ESP8266Audio.zip`` hinzuzufügen.


.. note::

   Die mit einem der oben genannten Verfahren installierten Bibliotheken befinden sich im standardmäßigen Bibliotheksverzeichnis der Arduino IDE, das üblicherweise unter ``C:\Users\xxx\Documents\Arduino\libraries`` zu finden ist.

   Wenn Ihr Bibliotheksverzeichnis anders ist, können Sie es überprüfen, indem Sie zu **Datei** -> **Einstellungen** gehen.

      .. image:: img/install_lib1.png