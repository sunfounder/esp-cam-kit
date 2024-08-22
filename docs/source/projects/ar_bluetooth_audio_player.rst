 .. note::

    こんにちは、SunFounderのRaspberry Pi & Arduino & ESP32 Enthusiasts Communityへようこそ！Raspberry Pi、Arduino、ESP32を使って仲間と一緒に深く探求しましょう。

    **参加する理由**

    - **専門家のサポート**：コミュニティやチームの助けを借りて、購入後の問題や技術的な課題を解決しましょう。
    - **学びと共有**：スキルを向上させるためのヒントやチュートリアルを交換しましょう。
    - **限定プレビュー**：新製品の発表やスニークピークへの早期アクセスを取得しましょう。
    - **特別割引**：最新製品の独占割引をお楽しみください。
    - **祭りのプロモーションとプレゼント**：プレゼントや休日のプロモーションに参加しましょう。

    👉 私たちと一緒に探求し、創造する準備はできましたか？[|link_sf_facebook|]をクリックして、今日から参加しましょう！

.. _bluetooth_audio_player:

2.9 Bluetoothオーディオプレイヤー
========================================================

このプロジェクトの目的は、Bluetooth対応デバイスからのオーディオをESP32の内蔵DACを通じて再生するシンプルなソリューションを提供することです。

プロジェクトには、「ESP32-A2DP」ライブラリを使用してBluetooth対応デバイスからオーディオデータを受信し、そのデータをI2Sインターフェイスを通じてESP32の内蔵DACに送信することが含まれます。I2Sインターフェイスはマスターモード、送信モード、内蔵DACモードで構成されています。次に、DACに接続されたスピーカーでオーディオデータが再生されます。

ESP32の内蔵DACを使用する際には、出力電圧レベルが1.1Vに制限されていることを考慮する必要があります。そのため、出力電圧レベルを望むレベルに増幅するために外部アンプの使用が推奨されます。また、再生中の歪みやノイズを避けるために、正しいフォーマットと適切なサンプルレートでオーディオデータが提供されていることを確認することが重要です。

**必要なコンポーネント**

このプロジェクトには、以下のコンポーネントが必要です。



.. list-table::
    :widths: 30 20
    :header-rows: 1

    *   - コンポーネント概要
        - 購入リンク

    *   - :ref:`cpn_esp32_wroom_32e`
        - |link_esp32_wroom_32e_buy|
    *   - :ref:`cpn_esp32_camera_extension`
        - \-
    *   - ブレッドボード
        - |link_breadboard_buy|
    *   - 複数のジャンパーワイヤ
        - |link_wires_buy|
    *   - 抵抗器
        - |link_resistor_buy|
    *   - スピーカー
        - \-


**操作手順**

#. 電路をオンにします。

    これはモノラルアンプですので、IO25をオーディオアンプモジュールのLまたはRピンに接続できます。

    10K抵抗器は、高周波ノイズを低減し音量を下げるために使用されます。この抵抗器は、DACとオーディオアンプの寄生容量とRCローパスフィルターを形成します。このフィルターは高周波信号の振幅を減少させ、効果的に高周波ノイズを低減します。10K抵抗器を追加すると、音楽がより柔らかくなり、望ましくない高周波ノイズが除去されます。

    SDカードの音楽が既に静かな場合、抵抗器を取り外すか、より小さい値のものに交換することができます。

    .. image:: img/7.3_bluetooth_audio_player_bb.png

#. このコードをダウンロードするか、直接Arduino IDEにコピーします。

    .. note::
        
        * :ref:`unknown_com_port`
        * ここでは「ESP32-A2DP」ライブラリが使用されています。詳細は :ref:`install_lib_man`を参照してください。
        * :download:`ESP32-A2DP </_static/zip/ESP32-A2DP.zip>`

    .. warning::

        ESP32開発ボードのバージョン3.0.0以上を使用している場合、コンパイルプロセス中にエラーが発生することがあります。
        この問題は、ボードの新しいバージョンが「ESP32-A2DP」ライブラリをサポートしなくなったためです。
        この例を正しく実行するには、ESP32ボードのファームウェアバージョンを2.0.17にダウングレードすることをお勧めします。
        この例を完了した後、最新バージョンに再度アップグレードしてください。

        .. image:: ../faq/img/version_2.0.17.png

    .. raw:: html

        <iframe src=https://create.arduino.cc/editor/sunfounder01/7bb7d6dd-72d4-4529-bb42-033b38558347/preview?embed style="height:510px;width:100%;margin:10px 0" frameborder=0></iframe>
        
#. 正しいボードとポートを選択した後、アップロードボタンをクリックします。

    * :ref:`unknown_com_port`

#. コードのアップロードが成功したら、Bluetooth対応デバイスをオンにし、利用可能なデバイスを探します。その後、「ESP32_Bluetooth」に接続します。

    .. image:: img/connect_bluetooth.png

#. デバイスでオーディオを再生すると、ESP32に接続されたスピーカーでオーディオデータが再生されるはずです。


**コード説明**

#. このコードは、Bluetooth対応デバイスからオーディオデータを受信するために使用される ``BluetoothA2DPSink.h``ライブラリの読み込みから始まります。次に、 ``BluetoothA2DPSink``オブジェクトが作成され、I2Sインターフェースの設定で構成されます。

    .. code-block:: arduino

        #include "BluetoothA2DPSink.h"

        BluetoothA2DPSink a2dp_sink;

#. セットアップ関数では、I2S（Inter-IC Sound）インターフェース用に設定された``i2s_config_t struct``を初期化します。

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

    * I2Sインターフェースは、デバイス間でデジタルオーディオデータを転送するために使用されます。
    * 設定には ``I2Sモード``、 ``サンプルレート``、 ``サンプルあたりのビット数``、 ``チャネルフォーマット``、 ``通信フォーマット``、 ``割り込み割当フラグ``、 ``DMAバッファ数``、 ``DMAバッファ長さ``、およびAPLL（オーディオPLL）の使用有無が含まれます。
    * これらの設定は ``BluetoothA2DPSink``オブジェクトの ``set_i2s_config``関数に引数として渡され、オーディオ再生のためにI2Sインターフェースを設定します。
    * ``BluetoothA2DPSink``オブジェクトの ``start``関数を呼び出すことで、Bluetoothオーディオシンクが開始され、内蔵DACを通じてのオーディオ再生が始まります。

