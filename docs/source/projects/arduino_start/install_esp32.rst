.. note::

    Hallo, willkommen in der SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasten-Gemeinschaft auf Facebook! Vertiefen Sie sich mit anderen Enthusiasten in die Welt von Raspberry Pi, Arduino und ESP32.

    **Warum beitreten?**

    - **Expertenunterstützung**: Lösen Sie Nachverkaufsprobleme und technische Herausforderungen mit Hilfe unserer Gemeinschaft und unseres Teams.
    - **Lernen & Teilen**: Tauschen Sie Tipps und Tutorials aus, um Ihre Fähigkeiten zu verbessern.
    - **Exklusive Vorschauen**: Erhalten Sie frühzeitigen Zugang zu neuen Produktankündigungen und exklusiven Einblicken.
    - **Sonderangebote**: Genießen Sie exklusive Rabatte auf unsere neuesten Produkte.
    - **Festliche Aktionen und Gewinnspiele**: Nehmen Sie an Gewinnspielen und Feiertagsaktionen teil.

    👉 Bereit, mit uns zu erkunden und zu kreieren? Klicken Sie auf [|link_sf_facebook|] und treten Sie heute bei!

1.3 Installieren des ESP32-Boards (Wichtig)
================================================

Um den Mikrocontroller ESP32 zu programmieren, müssen wir das ESP32-Board-Paket in der Arduino IDE installieren. Folgen Sie der nachstehenden Schritt-für-Schritt-Anleitung:

**ESP32-Board installieren**

#. Öffnen Sie die Arduino IDE. Gehen Sie zu **Datei** und wählen Sie **Voreinstellungen** aus dem Dropdown-Menü.

    .. image:: img/install_esp321.png

#. Im Fenster „Voreinstellungen“ finden Sie das Feld **Zusätzliche Boardverwalter-URLs**. Klicken Sie darauf, um das Textfeld zu aktivieren.

    .. image:: img/install_esp322.png

#. Fügen Sie die folgende URL in das Feld **Zusätzliche Boardverwalter-URLs** ein: https://espressif.github.io/arduino-esp32/package_esp32_index.json. Diese URL verweist auf die Paketindexdatei für die ESP32-Boards. Klicken Sie auf den **OK**-Button, um die Änderungen zu speichern.

    .. image:: img/install_esp323.png

#. Im Fenster **Boardverwalter** geben Sie **ESP32** in die Suchleiste ein. Klicken Sie auf den **Installieren**-Button, um den Installationsprozess zu starten. Dies lädt das ESP32-Board-Paket herunter und installiert es.

    .. image:: img/install_esp324.png

#. Herzlichen Glückwunsch! Sie haben das ESP32-Board-Paket erfolgreich in der Arduino IDE installiert.

**Code hochladen**

#. Verbinden Sie nun den ESP32 WROOM 32E mit Ihrem Computer über ein Mikro-USB-Kabel.

    .. image:: img/plugin_esp32.png
        :width: 600
        :align: center

#. Wählen Sie dann das richtige Board, **ESP32 Dev Module**, indem Sie auf **Werkzeuge** -> **Board** -> **esp32** klicken.

    .. image:: img/install_esp325.png

#. Wenn Ihr ESP32 mit dem Computer verbunden ist, können Sie den richtigen Port auswählen, indem Sie auf **Werkzeuge** -> **Port** klicken.

    .. image:: img/install_esp326.png

#. Zusätzlich hat Arduino 2.0 eine neue Möglichkeit eingeführt, um das Board und den Port schnell auszuwählen. Bei ESP32 wird es normalerweise nicht automatisch erkannt, daher müssen Sie auf **Anderes Board und anderen Port auswählen** klicken.

    .. image:: img/install_esp327.png

#. Geben Sie im Suchfeld **ESP32 Dev Module** ein und wählen Sie es aus, wenn es erscheint. Wählen Sie dann den richtigen Port aus und klicken Sie auf **OK**.

    .. image:: img/install_esp328.png

#. Anschließend können Sie es über dieses Schnellzugriffsfenster auswählen. Beachten Sie, dass es bei der nachfolgenden Nutzung Zeiten geben kann, in denen der ESP32 im Schnellzugriffsfenster nicht verfügbar ist, und Sie müssen die beiden obigen Schritte wiederholen.

    .. image:: img/install_esp329.png

#. Beide Methoden ermöglichen es Ihnen, das richtige Board und den richtigen Port auszuwählen, also wählen Sie die Methode, die Ihnen am besten passt. Jetzt ist alles bereit, um den Code auf den ESP32 hochzuladen.
