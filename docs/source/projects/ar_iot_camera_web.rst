 .. note::

    Hallo, willkommen in der SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasten-Gemeinschaft auf Facebook! Vertiefen Sie sich mit anderen Enthusiasten in die Welt von Raspberry Pi, Arduino und ESP32.

    **Warum beitreten?**

    - **Expertenunterstützung**: Lösen Sie Nachverkaufsprobleme und technische Herausforderungen mit Hilfe unserer Gemeinschaft und unseres Teams.
    - **Lernen & Teilen**: Tauschen Sie Tipps und Tutorials aus, um Ihre Fähigkeiten zu verbessern.
    - **Exklusive Vorschauen**: Erhalten Sie frühzeitigen Zugang zu neuen Produktankündigungen und exklusiven Einblicken.
    - **Sonderangebote**: Genießen Sie exklusive Rabatte auf unsere neuesten Produkte.
    - **Festliche Aktionen und Gewinnspiele**: Nehmen Sie an Gewinnspielen und Feiertagsaktionen teil.

    👉 Bereit, mit uns zu erkunden und zu kreieren? Klicken Sie auf [|link_sf_facebook|] und treten Sie heute bei!

.. _iot_camera_web:

2.13 Kamera-Webserver
=============================

Dieses Projekt kombiniert das ESP32-Board mit einem Kameramodul, um hochqualitatives Video über ein lokales Netzwerk zu streamen. 
Richten Sie Ihr eigenes Kamerasystem mühelos ein und überwachen Sie jeden Ort in Echtzeit.

Mit der Webschnittstelle des Projekts können Sie von jedem mit dem Netzwerk verbundenen Gerät auf den Kamera-Feed zugreifen und diesen steuern. 
Passen Sie die Kameraeinstellungen an, um das Streaming-Erlebnis zu optimieren, und nehmen Sie Einstellungen bequem über die benutzerfreundliche Oberfläche vor.

Erweitern Sie Ihre Überwachungs- oder Live-Streaming-Fähigkeiten mit dem vielseitigen ESP32-Kamera-Streaming-Projekt. Überwachen Sie Ihr Zuhause, Büro oder jeden gewünschten Ort mit Leichtigkeit und Zuverlässigkeit.

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

**Wie geht das?**

#. Zuerst die Kamera einstecken.

    .. raw:: html

        <video loop autoplay muted style = "max-width:100%">
            <source src="../_static/video/plugin_camera.mp4" type="video/mp4">
            Your browser does not support the video tag.
        </video>

#. Dann den ESP32-WROOM-32E mit dem USB-Kabel an den Computer anschließen.

    .. image:: img/plugin_esp32.png

#. Laden Sie diesen Code herunter oder kopieren Sie ihn direkt in die Arduino IDE.

    .. note::

        * :ref:`unknown_com_port`

    .. raw:: html

        <iframe src=https://create.arduino.cc/editor/sunfounder01/15e00b39-34e1-49f9-b039-f10053d31407/preview?embed style="height:510px;width:100%;margin:10px 0" frameborder=0></iframe>

#. Finden Sie die folgenden Zeilen und ändern Sie sie mit Ihrem ``<SSID>`` und ``<PASSWORD>``.

    .. code-block::  Arduino

        // Ersetzen Sie die nächsten Variablen mit Ihrer SSID/Passwort-Kombination
        const char* ssid = "<SSID>";
        const char* password = "<PASSWORD>";

#. Aktivieren Sie nun **PSRAM**.

    .. image:: img/sp230516_150554.png

#. Stellen Sie das Partitionsschema auf **Huge APP (3MB No OTA/1MB SPIFFS)** ein.

    .. image:: img/sp230516_150840.png

#. Wählen Sie das richtige Board (ESP32 Dev Module) und den richtigen Port aus und klicken Sie auf die Schaltfläche "Upload".

#. Im Seriellen Monitor sehen Sie eine erfolgreiche WLAN-Verbindungsnachricht und die zugewiesene IP-Adresse.

    .. code-block::

        .....
        WiFi connected
        Starting web server on port: '80'
        Starting stream server on port: '81'
        Camera Ready! Use 'http://192.168.18.77' to connect

#. Geben Sie die IP-Adresse in Ihren Webbrowser ein. Sie sehen eine Weboberfläche, auf der Sie auf **Start Stream** klicken können, um den Kamerastream anzuzeigen.

    .. image:: img/sp230516_151521.png

#. Scrollen Sie zurück nach oben, wo Sie den Live-Kamera-Feed sehen können. Sie können die Einstellungen auf der linken Seite der Oberfläche anpassen.

    .. image:: img/sp230516_180520.png

.. note:: 

    * Dieses ESP32-Modul unterstützt Gesichtserkennung. Um dies zu aktivieren, stellen Sie die Auflösung auf 240x240 ein und schalten Sie die Option Gesichtserkennung am unteren Rand der Oberfläche ein.
    * Dieses ESP32-Modul unterstützt keine Gesichtserkennung.
