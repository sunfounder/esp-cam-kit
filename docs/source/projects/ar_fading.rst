 .. note::

    こんにちは、SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasts Communityへようこそ！FacebookでRaspberry Pi、Arduino、ESP32をさらに深く探求しましょう。

    **なぜ参加するのか？**

    - **専門サポート**: コミュニティとチームの助けを借りて、販売後の問題や技術的な課題を解決。
    - **学びと共有**: スキルを向上させるためのヒントやチュートリアルを交換。
    - **独占プレビュー**: 新製品の発表や先行情報に早くアクセス。
    - **特別割引**: 最新製品の特別割引を享受。
    - **フェスティバルプロモーションとプレゼント**: プレゼントや休日のプロモーションに参加。

    👉 一緒に探求して創造する準備はできましたか？クリックして[|link_sf_facebook|]今日参加してください！

.. _ar_fading:

2.2 PWMを使用したアナログ出力
================================

前のプロジェクトでは、デジタル出力を使用してLEDを点灯および消灯しました。このプロジェクトでは、パルス幅変調（PWM）を利用してLEDに呼吸効果を与えます。PWMは、四角波信号のデューティサイクルを変えることで、LEDの明るさやモーターの速度を制御する技術です。

PWMを使用すると、単にLEDを点灯または消灯するのではなく、各サイクル内でLEDが点灯している時間と消灯している時間を調整します。LEDを急速に点滅させることで、LEDが徐々に明るくなり、暗くなる錯覚を作り出し、呼吸効果をシミュレートします。

ESP32 WROOM 32EのPWM機能を使用することで、LEDの明るさを滑らかで精密に制御できます。この呼吸効果は、プロジェクトに動的で視覚的に魅力的な要素を追加し、目を引くディスプレイや雰囲気を作り出します。

**使用可能なピン**

このプロジェクトでESP32ボードで使用可能なピンのリストは次の通りです。

.. list-table::
    :widths: 5 20 

    * - 使用可能なピン
      - IO13, IO12, IO14, IO27, IO26, IO25, IO33, IO32, IO15, IO2, IO0, IO4, IO5, IO18, IO19, IO21, IO22, IO23


**必要な部品**

このプロジェクトで必要な部品は次の通りです。

.. list-table::
    :widths: 30 20
    :header-rows: 1

    *   - 部品紹介
        - 購入リンク

    *   - :ref:`cpn_esp32_wroom_32e`
        - |link_esp32_wroom_32e_buy|
    *   - :ref:`cpn_esp32_camera_extension`
        - \-
    *   - ブレッドボード
        - |link_breadboard_buy|
    *   - ジャンパーワイヤー
        - |link_wires_buy|
    *   - 抵抗器
        - |link_resistor_buy|
    *   - LED
        - |link_led_buy|


**回路図**

.. image:: img/circuit_2.1_led.png

このプロジェクトは最初のプロジェクト :ref:`ar_blink` と同じ回路ですが、信号の種類が異なります。最初のプロジェクトでは、pin26から直接デジタルのハイおよびロー（0&1）レベルを出力してLEDを点灯または消灯させましたが、このプロジェクトでは、pin26からPWM信号を出力してLEDの明るさを制御します。

**配線図**

.. image:: img/2.1_hello_led_bb.png


**コード**

#. このコードをダウンロードするか、直接Arduino IDEにコピーします。
    
.. note::
    
    * :ref:`unknown_com_port`

.. raw:: html

    <iframe src=https://create.arduino.cc/editor/sunfounder01/aa898b09-be86-473b-9bfe-317556c696bb/preview?embed style="height:510px;width:100%;margin:10px 0" frameborder=0></iframe>

コードが正常にアップロードされると、LEDが呼吸するように点灯します。

**動作の仕組み**


#. 定数と変数を定義します。

    .. code-block:: arduino

        const int ledPin = 26; // LEDのGPIOピン
        int brightness = 0;
        int fadeAmount = 5;
   
    * ``ledPin``: LEDが接続されているGPIOピン番号（この場合、GPIO 26）。
    * ``brightness``: LEDの現在の明るさレベル（初期設定は0）。
    * ``fadeAmount``: LEDの明るさを各ステップで変える量（設定値は5）。

#. PWMチャネルを初期化し、LEDピンを設定します。

    .. code-block:: arduino

        void setup() {
            ledcSetup(0, 5000, 8); // PWMチャネル（0）を5000Hzの周波数と8ビットの解像度で設定
            ledcAttachPin(ledPin, 0); // LEDピンをPWMチャネルに接続
        }

    ここでは、|link_ledc| （LED制御）周辺機器を使用しています。これは主にLEDの強度を制御するために設計されていますが、他の目的のためにPWM信号を生成することもできます。

    * ``uint32_t ledcSetup(uint8_t channel, uint32_t freq, uint8_t resolution_bits);``: この関数はLEDCチャネルの周波数と解像度を設定します。LEDCチャネルが設定されると、 ``frequency`` が返されます。0が返された場合、エラーが発生し、ledcチャネルは設定されません。
            
        * ``channel``: 設定するLEDCチャネルを選択。
        * ``freq``: PWMの周波数を選択。
        * ``resolution_bits``: ledcチャネルの解像度を選択します。範囲は1-14ビット（ESP32では1-20ビット）。

    * ``void ledcAttachPin(uint8_t pin, uint8_t chan);``: この関数はピンをLEDCチャネルに接続します。

        * ``pin``: GPIOピンを選択。
        * ``chan``: LEDCチャネルを選択。

#. ``loop()`` 関数には、プログラムのメインロジックが含まれており、連続して実行されます。LEDの明るさを更新し、明るさが最小値または最大値に達したときにフェード量を反転させ、遅延を導入します。

    .. code-block:: arduino

        void loop() {
            ledcWrite(0, brightness); // PWMチャネルに新しい明るさの値を書き込む
            brightness = brightness + fadeAmount;

            if (brightness <= 0 || brightness >= 255) {
                fadeAmount = -fadeAmount;
            }
            
            delay(50); // 20ミリ秒待機
            }

    * ``void ledcWrite(uint8_t chan, uint32_t duty);``: この関数はLEDCチャネルのデューティを設定します。
        
        * ``chan``: デューティを書き込むLEDCチャネルを選択。
        * ``duty``: 選択されたチャネルに設定するデューティを選択。
