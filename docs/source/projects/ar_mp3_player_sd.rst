 .. note::

    Hallo, willkommen in der SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasten-Gemeinschaft auf Facebook! Vertiefen Sie sich mit anderen Enthusiasten in die Welt von Raspberry Pi, Arduino und ESP32.

    **Warum beitreten?**

    - **Expertenunterstützung**: Lösen Sie Nachverkaufsprobleme und technische Herausforderungen mit Hilfe unserer Gemeinschaft und unseres Teams.
    - **Lernen & Teilen**: Tauschen Sie Tipps und Tutorials aus, um Ihre Fähigkeiten zu verbessern.
    - **Exklusive Vorschauen**: Erhalten Sie frühzeitigen Zugang zu neuen Produktankündigungen und exklusiven Einblicken.
    - **Sonderangebote**: Genießen Sie exklusive Rabatte auf unsere neuesten Produkte.
    - **Festliche Aktionen und Gewinnspiele**: Nehmen Sie an Gewinnspielen und Feiertagsaktionen teil.

    👉 Bereit, mit uns zu erkunden und zu kreieren? Klicken Sie auf [|link_sf_facebook|] und treten Sie heute bei!

.. _ar_mp3_player_sd:

2.11 MP3-Player mit SD-Karten-Unterstützung
==============================================

Willkommen in der aufregenden Welt der Musik mit Ihrem ESP32! Dieses Projekt bringt die Leistung der Audiobearbeitung direkt in Ihre Hände und macht Ihren ESP32 nicht nur zu einem erstaunlichen Mikrocontroller für Rechenaufgaben, sondern auch zu Ihrem persönlichen Musikplayer. Stellen Sie sich vor, Sie betreten Ihr Zimmer und Ihr Lieblingslied spielt direkt von diesem kleinen Gerät. Das ist die Kraft, die wir Ihnen heute zur Verfügung stellen.

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
    *   - Lautsprecher
        - \-

**Betriebsschritte**

#. Stecken Sie Ihre SD-Karte mithilfe eines Kartenlesers in den Computer und formatieren Sie sie. Sie können das Tutorial unter :ref:`format_sd_card` verwenden.

#. Kopieren Sie Ihre Lieblings-MP3-Datei auf Ihre SD-Karte.

    .. image:: img/mp3_music.png

#. Stecken Sie die SD-Karte in den SD-Kartensteckplatz der Erweiterungsplatine.

    .. image:: img/insert_sd.png

#. Bauen Sie die Schaltung auf.

    Da dies ein Mono-Verstärker ist, können Sie IO25 mit dem L- oder R-Pin des Audioverstärkermoduls verbinden.

    Der 10K-Widerstand wird verwendet, um Hochfrequenzrauschen zu reduzieren und die Lautstärke zu verringern. Er bildet zusammen mit der parasitären Kapazität des DAC und des Audioverstärkers einen RC-Tiefpassfilter. Dieser Filter verringert die Amplitude von Hochfrequenzsignalen und reduziert somit effektiv Hochfrequenzrauschen. Das Hinzufügen des 10K-Widerstands macht die Musik weicher und beseitigt unerwünschtes Hochfrequenzrauschen.

    Wenn die Musik auf Ihrer SD-Karte bereits leise ist, können Sie den Widerstand entfernen oder durch einen kleineren Wert ersetzen.

    .. image:: img/7.3_bluetooth_audio_player_bb.png

#. Verbinden Sie den ESP32-WROOM-32E mit dem USB-Kabel mit dem Computer.

    .. image:: img/plugin_esp32.png

#. |link_download_this_code| herunter oder kopieren Sie ihn direkt in die Arduino IDE.

    Ändern Sie die Zeile des Codes ``file = new AudioFileSourceSD_MMC("/To Alice.mp3")``; entsprechend dem Namen und Pfad Ihrer Datei.

    .. note::

        * Die ``ESP8266Audio``-Bibliothek wird hier verwendet. Sehen Sie sich das Tutorial unter :ref:`install_lib_man` an, um sie zu installieren.
        * :download:`ESP8266Audio </_static/zip/ESP8266Audio.zip>`

    .. warning::

        Wenn Sie ein ESP32-Entwicklungsboard Version 3.0.0 oder höher verwenden, können während des Kompilierungsprozesses Fehler auftreten.
        Dieses Problem tritt normalerweise auf, weil neuere Versionen des Boards die ``ESP8266Audio``-Bibliothek nicht mehr unterstützen.
        Um dieses Beispiel ordnungsgemäß auszuführen, wird empfohlen, die Firmware-Version Ihres ESP32-Boards auf 2.0.17 herunterzustufen.
        Nachdem Sie dieses Beispiel abgeschlossen haben, aktualisieren Sie wieder auf die neueste Version.

        .. image:: ../faq/img/version_2.0.17.png

    .. code-block:: arduino

        #include "AudioFileSourceSD_MMC.h"
        #include "AudioOutputI2S.h"
        #include "AudioGeneratorMP3.h"
        #include "SD_MMC.h"
        #include "FS.h"

        // Declare pointers for the MP3 generator, file source, and output.
        AudioGeneratorMP3 *mp3;
        AudioFileSourceSD_MMC *file;
        AudioOutputI2S *out;

        void setup() {
          // Start the serial communication.
          Serial.begin(115200);
          delay(1000);

          // Initialize the SD card. If it fails, print an error message.
          if (!SD_MMC.begin()) {
            Serial.println("SD card mount failed!");
          }

          // Open the MP3 file from the SD card. Replace "/To Alice.mp3" with your own MP3 file name.
          file = new AudioFileSourceSD_MMC("/To Alice.mp3");

          // Set up the I2S output on ESP32's internal DAC.
          out = new AudioOutputI2S(0, 1);

          // Set the output to mono.
          out->SetOutputModeMono(true);

          // Initialize the MP3 generator with the file and output.
          mp3 = new AudioGeneratorMP3();
          mp3->begin(file, out);
        }

        void loop() {
          // If the MP3 is running, loop it. Otherwise, stop it.
          if (mp3->isRunning()) {
            if (!mp3->loop()) mp3->stop();
          }
          // If the MP3 is not running, print a message and wait for 1 second.
          else {
            Serial.println("MP3 done");
            delay(1000);
          }
        }
        
#. Wählen Sie den entsprechenden Port und das Board in der Arduino IDE aus und laden Sie den Code auf Ihren ESP32 hoch.

    * :ref:`unknown_com_port`

#. Nach erfolgreichem Hochladen des Codes hören Sie Ihre Lieblingsmusik spielen.


**Wie funktioniert das?**

* Der Code verwendet mehrere Klassen aus der ``ESP8266Audio``-Bibliothek, um eine MP3-Datei von einer SD-Karte über I2S abzuspielen:

    .. code-block:: arduino

        #include "AudioFileSourceSD_MMC.h"
        #include "AudioOutputI2S.h"
        #include "AudioGeneratorMP3.h"
        #include "SD_MMC.h"
        #include "FS.h"

    * ``AudioGeneratorMP3`` ist eine Klasse, die MP3-Audio dekodiert.
    * ``AudioFileSourceSD_MMC`` ist eine Klasse, die Audiodaten von einer SD-Karte liest.
    * ``AudioOutputI2S`` ist eine Klasse, die Audiodaten an die I2S-Schnittstelle sendet.

* In der ``setup()``-Funktion initialisieren wir die SD-Karte, öffnen die MP3-Datei von der SD-Karte, richten den I2S-Ausgang am internen DAC des ESP32 ein, stellen den Ausgang auf Mono und starten den MP3-Generator.

    .. code-block:: arduino

        void setup() {
            // Starten der seriellen Kommunikation.
            Serial.begin(115200);
            delay(1000);

            // Initialisieren der SD-Karte. Wenn es fehlschlägt, eine Fehlermeldung drucken.
            if (!SD_MMC.begin()) {
                Serial.println("SD-Karten-Mount fehlgeschlagen!");
            }

            // Öffnen der MP3-Datei von der SD-Karte. Ersetzen Sie "/To Alice.mp3" durch Ihren eigenen MP3-Dateinamen.
            file = new AudioFileSourceSD_MMC("/To Alice.mp3");
            
            // Einrichten des I2S-Ausgangs am internen DAC des ESP32.
            out = new AudioOutputI2S(0, 1);
            
            // Den Ausgang auf Mono einstellen.
            out->SetOutputModeMono(true);

            // Initialisieren des MP3-Generators mit der Datei und dem Ausgang.
            mp3 = new AudioGeneratorMP3();
            mp3->begin(file, out);
        }


* In der ``loop()``-Funktion überprüfen wir, ob der MP3-Generator läuft. Wenn er läuft, setzen wir ihn in einer Schleife fort; andernfalls stoppen wir ihn und drucken "MP3 done" auf den seriellen Monitor.

    .. code-block:: arduino

        void loop() {
            // Wenn der MP3-Generator läuft, in einer Schleife fortsetzen. Andernfalls stoppen.
            if (mp3->isRunning()) {
                if (!mp3->loop()) mp3->stop();
            } 
            // Wenn der MP3-Generator nicht läuft, eine Nachricht drucken und 1 Sekunde warten.
            else {
                Serial.println("MP3 done");
                delay(1000);
            }
        }


