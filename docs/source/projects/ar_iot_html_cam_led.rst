 .. note::

    こんにちは、SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasts Community on Facebookへようこそ！Raspberry Pi、Arduino、ESP32についての探求を仲間と共に深めましょう。

    **参加する理由**

    - **専門サポート**: コミュニティとチームの助けを借りて、購入後の問題や技術的な課題を解決しましょう。
    - **学びと共有**: スキルを向上させるためのヒントやチュートリアルを交換しましょう。
    - **独占プレビュー**: 新製品の発表やプレビューに早期アクセスできます。
    - **特別割引**: 最新製品の限定割引を楽しめます。
    - **フェスティブプロモーションとギブアウェイ**: ギブアウェイやホリデープロモーションに参加しましょう。

    👉 一緒に探求し、創造する準備はできましたか？[|link_sf_facebook|]をクリックして今日参加しましょう！

.. _iot_html_cam:

2.14 カスタムビデオストリーミングウェブサーバー
===================================================

このプロジェクトでは、最初からウェブページを作成し、ビデオストリームを再生するようにカスタマイズする方法を学びます。さらに、LEDの明るさを制御するONおよびOFFボタンなどのインタラクティブなボタンを組み込むことができます。

このプロジェクトを通じて、ウェブ開発、HTML、CSS、およびJavaScriptの実践的な経験を得ることができます。リアルタイムでビデオストリームを表示する応答性の高いウェブページを作成する方法を学びます。さらに、インタラクティブなボタンを統合してLEDの状態を制御し、動的なユーザー体験を提供する方法も学びます。

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

**手順**

#. まずカメラを接続します。

    .. raw:: html

        <video loop autoplay muted style = "max-width:100%">
            <source src="../_static/video/plugin_camera.mp4" type="video/mp4">
            Your browser does not support the video tag.
        </video>

#. 回路を構築します。

    .. image:: img/iot_3_html_led_bb.png

#. 次に、USBケーブルを使用してESP32-WROOM-32Eをコンピュータに接続します。

    .. image:: img/plugin_esp32.png

#. |link_download_this_code|、Arduino IDEに直接コピーします。

    .. note::
        
        * :ref:`unknown_com_port`
 
    .. raw:: html

        <iframe src=https://create.arduino.cc/editor/sunfounder01/a5e33c30-63dc-4987-94c3-89bc6a599e24/preview?embed style="height:510px;width:100%;margin:10px 0" frameborder=0></iframe>

#. 以下の行を見つけて、 ``SSID`` と ``PASSWORD`` で修正します。

    .. code-block::  Arduino

        // Replace the next variables with your SSID/Password combination
        const char* ssid = "SSID";
        const char* password = "PASSWORD";

#. 正しいボード（ESP32 Dev Module）とポートを選択した後、 **Upload** ボタンをクリックします。

#. シリアルモニタにWiFi接続成功メッセージと割り当てられたIPアドレスが表示されます。

    .. code-block:: 

        WiFi connected
        Camera Stream Ready! Go to: http://192.168.18.77

#. ウェブブラウザにIPアドレスを入力します。カスタマイズされたONおよびOFFボタンを使用してLEDを制御できるウェブページが表示されます。

    .. image:: img/sp230510_180503.png 

#. 拡張ボードにバッテリーを挿入し、USBケーブルを取り外します。これで、デバイスをWi-Fi範囲内の任意の場所に配置できます。

    .. image:: img/plugin_battery.png
