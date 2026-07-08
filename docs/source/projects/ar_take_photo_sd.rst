 .. note::

    Hallo und herzlich willkommen in der SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasts Community auf Facebook! Tauchen Sie tiefer in die Welt von Raspberry Pi, Arduino und ESP32 ein – gemeinsam mit anderen Enthusiasten.

    **Warum beitreten?**

    - **Expertenunterstützung**: Lösen Sie Probleme nach dem Kauf und technische Herausforderungen mit Hilfe unserer Community und unseres Teams.
    - **Lernen & Teilen**: Tauschen Sie Tipps und Tutorials aus, um Ihre Fähigkeiten zu verbessern.
    - **Exklusive Vorschauen**: Erhalten Sie frühzeitigen Zugang zu neuen Produktankündigungen und Vorschauen.
    - **Exklusive Rabatte**: Genießen Sie exklusive Rabatte auf unsere neuesten Produkte.
    - **Festliche Aktionen und Verlosungen**: Nehmen Sie an Verlosungen und festlichen Aktionen teil.

    👉 Bereit, mit uns zu erkunden und zu kreieren? Klicken Sie [|link_sf_facebook|] und treten Sie noch heute bei!

.. _ar_take_photo_sd:

2.12 Fotoaufnahme und SD-Karte
===================================

Dieses Dokument beschreibt ein Projekt, bei dem ein Foto mit der ESP32-CAM aufgenommen und auf einer SD-Karte gespeichert wird. 
Das Ziel des Projekts ist es, eine einfache Lösung für die Bildaufnahme mit der ESP32-CAM und die Speicherung auf einer SD-Karte zu bieten.

**Benötigte Komponenten**

In diesem Projekt benötigen wir die folgenden Komponenten. 

.. list-table::
    :widths: 30 20
    :header-rows: 1

    *   - KOMPONENTEN-EINFÜHRUNG
        - KAUFLINK

    *   - :ref:`cpn_esp32_wroom_32e`
        - |link_esp32_wroom_32e_buy|
    *   - :ref:`cpn_esp32_camera_extension`
        - \-


**Wichtige Hinweise**

Beim Einsatz der ESP32-CAM ist zu beachten, dass der GPIO 0 Pin mit GND verbunden sein muss, um einen Sketch hochzuladen. 
Zusätzlich muss nach dem Verbinden von GPIO 0 mit GND der Reset-Knopf der ESP32-CAM gedrückt werden, um das Board in den Flash-Modus zu versetzen. 
Es ist auch wichtig sicherzustellen, dass die SD-Karte richtig montiert ist, bevor Bilder darauf gespeichert werden.

**Betriebsschritte**

#. Setzen Sie Ihre SD-Karte in den Computer ein, indem Sie einen Kartenleser verwenden, und formatieren Sie sie. Eine Anleitung finden Sie unter :ref:`format_sd_card`.

#. Entfernen Sie dann den Kartenleser und setzen Sie die SD-Karte in die Erweiterungsplatine ein.

    .. image:: img/insert_sd.png

#. Jetzt die Kamera anschließen.

    .. raw:: html

        <video loop autoplay muted style = "max-width:100%">
            <source src="../_static/video/plugin_camera.mp4" type="video/mp4">
            Your browser does not support the video tag.
        </video>

#. Verbinden Sie die ESP32-WROOM-32E mit dem Computer über das USB-Kabel.

    .. image:: img/plugin_esp32.png

#. |link_download_this_code| herunter oder kopieren Sie ihn direkt in die Arduino IDE.

   .. code-block:: arduino

        /*********
          Rui Santos
          Complete project details at https://RandomNerdTutorials.com/esp32-cam-take-photo-save-microsd-card

          IMPORTANT!!!
           - Select Board "AI Thinker ESP32-CAM"
           - GPIO 0 must be connected to GND to upload a sketch
           - After connecting GPIO 0 to GND, press the ESP32-CAM on-board RESET button to put your board in flashing mode

          Permission is hereby granted, free of charge, to any person obtaining a copy
          of this software and associated documentation files.
          The above copyright notice and this permission notice shall be included in all
          copies or substantial portions of the Software.
        *********/

        #include "esp_camera.h"
        #include "Arduino.h"
        #include "FS.h"                // SD Card ESP32
        #include "SD_MMC.h"            // SD Card ESP32
        #include "soc/soc.h"           // Disable brownour problems
        #include "soc/rtc_cntl_reg.h"  // Disable brownour problems
        #include "driver/rtc_io.h"
        #include <EEPROM.h>  // read and write from flash memory

        // define the number of bytes you want to access
        #define EEPROM_SIZE 1

        // Pin definition for CAMERA_MODEL_AI_THINKER
        #define PWDN_GPIO_NUM 32
        #define RESET_GPIO_NUM -1
        #define XCLK_GPIO_NUM 0
        #define SIOD_GPIO_NUM 26
        #define SIOC_GPIO_NUM 27

        #define Y9_GPIO_NUM 35
        #define Y8_GPIO_NUM 34
        #define Y7_GPIO_NUM 39
        #define Y6_GPIO_NUM 36
        #define Y5_GPIO_NUM 21
        #define Y4_GPIO_NUM 19
        #define Y3_GPIO_NUM 18
        #define Y2_GPIO_NUM 5
        #define VSYNC_GPIO_NUM 25
        #define HREF_GPIO_NUM 23
        #define PCLK_GPIO_NUM 22

        int pictureNumber = 0;

        void setup() {
          WRITE_PERI_REG(RTC_CNTL_BROWN_OUT_REG, 0);  //disable brownout detector

          Serial.begin(115200);

          camera_config_t config;
          config.ledc_channel = LEDC_CHANNEL_0;
          config.ledc_timer = LEDC_TIMER_0;
          config.pin_d0 = Y2_GPIO_NUM;
          config.pin_d1 = Y3_GPIO_NUM;
          config.pin_d2 = Y4_GPIO_NUM;
          config.pin_d3 = Y5_GPIO_NUM;
          config.pin_d4 = Y6_GPIO_NUM;
          config.pin_d5 = Y7_GPIO_NUM;
          config.pin_d6 = Y8_GPIO_NUM;
          config.pin_d7 = Y9_GPIO_NUM;
          config.pin_xclk = XCLK_GPIO_NUM;
          config.pin_pclk = PCLK_GPIO_NUM;
          config.pin_vsync = VSYNC_GPIO_NUM;
          config.pin_href = HREF_GPIO_NUM;
          config.pin_sscb_sda = SIOD_GPIO_NUM;
          config.pin_sscb_scl = SIOC_GPIO_NUM;
          config.pin_pwdn = PWDN_GPIO_NUM;
          config.pin_reset = RESET_GPIO_NUM;
          config.xclk_freq_hz = 20000000;
          config.pixel_format = PIXFORMAT_JPEG;

          if (psramFound()) {
            config.frame_size = FRAMESIZE_UXGA;  // FRAMESIZE_ + QVGA|CIF|VGA|SVGA|XGA|SXGA|UXGA
            config.jpeg_quality = 10;
            config.fb_count = 2;
          } else {
            config.frame_size = FRAMESIZE_SVGA;
            config.jpeg_quality = 12;
            config.fb_count = 1;
          }

          // Init Camera
          esp_err_t err = esp_camera_init(&config);
          if (err != ESP_OK) {
            Serial.printf("Camera init failed with error 0x%x", err);
            return;
          }

          if (!SD_MMC.begin()) {
            Serial.println("SD Card Mount Failed");
            return;
          }

          uint8_t cardType = SD_MMC.cardType();
          if (cardType == CARD_NONE) {
            Serial.println("No SD Card attached");
            return;
          }

          // initialize EEPROM with predefined size
          EEPROM.begin(EEPROM_SIZE);
          pictureNumber = EEPROM.read(0) + 1;

          for (int shoot = 0; shoot < 5; shoot++) {
            camera_fb_t *fb = NULL;

            // Take Picture with Camera
            fb = esp_camera_fb_get();
            if (!fb) {
              Serial.println("Camera capture failed");
              return;
            }

            // Path where new picture will be saved in SD Card
            String path = "/picture" + String(pictureNumber) + ".jpg";

            fs::FS &fs = SD_MMC;

            File file = fs.open(path.c_str(), FILE_WRITE);
            if (!file) {
              Serial.println("Failed to open file in writing mode");
            } else {
              file.write(fb->buf, fb->len);  // Write image data to file
              if (shoot == 4) {
                Serial.printf("Saved file to path: %s\n", path.c_str());
              }else{
                Serial.printf("Shooting... \n");
              }
              EEPROM.write(0, pictureNumber);  // Update the picture number in EEPROM
              EEPROM.commit();
            }
            file.close();                      // Close the file
            esp_camera_fb_return(fb);          // Return the frame buffer back to the camera driver
            delay(200);                        // Short delay between shots
          }
          // Turns off the ESP32-CAM white on-board LED (flash) connected to GPIO 4
          pinMode(4, OUTPUT);
          digitalWrite(4, LOW);
          rtc_gpio_hold_en(GPIO_NUM_4);

          // Put the ESP32-CAM to deep sleep
          delay(2000);
          Serial.println("Going to sleep now");
          delay(2000);
          esp_deep_sleep_start();
          Serial.println("This will never be printed");
        }

        void loop() {
          // Empty loop as we are putting the ESP32-CAM to deep sleep
        }


#. Aktivieren Sie jetzt **PSRAM**.

    .. image:: img/sp230516_150554.png

#. Stellen Sie das Partitionsschema auf **Huge APP (3MB No OTA/1MB SPIFFS)** ein.

    .. image:: img/sp230516_150840.png   

#. Wählen Sie den entsprechenden Port und das Board in der Arduino IDE aus und laden Sie den Code auf Ihre ESP32 hoch.

    * :ref:`unknown_com_port`

#. Nach dem erfolgreichen Hochladen des Codes drücken Sie den **Reset**-Knopf, um ein Foto aufzunehmen. Zusätzlich können Sie im seriellen Monitor die folgende Information sehen, die die erfolgreiche Aufnahme anzeigt.

    .. code-block:: arduino

        Picture file name: /picture9.jpg
        Saved file to path: /picture9.jpg
        Going to sleep now

    .. image:: img/press_reset.png

#. Entfernen Sie nun die SD-Karte von der Erweiterungsplatine und setzen Sie sie in Ihren Computer ein. Sie können nun die gerade aufgenommenen Fotos ansehen.

    .. image:: img/take_photo1.png

**Wie es funktioniert**

Dieser Code betreibt eine AI Thinker ESP32-CAM, um ein Foto aufzunehmen, es auf einer SD-Karte zu speichern und dann die ESP32-CAM in den Tiefschlaf zu versetzen. Hier ist eine Aufschlüsselung der wichtigsten Teile:

* **Bibliotheken**: Der Code beginnt mit der Einbindung der notwendigen Bibliotheken für die ESP32-CAM, das Dateisystem (FS), die SD-Karte und das EEPROM (zum Speichern von Daten über Stromzyklen hinweg).

    .. code-block:: arduino

        #include "esp_camera.h"
        #include "Arduino.h"
        #include "FS.h"                // SD-Karte ESP32
        #include "SD_MMC.h"            // SD-Karte ESP32
        #include "soc/soc.h"           // Deaktivierung von Brownout-Problemen
        #include "soc/rtc_cntl_reg.h"  // Deaktivierung von Brownout-Problemen
        #include "driver/rtc_io.h"
        #include <EEPROM.h>  // Lesen und Schreiben aus dem Flash-Speicher

* **Pin-Definitionen**: Dieser Abschnitt legt Konstanten fest, die die Pin-Verbindungen der ESP32-CAM zum Kameramodul darstellen.

    .. code-block:: arduino

        #define PWDN_GPIO_NUM 32
        #define RESET_GPIO_NUM -1
        #define XCLK_GPIO_NUM 0
        #define SIOD_GPIO_NUM 26
        #define SIOC_GPIO_NUM 27

        #define Y9_GPIO_NUM 35
        #define Y8_GPIO_NUM 34
        #define Y7_GPIO_NUM 39
        #define Y6_GPIO_NUM 36
        #define Y5_GPIO_NUM 21
        #define Y4_GPIO_NUM 19
        #define Y3_GPIO_NUM 18
        #define Y2_GPIO_NUM 5
        #define VSYNC_GPIO_NUM 25
        #define HREF_GPIO_NUM 23
        #define PCLK_GPIO_NUM 22


* **Globale Variablen**: Eine globale Variable ``pictureNumber`` wird deklariert, um die Anzahl der aufgenommenen und auf der SD-Karte gespeicherten Bilder zu verfolgen.

    .. code-block:: arduino

        int pictureNumber = 0;


* **Setup-Funktion**: In der ``setup()``-Funktion werden mehrere Aufgaben erledigt:

    * Zunächst wird der Brownout-Detektor deaktiviert, um zu verhindern, dass die ESP32-CAM während hoher Stromlasten (wie beim Betrieb der Kamera) zurückgesetzt wird.
    
        .. code-block:: arduino

            WRITE_PERI_REG(RTC_CNTL_BROWN_OUT_REG, 0);  // Brownout-Detektor deaktivieren

    * Die serielle Kommunikation wird zur Fehlerbehebung initialisiert.

        .. code-block:: arduino

            Serial.begin(115200);

    * Die Kamerakonfiguration wird mit ``camera_config_t`` eingerichtet, einschließlich der GPIO-Pins, XCLK-Frequenz, Pixelformat, Bildgröße, JPEG-Qualität und Framebuffer-Anzahl.
    
        .. code-block:: arduino

            camera_config_t config;
            config.ledc_channel = LEDC_CHANNEL_0;
            config.ledc_timer = LEDC_TIMER_0;
            config.pin_d0 = Y2_GPIO_NUM;
            config.pin_d1 = Y3_GPIO_NUM;
            config.pin_d2 = Y4_GPIO_NUM;
            config.pin_d3 = Y5_GPIO_NUM;
            config.pin_d4 = Y6_GPIO_NUM;
            config.pin_d5 = Y7_GPIO_NUM;
            config.pin_d6 = Y8_GPIO_NUM;
            config.pin_d7 = Y9_GPIO_NUM;
            config.pin_xclk = XCLK_GPIO_NUM;
            config.pin_pclk = PCLK_GPIO_NUM;
            config.pin_vsync = VSYNC_GPIO_NUM;
            config.pin_href = HREF_GPIO_NUM;
            config.pin_sscb_sda = SIOD_GPIO_NUM;
            config.pin_sscb_scl = SIOC_GPIO_NUM;
            config.pin_pwdn = PWDN_GPIO_NUM;
            config.pin_reset = RESET_GPIO_NUM;
            config.xclk_freq_hz = 20000000;
            config.pixel_format = PIXFORMAT_JPEG;
    
    * Die Kamera wird dann mit der Konfiguration initialisiert, und falls dies fehlschlägt, wird eine Fehlermeldung ausgegeben.

        .. code-block:: arduino

            esp_err_t err = esp_camera_init(&config);
            if (err != ESP_OK) {
                Serial.printf("Camera init failed with error 0x%x", err);
                return;
            }

    * Die SD-Karte wird initialisiert, und falls dies fehlschlägt, wird eine Fehlermeldung ausgegeben.

        .. code-block:: arduino
            
            if (!SD_MMC.begin()) {
                Serial.println("SD Card Mount Failed");
                return;
            }   

            uint8_t cardType = SD_MMC.cardType();
            if (cardType == CARD_NONE) {
                Serial.println("No SD Card attached");
                return;
            }        

    * Ein Foto wird mit der Kamera aufgenommen und im Framebuffer gespeichert.

        .. code-block:: arduino

            fb = esp_camera_fb_get();
            if (!fb) {
                Serial.println("Camera capture failed");
                return;
            }

    * Das EEPROM wird gelesen, um die Nummer des letzten Bildes abzurufen, dann wird die Bildnummer für das neue Foto inkrementiert.

        .. code-block:: arduino

            EEPROM.begin(EEPROM_SIZE);
            pictureNumber = EEPROM.read(0) + 1;

    * Ein Pfad für das neue Bild wird auf der SD-Karte erstellt, mit einem Dateinamen, der der Bildnummer entspricht.

        .. code-block:: arduino

            String path = "/picture" + String(pictureNumber) + ".jpg";

            fs::FS &fs = SD_MMC;
            Serial.printf("Picture file name: %s\n", path.c_str());

    * Nach dem Speichern des Fotos wird die Bildnummer wieder ins EEPROM geschrieben, um sie im nächsten Stromzyklus abzurufen.

        .. code-block:: arduino

            File file = fs.open(path.c_str(), FILE_WRITE);
            if (!file) {
                Serial.println("Failed to open file in writing mode");
            } else {
                file.write(fb->buf, fb->len);  // payload (image), payload length
                Serial.printf("Saved file to path: %s\n", path.c_str());
                EEPROM.write(0, pictureNumber);
                EEPROM.commit();
            }
            file.close();
            esp_camera_fb_return(fb); 

    * Schließlich wird die Onboard-LED (Blitz) ausgeschaltet und die ESP32-CAM geht in den Tiefschlafmodus.

        .. code-block:: arduino

            pinMode(4, OUTPUT);
            digitalWrite(4, LOW);
            rtc_gpio_hold_en(GPIO_NUM_4);

    * Schlafmodus: Die ESP32-CAM geht nach jeder Fotoaufnahme in den Tiefschlafmodus, um Energie zu sparen. Sie kann durch einen Reset oder ein Signal an bestimmten Pins geweckt werden.

        .. code-block:: arduino

            delay(2000);
            Serial.println("Going to sleep now");
            delay(2000);
            esp_deep_sleep_start();
            Serial.println("This will never be printed");


* Loop-Funktion: Die ``loop()``-Funktion ist leer, da nach dem Setup-Prozess die ESP32-CAM sofort in den Tiefschlafmodus geht.

Beachten Sie, dass für diesen Code GPIO 0 mit GND verbunden sein muss, wenn der Sketch hochgeladen wird, und Sie möglicherweise die Reset-Taste auf dem Board drücken müssen, um Ihr Board in den Flash-Modus zu versetzen. Denken Sie auch daran, "/picture" durch Ihren eigenen Dateinamen zu ersetzen. Die Größe des EEPROM ist auf 1 gesetzt, was bedeutet, dass es Werte von 0 bis 255 speichern kann. Wenn Sie mehr als 255 Bilder aufnehmen möchten, müssen Sie die Größe des EEPROM erhöhen und anpassen, wie Sie pictureNumber speichern und lesen.

