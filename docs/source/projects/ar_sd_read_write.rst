 .. note::

    Hallo, willkommen in der SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasten-Community auf Facebook! Tauchen Sie gemeinsam mit anderen Enthusiasten tiefer in die Welt von Raspberry Pi, Arduino und ESP32 ein.

    **Warum beitreten?**

    - **Expertenunterstützung**: Lösen Sie Nachverkaufsprobleme und technische Herausforderungen mit Hilfe unserer Gemeinschaft und unseres Teams.
    - **Lernen & Teilen**: Tauschen Sie Tipps und Tutorials aus, um Ihre Fähigkeiten zu verbessern.
    - **Exklusive Vorschauen**: Erhalten Sie frühzeitigen Zugang zu neuen Produktankündigungen und exklusiven Einblicken.
    - **Sonderangebote**: Genießen Sie exklusive Rabatte auf unsere neuesten Produkte.
    - **Festliche Aktionen und Gewinnspiele**: Nehmen Sie an Gewinnspielen und Feiertagsaktionen teil.

    👉 Bereit, mit uns zu erkunden und zu kreieren? Klicken Sie auf [|link_sf_facebook|] und treten Sie heute bei!

.. _ar_sd_write:

2.10 SD-Karte Schreiben und Lesen
=================================
Dieses Projekt demonstriert die grundlegenden Fähigkeiten der Verwendung einer SD-Karte mit dem ESP32-Mikrocontroller. 
Es zeigt wesentliche Operationen wie das Einbinden der SD-Karte, das Erstellen einer Datei, das Schreiben von Daten in die Datei 
und das Auflisten aller Dateien im Stammverzeichnis. Diese Operationen bilden die Grundlage vieler Datenprotokollierungs- und Speicheranwendungen und machen dieses Projekt zu einem wichtigen Schritt im Verständnis und der Nutzung des integrierten SDMMC-Host-Peripheriegeräts des ESP32.

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


**Betriebsschritte**

#. Bevor Sie das USB-Kabel anschließen, stecken Sie die SD-Karte in den SD-Kartensteckplatz der Erweiterungsplatine.

    .. image:: img/insert_sd.png

#. Verbinden Sie das ESP32-WROOM-32E mit dem Computer über das USB-Kabel.

    .. image:: img/plugin_esp32.png

#. |link_download_this_code| herunter oder kopieren Sie ihn direkt in die Arduino IDE.Wählen Sie den entsprechenden Port und das Board in der Arduino IDE und laden Sie den Code auf Ihr ESP32 hoch.

   .. code-block:: arduino

        #include "SD_MMC.h"

        void setup() {
          Serial.begin(115200); // Initialize serial communication
          if (!SD_MMC.begin()) { // Check if SD card is mounted successfully
            Serial.println("Failed to mount SD card"); // Print error message if SD card failed to mount
            return;
          }

          // Create a file in the root directory of SD card and open it in write mode.
          File file = SD_MMC.open("/test.txt", FILE_WRITE);
          if (!file) {
            Serial.println("Failed to open file for writing"); // Print error message if file failed to open
            return;
          }
          if (file.println("Test file write")) { // Write a line of text to the file
            Serial.println("File write successful"); // Print success message if write operation is successful
          } else {
            Serial.println("File write failed"); // Print error message if write operation failed
          }
          file.close(); // Close the file

          File root = SD_MMC.open("/"); // Open the root directory of SD card
          if (!root) {
            Serial.println("Failed to open directory"); // Print error message if directory failed to open
            return;
          }
          Serial.println("Files found in root directory:"); // Print the list of files found in the root directory
          while (File file = root.openNextFile()) { // Loop through all the files in the root directory
            Serial.print("  ");
            Serial.print(file.name()); // Print the filename
            Serial.print("\t");
            Serial.println(file.size()); // Print the filesize
            file.close(); // Close the file
          }
        }
        void loop() {} // Empty loop function, does nothing

   * :ref:`unknown_com_port`
        

#. Nachdem der Code erfolgreich hochgeladen wurde, sehen Sie eine Meldung, die den erfolgreichen Dateischreibvorgang anzeigt, zusammen mit einer Liste aller Dateinamen und Größen auf der SD-Karte. Wenn Sie nach dem Öffnen des seriellen Monitors keine Ausgabe sehen, müssen Sie die EN (RST)-Taste drücken, um den Code erneut auszuführen.

    .. image:: img/sd_write_read.png

    .. note::

        Wenn Sie die folgende Information sehen.

        .. code-block::

            E (528) vfs_fat_sdmmc: mount_to_vfs failed (0xffffffff).
            Failed to mount SD card

        Überprüfen Sie zunächst, ob Ihre SD-Karte ordnungsgemäß in die Erweiterungsplatine eingesteckt ist.

        Wenn sie korrekt eingesteckt ist, könnte es ein Problem mit Ihrer SD-Karte geben. Sie können versuchen, die Metallkontakte mit einem Radiergummi zu reinigen.

        Wenn das Problem weiterhin besteht, wird empfohlen, die SD-Karte zu formatieren. Bitte beziehen Sie sich auf :ref:`format_sd_card`.


**Wie funktioniert das?**

Der Zweck dieses Projekts ist es, die Verwendung der SD-Karte mit dem ESP32-Board zu demonstrieren. Das integrierte SDMMC-Host-Peripheriegerät des ESP32 wird verwendet, um eine Verbindung zur SD-Karte herzustellen.

Das Projekt beginnt mit der Initialisierung der seriellen Kommunikation und versucht dann, die SD-Karte einzubinden. Wenn das Einbinden der SD-Karte fehlschlägt, gibt das Programm eine Fehlermeldung aus und beendet die Setup-Funktion.

Sobald die SD-Karte erfolgreich eingebunden ist, erstellt das Programm eine Datei namens "test.txt" im Stammverzeichnis der SD-Karte. Wenn die Datei im Schreibmodus erfolgreich geöffnet wird, schreibt das Programm eine Textzeile - "Hello, world!" - in die Datei. Das Programm gibt eine Erfolgsmeldung aus, wenn der Schreibvorgang erfolgreich ist, andernfalls wird eine Fehlermeldung gedruckt.

Nach Abschluss des Schreibvorgangs wird die Datei geschlossen und das Stammverzeichnis der SD-Karte geöffnet. Anschließend beginnt das Programm, durch alle Dateien im Stammverzeichnis zu schleifen und den Dateinamen sowie die Dateigröße jeder gefundenen Datei im seriellen Monitor anzuzeigen.

In der Hauptschleifenfunktion gibt es keine Operationen. Dieses Projekt konzentriert sich auf SD-Karten-Operationen wie das Einbinden der Karte, das Erstellen einer Datei, das Schreiben in eine Datei und das Lesen des Datei-Verzeichnisses, die alle in der Setup-Funktion ausgeführt werden.

Dieses Projekt dient als nützliche Einführung in den Umgang mit SD-Karten beim ESP32, was bei Anwendungen, die Datenprotokollierung oder -speicherung erfordern, von entscheidender Bedeutung sein kann.


Hier ist eine Analyse des Codes:

#. Einbinden der ``SD_MMC``-Bibliothek, die benötigt wird, um mit SD-Karten über das integrierte SDMMC-Host-Peripheriegerät des ESP32 zu arbeiten.

    .. code-block:: arduino

        #include "SD_MMC.h"

#. Innerhalb der ``setup()``-Funktion werden die folgenden Aufgaben ausgeführt.

    * **Initialisieren der SD-Karte**

    Initialisieren und Einbinden der SD-Karte. Wenn das Einbinden der SD-Karte fehlschlägt, wird "Failed to mount SD card" im seriellen Monitor gedruckt und die Ausführung gestoppt.

    .. code-block:: arduino
        
        if(!SD_MMC.begin()) { // Versuch, die SD-Karte einzubinden
            Serial.println("Failed to mount card"); // Wenn das Einbinden fehlschlägt, im seriellen Monitor drucken und Setup beenden
            return;
        } 
      
    * **Öffnen der Datei**

    Öffnen einer Datei namens ``"test.txt"``, die sich im Stammverzeichnis der SD-Karte befindet, im Schreibmodus. Wenn das Öffnen der Datei fehlschlägt, wird "Failed to open file for writing" gedruckt und zurückgegeben.

    .. code-block:: arduino

        File file = SD_MMC.open("/test.txt", FILE_WRITE); 
        if (!file) {
            Serial.println("Failed to open file for writing"); // Print error message if file failed to open
            return;
        }

    * **Daten in die Datei schreiben**

    Schreiben des Textes "Test file write" in die Datei. 
    Wenn die Schreiboperation erfolgreich ist, wird "File write successful" gedruckt; andernfalls wird "File write failed" gedruckt.

    .. code-block:: arduino

        if(file.print("Test file write")) { // Die Nachricht in die Datei schreiben
            Serial.println("File write success"); // Wenn das Schreiben erfolgreich ist, im seriellen Monitor drucken
        } else {
            Serial.println("File write failed"); // Wenn das Schreiben fehlschlägt, im seriellen Monitor drucken
        } 

    * **Schließen der Datei**
        
    Schließen der geöffneten Datei. Dies stellt sicher, dass alle gepufferten Daten in die Datei geschrieben und die Datei ordnungsgemäß geschlossen wird.

    .. code-block:: arduino

        file.close(); // Die Datei schließen

    * **Öffnen des Stammverzeichnisses**

    Öffnen des Stammverzeichnisses der SD-Karte. Wenn das Öffnen des Verzeichnisses fehlschlägt, wird "Failed to open directory" gedruckt und zurückgegeben.

    .. code-block:: arduino

        File root = SD_MMC.open("/"); // Öffnen des Stammverzeichnisses der SD-Karte
        if (!root) {
            Serial.println("Failed to open directory"); // Fehlernachricht drucken, wenn das Öffnen des Verzeichnisses fehlschlägt
            return;
        }

    * **Drucken der Namen und Größen der Dateien**
    
    Die Schleife, beginnend mit ``while (File file = root.openNextFile())``, iteriert über alle Dateien im Stammverzeichnis und druckt den Dateinamen und die Dateigröße jeder gefundenen Datei im seriellen Monitor.

    .. code-block:: arduino
    
        Serial.println("Files found in root directory:"); // Die Liste der im Stammverzeichnis gefundenen Dateien drucken
        while (File file = root.openNextFile()) { // Durch alle Dateien im Stammverzeichnis iterieren
              Serial.print("  ");
              Serial.print(file.name()); // Den Dateinamen drucken
              Serial.print("\t");
              Serial.println(file.size()); // Die Dateigröße drucken
              file.close(); // Die Datei schließen
        }

#.  Diese ``loop()``-Funktion ist eine leere Schleife und führt im aktuellen Programm keine Operationen aus. In einem typischen Arduino-Programm würde diese Funktion kontinuierlich ausgeführt werden und den darin enthaltenen Code ausführen. In diesem Fall, da alle erforderlichen Aufgaben in der ``setup()``-Funktion ausgeführt wurden, wird die ``loop()``-Funktion nicht benötigt.

    .. code-block:: arduino

        void loop() {} // Leere loop-Funktion, tut nichts
