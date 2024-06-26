 .. note::

    こんにちは、SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasts Community on Facebookへようこそ！Raspberry Pi、Arduino、ESP32についての探求を仲間と共に深めましょう。

    **なぜ参加するのか？**

    - **専門サポート**: 私たちのコミュニティとチームの助けを借りて、購入後の問題や技術的な課題を解決しましょう。
    - **学びと共有**: スキルを向上させるためのヒントやチュートリアルを交換しましょう。
    - **独占プレビュー**: 新製品の発表やプレビューに早期アクセスできます。
    - **特別割引**: 最新製品の限定割引を楽しめます。
    - **フェスティブプロモーションとギブアウェイ**: ギブアウェイやホリデープロモーションに参加しましょう。

    👉 一緒に探求し、創造する準備はできましたか？[|link_sf_facebook|]をクリックして今日参加しましょう！

.. _iot_camera_web:

2.13 カメラウェブサーバー
=============================

このプロジェクトでは、ESP32ボードとカメラモジュールを組み合わせて、高品質のビデオをローカルネットワーク上でストリーミングします。手軽にカメラシステムをセットアップし、リアルタイムで任意の場所を監視しましょう。

プロジェクトのウェブインターフェイスを使用すれば、ネットワークに接続された任意のデバイスからカメラフィードにアクセスして制御できます。ユーザーフレンドリーなインターフェイスで、ストリーミング体験を最適化し、設定を簡単に調整できます。

この多用途なESP32カメラストリーミングプロジェクトを使用して、監視やライブストリーミングの機能を強化しましょう。家庭、オフィス、または任意の場所を簡単かつ信頼性を持って監視できます。

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

**手順**

#. まずカメラを接続します。

    .. raw:: html

        <video loop autoplay muted style = "max-width:100%">
            <source src="../_static/video/plugin_camera.mp4" type="video/mp4">
            Your browser does not support the video tag.
        </video>

#. 次に、USBケーブルを使用してESP32-WROOM-32Eをコンピュータに接続します。

    .. image:: img/plugin_esp32.png

#. このコードをダウンロードするか、Arduino IDEに直接コピーします。

    .. note::

        * :ref:`unknown_com_port`

    .. raw:: html

        <iframe src=https://create.arduino.cc/editor/sunfounder01/15e00b39-34e1-49f9-b039-f10053d31407/preview?embed style="height:510px;width:100%;margin:10px 0" frameborder=0></iframe>
        

#. 以下の行を見つけて、 ``<SSID>`` と ``<PASSWORD>`` で修正します。

    .. code-block::  Arduino

        // Replace the next variables with your SSID/Password combination
        const char* ssid = "<SSID>";
        const char* password = "<PASSWORD>";

#. 次に、 **PSRAM** を有効にします。

    .. image:: img/sp230516_150554.png

#. パーティションスキームを **Huge APP (3MB No OTA/1MB SPIFFS)** に設定します。

    .. image:: img/sp230516_150840.png

#. 正しいボード（ESP32 Dev Module）とポートを選択した後、"Upload" ボタンをクリックします。

#. シリアルモニタにWiFi接続成功メッセージと割り当てられたIPアドレスが表示されます。

    .. code-block::

        .....
        WiFi connected
        Starting web server on port: '80'
        Starting stream server on port: '81'
        Camera Ready! Use 'http://192.168.18.77' to connect

#. ウェブブラウザにIPアドレスを入力します。ウェブインターフェイスが表示され、 **Start Stream** をクリックしてカメラフィードを表示できます。

    .. image:: img/sp230516_151521.png

#. ページの上部にスクロールすると、ライブカメラフィードが表示されます。インターフェイスの左側で設定を調整できます。

    .. image:: img/sp230516_180520.png

.. note:: 

    * このESP32モジュールは顔検出をサポートしています。有効にするには、解像度を240x240に設定し、インターフェイスの下部にある顔検出オプションを切り替えます。
    * このESP32モジュールは顔認識をサポートしていません。
