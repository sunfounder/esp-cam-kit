 .. note::

    こんにちは、SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasts Communityへようこそ！FacebookでRaspberry Pi、Arduino、ESP32をさらに深く探求しましょう。

    **参加する理由**

    - **専門サポート**: コミュニティとチームの助けを借りて、販売後の問題や技術的な課題を解決。
    - **学びと共有**: スキルを向上させるためのヒントやチュートリアルを交換。
    - **独占プレビュー**: 新製品の発表や先行情報に早くアクセス。
    - **特別割引**: 最新製品の特別割引を享受。
    - **フェスティブプロモーションとプレゼント**: プレゼントや休日のプロモーションに参加。

    👉 一緒に探求して創造する準備はできましたか？クリックして[|link_sf_facebook|]今日参加してください！

.. _iot_intrusion_alert_system:

2.15 Blynkを使った侵入通知システム
==================================================

このプロジェクトでは、PIRモーションセンサー（HC-SR501）を使用したシンプルな家庭用侵入検知システムを紹介します。
Blynkアプリを介してシステムが「外出モード」に設定されると、PIRセンサーが動きを監視します。
検知された動きはBlynkアプリで通知をトリガーし、ユーザーに侵入の可能性を警告します。

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
    *   - ジャンパーワイヤー
        - |link_wires_buy|
    *   - PIRモーションセンサーモジュール
        - |link_pir_buy|

1. 回路の組み立て
--------------------

.. image:: img/iot_9_blynk_bb.png
    :width: 60%
    :align: center

2. Blynkの設定
----------------------

**2.1 Blynkの初期化**

#. |link_blynk| にアクセスし、 **START FREE** を選択します。

   .. image:: img/09_blynk_access.png
        :width: 90%

#. メールアドレスを入力して登録プロセスを開始します。

   .. image:: img/09_blynk_sign_in.png
        :width: 70%
        :align: center

#. メールで登録を確認します。

    .. image:: img/09_blynk_password.png
        :width: 90%

#. 確認後、 **Blynk Tour** が表示されます。「スキップ」を選択することをお勧めします。 **Quick Start** が表示された場合もスキップを検討してください。
   
    .. image:: img/09_blynk_tour.png
        :width: 90%

**2.2 テンプレートの作成**

#. まず、Blynkでテンプレートを作成します。以下の手順に従って **Intrusion Alert System** テンプレートを作成します。

    .. image:: img/09_create_template_1_shadow.png
        :width: 700
        :align: center

#. テンプレートに名前を付け、 **ESP32** ハードウェアを選択し、 **Connection Type** を **WiFi** に設定し、 **Done** を選択します。

    .. image:: img/09_create_template_2_shadow.png
        :width: 700
        :align: center

**2.3 データストリームの生成**

設定したテンプレートを開き、2つのデータストリームを作成します。

#. **New Datastream** をクリックします。

    .. image:: img/09_blynk_new_datastream.png
        :width: 700
        :align: center

#. ポップアップで **Virtual Pin** を選択します。

    .. image:: img/09_blynk_datastream_virtual.png
        :width: 700
        :align: center

#. **Virtual Pin V0**に **AwayMode** という名前を付けます。 **DATA TYPE** を **Integer** に設定し、 **MIN** および **MAX** 値をそれぞれ **0** および **1** に設定します。

    .. image:: img/09_create_template_shadow.png
        :width: 700
        :align: center

#. 同様に、もう1つの **Virtual Pin** データストリームを作成します。これに **Current Status** という名前を付け、 **DATA TYPE** を **String** に設定します。

    .. image:: img/09_datastream_1_shadow.png
        :width: 700
        :align: center

**2.4 イベントの設定**

次に、侵入が検出された場合にメール通知を送信するイベントを設定します。

#. **Add New Event** をクリックします。

    .. image:: img/09_blynk_event_add.png

#. イベントの名前と特定のコードを定義します。 **TYPE** には **Warning** を選択し、イベントが発生したときに送信されるメールの簡単な説明を書きます。通知の頻度も調整できます。

    .. note::
        
        **EVENT CODE** が ``intrusion_detected`` に設定されていることを確認してください。これはコード内で事前に定義されているため、変更する場合はコードも調整する必要があります。

    .. image:: img/09_event_1_shadow.png
        :width: 700
        :align: center

#. **Notifications** セクションに移動して通知をオンにし、メールの詳細を設定します。

    .. image:: img/09_event_2_shadow.png
        :width: 80%
        :align: center

.. raw:: html
    
    <br/> 

**2.5 Webダッシュボードの微調整**

Webダッシュボードが侵入警報システムと完璧に連動するようにすることが重要です。

#. **Webダッシュボード** に **スイッチウィジェット** と **ラベルウィジェット** をドラッグして配置します。

    .. image:: img/09_web_dashboard_1_shadow.png
        :width: 100%
        :align: center

#. ウィジェットにカーソルを合わせると、3つのアイコンが表示されます。設定アイコンを使用してウィジェットのプロパティを調整します。

    .. image:: img/09_blynk_dashboard_set.png
        :width: 100%
        :align: center

#. **スイッチウィジェット** の設定で、 **Datastream** を **AwayMode(V0)** に選択します。 **ONLABEL** と **OFFLABEL** にはそれぞれ **away** と **home** を表示するように設定します。

    .. image:: img/09_web_dashboard_2_shadow.png
        :width: 100%
        :align: center

#. **ラベルウィジェット** の設定で、 **Datastream** を **Current Status(V1)** に選択します。

    .. image:: img/09_web_dashboard_3_shadow.png
        :width: 100%
        :align: center

**2.6 テンプレートの保存**

最後に、テンプレートを保存することを忘れないでください。

    .. image:: img/09_save_template_shadow.png
        :width: 100%
        :align: center

**2.7 デバイスの作成**

#. 新しいデバイスを作成する時です。

    .. image:: img/09_blynk_device_new.png
        :width: 700
        :align: center

#. **テンプレートから** をクリックして新しいセットアップを開始します。

    .. image:: img/09_blynk_device_template.png
        :width: 700
        :align: center

#. 次に、 **Intrusion Alert System** テンプレートを選択し、 **作成** をクリックします。

    .. image:: img/09_blynk_device_template2.png
        :width: 700
        :align: center

#. ここで、 ``Template ID`` 、 ``Device Name`` 、および ``AuthToken`` が表示されます。これらをコードにコピーして、ESP32がBlynkと連携できるようにします。

    .. image:: img/09_blynk_device_code.png
        :width: 700
        :align: center

**3. コードの実行**
-----------------------------

#. コードを実行する前に、Arduino IDEの **ライブラリマネージャ** から ``Blynk`` ライブラリをインストールしてください。

    .. image:: img/09_blynk_add_library.png
        :width: 700
        :align: center

#. このコードをダウンロードするか、Arduino IDEに直接コピーします。

    .. raw:: html

        <iframe src=https://create.arduino.cc/editor/sunfounder01/16bca228-64d7-4519-ac3b-833afecfcc65/preview?embed style="height:510px;width:100%;margin:10px 0" frameborder=0></iframe>

#. ``BLYNK_TEMPLATE_ID``、 ``BLYNK_TEMPLATE_NAME`` 、および ``BLYNK_AUTH_TOKEN`` のプレースホルダーを自分のIDに置き換えます。

    .. code-block:: arduino
    
        #define BLYNK_TEMPLATE_ID "TMPxxxxxxx"
        #define BLYNK_TEMPLATE_NAME "Intrusion Alert System"
        #define BLYNK_AUTH_TOKEN "xxxxxxxxxxxxx"

#. WiFiネットワークの ``ssid`` と ``password`` も入力してください。

   .. code-block:: arduino

        char ssid[] = "your_ssid";
        char pass[] = "your_password";

#. 正しいボード（ **ESP32 Dev Module** ）とポートを選択し、 **アップロード** ボタンをクリックします。

#. シリアルモニタを開き（ボーレートを115200に設定）、成功の接続メッセージを待ちます。

    .. image:: img/09_blynk_upload_code.png
        :align: center

#. 成功の接続後、Blynkでスイッチを有効にすると、PIRモジュールの監視が開始されます。動きが検知されると（状態が1になると）、"Somebody here!"と表示され、メールにアラートが送信されます。

    .. image:: img/09_blynk_code_alarm.png
        :width: 700
        :align: center

4. コードの説明
-----------------------------

#. **設定とライブラリ**

   ここでは、Blynkの定数と認証情報を設定します。また、ESP32とBlynkに必要なライブラリを含めます。

    .. code-block:: arduino

        /* 印刷を無効にしてスペースを節約するには、これをコメントアウトします */
        #define BLYNK_PRINT Serial

        #define BLYNK_TEMPLATE_ID "xxxxxxxxxxx"
        #define BLYNK_TEMPLATE_NAME "Intrusion Alert System"
        #define BLYNK_AUTH_TOKEN "xxxxxxxxxxxxxxxxxxxxxxxxxxx"

        #include <WiFi.h>
        #include <WiFiClient.h>
        #include <BlynkSimpleEsp32.h>
#. **WiFi 設定**

   WiFi の認証情報を入力します。

   .. code-block:: arduino

        char ssid[] = "your_ssid";
        char pass[] = "your_password";

#. **PIR センサー設定**

   PIR センサーが接続されているピンを設定し、状態変数を初期化します。

   .. code-block:: arduino

      const int sensorPin = 14;
      int state = 0;
      int awayHomeMode = 0;
      BlynkTimer timer;

#. **setup() 関数**

   この関数は PIR センサーを入力として初期化し、シリアル通信を設定し、WiFi に接続し、Blynk を構成します。

   - ``timer.setInterval(1000L, myTimerEvent)`` を使用して ``setup()`` 内でタイマーの間隔を設定します。ここでは ``myTimerEvent()`` 関数を **1000ms** 毎に実行するように設定しています。 ``timer.setInterval(1000L, myTimerEvent)`` の最初のパラメーターを変更することで、 ``myTimerEvent`` 実行の間隔を変更できます。

   .. raw:: html
    
    <br/> 

   .. code-block:: arduino

        void setup() {

            pinMode(sensorPin, INPUT);  // Set PIR sensor pin as input
            Serial.begin(115200);           // Start serial communication at 115200 baud rate for debugging
            
            // Configure Blynk and connect to WiFi
            Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass);
            
            timer.setInterval(1000L, myTimerEvent);  // Setup a function to be called every second
        }

#. **loop() 関数**

   loop 関数は Blynk および Blynk タイマー関数を継続的に実行します。

   .. code-block:: arduino

        void loop() {
           Blynk.run();
           timer.run();
        }

#. **Blynk アプリとの連携**

   これらの関数は、デバイスが Blynk に接続されたとき、または Blynk アプリ上の仮想ピン V0 の状態が変更されたときに呼び出されます。

   - デバイスが Blynk サーバーに接続されるたび、またはネットワーク状態が悪いときに再接続されるたびに、 ``BLYNK_CONNECTED()`` 関数が呼び出されます。 ``Blynk.syncVirtual()`` コマンドは単一の仮想ピン値を要求します。指定された仮想ピンは ``BLYNK_WRITE()`` を実行します。

   - Blynk サーバー上の仮想ピンの値が変更されるたびに、 ``BLYNK_WRITE()``  がトリガーされます。

   .. raw:: html
    
    <br/> 

   .. code-block:: arduino
      
        // この関数はデバイスが Blynk.Cloud に接続されるたびに呼び出されます
        BLYNK_CONNECTED() {
            Blynk.syncVirtual(V0);
        }
      
        // この関数は仮想ピン 0 の状態が変更されるたびに呼び出されます
        BLYNK_WRITE(V0) {
            awayHomeMode = param.asInt();
            // 追加のロジック
        }

#. **データ処理**

   毎秒、 ``myTimerEvent()`` 関数が ``sendData()`` を呼び出します。Blynk で外出モードが有効になっている場合、PIR センサーをチェックし、動きが検知されると Blynk に通知を送信します。

   - ``Blynk.virtualWrite(V1, "Somebody in your house! Please check!");`` を使用してラベルのテキストを変更します。

   - ``Blynk.logEvent("intrusion_detected");`` を使用して Blynk にイベントをログします。

   .. raw:: html
    
    <br/> 

   .. code-block:: arduino

        void myTimerEvent() {
           sendData();
        }

        void sendData() {
           if (awayHomeMode == 1) {
              state = digitalRead(sensorPin);  // PIR センサーの状態を読み取る

              Serial.print("state:");
              Serial.println(state);

              // センサーが動きを検知した場合、Blynk アプリにアラートを送信する
              if (state == HIGH) {
                Serial.println("Somebody here!");
                Blynk.virtualWrite(V1, "Somebody in your house! Please check!");
                Blynk.logEvent("intrusion_detected");
              }
           }
        }

**参考**

- |link_blynk_doc|
- |link_blynk_quickstart| 
- |link_blynk_virtualWrite|
- |link_blynk_logEvent|
- |link_blynk_timer_intro|
- |link_blynk_syncing| 
- |link_blynk_write|