.. note::

    Hallo, willkommen in der SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasten-Gemeinschaft auf Facebook! Vertiefen Sie sich mit anderen Enthusiasten in die Welt von Raspberry Pi, Arduino und ESP32.

    **Warum beitreten?**

    - **Expertenunterstützung**: Lösen Sie Nachverkaufsprobleme und technische Herausforderungen mit Hilfe unserer Gemeinschaft und unseres Teams.
    - **Lernen & Teilen**: Tauschen Sie Tipps und Tutorials aus, um Ihre Fähigkeiten zu verbessern.
    - **Exklusive Vorschauen**: Erhalten Sie frühzeitigen Zugang zu neuen Produktankündigungen und exklusiven Einblicken.
    - **Sonderangebote**: Genießen Sie exklusive Rabatte auf unsere neuesten Produkte.
    - **Festliche Aktionen und Gewinnspiele**: Nehmen Sie an Gewinnspielen und Feiertagsaktionen teil.

    👉 Bereit, mit uns zu erkunden und zu kreieren? Klicken Sie auf [|link_sf_facebook|] und treten Sie heute bei!

.. _ar_bluetooth:

2.8 Nutzung der Bluetooth-Funktion
==========================================

Dieses Projekt bietet eine Anleitung zur Entwicklung einer einfachen Bluetooth Low Energy (BLE) seriellen Kommunikationsanwendung 
mit dem ESP32-Mikrocontroller. Der ESP32 ist ein leistungsstarker Mikrocontroller, der Wi-Fi- und Bluetooth-Konnektivität integriert und sich daher ideal für die Entwicklung drahtloser Anwendungen eignet. BLE ist ein 
energieeffizientes drahtloses Kommunikationsprotokoll, das für die Kurzstreckenkommunikation entwickelt wurde. 
Dieses Dokument behandelt die Schritte, um den ESP32 so einzurichten, dass er als BLE-Server fungiert und mit einem BLE-Client über eine serielle Verbindung kommuniziert.

**Über die Bluetooth-Funktion**

Der ESP32 WROOM 32E ist ein Modul, das Wi-Fi- und Bluetooth-Konnektivität in einem einzigen Chip integriert. 
Es unterstützt sowohl Bluetooth Low Energy (BLE) als auch klassische Bluetooth-Protokolle.

Das Modul kann als Bluetooth-Client oder -Server verwendet werden. Als Bluetooth-Client kann das Modul eine Verbindung zu 
anderen Bluetooth-Geräten herstellen und Daten mit ihnen austauschen. Als Bluetooth-Server kann das Modul 
Dienste für andere Bluetooth-Geräte bereitstellen.

Der ESP32 WROOM 32E unterstützt verschiedene Bluetooth-Profile, einschließlich des Generic Access Profile (GAP), des Generic Attribute Profile (GATT) 
und des Serial Port Profile (SPP). Das SPP-Profil ermöglicht es dem Modul, einen seriellen Port über Bluetooth zu emulieren 
und eine serielle Kommunikation mit anderen Bluetooth-Geräten zu ermöglichen.

Um die Bluetooth-Funktion des ESP32 WROOM 32E zu nutzen, müssen Sie ihn mit einem geeigneten Software- 
Development Kit (SDK) programmieren oder die Arduino IDE mit der ESP32 BLE-Bibliothek verwenden. 
Die ESP32 BLE-Bibliothek bietet eine hochrangige Schnittstelle für die Arbeit mit BLE. Sie enthält Beispiele, die demonstrieren, 
wie das Modul als BLE-Client und -Server verwendet wird.

Insgesamt bietet die Bluetooth-Funktion des ESP32 WROOM 32E eine bequeme und energieeffiziente Möglichkeit, drahtlose 
Kommunikation in Ihren Projekten zu ermöglichen.

**Betriebsschritte**

Hier sind die Schritt-für-Schritt-Anweisungen, um die Bluetooth-Kommunikation zwischen Ihrem ESP32 und einem mobilen Gerät mithilfe der LightBlue-App einzurichten:

#. Laden Sie die LightBlue-App aus dem **App Store** (für iOS) oder **Google Play** (für Android) herunter.

    .. image:: img/bluetooth_lightblue.png

#. Laden Sie diesen Code herunter oder kopieren Sie ihn direkt in die Arduino IDE.

    .. note::
        
        * :ref:`unknown_com_port`

    .. raw:: html
        
        <iframe src=https://create.arduino.cc/editor/sunfounder01/388f6d9d-65bf-4eaa-b29a-7cebf0b92f74/preview?embed style="height:510px;width:100%;margin:10px 0" frameborder=0></iframe>

#. Um UUID-Konflikte zu vermeiden, wird empfohlen, drei neue UUIDs zufällig zu generieren, indem Sie den |link_uuid| verwenden, und sie in den folgenden Codezeilen einfügen.

    .. code-block:: arduino

        #define SERVICE_UUID           "your_service_uuid_here" 
        #define CHARACTERISTIC_UUID_RX "your_rx_characteristic_uuid_here"
        #define CHARACTERISTIC_UUID_TX "your_tx_characteristic_uuid_here"

    .. image:: img/uuid_generate.png

#. Wählen Sie das richtige Board und den richtigen Port aus, und klicken Sie dann auf die Schaltfläche **Hochladen**.

    .. image:: img/bluetooth_upload.png

#. Nachdem der Code erfolgreich hochgeladen wurde, schalten Sie **Bluetooth** auf Ihrem mobilen Gerät ein und öffnen Sie die **LightBlue**-App.

    .. image:: img/bluetooth_open.png

#. Auf der Seite **Scan** finden Sie **ESP32-Bluetooth** und klicken Sie auf **CONNECT**. Wenn Sie es nicht sehen, versuchen Sie, die Seite ein paar Mal zu aktualisieren. Wenn **"Connected to device!"** erscheint, ist die Bluetooth-Verbindung erfolgreich. Scrollen Sie nach unten, um die im Code festgelegten drei UUIDs zu sehen.

    .. image:: img/bluetooth_connect.png
        :width: 800

#. Klicken Sie auf die **Receive**-UUID. Wählen Sie das entsprechende Datenformat im Feld rechts neben **Data Format** aus, wie z.B. "HEX" für Hexadezimal, "UTF-8 String" für Zeichen oder "Binary" für Binär, usw. Klicken Sie dann auf **SUBSCRIBE**.

    .. image:: img/bluetooth_read.png
        :width: 300

#. Gehen Sie zurück zur Arduino IDE, öffnen Sie den Serial Monitor, stellen Sie die Baudrate auf 115200 ein, geben Sie "welcome" ein und drücken Sie Enter.

    .. image:: img/bluetooth_serial.png

#. Sie sollten jetzt die Nachricht "welcome" in der LightBlue-App sehen.

    .. image:: img/bluetooth_welcome.png
        :width: 400

#. Um Informationen vom mobilen Gerät an den Serial Monitor zu senden, klicken Sie auf die Send UUID, stellen Sie das Datenformat auf "UTF-8 String" ein und schreiben Sie eine Nachricht.

    .. image:: img/bluetooth_send.png

#. Sie sollten die Nachricht im Serial Monitor sehen.

    .. image:: img/bluetooth_receive.png

**Wie funktioniert das?**

Dieser Arduino-Code ist für den ESP32-Mikrocontroller geschrieben und richtet ihn so ein, dass er mit einem Bluetooth Low Energy (BLE)-Gerät kommuniziert.

Hier ist eine kurze Zusammenfassung des Codes:

* **Notwendige Bibliotheken einbinden**: Der Code beginnt mit dem Einbinden der notwendigen Bibliotheken für die Arbeit mit Bluetooth Low Energy (BLE) auf dem ESP32.

    .. code-block:: arduino

        #include "BLEDevice.h"
        #include "BLEServer.h"
        #include "BLEUtils.h"
        #include "BLE2902.h"

* **Globale Variablen**: Der Code definiert eine Reihe von globalen Variablen, einschließlich des Bluetooth-Gerätenamens (``bleName``), Variablen zur Verfolgung des empfangenen Textes und der Zeit der letzten Nachricht, UUIDs für den Dienst und die Eigenschaften und ein ``BLECharacteristic``-Objekt (``pCharacteristic``).

    .. code-block:: arduino

        // Definiere den Bluetooth-Gerätenamen
        const char *bleName = "ESP32_Bluetooth";

        // Definiere den empfangenen Text und die Zeit der letzten Nachricht
        String receivedText = "";
        unsigned long lastMessageTime = 0;

        // Definiere die UUIDs des Dienstes und der Eigenschaften
        #define SERVICE_UUID           "your_service_uuid_here"
        #define CHARACTERISTIC_UUID_RX "your_rx_characteristic_uuid_here"
        #define CHARACTERISTIC_UUID_TX "your_tx_characteristic_uuid_here"

        // Definiere die Bluetooth-Eigenschaft
        BLECharacteristic *pCharacteristic;

* **Setup**: In der ``setup()``-Funktion wird der serielle Port mit einer Baudrate von 115200 initialisiert und die ``setupBLE()``-Funktion aufgerufen, um das Bluetooth BLE einzurichten.

    .. code-block:: arduino
    
        void setup() {
            Serial.begin(115200);  // Initialize the serial port
            setupBLE();            // Initialize the Bluetooth BLE
        }

* **Hauptschleife**: In der ``loop()``-Funktion, wenn eine Zeichenkette über BLE empfangen wurde (d.h. ``receivedText`` ist nicht leer) und seit der letzten Nachricht mindestens 1 Sekunde vergangen ist, druckt der Code die empfangene Zeichenkette auf den seriellen Monitor, setzt den Eigenschaftswert auf die empfangene Zeichenkette, sendet eine Benachrichtigung und löscht dann die empfangene Zeichenkette. Wenn Daten auf dem seriellen Port verfügbar sind, liest er die Zeichenkette, bis ein Zeilenumbruchzeichen auftritt, setzt den Eigenschaftswert auf diese Zeichenkette und sendet eine Benachrichtigung.

    .. code-block:: arduino

        void loop() {
            // When the received text is not empty and the time since the last message is over 1 second
            // Send a notification and print the received text
            if (receivedText.length() > 0 && millis() - lastMessageTime > 1000) {
                Serial.print("Received message: ");
                Serial.println(receivedText);
                pCharacteristic->setValue(receivedText.c_str());
                pCharacteristic->notify();
                receivedText = "";
            }

            // Read data from the serial port and send it to BLE characteristic
            if (Serial.available() > 0) {
                String str = Serial.readStringUntil('\n');
                const char *newValue = str.c_str();
                pCharacteristic->setValue(newValue);
                pCharacteristic->notify();
            }
        }

* **Callbacks**: Zwei Callback-Klassen (``MyServerCallbacks`` und ``MyCharacteristicCallbacks``) sind definiert, um Ereignisse im Zusammenhang mit der Bluetooth-Kommunikation zu verarbeiten. ``MyServerCallbacks`` wird verwendet, um Ereignisse im Zusammenhang mit dem Verbindungsstatus (verbunden oder getrennt) des BLE-Servers zu verarbeiten. ``MyCharacteristicCallbacks`` wird verwendet, um Schreibereignisse auf der BLE-Eigenschaft zu verarbeiten, d.h. wenn ein verbundenes Gerät eine Zeichenkette über BLE an den ESP32 sendet, wird diese erfasst und in ``receivedText`` gespeichert und die aktuelle Zeit in ``lastMessageTime`` aufgezeichnet.

    .. code-block:: arduino

        // Definiere die BLE-Server-Callbacks
        class MyServerCallbacks : public BLEServerCallbacks {
            // Verbindungsmeldung drucken, wenn ein Client verbunden ist
            void onConnect(BLEServer *pServer) {
            Serial.println("Connected");
            }
            // Trennungsmeldung drucken, wenn ein Client getrennt ist
            void onDisconnect(BLEServer *pServer) {
            Serial.println("Disconnected");
            }
        };

        // Definiere die BLE-Eigenschafts-Callbacks
        class MyCharacteristicCallbacks : public BLECharacteristicCallbacks {
            void onWrite(BLECharacteristic *pCharacteristic) {
                // Wenn Daten empfangen werden, die Daten abrufen und in receivedText speichern und die Zeit aufzeichnen
                std::string value = std::string(pCharacteristic->getValue().c_str());
                receivedText = String(value.c_str());
                lastMessageTime = millis();
                Serial.print("Received: ");
                Serial.println(receivedText);
            }
        };

* **Setup BLE**: In der ``setupBLE()``-Funktion werden das BLE-Gerät und der Server initialisiert, die Server-Callbacks gesetzt, der BLE-Dienst mithilfe der definierten UUID erstellt, Eigenschaften zum Senden von Benachrichtigungen und zum Empfangen von Daten erstellt und zum Dienst hinzugefügt und die Eigenschafts-Callbacks gesetzt. Schließlich wird der Dienst gestartet und der Server beginnt zu werben.

    .. code-block:: arduino

        // Initialize the Bluetooth BLE
        void setupBLE() {
            BLEDevice::init(bleName);                        // Initialize the BLE device
            BLEServer *pServer = BLEDevice::createServer();  // Create the BLE server
            // Print the error message if the BLE server creation fails
            if (pServer == nullptr) {
                Serial.println("Error creating BLE server");
                return;
            }
            pServer->setCallbacks(new MyServerCallbacks());  // Set the BLE server callbacks

            // Create the BLE service
            BLEService *pService = pServer->createService(SERVICE_UUID);
            // Print the error message if the BLE service creation fails
            if (pService == nullptr) {
                Serial.println("Error creating BLE service");
                return;
            }
            // Create the BLE characteristic for sending notifications
            pCharacteristic = pService->createCharacteristic(CHARACTERISTIC_UUID_TX, BLECharacteristic::PROPERTY_NOTIFY);
            pCharacteristic->addDecodeor(new BLE2902());  // Add the decodeor
            // Create the BLE characteristic for receiving data
            BLECharacteristic *pCharacteristicRX = pService->createCharacteristic(CHARACTERISTIC_UUID_RX, BLECharacteristic::PROPERTY_WRITE);
            pCharacteristicRX->setCallbacks(new MyCharacteristicCallbacks());  // Set the BLE characteristic callbacks
            pService->start();                                                 // Start the BLE service
            pServer->getAdvertising()->start();                                // Start advertising
            Serial.println("Waiting for a client connection...");              // Wait for a client connection
        }

Bitte beachten Sie, dass dieser Code eine bidirektionale Kommunikation ermöglicht - er kann 
Daten über BLE senden und empfangen. 
Um jedoch mit spezifischer Hardware wie dem Ein- und Ausschalten einer LED zu interagieren, 
sollte zusätzlicher Code hinzugefügt werden, um die empfangenen Zeichenketten zu verarbeiten 
und entsprechend zu handeln.
