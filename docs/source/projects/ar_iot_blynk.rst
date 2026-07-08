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
    :width: 400

2. Blynkの設定
----------------------

**2.1 Blynkの初期化**

1. |link_blynk| ページにアクセスし、 **Sign Up FREE** または **Enterprise Solution** を選択します。

    .. image:: img/09_blynk_access.png

2. 登録プロセスを開始するために、メールアドレスを入力します。

    .. image:: img/09_blynk_sign_in.png

3. メールを確認し、メール内の **Create Password** リンクをクリックしてパスワードを設定します。

    .. image:: img/09_blynk_password.png

4. 確認後、 **Blynk Tour** が始まり、Blynkの主要な機能について簡単に学ぶことができます。

    .. image:: img/09_blynk_tour.png

5. Blynk Tourを完了すると、Blueprintsを探索するか、Quick Startをクリックしてデバイスを迅速に接続するかを選択できるウィンドウが表示されます。ただし、今回は「Have a look around first」を選択します。

    .. image:: img/09_blynk_skip.png

**2.2 テンプレートの作成**

1. Blynkでテンプレートを作成することから始めます。 **Intrusion Alert System** テンプレートを設定する手順に従います。

    .. image:: img/09_create_template_1_shadow.png
        :width: 600

2. テンプレートに名前を付け、 **ESP32** をハードウェアとして選択し、 **WiFi** を **接続タイプ** として選択し、 **Done** をクリックします。

   .. image:: img/09_create_template_2_shadow.png
        :width: 600

3. テンプレートに入り、次のステップが表示されます。 **Configure template** をクリックしてカバー画像をアップロードし、説明を強化します。残りの3つのステップに従ってセットアップを完了します。

    .. image:: img/09_blynk_temp_steps.png
        :width: 600

**2.3 データストリームの設定**

1. 新しく作成されたテンプレートを開き、データストリーム設定ページに移動します。

   .. image:: img/09_blynk_new_datastream.png
        :width: 600

2. **New Datastream** をクリックし、ポップアップで **Virtual Pin** を選択します。

   .. image:: img/09_blynk_datastream_virtual.png
        :width: 600

3. **Virtual Pin V0** を **AwayMode** と名付け、 **データタイプ** を **Integer** に設定し、 **MIN** および **MAX** の値を **0** および **1** に設定します。

   .. image:: img/09_create_template_shadow.png
        :width: 600

4. 同様に、 **Virtual Pin** をもう一つ作成し、 **Current Status** と名付け、 **データタイプ** を **String** に設定します。

   .. image:: img/09_datastream_1_shadow.png
        :width: 600

**2.4 ウェブダッシュボードの設定**

1. **Switch widget** と **Label widget** の両方を **ウェブダッシュボード** にドラッグ＆ドロップします。

   .. image:: img/09_web_dashboard_1_shadow.png
        :width: 600

2. ウィジェットの上にカーソルを置くと、3つのアイコンが表示されます。 **設定** アイコンを使用してウィジェットのプロパティを構成します。

   .. image:: img/09_blynk_dashboard_set.png
        :width: 600

3. **Switch widget** を **AwayMode(V0)** データストリームにリンクするように設定し、 **ONLABEL** と **OFFLABEL** をそれぞれ **"away home"** と **"at home"** に設定します。

   .. image:: img/09_web_dashboard_2_shadow.png
        :width: 600

4. **Label widget**の設定で、 **Current Status(V1)** データストリームにリンクします。

   .. image:: img/09_web_dashboard_3_shadow.png
        :width: 600

**2.5 イベントの設定**

1. **Events & Notifications** をクリックし、次に **Create Event** をクリックします。

   .. image:: img/09_blynk_event_add.png
        :width: 600

2. イベントに名前を付け、そのコードを指定します。**タイプ** に **Warning** を選択し、通知メールの簡単な説明を提供します。通知頻度を希望に応じて調整します。

   .. note::

      **イベントコード** が ``intrusion_detected`` に設定されていることを確認してください。ここでの変更は、対応するコードの調整が必要です。

   .. image:: img/09_event_1_shadow.png
        :width: 600

3. **Notifications** セクションに移動して通知を有効にし、メール設定を構成します。

   .. image:: img/09_event_2_shadow.png
        :width: 600

4. **Settings** で、イベントが通知をトリガーする頻度を定義し、希望に応じて間隔を設定します。設定を保存するために **Create** をクリックすることを忘れないでください。

   .. image:: img/09_event_3_shadow.png
        :width: 600

**2.6 テンプレートの保存**

1. テンプレートの変更を保存することを忘れないでください。

   .. image:: img/09_save_template_shadow.png
        :width: 600

**2.7 デバイスの作成**

1. テンプレートから新しいデバイスを作成する時が来ました。

   .. image:: img/09_blynk_device_new.png
        :width: 600

2. **From template** を選択して開始します。

   .. image:: img/09_blynk_device_template.png
        :width: 600

3. **Intrusion Alert System** テンプレートを選択し、 **Create** をクリックします。

   .. image:: img/09_blynk_device_template2.png
        :width: 600

4. ESP32との統合のために **Template ID**、 **Device Name**、および **AuthToken** をメモします。

   .. image:: img/09_blynk_device_code.png
        :width: 600

**3. コードの実行**
-----------------------------

#. コードを実行する前に、Arduino IDEの **ライブラリマネージャ** から ``Blynk`` ライブラリをインストールしてください。

    .. image:: img/09_blynk_add_library.png
        :width: 700

#. |link_download_this_code|、Arduino IDEに直接コピーします。

   .. code-block:: arduino

        /* Comment this out to disable prints and save space */
        #define BLYNK_PRINT Serial

        #define BLYNK_TEMPLATE_ID "TMPxxxxxxx"
        #define BLYNK_TEMPLATE_NAME "Intrusion Alert System"
        #define BLYNK_AUTH_TOKEN "xxxxxxxxxxxxxxxx"

        #include <WiFi.h>
        #include <WiFiClient.h>
        #include <BlynkSimpleEsp32.h>


        // Define WiFi credentials
        char ssid[] = "your_ssid";
        char pass[] = "your_password";

        // Define the PIR sensor pin and related variables
        const int sensorPin = 14;
        int state = 0;
        int awayHomeMode = 0;

        // Create Blynk Timer object
        BlynkTimer timer;

        void setup() {

          pinMode(sensorPin, INPUT);  // Set PIR sensor pin as input
          Serial.begin(115200);           // Start serial communication at 115200 baud rate for debugging

          // Configure Blynk and connect to WiFi
          Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass);

          timer.setInterval(1000L, myTimerEvent);  // Setup a function to be called every second
        }

        void loop() {
          Blynk.run();  // Run Blynk
          timer.run();  // Run BlynkTimer
        }

        // This function is called every time the device is connected to the Blynk.Cloud
        BLYNK_CONNECTED() {
          Blynk.syncVirtual(V0);
        }

        // This function is called every time the Virtual Pin 0 state changes
        BLYNK_WRITE(V0) {
          awayHomeMode = param.asInt();  // Set incoming value from pin V0 to a variable

          if (awayHomeMode == 1) {
            Serial.println("The switch on Blynk has been turned on.");
            Blynk.virtualWrite(V1, "Detecting signs of intrusion...");
          } else {
            Serial.println("The switch on Blynk has been turned off.");
            Blynk.virtualWrite(V1, "Away home mode close");
          }
        }

        void myTimerEvent() {
          // Please don't send more that 10 values per second.
          sendData();  // Call function to send sensor data to Blynk app
        }

        // Function to send sensor data to Blynk app
        void sendData() {
          if (awayHomeMode == 1) {
            state = digitalRead(sensorPin);  // Read the state of the PIR sensor

            Serial.print("state:");
            Serial.println(state);

            // If the sensor detects movement, send an alert to the Blynk app
            if (state == HIGH) {
              Serial.println("Somebody here!");
              Blynk.virtualWrite(V1, "Somebody in your house! Please check!");
              Blynk.logEvent("intrusion_detected");
            }
          }
        }

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

#. 成功の接続後、Blynkでスイッチを有効にすると、PIRモジュールの監視が開始されます。動きが検知されると（状態が1になると）、"Somebody here!"と表示され、メールにアラートが送信されます。

    .. image:: img/09_blynk_code_alarm.png
        :width: 700

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