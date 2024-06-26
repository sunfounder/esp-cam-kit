 .. note::

    こんにちは、SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasts Community on Facebookへようこそ！Raspberry Pi、Arduino、ESP32についての探求を仲間と共に深めましょう。

    **参加する理由**

    - **専門サポート**: コミュニティとチームの助けを借りて、購入後の問題や技術的な課題を解決しましょう。
    - **学びと共有**: スキルを向上させるためのヒントやチュートリアルを交換しましょう。
    - **独占プレビュー**: 新製品の発表やプレビューに早期アクセスできます。
    - **特別割引**: 最新製品の限定割引を楽しめます。
    - **フェスティブプロモーションとギブアウェイ**: ギブアウェイやホリデープロモーションに参加しましょう。

    👉 一緒に探求し、創造する準備はできましたか？[|link_sf_facebook|]をクリックして今日参加しましょう！

.. _ar_lcd1602:

2.5 I2Cインターフェース
===========================

このレッスンでは、マイクロコントローラとさまざまな周辺機器間の通信の基盤であるI2Cインターフェースの機能について詳しく説明します。ここでは、ESP32のI2Cインターフェースを使用して、LCD1602モジュールを駆動し、文字を表示する方法に焦点を当てます。LCDモジュールの初期化、表示パラメータの設定、およびテキストデータの送信方法を学びます。カスタムメッセージ、センサーの読み取り値、またはインタラクティブなメニューを表示することを目指している場合でも、LCD1602をマスターすることで、情報豊かでインタラクティブなディスプレイを作成する能力が広がります。

**使用可能なピン**

このプロジェクトで使用するESP32ボードの使用可能なピンの一覧です。

.. list-table::
    :widths: 5 15
    :header-rows: 1

    *   - 使用可能なピン
        - 使用説明

    *   - IO21
        - SDA
    *   - IO22
        - SCL

**必要なコンポーネント**

このプロジェクトで必要なコンポーネントは次のとおりです。

.. list-table::
    :widths: 30 20
    :header-rows: 1

    *   - コンポーネント紹介
        - 購入リンク

    *   - :ref:`cpn_esp32_wroom_32e`
        - |link_esp32_wroom_32e_buy|
    *   - :ref:`cpn_esp32_camera_extension`
        - \-
    *   - ジャンパーワイヤー
        - |link_wires_buy|
    *   - I2C LCD1602
        - |link_i2clcd1602_buy|

**回路図**

.. image:: img/circuit_2.6_lcd.png

**配線図**

.. image:: img/2.6_i2clcd1602_bb.png
    :width: 800

**コード**

#. このコードをダウンロードするか、Arduino IDEに直接コピーします。

.. note::
    
    * :ref:`unknown_com_port`
    * ここでは ``LiquidCrystal I2C`` ライブラリを使用しています。 **ライブラリマネージャ** からインストールできます。

        .. image:: img/lcd_lib.png

.. raw:: html

    <iframe src=https://create.arduino.cc/editor/sunfounder01/31e33e53-67b2-4e29-b78b-f647fd45fb0b/preview?embed style="height:510px;width:100%;margin:10px 0" frameborder=0></iframe>

このプログラムがアップロードされると、I2C LCD1602は3秒間 "Hello, Sunfounder!" のウェルカムメッセージを表示します。その後、画面には「COUNT:」ラベルとカウント値が表示され、毎秒増加します。

.. note:: 

    コードと配線が正しくてもLCDが表示しない場合、背面のポテンショメータを調整してコントラストを上げることができます。

**どのように動作しますか？**

``LiquidCrystal_I2C.h`` ライブラリを呼び出すことで、簡単にLCDを駆動できます。

.. code-block:: arduino

    #include <LiquidCrystal_I2C.h>

ライブラリの関数：

* Arduinoボードに接続された特定のLCDを表す ``LiquidCrystal_I2C`` クラスの新しいインスタンスを作成します。

    .. code-block:: arduino

        LiquidCrystal_I2C(uint8_t lcd_Addr,uint8_t lcd_cols,uint8_t lcd_rows)

    * ``lcd_Addr``: LCDのアドレス。デフォルトは0x27です。
    * ``lcd_cols``: LCD1602には16列があります。
    * ``lcd_rows``: LCD1602には2行があります。

* LCDを初期化します。

    .. code-block:: arduino

        void init()

* (オプションの)バックライトを点灯させます。

    .. code-block:: arduino

        void backlight()

* (オプションの)バックライトを消灯させます。

    .. code-block:: arduino

        void nobacklight()

* LCDディスプレイをオンにします。

    .. code-block:: arduino

        void display()

* LCDディスプレイをすばやくオフにします。

    .. code-block:: arduino

        void nodisplay()

* ディスプレイをクリアし、カーソル位置をゼロに設定します。

    .. code-block:: arduino

        void clear()

* カーソル位置を指定した列と行に設定します。

    .. code-block:: arduino

        void setCursor(uint8_t col,uint8_t row)

* テキストをLCDに表示します。

    .. code-block:: arduino

        void print(data,BASE)

    * ``data``: 表示するデータ (char, byte, int, long, または string)。
    * ``BASE (optional)``: 数値を表示する際の基数。

        * ``BIN`` バイナリ (基数2)
        * ``DEC`` デシマル (基数10)
        * ``OCT`` オクタル (基数8)
        * ``HEX`` 16進数 (基数16)
