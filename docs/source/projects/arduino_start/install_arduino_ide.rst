.. note::

    Hallo, willkommen in der SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasten-Gemeinschaft auf Facebook! Vertiefen Sie sich mit anderen Enthusiasten in die Welt von Raspberry Pi, Arduino und ESP32.

    **Warum beitreten?**

    - **Expertenunterstützung**: Lösen Sie Nachverkaufsprobleme und technische Herausforderungen mit Hilfe unserer Gemeinschaft und unseres Teams.
    - **Lernen & Teilen**: Tauschen Sie Tipps und Tutorials aus, um Ihre Fähigkeiten zu verbessern.
    - **Exklusive Vorschauen**: Erhalten Sie frühzeitigen Zugang zu neuen Produktankündigungen und exklusiven Einblicken.
    - **Sonderangebote**: Genießen Sie exklusive Rabatte auf unsere neuesten Produkte.
    - **Festliche Aktionen und Gewinnspiele**: Nehmen Sie an Gewinnspielen und Feiertagsaktionen teil.

    👉 Bereit, mit uns zu erkunden und zu kreieren? Klicken Sie auf [|link_sf_facebook|] und treten Sie heute bei!

.. _install_arduino:

1.1 Arduino IDE installieren (Wichtig)
===========================================

Die Arduino IDE, bekannt als Arduino Integrated Development Environment, bietet alle notwendige Softwareunterstützung, um ein Arduino-Projekt zu vollenden. Es ist eine speziell für Arduino entwickelte Programmiersoftware, die vom Arduino-Team bereitgestellt wird und es uns ermöglicht, Programme zu schreiben und sie auf das Arduino-Board hochzuladen.

Die Arduino IDE 2.0 ist ein Open-Source-Projekt. Sie ist ein großer Schritt von ihrem robusten Vorgänger, der Arduino IDE 1.x, und kommt mit einer überarbeiteten Benutzeroberfläche, verbessertem Board- und Bibliotheksmanager, Debugger, Autocomplete-Funktion und vielem mehr.

In diesem Tutorial zeigen wir, wie Sie die Arduino IDE 2.0 auf Ihrem Windows-, Mac- oder Linux-Computer herunterladen und installieren.

Voraussetzungen
----------------------

* Windows - Win 10 und neuer, 64 Bit
* Linux - 64 Bit
* Mac OS X - Version 10.14: „Mojave“ oder neuer, 64 Bit

Download der Arduino IDE 2.0
----------------------------------

#. Besuchen Sie |link_download_arduino|.

#. Laden Sie die IDE für Ihre OS-Version herunter.

    .. image:: img/sp_001.png

Installation
------------------------------

Windows
^^^^^^^^^^^^^

#. Doppelklicken Sie auf die Datei ``arduino-ide_xxxx.exe``, um die heruntergeladene Datei auszuführen.

#. Lesen Sie die Lizenzvereinbarung und stimmen Sie ihr zu.

    .. image:: img/sp_002.png

#. Wählen Sie Installationsoptionen.

    .. image:: img/sp_003.png

#. Wählen Sie den Installationsort. Es wird empfohlen, die Software auf einem anderen Laufwerk als dem Systemlaufwerk zu installieren.

    .. image:: img/sp_004.png

#. Dann beenden.

    .. image:: img/sp_005.png

macOS
^^^^^^^^^^^^^^^^

Doppelklicken Sie auf die heruntergeladene Datei „arduino_ide_xxxx.dmg“ und folgen Sie den Anweisungen, um die **Arduino IDE.app** in den **Anwendungen**-Ordner zu kopieren. Sie werden sehen, dass die Arduino IDE nach wenigen Sekunden erfolgreich installiert wurde.

.. image:: img/macos_install_ide.png
    :width: 800

Linux
^^^^^^^^^^^^

Für das Tutorial zur Installation der Arduino IDE 2.0 auf einem Linux-System, besuchen Sie bitte: https://docs.arduino.cc/software/ide-v2/tutorials/getting-started/ide-v2-downloading-and-installing#linux

IDE öffnen
--------------

#. Wenn Sie die Arduino IDE 2.0 zum ersten Mal öffnen, installiert sie automatisch die Arduino AVR-Boards, integrierte Bibliotheken und andere erforderliche Dateien.

    .. image:: img/sp_901.png

#. Außerdem könnte Ihr Firewall- oder Sicherheitszentrum einige Male aufpoppen und fragen, ob Sie einige Gerätetreiber installieren möchten. Bitte installieren Sie alle.

    .. image:: img/sp_104.png

#. Jetzt ist Ihre Arduino IDE bereit!

    .. note::
        Falls einige Installationen aufgrund von Netzwerkproblemen oder anderen Gründen nicht funktioniert haben, können Sie die Arduino IDE erneut öffnen und die restliche Installation wird abgeschlossen. Das Ausgabefenster wird nicht automatisch geöffnet, nachdem alle Installationen abgeschlossen sind, es sei denn, Sie klicken auf Überprüfen oder Hochladen.
