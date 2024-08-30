 .. note::

    Hallo, willkommen in der SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasten-Gemeinschaft auf Facebook! Vertiefen Sie sich mit anderen Enthusiasten in die Welt von Raspberry Pi, Arduino und ESP32.

    **Warum beitreten?**

    - **Expertenunterstützung**: Lösen Sie Nachverkaufsprobleme und technische Herausforderungen mit Hilfe unserer Gemeinschaft und unseres Teams.
    - **Lernen & Teilen**: Tauschen Sie Tipps und Tutorials aus, um Ihre Fähigkeiten zu verbessern.
    - **Exklusive Vorschauen**: Erhalten Sie frühzeitigen Zugang zu neuen Produktankündigungen und exklusiven Einblicken.
    - **Sonderangebote**: Genießen Sie exklusive Rabatte auf unsere neuesten Produkte.
    - **Festliche Aktionen und Gewinnspiele**: Nehmen Sie an Gewinnspielen und Feiertagsaktionen teil.

    👉 Bereit, mit uns zu erkunden und zu kreieren? Klicken Sie auf [|link_sf_facebook|] und treten Sie heute bei!

.. _iot_html_cam:

2.14 Benutzerdefinierter Video-Streaming-Webserver
=======================================================

Das Projekt des benutzerdefinierten Video-Streaming-Webservers bietet die Möglichkeit, zu lernen, wie man eine Webseite von Grund auf erstellt und sie anpasst, um Videostreams abzuspielen. Darüber hinaus können Sie interaktive Schaltflächen wie EIN und AUS einbinden, um die Helligkeit der LED zu steuern.

Durch den Aufbau dieses Projekts erhalten Sie praktische Erfahrungen in Webentwicklung, HTML, CSS und JavaScript. Sie lernen, wie Sie eine responsive Webseite erstellen, die Videostreams in Echtzeit anzeigen kann. Außerdem entdecken Sie, wie Sie interaktive Schaltflächen integrieren, um den Zustand der LED zu steuern, und so ein dynamisches Benutzererlebnis bieten.

**Benötigte Komponenten**

In diesem Projekt benötigen wir die folgenden Komponenten.

.. list-table::
    :widths: 30 20
    :header-rows: 1

    *   - KOMPONENTEN-BESCHREIBUNG
        - KAUFLINK

    *   - :ref:`cpn_esp32_wroom_32e`
        - |link_esp32_wroom_32e_buy|
    *   - :ref:`cpn_esp32_camera_extension`
        - \-
    *   - Steckbrett
        - |link_breadboard_buy|
    *   - Mehrere Verbindungskabel
        - |link_wires_buy|
    *   - Widerstand
        - |link_resistor_buy|
    *   - LED
        - |link_led_buy|

**Wie geht das?**

#. Zuerst die Kamera einstecken.

    .. raw:: html

        <video loop autoplay muted style = "max-width:100%">
            <source src="../_static/video/plugin_camera.mp4" type="video/mp4">
            Your browser does not support the video tag.
        </video>

#. Den Schaltkreis aufbauen.

    .. image:: img/iot_3_html_led_bb.png

#. Dann den ESP32-WROOM-32E mit dem USB-Kabel an den Computer anschließen.

    .. image:: img/plugin_esp32.png

#. |link_download_this_code| herunter oder kopieren Sie ihn direkt in die Arduino IDE.

    .. note::
        
        * :ref:`unknown_com_port`
 
    .. raw:: html

        <iframe src=https://create.arduino.cc/editor/sunfounder01/a5e33c30-63dc-4987-94c3-89bc6a599e24/preview?embed style="height:510px;width:100%;margin:10px 0" frameborder=0></iframe>

#. Finden Sie die folgenden Zeilen und ändern Sie sie mit Ihrem ``<SSID>`` und ``<PASSWORD>``.

    .. code-block::  Arduino

        // Ersetzen Sie die nächsten Variablen mit Ihrer SSID/Passwort-Kombination
        const char* ssid = "<SSID>";
        const char* password = "<PASSWORD>";

#. Wählen Sie das richtige Board (ESP32 Dev Module) und den richtigen Port aus und klicken Sie auf die Schaltfläche **Upload**.

#. Im Seriellen Monitor sehen Sie eine erfolgreiche WLAN-Verbindungsnachricht und die zugewiesene IP-Adresse.

    .. code-block:: 

        WiFi connected
        Camera Stream Ready! Go to: http://192.168.18.77

#. Geben Sie die IP-Adresse in Ihren Webbrowser ein. Sie werden auf die unten gezeigte Webseite weitergeleitet, auf der Sie die benutzerdefinierten EIN- und AUS-Schaltflächen verwenden können, um die LED zu steuern.

    .. image:: img/sp230510_180503.png 

#. Setzen Sie eine Batterie in das Erweiterungsboard ein und entfernen Sie das USB-Kabel. Nun können Sie das Gerät überall innerhalb der Reichweite des Wi-Fi platzieren.

    .. image:: img/plugin_battery.png
