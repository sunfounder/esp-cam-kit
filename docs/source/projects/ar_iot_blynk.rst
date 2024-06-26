.. note::

    Hallo, willkommen in der SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasten-Gemeinschaft auf Facebook! Vertiefen Sie sich mit anderen Enthusiasten in die Welt von Raspberry Pi, Arduino und ESP32.

    **Warum beitreten?**

    - **Expertenunterstützung**: Lösen Sie Nachverkaufsprobleme und technische Herausforderungen mit Hilfe unserer Gemeinschaft und unseres Teams.
    - **Lernen & Teilen**: Tauschen Sie Tipps und Tutorials aus, um Ihre Fähigkeiten zu verbessern.
    - **Exklusive Vorschauen**: Erhalten Sie frühzeitigen Zugang zu neuen Produktankündigungen und exklusiven Einblicken.
    - **Sonderangebote**: Genießen Sie exklusive Rabatte auf unsere neuesten Produkte.
    - **Festliche Aktionen und Gewinnspiele**: Nehmen Sie an Gewinnspielen und Feiertagsaktionen teil.

    👉 Bereit, mit uns zu erkunden und zu kreieren? Klicken Sie auf [|link_sf_facebook|] und treten Sie heute bei!

.. _iot_intrusion_alert_system:

2.15 Blynk-basiertes Einbruchmeldesystem
==================================================

Dieses Projekt demonstriert ein einfaches Einbruchmeldesystem für Zuhause mithilfe eines PIR-Bewegungssensors (HC-SR501).
Wenn das System über die Blynk-App in den "Abwesenheitsmodus" versetzt wird, überwacht der PIR-Sensor Bewegungen.
Jede erkannte Bewegung löst eine Benachrichtigung in der Blynk-App aus und warnt den Benutzer vor einem möglichen Einbruch.

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
    *   - Mehrere Verbindungskabel
        - |link_wires_buy|
    *   - PIR-Bewegungssensor-Modul
        - |link_pir_buy|

1. Schaltungsaufbau
----------------------

.. image:: img/iot_9_blynk_bb.png
    :width: 60%
    :align: center

2. Blynk-Konfiguration
-----------------------

**2.1 Initialisierung von Blynk**

#. Navigieren Sie zu |link_blynk| und wählen Sie **START FREE**.

   .. image:: img/09_blynk_access.png
        :width: 90%

#. Geben Sie Ihre E-Mail-Adresse ein, um den Registrierungsprozess zu starten.

   .. image:: img/09_blynk_sign_in.png
        :width: 70%
        :align: center

#. Bestätigen Sie Ihre Registrierung über Ihre E-Mail.

    .. image:: img/09_blynk_password.png
        :width: 90%

#. Nach der Bestätigung erscheint die **Blynk Tour**. Es wird empfohlen, "Skip" auszuwählen. Wenn auch **Quick Start** erscheint, können Sie auch diesen überspringen.
   
    .. image:: img/09_blynk_tour.png
        :width: 90%

**2.2 Erstellung einer Vorlage**

#. Erstellen Sie zunächst eine Vorlage in Blynk. Folgen Sie den nachstehenden Anweisungen, um die Vorlage **Intrusion Alert System** zu erstellen.

    .. image:: img/09_create_template_1_shadow.png
        :width: 700
        :align: center

#. Weisen Sie der Vorlage einen Namen zu, wählen Sie die Hardware **ESP32** und den Verbindungstyp **WiFi**, und klicken Sie dann auf **Done**.

    .. image:: img/09_create_template_2_shadow.png
        :width: 700
        :align: center

**2.3 Datenstromerstellung**

Öffnen Sie die gerade erstellte Vorlage und erstellen Sie zwei Datenströme.

#. Klicken Sie auf **New Datastream**.

    .. image:: img/09_blynk_new_datastream.png
        :width: 700
        :align: center

#. Wählen Sie im Popup **Virtual Pin**.

    .. image:: img/09_blynk_datastream_virtual.png
        :width: 700
        :align: center

#. Benennen Sie den **Virtual Pin V0** als **AwayMode**. Setzen Sie den **DATENTYP** auf **Integer** mit **MIN**- und **MAX**-Werten von **0** und **1**.

    .. image:: img/09_create_template_shadow.png
        :width: 700
        :align: center

#. Erstellen Sie auf ähnliche Weise einen weiteren **Virtual Pin** Datenstrom. Benennen Sie ihn **Current Status** und setzen Sie den **DATENTYP** auf **String**.

    .. image:: img/09_datastream_1_shadow.png
        :width: 700
        :align: center

**2.4 Einrichtung eines Ereignisses**

Als Nächstes richten wir ein Ereignis ein, das eine E-Mail-Benachrichtigung sendet, wenn ein Einbruch festgestellt wird.

#. Klicken Sie auf **Add New Event**.

    .. image:: img/09_blynk_event_add.png

#. Definieren Sie den Namen des Ereignisses und dessen spezifischen Code. Wählen Sie für **TYPE** die Option **Warning** und schreiben Sie eine kurze Beschreibung für die E-Mail, die gesendet wird, wenn das Ereignis eintritt. Sie können auch einstellen, wie oft Sie benachrichtigt werden möchten.

    .. note::
        
        Stellen Sie sicher, dass der **EVENT CODE** auf ``intrusion_detected`` gesetzt ist. Dies ist im Code vordefiniert, daher müssen Sie bei Änderungen auch den Code anpassen.

    .. image:: img/09_event_1_shadow.png
        :width: 700
        :align: center

#. Gehen Sie zum Abschnitt **Notifications**, um Benachrichtigungen zu aktivieren und E-Mail-Details einzurichten.

    .. image:: img/09_event_2_shadow.png
        :width: 80%
        :align: center

.. raw:: html
    
    <br/> 

**2.5 Feinabstimmung des Web-Dashboards**

Stellen Sie sicher, dass das **Web Dashboard** perfekt mit dem Einbruchmeldesystem interagiert.

#. Ziehen Sie einfach das **Switch Widget** und das **Label Widget** auf das **Web Dashboard**.

    .. image:: img/09_web_dashboard_1_shadow.png
        :width: 100%
        :align: center

#. Wenn Sie über ein Widget fahren, erscheinen drei Symbole. Verwenden Sie das Einstellungssymbol, um die Eigenschaften des Widgets anzupassen.

    .. image:: img/09_blynk_dashboard_set.png
        :width: 100%
        :align: center

#. Wählen Sie in den Einstellungen des **Switch Widget** den **Datastream** als **AwayMode(V0)**. Setzen Sie **ONLABEL** und **OFFLABEL** auf **"away"** bzw. **"home"**.

    .. image:: img/09_web_dashboard_2_shadow.png
        :width: 100%
        :align: center

#. Wählen Sie in den Einstellungen des **Label Widget** den **Datastream** als **Current Status(V1)**.

    .. image:: img/09_web_dashboard_3_shadow.png
        :width: 100%
        :align: center

**2.6 Speichern der Vorlage**

Vergessen Sie zuletzt nicht, Ihre Vorlage zu speichern.

    .. image:: img/09_save_template_shadow.png
        :width: 100%
        :align: center
        
**2.7 Ein Gerät erstellen**

#. Es ist Zeit, ein neues Gerät zu erstellen.

    .. image:: img/09_blynk_device_new.png
        :width: 700
        :align: center

#. Klicken Sie auf **From template**, um mit einer neuen Einrichtung zu beginnen.

    .. image:: img/09_blynk_device_template.png
        :width: 700
        :align: center

#. Wählen Sie die Vorlage **Intrusion Alert System** und klicken Sie auf **Create**.

    .. image:: img/09_blynk_device_template2.png
        :width: 700
        :align: center

#. Hier sehen Sie die ``Template ID``, den ``Device Name`` und das ``AuthToken``. Diese müssen Sie in Ihren Code kopieren, damit der ESP32 mit Blynk arbeiten kann.

    .. image:: img/09_blynk_device_code.png
        :width: 700
        :align: center

3. Code-Ausführung
-----------------------------

#. Stellen Sie vor dem Ausführen des Codes sicher, dass Sie die ``Blynk``-Bibliothek aus dem **Library Manager** der Arduino IDE installiert haben.

    .. image:: img/09_blynk_add_library.png
        :width: 700
        :align: center

#. Laden Sie diesen Code herunter oder kopieren Sie ihn direkt in die Arduino IDE.

    .. raw:: html

        <iframe src=https://create.arduino.cc/editor/sunfounder01/16bca228-64d7-4519-ac3b-833afecfcc65/preview?embed style="height:510px;width:100%;margin:10px 0" frameborder=0></iframe>

#. Ersetzen Sie die Platzhalter für ``BLYNK_TEMPLATE_ID``, ``BLYNK_TEMPLATE_NAME`` und ``BLYNK_AUTH_TOKEN`` durch Ihre eigenen eindeutigen IDs.

    .. code-block:: arduino
    
        #define BLYNK_TEMPLATE_ID "TMPxxxxxxx"
        #define BLYNK_TEMPLATE_NAME "Intrusion Alert System"
        #define BLYNK_AUTH_TOKEN "xxxxxxxxxxxxx"

#. Geben Sie auch den ``ssid`` und das ``password`` Ihres WLAN-Netzwerks ein.

    .. code-block:: arduino

        char ssid[] = "your_ssid";
        char pass[] = "your_password";

#. Wählen Sie das richtige Board (**ESP32 Dev Module**) und den richtigen Port aus und klicken Sie dann auf die Schaltfläche **Upload**.

#. Öffnen Sie den Seriellen Monitor (stellen Sie die Baudrate auf 115200 ein) und warten Sie auf eine erfolgreiche Verbindungsnachricht.

    .. image:: img/09_blynk_upload_code.png
        :align: center

#. Nach einer erfolgreichen Verbindung wird durch Aktivieren des Schalters in Blynk das Überwachungsmodul des PIR-Moduls gestartet. Wenn eine Bewegung erkannt wird (Zustand 1), erscheint die Nachricht "Somebody here!" und es wird eine Warnung an Ihre E-Mail gesendet.

    .. image:: img/09_blynk_code_alarm.png
        :width: 700
        :align: center

4. Code-Erklärung
-----------------------------

#. **Konfiguration & Bibliotheken**

   Hier setzen Sie die Blynk-Konstanten und -Anmeldeinformationen. Außerdem fügen Sie die notwendigen Bibliotheken für den ESP32 und Blynk ein.

    .. code-block:: arduino

        /* Kommentieren Sie dies aus, um Ausgaben zu deaktivieren und Speicherplatz zu sparen */
        #define BLYNK_PRINT Serial

        #define BLYNK_TEMPLATE_ID "xxxxxxxxxxx"
        #define BLYNK_TEMPLATE_NAME "Intrusion Alert System"
        #define BLYNK_AUTH_TOKEN "xxxxxxxxxxxxxxxxxxxxxxxxxxx"

        #include <WiFi.h>
        #include <WiFiClient.h>
        #include <BlynkSimpleEsp32.h>
#. **WiFi-Einrichtung**

   Geben Sie Ihre WiFi-Zugangsdaten ein.

   .. code-block:: arduino

        char ssid[] = "your_ssid";
        char pass[] = "your_password";

#. **PIR-Sensor-Konfiguration**

   Legen Sie den Pin fest, an dem der PIR-Sensor angeschlossen ist, und initialisieren Sie die Zustandsvariablen.

   .. code-block:: arduino

      const int sensorPin = 14;
      int state = 0;
      int awayHomeMode = 0;
      BlynkTimer timer;

#. **setup() Funktion**

   Diese Funktion initialisiert den PIR-Sensor als Eingang, richtet die serielle Kommunikation ein, verbindet sich mit dem WiFi und konfiguriert Blynk.

   - Wir verwenden ``timer.setInterval(1000L, myTimerEvent)``, um das Zeitintervall im ``setup()`` festzulegen. Hier wird die Funktion ``myTimerEvent()`` alle **1000ms** ausgeführt. Sie können den ersten Parameter von ``timer.setInterval(1000L, myTimerEvent)`` ändern, um das Intervall zwischen den Ausführungen von ``myTimerEvent`` anzupassen.

   .. raw:: html
    
    <br/> 

   .. code-block:: arduino

        void setup() {

            pinMode(sensorPin, INPUT);  // Setzen Sie den PIR-Sensor-Pin als Eingang
            Serial.begin(115200);       // Starten Sie die serielle Kommunikation mit 115200 Baud für das Debugging
            
            // Konfigurieren Sie Blynk und verbinden Sie sich mit dem WiFi
            Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass);
            
            timer.setInterval(1000L, myTimerEvent);  // Richten Sie eine Funktion ein, die jede Sekunde aufgerufen wird
        }

#. **loop() Funktion**

   Die loop-Funktion führt kontinuierlich die Blynk- und die Blynk-Timer-Funktionen aus.

   .. code-block:: arduino

        void loop() {
           Blynk.run();
           timer.run();
        }

#. **Interaktion mit der Blynk-App**

   Diese Funktionen werden aufgerufen, wenn das Gerät eine Verbindung zu Blynk herstellt und wenn sich der Zustand des virtuellen Pins V0 in der Blynk-App ändert.

   - Jedes Mal, wenn das Gerät eine Verbindung zum Blynk-Server herstellt oder aufgrund schlechter Netzwerkbedingungen erneut verbindet, wird die Funktion ``BLYNK_CONNECTED()`` aufgerufen. Der Befehl ``Blynk.syncVirtual()`` fordert den Wert eines virtuellen Pins an. Der angegebene virtuelle Pin führt den Aufruf ``BLYNK_WRITE()`` aus. 

   - Wann immer sich der Wert eines virtuellen Pins auf dem Blynk-Server ändert, wird ``BLYNK_WRITE()`` ausgelöst.

   .. raw:: html
    
    <br/> 

   .. code-block:: arduino
      
        // Diese Funktion wird jedes Mal aufgerufen, wenn das Gerät eine Verbindung zu Blynk.Cloud herstellt
        BLYNK_CONNECTED() {
            Blynk.syncVirtual(V0);
        }
      
        // Diese Funktion wird jedes Mal aufgerufen, wenn sich der Zustand des virtuellen Pins 0 ändert
        BLYNK_WRITE(V0) {
            awayHomeMode = param.asInt();
            // zusätzliche Logik
        }

#. **Datenverarbeitung**

   Jede Sekunde ruft die Funktion ``myTimerEvent()`` die Funktion ``sendData()`` auf. Wenn der Abwesenheitsmodus in Blynk aktiviert ist, überprüft er den PIR-Sensor und sendet eine Benachrichtigung an Blynk, wenn eine Bewegung erkannt wird.

   - Wir verwenden ``Blynk.virtualWrite(V1, "Somebody in your house! Please check!");``, um den Text eines Labels zu ändern.

   - Verwenden Sie ``Blynk.logEvent("intrusion_detected");``, um ein Ereignis in Blynk zu protokollieren.

   .. raw:: html
    
    <br/> 

   .. code-block:: arduino

        void myTimerEvent() {
           sendData();
        }

        void sendData() {
           if (awayHomeMode == 1) {
              state = digitalRead(sensorPin);  // Lesen Sie den Zustand des PIR-Sensors

              Serial.print("state:");
              Serial.println(state);

              // Wenn der Sensor eine Bewegung erkennt, senden Sie eine Warnung an die Blynk-App
              if (state == HIGH) {
                Serial.println("Somebody here!");
                Blynk.virtualWrite(V1, "Somebody in your house! Please check!");
                Blynk.logEvent("intrusion_detected");
              }
           }
        }

**Referenzen**

- |link_blynk_doc|
- |link_blynk_quickstart| 
- |link_blynk_virtualWrite|
- |link_blynk_logEvent|
- |link_blynk_timer_intro|
- |link_blynk_syncing| 
- |link_blynk_write|
