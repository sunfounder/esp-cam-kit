.. note::

    Hallo, willkommen in der SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasten-Gemeinschaft auf Facebook! Vertiefen Sie sich mit anderen Enthusiasten in die Welt von Raspberry Pi, Arduino und ESP32.

    **Warum beitreten?**

    - **Expertenunterstützung**: Lösen Sie Nachverkaufsprobleme und technische Herausforderungen mit Hilfe unserer Gemeinschaft und unseres Teams.
    - **Lernen & Teilen**: Tauschen Sie Tipps und Tutorials aus, um Ihre Fähigkeiten zu verbessern.
    - **Exklusive Vorschauen**: Erhalten Sie frühzeitigen Zugang zu neuen Produktankündigungen und exklusiven Einblicken.
    - **Sonderangebote**: Genießen Sie exklusive Rabatte auf unsere neuesten Produkte.
    - **Festliche Aktionen und Gewinnspiele**: Nehmen Sie an Gewinnspielen und Feiertagsaktionen teil.

    👉 Bereit, mit uns zu erkunden und zu kreieren? Klicken Sie auf [|link_sf_facebook|] und treten Sie heute bei!

.. _format_sd_card:

Wie formatiert man eine SD-Karte?
====================================

Die Schritte zur korrekten Formatierung Ihrer SD-Karte können je nach Betriebssystem variieren. Hier sind einfache Schritte, um eine SD-Karte unter Windows, MacOS und Linux zu formatieren:

**Windows**

   #. Stecken Sie Ihre SD-Karte in den Computer, öffnen Sie „Mein Computer“ oder „Dieser PC“. Klicken Sie mit der rechten Maustaste auf Ihre SD-Karte und wählen Sie „Formatieren“.

        .. image:: img/sd_format_win1.png

   #. Wählen Sie im Dropdown-Menü des Dateisystems das gewünschte Dateisystem aus (normalerweise wählen Sie FAT32 oder für SD-Karten über 32GB, müssen Sie vielleicht exFAT wählen). Markieren Sie „Schnellformatierung“ und klicken Sie dann auf „Start“.

        .. image:: img/sd_format_win2.png

**MacOS**
   
   #. Stecken Sie Ihre SD-Karte in den Computer. Öffnen Sie die Anwendung „Festplattendienstprogramm“ (im Ordner „Dienstprogramme“ zu finden).

        .. image:: img/sd_format_mac1.png
    
   #. Wählen Sie Ihre SD-Karte aus der Liste links aus und klicken Sie dann auf „Löschen“.

        .. image:: img/sd_format_mac2.png

   #. Wählen Sie aus dem Dropdown-Menü für das Format das gewünschte Dateisystem aus (normalerweise wählen Sie MS-DOS (FAT) für FAT32 oder ExFAT für SD-Karten über 32GB) und klicken Sie dann auf „Löschen“.

        .. image:: img/sd_format_mac3.png

   #. Warten Sie schließlich, bis die Formatierung abgeschlossen ist.

        .. image:: img/sd_format_mac3.png

**Linux**

   * Zuerst stecken Sie Ihre SD-Karte ein und öffnen dann ein Terminal.
   * Geben Sie ``lsblk`` ein und finden Sie den Namen Ihrer SD-Karte in der Geräteliste (z.B. könnte es „sdb“ sein).
   * Verwenden Sie den Befehl ``umount``, um die SD-Karte auszuhängen, wie ``sudo umount /dev/sdb*``.
   * Verwenden Sie den Befehl ``mkfs``, um die SD-Karte zu formatieren. Zum Beispiel wird ``sudo mkfs.vfat /dev/sdb1`` die SD-Karte im FAT32-Dateisystem formatieren (für SD-Karten über 32GB, könnten Sie ``mkfs.exfat`` benötigen).

Bevor Sie Ihre SD-Karte formatieren, stellen Sie sicher, dass Sie alle wichtigen Daten auf der SD-Karte sichern, da der Formatierungsvorgang alle Dateien auf der SD-Karte löschen wird.