 .. note::

    Hallo, willkommen in der SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasten Community auf Facebook! Tauchen Sie mit anderen Enthusiasten tiefer in Raspberry Pi, Arduino und ESP32 ein.

    **Warum mitmachen?**

    - **Expertenunterstützung**: Lösen Sie Probleme nach dem Verkauf und technische Herausforderungen mit Hilfe unserer Community und unseres Teams.
    - **Lernen & Teilen**: Tauschen Sie Tipps und Tutorials aus, um Ihre Fähigkeiten zu verbessern.
    - **Exklusive Vorschauen**: Erhalten Sie frühzeitigen Zugang zu neuen Produktankündigungen und Sneak Peeks.
    - **Exklusive Rabatte**: Genießen Sie exklusive Rabatte auf unsere neuesten Produkte.
    - **Festliche Aktionen und Gewinnspiele**: Nehmen Sie an Gewinnspielen und Feiertagsaktionen teil.

    👉 Bereit, mit uns zu erkunden und zu kreieren? Klicken Sie auf [|link_sf_facebook|] und treten Sie noch heute bei!

.. _cpn_esp32_wroom_32e:

ESP32 WROOM 32E
=================

Das ESP32 WROOM-32E ist ein vielseitiges und leistungsstarkes Modul, das auf dem ESP32-Chipsatz von Espressif basiert. Es bietet Dual-Core-Prozessoren, integrierte Wi-Fi- und Bluetooth-Konnektivität und verfügt über eine breite Palette von Peripherieschnittstellen. Bekannt für seinen niedrigen Stromverbrauch, ist das Modul ideal für IoT-Anwendungen und ermöglicht eine intelligente Konnektivität und robuste Leistung in kompakten Formfaktoren.

.. image:: img/esp32_wroom_32e.jpg
    :width: 600
    :align: center

Die wichtigsten Merkmale umfassen:

* **Rechenleistung**: Ausgestattet mit einem Dual-Core Xtensa® 32-Bit LX6 Mikroprozessor bietet es Skalierbarkeit und Flexibilität.
* **Drahtlose Fähigkeiten**: Mit integriertem 2,4 GHz Wi-Fi und Dual-Mode Bluetooth ist es perfekt für Anwendungen geeignet, die stabile drahtlose Kommunikation erfordern.
* **Speicher & Speicherplatz**: Es verfügt über ausreichend SRAM und Hochleistungs-Flash-Speicher, um den Anforderungen von Benutzerprogrammen und Datenspeicherung gerecht zu werden.
* **GPIO**: Mit bis zu 38 GPIO-Pins unterstützt es eine Vielzahl von externen Geräten und Sensoren.
* **Niedriger Stromverbrauch**: Mehrere Energiesparmodi stehen zur Verfügung, was es ideal für batteriebetriebene oder energieeffiziente Szenarien macht.
* **Sicherheit**: Integrierte Verschlüsselungs- und Sicherheitsfunktionen sorgen dafür, dass Benutzerdaten und Privatsphäre gut geschützt sind.
* **Vielseitigkeit**: Vom einfachen Haushaltsgerät bis hin zu komplexen industriellen Maschinen bietet das WROOM-32E konsistente, effiziente Leistung.

Zusammenfassend bietet das ESP32 WROOM-32E nicht nur robuste Rechenfähigkeiten und vielfältige Konnektivitätsoptionen, sondern verfügt auch über eine Reihe von Funktionen, die es zur bevorzugten Wahl im IoT- und Smart-Device-Sektor machen.

* |link_esp32_datasheet|

.. _esp32_pinout:

Pinout-Diagramm
-------------------------

Der ESP32 hat einige Pin-Nutzungsbeschränkungen aufgrund verschiedener Funktionen, die bestimmte Pins teilen. Bei der Planung eines Projekts ist es ratsam, die Pin-Nutzung sorgfältig zu planen und auf potenzielle Konflikte zu prüfen, um eine ordnungsgemäße Funktion sicherzustellen und Probleme zu vermeiden.

.. image:: img/esp32_pinout.jpg
    :width: 800
    :align: center

Hier sind einige der wichtigsten Einschränkungen und Überlegungen:

* **ADC1 und ADC2**: ADC2 kann nicht verwendet werden, wenn WiFi oder Bluetooth aktiv ist. ADC1 kann jedoch uneingeschränkt verwendet werden.
* **Bootstrapping-Pins**: GPIO0, GPIO2, GPIO5, GPIO12 und GPIO15 werden während des Bootvorgangs für das Bootstrapping verwendet. Es sollte darauf geachtet werden, keine externen Komponenten anzuschließen, die den Bootvorgang stören könnten.
* **JTAG-Pins**: GPIO12, GPIO13, GPIO14 und GPIO15 können als JTAG-Pins für Debugging-Zwecke verwendet werden. Wenn kein JTAG-Debugging erforderlich ist, können diese Pins als normale GPIOs verwendet werden.
* **Touch-Pins**: Einige Pins unterstützen Touch-Funktionen. Diese Pins sollten sorgfältig verwendet werden, wenn sie für die Touch-Erkennung verwendet werden sollen.
* **Power-Pins**: Einige Pins sind für strombezogene Funktionen reserviert und sollten entsprechend verwendet werden. Beispielsweise sollte vermieden werden, übermäßigen Strom aus den Versorgungspins wie 3V3 und GND zu ziehen.
* **Nur-Eingang-Pins**: Einige Pins sind nur für Eingänge und sollten nicht als Ausgänge verwendet werden.

.. _esp32_strapping:

**Strapping-Pins**
--------------------------

Der ESP32 hat fünf Strapping-Pins:

.. list-table::
    :widths: 5 15
    :header-rows: 1

    *   - Strapping Pins
        - Beschreibung
    *   - IO5
        - Standardmäßig auf Pull-up, die Spannungspegel von IO5 und IO15 beeinflussen das Timing des SDIO-Slave.
    *   - IO0
        - Standardmäßig auf Pull-up, bei Pull-down wird der Download-Modus aktiviert.
    *   - IO2
        - Standardmäßig auf Pull-down, IO0 und IO2 schalten den ESP32 in den Download-Modus.
    *   - IO12(MTDI)
        - Standardmäßig auf Pull-down, bei Pull-up startet der ESP32 nicht normal.
    *   - IO15(MTDO)
        - Standardmäßig auf Pull-up, bei Pull-down sind Debug-Logs nicht sichtbar. Außerdem beeinflussen die Spannungspegel von IO5 und IO15 das Timing des SDIO-Slave.

Software kann die Werte dieser fünf Bits aus dem Register "GPIO_STRAPPING" lesen.
Während des System-Reset-Freigabe des Chips (Power-on-Reset, RTC-Watchdog-Reset und 
Brownout-Reset) werden die Latches der Strapping-Pins auf Spannungspegel als Strapping-Bits 
von "0" oder "1" gesampelt und halten diese Bits, bis der Chip ausgeschaltet oder 
heruntergefahren wird. Die Strapping-Bits konfigurieren den Boot-Modus des Geräts, 
die Betriebsspannung von VDD_SDIO und andere anfängliche Systemeinstellungen.

Jeder Strapping-Pin ist während des Chip-Resets mit seinem internen Pull-up/Pull-down verbunden. 
Folglich, wenn ein Strapping-Pin nicht verbunden ist oder der angeschlossene externe Schaltkreis 
hochohmig ist, bestimmt der interne schwache Pull-up/Pull-down den Standard-Eingangspegel der Strapping-Pins.

Um die Werte der Strapping-Bits zu ändern, können Benutzer externe Pull-down/Pull-up-Widerstände 
anwenden oder die GPIOs des Host-MCUs verwenden, um den Spannungspegel dieser Pins beim Einschalten 
des ESP32 zu steuern.

Nach der Reset-Freigabe arbeiten die Strapping-Pins als normale Funktion-Pins.
Siehe folgende Tabelle für eine detaillierte Boot-Modus-Konfiguration durch Strapping-Pins.

.. image:: img/esp32_strapping.png

* FE: Falling-Edge, RE: Rising-Edge
* Die Firmware kann Registerbits konfigurieren, um die Einstellungen von "Spannung des internen LDO (VDD_SDIO)" und "Timing des SDIO-Slave" nach dem Booten zu ändern.
* Das Modul integriert ein 3,3 V SPI-Flash, daher kann der Pin MTDI beim Einschalten des Moduls nicht auf 1 gesetzt werden.
