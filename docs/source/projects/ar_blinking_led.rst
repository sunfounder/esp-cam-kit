 .. note::

    こんにちは、SunFounderのRaspberry Pi & Arduino & ESP32 Enthusiasts Communityへようこそ！Raspberry Pi、Arduino、ESP32を使って仲間と一緒に深く探求しましょう。

    **参加する理由**

    - **専門家のサポート**：コミュニティやチームの助けを借りて、購入後の問題や技術的な課題を解決しましょう。
    - **学びと共有**：スキルを向上させるためのヒントやチュートリアルを交換しましょう。
    - **限定プレビュー**：新製品の発表やスニークピークへの早期アクセスを取得しましょう。
    - **特別割引**：最新製品の独占割引をお楽しみください。
    - **祭りのプロモーションとプレゼント**：プレゼントや休日のプロモーションに参加しましょう。

    👉 私たちと一緒に探求し、創造する準備はできましたか？[|link_sf_facebook|]をクリックして、今日から参加しましょう！

.. _ar_blink:

2.1 デジタル出力
=======================================

数多くのマイクロコントローラ開発ボードの中で、ESP32はその高性能と多用途性で際立っています。このプロジェクトでは、ESP32ボードのデジタル出力ピンを使用して外部デバイス（この場合はLEDを点灯）を制御する方法を紹介します。これはESP32プログラミングを学ぶ基礎として、IoTアプリケーションの探索への入り口となります。

**使用可能なピン**

このプロジェクトで使用するESP32ボードの使用可能なピンのリストです。

.. list-table::
    :widths: 5 20 

    * - 使用可能なピン
      - IO13, IO12, IO14, IO27, IO26, IO25, IO33, IO32, IO15, IO2, IO0, IO4, IO5, IO18, IO19, IO21, IO22, IO23



**必要なコンポーネント**

このプロジェクトでは、以下のコンポーネントが必要です。

.. list-table::
    :widths: 30 20
    :header-rows: 1

    *   - コンポーネント紹介
        - 購入リンク

    *   - :ref:`cpn_esp32_wroom_32e`
        - |link_esp32_wroom_32e_buy|
    *   - :ref:`cpn_esp32_camera_extension`
        - \-
    *   - ブレッドボード
        - |link_breadboard_buy|
    *   - ジャンパーワイヤー
        - |link_wires_buy|
    *   - 抵抗
        - |link_resistor_buy|
    *   - LED
        - |link_led_buy|

**回路図**

.. image:: img/circuit_2.1_led.png

この回路は単純な原理で動作し、図に示される電流の方向に従います。ピン26が高レベルを出力すると、220オームの電流制限抵抗の後にLEDが点灯します。ピン26が低レベルを出力すると、LEDは消灯します。

**配線**

.. image:: img/2.1_hello_led_bb.png


**コードのアップロード**

#. このコードをダウンロードするか、直接Arduino IDEにコピーしてください。

    .. note::
        
        * :ref:`unknown_com_port`

    .. raw:: html

        <iframe src=https://create.arduino.cc/editor/sunfounder01/1bff2463-40ad-43c1-8815-9f448bab3735/preview?embed style="height:510px;width:100%;margin:10px 0" frameborder=0></iframe>
    
#. 次に、ESP32 WROOM 32EをMicro USBケーブルでコンピュータに接続します。

    * :ref:`unknown_com_port`

    .. image:: img/plugin_esp32.png
        :width: 600
        :align: center

#. ボード（ESP32 Dev Module）と適切なポートを選択します。

    .. image:: img/choose_board.png

#. そして、 **Upload** ボタンをクリックしてコードをESP32ボードにアップロードします。
    
    .. image:: img/click_upload.png

#. コードが正常にアップロードされると、LEDが点滅するのが見えます。

**動作の仕組み**

#. ``ledPin`` という名前の整数定数を宣言し、値26を割り当てます。

    .. code-block:: arduino

        const int ledPin = 26;  // LEDのGPIOピン


#. 次に、 ``setup()`` 関数でピンを初期化し、ピンを ``OUTPUT`` モードに設定します。

    .. code-block:: arduino

        void setup() {
            pinMode(ledPin, OUTPUT);
        }

    * ``void pinMode(uint8_t pin, uint8_t mode);``: この関数は、特定のピンのGPIO操作モードを定義するために使用されます。

        * ``pin`` はGPIOピン番号を定義します。
        * ``mode`` は操作モードを設定します。

        基本的な入力と出力には以下のモードがサポートされています：

        * ``INPUT`` はプルアップまたはプルダウンなしでGPIOを入力（高インピーダンス）として設定します。
        * ``OUTPUT`` はGPIOを出力/読み取りモードとして設定します。
        * ``INPUT_PULLDOWN`` は内部プルダウンでGPIOを入力として設定します。
        * ``INPUT_PULLUP`` は内部プルアップでGPIOを入力として設定します。

#. ``loop()`` 関数には、プログラムの主なロジックが含まれており、継続的に実行されます。これは、1秒ごとにピンを高低に設定することを交互に行います。

    .. code-block:: arduino

        void loop() {
            digitalWrite(ledPin, HIGH);   // turn the LED on (HIGH is the voltage level)
            delay(1000);                       // wait for a second
            digitalWrite(ledPin, LOW);    // turn the LED off by making the voltage LOW
            delay(1000);                       // wait for a second
        }

    * ``void digitalWrite(uint8_t pin, uint8_t val);`` : この関数は、選択したGPIOの状態を ``HIGH`` または ``LOW`` に設定します。この関数は、 ``pinMode`` が ``OUTPUT`` として構成された場合にのみ使用されます。
    
        * ``pin`` はGPIOピン番号を定義します。
        * ``val`` は出力デジタル状態を ``HIGH`` または ``LOW`` に設定します。
