 .. note::

    Hallo, willkommen in der SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasten-Gemeinschaft auf Facebook! Vertiefen Sie sich mit anderen Enthusiasten in die Welt von Raspberry Pi, Arduino und ESP32.

    **Warum beitreten?**

    - **Expertenunterstützung**: Lösen Sie Nachverkaufsprobleme und technische Herausforderungen mit Hilfe unserer Gemeinschaft und unseres Teams.
    - **Lernen & Teilen**: Tauschen Sie Tipps und Tutorials aus, um Ihre Fähigkeiten zu verbessern.
    - **Exklusive Vorschauen**: Erhalten Sie frühzeitigen Zugang zu neuen Produktankündigungen und exklusiven Einblicken.
    - **Sonderangebote**: Genießen Sie exklusive Rabatte auf unsere neuesten Produkte.
    - **Festliche Aktionen und Gewinnspiele**: Nehmen Sie an Gewinnspielen und Feiertagsaktionen teil.

    👉 Bereit, mit uns zu erkunden und zu kreieren? Klicken Sie auf [|link_sf_facebook|] und treten Sie heute bei!

.. _ar_fading:

2.2 Analoge Ausgabe über PWM
=================================

Im vorherigen Projekt haben wir die LED durch Ein- und Ausschalten mithilfe eines digitalen Ausgangs gesteuert. In diesem Projekt werden wir einen Atemeffekt auf der LED erzeugen, indem wir die Pulsweitenmodulation (PWM) verwenden. PWM ist eine Technik, die es uns ermöglicht, die Helligkeit einer LED oder die Geschwindigkeit eines Motors zu steuern, indem wir das Tastverhältnis eines Rechtecksignals variieren.

Mit PWM werden wir anstelle des einfachen Ein- und Ausschaltens der LED die Zeit, in der die LED an ist, gegenüber der Zeit, in der sie aus ist, innerhalb jedes Zyklus anpassen. Durch schnelles Ein- und Ausschalten der LED in variierenden Intervallen können wir die Illusion erzeugen, dass die LED allmählich heller und dunkler wird, was einen Atemeffekt simuliert.

Durch die Nutzung der PWM-Funktionen des ESP32 WROOM 32E können wir eine reibungslose und präzise Steuerung der LED-Helligkeit erreichen. Dieser Atemeffekt fügt Ihren Projekten ein dynamisches und visuell ansprechendes Element hinzu und schafft ein auffälliges Display oder Ambiente.

**Verfügbare Pins**

Hier ist eine Liste der verfügbaren Pins auf dem ESP32-Board für dieses Projekt.

.. list-table::
    :widths: 5 20 

    * - Verfügbare Pins
      - IO13, IO12, IO14, IO27, IO26, IO25, IO33, IO32, IO15, IO2, IO0, IO4, IO5, IO18, IO19, IO21, IO22, IO23



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
    *   - Breadboard
        - |link_breadboard_buy|
    *   - Mehrere Verbindungskabel
        - |link_wires_buy|
    *   - Widerstand
        - |link_resistor_buy|
    *   - LED
        - |link_led_buy|

**Schaltplan**

.. image:: img/circuit_2.1_led.png

Dieses Projekt verwendet denselben Schaltkreis wie das erste Projekt :ref:`ar_blink`, aber der Signaltyp ist unterschiedlich. Im ersten Projekt wird ein digitales High- und Low-Signal (0&1) direkt von Pin 26 ausgegeben, um die LED ein- oder auszuschalten. In diesem Projekt wird ein PWM-Signal von Pin 26 ausgegeben, um die Helligkeit der LED zu steuern.

**Verdrahtung**

.. image:: img/2.1_hello_led_bb.png

**Code**

#. |link_download_this_code| herunter oder kopieren Sie ihn direkt in die Arduino IDE.

   .. code-block:: arduino

        const int ledPin = 26;  // The GPIO pin for the LED
        int brightness = 0;
        int fadeAmount = 5;

        void setup() {
          ledcAttach(ledPin, 5000, 8);  // Attach the LED pin
        }

        void loop() {
          ledcWrite(ledPin, brightness);  // Write the new brightness value to the PWM pin
          brightness = brightness + fadeAmount;
          if (brightness <= 0 || brightness >= 255) {
            fadeAmount = -fadeAmount;
          }
          delay(50);  // Wait for 50 milliseconds
        }

Nachdem der Code erfolgreich hochgeladen wurde, können Sie sehen, wie die LED atmet.

**Wie es funktioniert**

#. Konstanten und Variablen definieren.

    .. code-block:: arduino

        const int ledPin = 26; // Der GPIO-Pin für die LED
        int brightness = 0;
        int fadeAmount = 5;
   
    * ``ledPin``: Die GPIO-Pin-Nummer, an der die LED angeschlossen ist (in diesem Fall GPIO 26).
    * ``brightness``: Der aktuelle Helligkeitsgrad der LED (initial auf 0 gesetzt).
    * ``fadeAmount``: Der Betrag, um den sich die Helligkeit der LED bei jedem Schritt ändert (auf 5 gesetzt).

#. Konfigurieren des LED-Pins.

    .. code-block:: arduino

        void setup() {
            ledcSetup(ledPin, 5000, 8); // Konfigurieren des PWM-Kanals (0) mit 5000Hz Frequenz und 8-Bit-Auflösung
        }

    Hier verwenden wir das |link_ledc| (LED Control)-Peripheriegerät, das hauptsächlich zur Steuerung der Helligkeit von LEDs entwickelt wurde, aber auch zur Erzeugung von PWM-Signalen für andere Zwecke verwendet werden kann.

    * ``bool ledcAttach(uint8_t pin, uint32_t freq, uint8_t resolution_bits);``: Diese Funktion wird verwendet, um die LEDC-Pins-Frequenz und -Auflösung einzustellen. Sie gibt die ``Frequenz`` zurück, die für den LEDC-Pins konfiguriert wurde. Wenn 0 zurückgegeben wird, ist ein Fehler aufgetreten und der LEDC-Pins wurde nicht konfiguriert.
            
        * ``pin``: Wählt den GPIO-Pin aus.
        * ``freq``: Wählt die PWM-Frequenz aus.
        * ``resolution_bits``: Wählt die Auflösung für den LEDC-Pins aus. Der Bereich liegt zwischen 1-14 Bit (1-20 Bit für ESP32).


#. Die Funktion ``loop()`` enthält die Hauptlogik des Programms und läuft kontinuierlich. Sie aktualisiert die Helligkeit der LED, invertiert die Fade-Menge, wenn die Helligkeit den Mindest- oder Höchstwert erreicht, und fügt eine Verzögerung ein.

    .. code-block:: arduino

        void loop() {
            ledcWrite(ledPin, brightness); // Schreiben des neuen Helligkeitswerts auf den PWM-Pins
            brightness = brightness + fadeAmount;

            if (brightness <= 0 || brightness >= 255) {
                fadeAmount = -fadeAmount;
            }
            
            delay(50); // Warten für 20 Millisekunden
            }

    * ``void ledcWrite(uint8_t pin, uint32_t duty);``: Diese Funktion wird verwendet, um die Duty-Cycle für den LEDC-Pins einzustellen.
        
        * ``pin``: Wählt den LEDC-Pins für die Duty-Cycle-Einstellung aus.
        * ``duty``: Wählt die Duty-Cycle aus, die für den ausgewählten Pins eingestellt werden soll.
