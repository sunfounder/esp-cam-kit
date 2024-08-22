.. note::

    こんにちは、SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasts CommunityのFacebookページへようこそ！Raspberry Pi、Arduino、ESP32の愛好者と共にさらに深く探求しましょう。

    **参加する理由**

    - **専門サポート**: コミュニティとチームの助けを借りて、購入後の問題や技術的な課題を解決します。
    - **学びと共有**: スキルを向上させるためのヒントやチュートリアルを交換します。
    - **限定プレビュー**: 新製品の発表や先行情報を早期に入手できます。
    - **特別割引**: 最新製品に対する特別割引をお楽しみいただけます。
    - **フェスティブプロモーションとプレゼント**: プレゼントやホリデープロモーションに参加できます。

    👉 私たちと一緒に探求し、創造する準備はできましたか？[|link_sf_facebook|]をクリックして、今日参加しましょう！

.. _ar_potentiometer:

2.4 アナログ入力
==========================

このレッスンでは、アナログ入力デバイスとしてポテンショメータを使用してLEDの明るさを調整する方法を探ります。ポテンショメータのノブを回すだけで、デスクランプの明るさを調整するのと同様に、LEDの光の強さを変えることができます。このシンプルなセットアップは、アナログ入力が現実世界のアプリケーションに与える直接的な影響を示し、入力の変化が電子部品の制御にどのように役立つかを直感的に理解することができます。


**利用可能なピン**

* **利用可能なピン**

    ここでは、このプロジェクトで使用するESP32ボードの利用可能なピンのリストを示します。

    .. list-table::
        :widths: 5 15

        *   - 利用可能なピン
            - IO14, IO25, I35, I34, I39, I36

* **ストラップピン**

    以下のピンはストラップピンであり、電源オンやリセット時のESP32の起動プロセスに影響を与えます。ただし、ESP32が正常に起動した後は、通常のピンとして使用できます。

    .. list-table::
        :widths: 5 15

        *   - ストラップピン
            - IO0, IO12


**必要なコンポーネント**

このプロジェクトでは、以下のコンポーネントが必要です。

.. list-table::
    :widths: 30 20
    :header-rows: 1

    *   - コンポーネントの紹介
        - 購入リンク

    *   - :ref:`cpn_esp32_wroom_32e`
        - |link_esp32_wroom_32e_buy|
    *   - :ref:`cpn_esp32_camera_extension`
        - \-
    *   - ブレッドボード
        - |link_breadboard_buy|
    *   - いくつかのジャンプワイヤー
        - |link_wires_buy|
    *   - 抵抗
        - |link_resistor_buy|
    *   - LED
        - |link_led_buy|
    *   - ポテンショメータ
        - |link_potentiometer_buy|


**回路図**

.. image:: img/circuit_5.8_potentiometer.png

ポテンショメータを回転させると、I35の値が変化します。プログラムを使用して、I35の値を使用してLEDの明るさを制御できます。したがって、ポテンショメータを回転させると、LEDの明るさもそれに応じて変化します。


**配線**

.. image:: img/5.8_potentiometer_bb.png

**コード**

このコードをダウンロードするか、直接Arduino IDEにコピーします。

.. note::

    * :ref:`unknown_com_port`
   
.. raw:: html
     
    <iframe src=https://create.arduino.cc/editor/sunfounder01/aadce2e7-fd5d-4608-a557-f1e4d07ba795/preview?embed style="height:510px;width:100%;margin:10px 0" frameborder=0></iframe>

コードが正常にアップロードされた後、ポテンショメータを回転させると、LEDの明るさがそれに応じて変化します。同時にシリアルモニタでポテンショメータのアナログ値と電圧値を確認できます。


**動作の仕組み**

#. ピン接続とPWM設定のための定数を定義します。

    .. code-block:: arduino

        const int potPin = 35; // ポテンショメータはGPIO35に接続
        const int ledPin = 26; // LEDはGPIO26に接続

        // PWM設定
        const int freq = 5000; // PWM周波数
        const int resolution = 12; // PWM解像度（ビット）

    ここでは、PWMの解像度を12ビットに設定し、範囲は0-4095です。

#. ``setup()`` 関数でシステムを設定します。

    .. code-block:: arduino

        void setup() {
            Serial.begin(115200);

            // Configure PWM
            ledcAttach(ledPin, freq, resolution);
        }

    * ``setup()`` 関数では、シリアル通信を115200ボーレートで開始します。
    * 指定されたLEDピンを指定された周波数と解像度で設定するために ``ledcAttach()`` 関数が呼び出されます。

#. メインループ（繰り返し実行）を ``loop()``関数で設定します。

    .. code-block:: arduino

        void loop() {

            int potValue = analogRead(potPin); // ポテンショメータの値を読み取る
            uint32_t voltage_mV = analogReadMilliVolts(potPin); // ミリボルト単位で電圧を読み取る
            
            ledcWrite(ledPin, potValue);
            
            Serial.print("ポテンショメータの値: ");
            Serial.print(potValue);
            Serial.print(", 電圧: ");
            Serial.print(voltage_mV / 1000.0); // ミリボルトをボルトに変換
            Serial.println(" V");
            
            delay(100);
        }

    * ``uint32_t analogReadMilliVolts(uint8_t pin);``: この関数は、指定されたピン/ADCチャネルのADC値をミリボルト単位で取得するために使用されます。

        * ``pin``: アナログ値を読み取るGPIOピン。

    ポテンショメータの値は、 ``ledcWrite()`` 関数を介してLEDの明るさを制御するために直接PWMデューティサイクルとして使用されます。値の範囲も0から4095です。
