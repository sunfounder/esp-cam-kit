.. note::

    Hallo, willkommen in der SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasten-Gemeinschaft auf Facebook! Vertiefen Sie sich mit anderen Enthusiasten in die Welt von Raspberry Pi, Arduino und ESP32.

    **Warum beitreten?**

    - **Expertenunterstützung**: Lösen Sie Nachverkaufsprobleme und technische Herausforderungen mit Hilfe unserer Gemeinschaft und unseres Teams.
    - **Lernen & Teilen**: Tauschen Sie Tipps und Tutorials aus, um Ihre Fähigkeiten zu verbessern.
    - **Exklusive Vorschauen**: Erhalten Sie frühzeitigen Zugang zu neuen Produktankündigungen und exklusiven Einblicken.
    - **Sonderangebote**: Genießen Sie exklusive Rabatte auf unsere neuesten Produkte.
    - **Festliche Aktionen und Gewinnspiele**: Nehmen Sie an Gewinnspielen und Feiertagsaktionen teil.

    👉 Bereit, mit uns zu erkunden und zu kreieren? Klicken Sie auf [|link_sf_facebook|] und treten Sie heute bei!

.. _bluetooth_audio_player:

2.9 Bluetooth-Player
==============================

Ziel dieses Projekts ist es, eine einfache Lösung zum Abspielen von Audio von einem 
Bluetooth-fähigen Gerät mithilfe des eingebauten DAC des ESP32 bereitzustellen.

Das Projekt umfasst die Verwendung der ``ESP32-A2DP``-Bibliothek, um Audiodaten von 
einem Bluetooth-fähigen Gerät zu empfangen. Die empfangenen Audiodaten werden dann 
über die I2S-Schnittstelle an den internen DAC des ESP32 übertragen. Die I2S-Schnittstelle 
ist so konfiguriert, dass sie im Master-Modus, im Übertragungsmodus und im integrierten 
DAC-Modus arbeitet. Die Audiodaten werden dann über den an den DAC angeschlossenen Lautsprecher wiedergegeben.

Bei Verwendung des internen DAC des ESP32 ist zu beachten, dass das Ausgangsspannungsniveau 
auf 1,1 V begrenzt ist. Es wird daher empfohlen, einen externen Verstärker zu verwenden, 
um das Ausgangsspannungsniveau auf das gewünschte Niveau zu erhöhen. Es ist auch wichtig 
sicherzustellen, dass die Audiodaten im richtigen Format und mit der richtigen Abtastrate 
vorliegen, um Verzerrungen oder Geräusche während der Wiedergabe zu vermeiden.


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
    *   - Lautsprecher
        - \-

**Betriebsschritte**

#. Bauen Sie die Schaltung auf.

    Da dies ein Mono-Verstärker ist, können Sie IO25 mit dem L- oder R-Pin des Audioverstärkermoduls verbinden.

    Der 10K-Widerstand wird verwendet, um Hochfrequenzrauschen zu reduzieren und die Lautstärke des Audios zu verringern. Er bildet einen RC-Tiefpassfilter mit der parasitären Kapazität des DAC und des Audioverstärkers. Dieser Filter verringert die Amplitude von Hochfrequenzsignalen und reduziert somit effektiv das Hochfrequenzrauschen. Das Hinzufügen des 10K-Widerstands macht die Musik weicher und eliminiert unerwünschtes Hochfrequenzrauschen.

    Wenn die Musik auf Ihrer SD-Karte bereits leise ist, können Sie den Widerstand entfernen oder durch einen kleineren Wert ersetzen.

    .. image:: img/7.3_bluetooth_audio_player_bb.png

#. Laden Sie diesen Code herunter oder kopieren Sie ihn direkt in die Arduino IDE.

    .. note::
        
        * :ref:`unknown_com_port`
        * Die ``ESP32-A2DP``-Bibliothek wird hier verwendet. Weitere Informationen zur Installation finden Sie unter :ref:`install_lib_man`.
        * :download:`ESP32-A2DP </_static/zip/ESP32-A2DP.zip>`

    .. warning::

        Wenn Sie ein ESP32-Entwicklungsboard Version 3.0.0 oder höher verwenden, können während des Kompilierungsprozesses Fehler auftreten.
        Dieses Problem tritt normalerweise auf, weil neuere Versionen des Boards die ``ESP32-A2DP``-Bibliothek nicht mehr unterstützen.
        Um dieses Beispiel ordnungsgemäß auszuführen, wird empfohlen, die Firmware-Version Ihres ESP32-Boards auf 2.0.17 herunterzustufen.
        Nachdem Sie dieses Beispiel abgeschlossen haben, aktualisieren Sie wieder auf die neueste Version.

        .. image:: ../faq/img/version_2.0.17.png


    .. raw:: html

        <iframe src=https://create.arduino.cc/editor/sunfounder01/7bb7d6dd-72d4-4529-bb42-033b38558347/preview?embed style="height:510px;width:100%;margin:10px 0" frameborder=0></iframe>
        
#. Nachdem Sie das richtige Board und den richtigen Port ausgewählt haben, klicken Sie auf die Schaltfläche **Hochladen**.

    * :ref:`unknown_com_port`

#. Sobald der Code erfolgreich hochgeladen wurde, schalten Sie das Bluetooth-fähige Gerät ein und suchen Sie nach verfügbaren Geräten. Verbinden Sie sich dann mit dem ``ESP32_Bluetooth``.

    .. image:: img/connect_bluetooth.png

#. Spielen Sie Audio auf dem Gerät ab, und das Audio sollte über den an den ESP32 angeschlossenen Lautsprecher wiedergegeben werden.


**Code-Erklärung**

#. Der Code beginnt mit dem Einbinden der ``BluetoothA2DPSink.h``-Bibliothek, die verwendet wird, um Audiodaten von dem Bluetooth-fähigen Gerät zu empfangen. Das ``BluetoothA2DPSink``-Objekt wird dann erstellt und mit den I2S-Schnittstelleneinstellungen konfiguriert.

    .. code-block:: arduino

        #include "BluetoothA2DPSink.h"

        BluetoothA2DPSink a2dp_sink;

#. In der Setup-Funktion initialisiert der Code eine ``i2s_config_t``-Struktur mit der gewünschten Konfiguration für die I2S-Schnittstelle (Inter-IC Sound).

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
    * Die Konfiguration umfasst den ``I2S-Modus``, die ``Abtastrate``, die ``Bits pro Sample``, das ``Kanalformat``, das ``Kommunikationsformat``, die ``Interrupt-Allocations-Flags``, die ``DMA-Pufferanzahl``, die ``DMA-Pufferlänge`` und ob das APLL (Audio PLL) verwendet werden soll oder nicht.
    * Die ``i2s_config_t``-Struktur wird dann als Argument an die ``set_i2s_config``-Funktion des ``BluetoothA2DPSink``-Objekts übergeben, um die I2S-Schnittstelle für die Audiowiedergabe zu konfigurieren.
    * Die ``start``-Funktion des ``BluetoothA2DPSink``-Objekts wird aufgerufen, um den Bluetooth-Audio-Sink zu starten und die Audiowiedergabe über den integrierten DAC zu beginnen.
