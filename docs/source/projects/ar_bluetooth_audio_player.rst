 .. note::

    Hallo und willkommen in der SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasten-Community auf Facebook! Tauchen Sie tiefer in Raspberry Pi, Arduino und ESP32 mit anderen Enthusiasten ein.

    **Warum beitreten?**

    - **Expertenunterstützung**: Lösen Sie nach dem Kauf auftretende Probleme und technische Herausforderungen mit Hilfe unserer Community und unseres Teams.
    - **Lernen & Teilen**: Tauschen Sie Tipps und Tutorials aus, um Ihre Fähigkeiten zu verbessern.
    - **Exklusive Vorschauen**: Erhalten Sie frühzeitigen Zugang zu neuen Produktankündigungen und Sneak Peeks.
    - **Sonderrabatte**: Genießen Sie exklusive Rabatte auf unsere neuesten Produkte.
    - **Festliche Aktionen und Gewinnspiele**: Nehmen Sie an Gewinnspielen und Feiertagsaktionen teil.

    👉 Bereit, mit uns zu erkunden und zu kreieren? Klicken Sie auf [|link_sf_facebook|] und treten Sie noch heute bei!

.. _bluetooth_audio_player:

2.9 Bluetooth Player
==============================

Ziel dieses Projekts ist es, eine einfache Lösung zum Abspielen von Audio von einem Bluetooth-fähigen Gerät über den integrierten DAC des ESP32 bereitzustellen.

Das Projekt beinhaltet die Verwendung der ``ESP32-A2DP``-Bibliothek zum Empfangen von Audiodaten von einem Bluetooth-fähigen Gerät. Die empfangenen Audiodaten werden dann über die I2S-Schnittstelle an den internen DAC des ESP32 übertragen. Die I2S-Schnittstelle ist so konfiguriert, dass sie im Master-Modus, Übertragungsmodus und DAC-Built-in-Modus arbeitet. Die Audiodaten werden dann über den an den DAC angeschlossenen Lautsprecher wiedergegeben.

Beim Einsatz des internen DAC des ESP32 ist zu beachten, dass das Ausgangsspannungsniveau auf 1,1 V begrenzt ist. Daher wird empfohlen, einen externen Verstärker zu verwenden, um das Ausgangsspannungsniveau auf das gewünschte Niveau zu erhöhen. Es ist auch wichtig sicherzustellen, dass die Audiodaten im richtigen Format und mit der richtigen Abtastrate vorliegen, um Verzerrungen oder Rauschen während der Wiedergabe zu vermeiden.

**Benötigte Komponenten**

In diesem Projekt benötigen wir die folgenden Komponenten. 



.. list-table::
    :widths: 30 20
    :header-rows: 1

    *   - KOMPONENTENÜBERSICHT
        - KAUFLINK

    *   - :ref:`cpn_esp32_wroom_32e`
        - |link_esp32_wroom_32e_buy|
    *   - :ref:`cpn_esp32_camera_extension`
        - \-
    *   - Steckbrett
        - |link_breadboard_buy|
    *   - Mehrere Jumperkabel
        - |link_wires_buy|
    *   - Widerstand
        - |link_resistor_buy|
    *   - Lautsprecher
        - \-


**Betriebsschritte**

#. Schalten Sie den Schaltkreis ein.

    Da dies ein Mono-Verstärker ist, können Sie IO25 mit dem L- oder R-Pin des Audioverstärkermoduls verbinden.

    Der 10K-Widerstand wird verwendet, um Hochfrequenzrauschen zu reduzieren und die Lautstärke zu verringern. Er bildet einen RC-Tiefpassfilter mit der parasitären Kapazität des DAC und des Audioverstärkers. Dieser Filter verringert die Amplitude von Hochfrequenzsignalen und reduziert so effektiv das Hochfrequenzrauschen. Das Hinzufügen des 10K-Widerstands macht die Musik weicher und beseitigt unerwünschtes Hochfrequenzrauschen.

    Wenn die Musik auf Ihrer SD-Karte bereits leise ist, können Sie den Widerstand entfernen oder durch einen kleineren Wert ersetzen.

    .. image:: img/7.3_bluetooth_audio_player_bb.png

#. Laden Sie diesen Code herunter oder kopieren Sie ihn direkt in die Arduino IDE.

    .. note::
        
        * :ref:`unknown_com_port`
        * Die ``ESP32-A2DP``-Bibliothek wird hier verwendet. Weitere Informationen finden Sie unter :ref:`install_lib_man`.
        * :download:`ESP32-A2DP </_static/zip/ESP32-A2DP.zip>`

    .. raw:: html

        <iframe src=https://create.arduino.cc/editor/sunfounder01/7bb7d6dd-72d4-4529-bb42-033b38558347/preview?embed style="height:510px;width:100%;margin:10px 0" frameborder=0></iframe>
        
#. Wählen Sie nach Auswahl der richtigen Platine und des richtigen Ports die Upload-Schaltfläche.

    * :ref:`unknown_com_port`

#. Sobald der Code erfolgreich hochgeladen wurde, schalten Sie das Bluetooth-fähige Gerät ein und suchen Sie nach verfügbaren Geräten. Verbinden Sie sich dann mit dem ``ESP32_Bluetooth``.

    .. image:: img/connect_bluetooth.png

#. Spielen Sie Audio auf dem Gerät ab und die Audiodaten sollten über den mit dem ESP32 verbundenen Lautsprecher abgespielt werden.


**Code-Erklärung**

#. Der Code beginnt mit der Einbindung der ``BluetoothA2DPSink.h``-Bibliothek, die zum Empfangen von Audiodaten von einem Bluetooth-fähigen Gerät verwendet wird. Das ``BluetoothA2DPSink``-Objekt wird dann erstellt und mit den I2S-Schnittstelleneinstellungen konfiguriert. 

    .. code-block:: arduino

        #include "BluetoothA2DPSink.h"

        BluetoothA2DPSink a2dp_sink;


#. In der Setup-Funktion initialisiert der Code eine ``i2s_config_t struct`` mit der gewünschten Konfiguration für die I2S (Inter-IC Sound)-Schnittstelle. 

    .. code-block:: arduino

        void setup() {
        const i2s_config_t i2s_config = {
            .mode = (i2s_mode_t) (I2S_MODE_MASTER | I2S_MODE_TX | I2S_MODE_DAC_BUILT_IN),
            .sample_rate = 44100, // corrected by info from bluetooth
            .bits_per_sample = (i2s_bits_per_sample_t) 16, // the DAC module will only take the 8bits from MSB
            .channel_format =  I2S_CHANNEL_FMT_RIGHT_LEFT,
            .communication_format = (i2s_comm_format_t)I2S_COMM_FORMAT_STAND_MSB,
            .intr_alloc_flags = 0, // default interrupt priority
            .dma_buf_count = 8,
            .dma_buf_len = 64,
            .use_apll = false
        };

        a2dp_sink.set_i2s_config(i2s_config);  
        a2dp_sink.start("ESP32_Bluetooth");  

        }

    * Die I2S-Schnittstelle wird verwendet, um digitale Audiodaten zwischen Geräten zu übertragen. 
    * Die Konfiguration umfasst den ``I2S-Modus``, die ``Abtastrate``, die ``Bits pro Sample``, das ``Kanalformat``, das ``Kommunikationsformat``, die ``Interrupt-Zuweisungsflags``, die ``DMA-Pufferanzahl``, die ``DMA-Pufferlänge`` und ob der APLL (Audio PLL) verwendet werden soll oder nicht.
    * Die ``i2s_config_t struct`` wird dann als Argument an die Funktion ``set_i2s_config`` des ``BluetoothA2DPSink``-Objekts übergeben, um die I2S-Schnittstelle für die Audiowiedergabe zu konfigurieren.
    * Die ``start``-Funktion des ``BluetoothA2DPSink``-Objekts wird aufgerufen, um das Bluetooth-Audio-Sink zu starten und die Audiowiedergabe über den eingebauten DAC zu beginnen.

