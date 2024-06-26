.. note::

    こんにちは、SunFounderのRaspberry Pi & Arduino & ESP32 Enthusiasts Communityへようこそ！Facebookで一緒にRaspberry Pi、Arduino、ESP32の世界を深く探求しましょう。

    **なぜ参加するのか？**

    - **専門サポート**：コミュニティとチームの助けを借りて、販売後の問題や技術的な課題を解決しましょう。
    - **学び＆共有**：スキルを向上させるためのヒントやチュートリアルを交換しましょう。
    - **限定プレビュー**：新製品の発表や予告編をいち早く入手しましょう。
    - **特別割引**：最新製品の限定割引をお楽しみください。
    - **フェスティブプロモーションとプレゼント**：プレゼントやホリデープロモーションに参加しましょう。

    👉 私たちと一緒に探求し、創造する準備はできましたか？[|link_sf_facebook|]をクリックして、今すぐ参加しましょう！

.. _ar_servo:

2.7 サーボモーターを駆動する
============================

サーボは特定の角度を維持し、正確な回転を提供する能力で知られる位置ベースのデバイスの一種です。これにより、一貫した角度調整が求められる制御システムにおいて非常に有用です。サーボは、高級リモートコントロール玩具、飛行機モデル、潜水艦レプリカ、複雑なリモートコントロールロボットなどで広く使用されています。

この興味深いプロジェクトでは、サーボを独特な方法で操作し、揺れ動かすことに挑戦します！このプロジェクトは、サーボの動力学を深く理解し、正確な制御システムのスキルを向上させ、その操作の理解を深める素晴らしい機会を提供します。

サーボをあなたのメロディに合わせて踊らせる準備はできましたか？さあ、このエキサイティングな旅に出発しましょう！

**必要な部品**

このプロジェクトでは、以下の部品が必要です。



.. list-table::
    :widths: 30 20
    :header-rows: 1

    *   - 部品紹介
        - 購入リンク

    *   - :ref:`cpn_esp32_wroom_32e`
        - |link_esp32_wroom_32e_buy|
    *   - :ref:`cpn_esp32_camera_extension`
        - \-
    *   - 数本のジャンパーワイヤー
        - |link_wires_buy|
    *   - サーボモーター
        - |link_servo_buy|


**利用可能なピン**

このプロジェクトでESP32ボードに利用可能なピンの一覧です。

.. list-table::
    :widths: 5 20 

    * - 利用可能なピン
      - IO13, IO12, IO14, IO27, IO26, IO25, IO33, IO32, IO15, IO2, IO0, IO4, IO5, IO18, IO19, IO21, IO22, IO23


**回路図**

.. image:: img/circuit_4.3_servo.png

**配線**

* オレンジのワイヤーは信号で、IO25に接続します。
* 赤のワイヤーはVCCで、5Vに接続します。
* 茶色のワイヤーはGNDで、GNDに接続します。

.. image:: img/4.3_swinging_servo_bb.png

**コード**

このコードをダウンロードするか、Arduino IDEに直接コピーします。

.. note::

    * :ref:`unknown_com_port`
    * ここでは ``ESP32Servo`` ライブラリを使用します。ライブラリマネージャーからインストールできます。

        .. image:: img/servo_lib.png

.. raw:: html

    <iframe src=https://create.arduino.cc/editor/sunfounder01/34c7969e-fee3-413c-9fe7-9d38ca6fb906/preview?embed style="height:510px;width:100%;margin:10px 0" frameborder=0></iframe>

コードのアップロードが完了すると、サーボアームが0°〜180°の範囲で回転するのが見えます。

**仕組みはどうなっていますか？**

#. |link_esp32servo| ライブラリをインクルードします。この行は、サーボモーターを制御するために必要なESP32Servoライブラリをインポートします。

    .. code-block:: arduino

        #include <ESP32Servo.h>

#. サーボと接続ピンを定義します。このセクションでは、Servoオブジェクト（ ``myServo`` ）とサーボモーターが接続されているピン（ピン25）を表す定数整数（ ``servoPin`` ）を宣言します。

    .. code-block:: arduino

        // サーボと接続ピンを定義する
        Servo myServo;
        const int servoPin = 25;

#. サーボの最小および最大パルス幅を定義します。このセクションでは、サーボモーターの最小および最大パルス幅（それぞれ0.5 msと2.5 ms）を設定します。

    .. code-block:: arduino

        // サーボの最小および最大パルス幅を定義する
        const int minPulseWidth = 500; // 0.5 ms
        const int maxPulseWidth = 2500; // 2.5 ms


#. ``setup`` 関数は、サーボモーターを指定されたピンに接続し、そのパルス幅の範囲を設定することで初期化します。また、サーボのPWM周波数を標準の50Hzに設定します。

    .. code-block:: arduino

        void setup() {
            // サーボを指定されたピンに接続し、そのパルス幅の範囲を設定する
            myServo.attach(servoPin, minPulseWidth, maxPulseWidth);

            // サーボのPWM周波数を設定する
            myServo.setPeriodHertz(50); // 標準の50Hzサーボ
        }
    
    * ``attach (int pin, int min, int max)``: この関数は、サーボモーターを指定されたGPIOピンに接続し、サーボの最小および最大パルス幅を設定します。

        * ``pin``: サーボが接続されているGPIOピンの番号。
        * ``min`` と ``max``: 最小および最大パルス幅（マイクロ秒単位）。これらの値はサーボモーターの動作範囲を定義します。

    * ``setPeriodHertz(int hertz)``: この関数は、サーボモーターのPWM周波数をヘルツ単位で設定します。

        * ``hertz``: 希望するPWM周波数（ヘルツ単位）。サーボのデフォルトのPWM周波数は50Hzであり、ほとんどのアプリケーションに適しています。 


#. ``loop`` 関数はコードのメイン部分で、連続して実行されます。この関数は、サーボモーターを0度から180度まで回転させ、次に0度に戻します。これは、角度を対応するパルス幅にマッピングし、新しいパルス幅値でサーボモーターを更新することで行われます。

    .. code-block:: arduino

        void loop() {
            // サーボを0度から180度まで回転させる
            for (int angle = 0; angle <= 180; angle++) {
                int pulseWidth = map(angle, 0, 180, minPulseWidth, maxPulseWidth);
                myServo.writeMicroseconds(pulseWidth);
                delay(15);
            }
    
            // サーボを180度から0度まで回転させる
            for (int angle = 180; angle >= 0; angle--) {
                int pulseWidth = map(angle, 0, 180, minPulseWidth, maxPulseWidth);
                myServo.writeMicroseconds(pulseWidth);
                delay(15);
            }
        }

    * ``writeMicroseconds(int value)``: この関数は、サーボモーターのパルス幅をマイクロ秒単位で設定します。 
    
        * ``value``: 希望するパルス幅（マイクロ秒単位）。
        
        ``writeMicroseconds(int value)`` 関数は、引数として希望するパルス幅をマイクロ秒単位で受け取ります。この値は通常、コード内で定義された最小および最大パルス幅（ ``minPulseWidth`` と ``maxPulseWidth`` ）の範囲内でなければなりません。この関数は、サーボモーターのパルス幅を設定し、それによってサーボを対応する位置に移動させます。
