 .. note::

    こんにちは、SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasts Communityへようこそ！Raspberry Pi、Arduino、ESP32を使って仲間と一緒に深く探求しましょう。

    **参加する理由**

    - **専門家のサポート**：コミュニティやチームの助けを借りて、購入後の問題や技術的な課題を解決しましょう。
    - **学びと共有**：スキルを向上させるためのヒントやチュートリアルを交換しましょう。
    - **限定プレビュー**：新製品の発表やスニークピークへの早期アクセスを取得しましょう。
    - **特別割引**：最新製品の独占割引をお楽しみください。
    - **祭りのプロモーションとプレゼント**：プレゼントや休日のプロモーションに参加しましょう。

    👉 私たちと一緒に探求し、創造する準備はできましたか？[|link_sf_facebook|]をクリックして、今日から参加しましょう！

.. _ar_bluetooth:

2.8 Bluetooth機能の使用
==========================================

このプロジェクトは、ESP32マイクロコントローラを使用して簡単なBluetooth Low Energy（BLE）
シリアル通信アプリケーションを開発するためのガイドを提供します。ESP32は、Wi-FiとBluetooth
接続を統合した強力なマイクロコントローラであり、ワイヤレスアプリケーションの開発に最適です。
BLEは、短距離通信用に設計された低電力のワイヤレス通信プロトコルです。このドキュメントでは、
ESP32をBLEサーバーとして設定し、シリアル接続を介してBLEクライアントと通信する手順を説明します。

**Bluetooth機能について**

ESP32 WROOM 32Eは、Wi-FiとBluetooth接続を単一のチップに統合したモジュールです。このモジュールは、
Bluetooth Low Energy（BLE）およびクラシックBluetoothプロトコルをサポートしています。

このモジュールは、Bluetoothクライアントまたはサーバーとして使用できます。Bluetoothクライアントとして、
他のBluetoothデバイスに接続し、それらとデータを交換することができます。Bluetoothサーバーとして、
他のBluetoothデバイスにサービスを提供することができます。

ESP32 WROOM 32Eは、Generic Access Profile（GAP）、Generic Attribute Profile（GATT）、
Serial Port Profile（SPP）など、さまざまなBluetoothプロファイルをサポートしています。
SPPプロファイルを使用すると、モジュールはBluetoothを介してシリアルポートをエミュレートし、
他のBluetoothデバイスとのシリアル通信を可能にします。

ESP32 WROOM 32EのBluetooth機能を使用するには、適切なソフトウェア開発キット（SDK）を使用してプ
ログラムする必要があります。または、Arduino IDEとESP32 BLEライブラリを使用してプログラムするこ
ともできます。ESP32 BLEライブラリは、BLEを使用するための高レベルのインターフェースを提供し、
モジュールをBLEクライアントおよびサーバーとして使用する方法を示す例を含んでいます。

全体として、ESP32 WROOM 32EのBluetooth機能は、プロジェクトにワイヤレス通信を低電力で簡単に実現
する方法を提供します。

**操作手順**

以下は、LightBlueアプリを使用してESP32とモバイルデバイス間でBluetooth通信を設定するための手順です。

#. iOSの場合は **App Store** 、Androidの場合は **Google Play** からLightBlueアプリをダウンロードします。

    .. image:: img/bluetooth_lightblue.png

#. このコードをダウンロードするか、直接Arduino IDEにコピーしてください。

    .. note::
        
        * :ref:`unknown_com_port`

    .. raw:: html
        
        <iframe src=https://create.arduino.cc/editor/sunfounder01/388f6d9d-65bf-4eaa-b29a-7cebf0b92f74/preview?embed style="height:510px;width:100%;margin:10px 0" frameborder=0></iframe>

#. UUIDの競合を避けるために、|link_uuid|を使用して新しいUUIDを3つランダムに生成し、以下のコード行に入力することをお勧めします。

    .. code-block:: arduino

        #define SERVICE_UUID           "your_service_uuid_here" 
        #define CHARACTERISTIC_UUID_RX "your_rx_characteristic_uuid_here"
        #define CHARACTERISTIC_UUID_TX "your_tx_characteristic_uuid_here"

    .. image:: img/uuid_generate.png

#. 正しいボードとポートを選択し、 **Upload** ボタンをクリックします。

    .. image:: img/bluetooth_upload.png

#. コードが正常にアップロードされたら、モバイルデバイスで **Bluetooth** をオンにし、 **LightBlue** アプリを開きます。

    .. image:: img/bluetooth_open.png

#. **Scan** ページで **ESP32-Bluetooth** を見つけて **CONNECT** をクリックします。表示されない場合は、ページを数回更新してみてください。**"Connected to device!"**が表示されたら、Bluetooth接続は成功です。コードに設定した3つのUUIDを確認するためにスクロールダウンします。

    .. image:: img/bluetooth_connect.png
        :width: 800

#. **Receive** UUIDをクリックします。 **Data Format** の右側のボックスで適切なデータ形式を選択します。例えば、「HEX」は16進数、「UTF-8 String」は文字列、「Binary」はバイナリなどです。次に、 **SUBSCRIBE** をクリックします。

    .. image:: img/bluetooth_read.png
        :width: 300

#. Arduino IDEに戻り、シリアルモニタを開いて、ボーレートを115200に設定し、「welcome」と入力してEnterキーを押します。

    .. image:: img/bluetooth_serial.png

#. LightBlueアプリに「welcome」メッセージが表示されるはずです。

    .. image:: img/bluetooth_welcome.png
        :width: 400

#. モバイルデバイスからシリアルモニタに情報を送信するには、Send UUIDをクリックし、データ形式を「UTF-8 String」に設定し、メッセージを書きます。

    .. image:: img/bluetooth_send.png

#. メッセージがシリアルモニタに表示されるはずです。

    .. image:: img/bluetooth_receive.png

**動作の仕組み**

このArduinoコードはESP32マイクロコントローラー用に書かれており、Bluetooth Low Energy（BLE）デバイスと通信するように設定されています。

以下はコードの概要です：

* **必要なライブラリのインクルード**: コードは、ESP32でBLEを使用するために必要なライブラリをインクルードすることから始まります。

    .. code-block:: arduino

        #include "BLEDevice.h"
        #include "BLEServer.h"
        #include "BLEUtils.h"
        #include "BLE2902.h"

* **グローバル変数**: コードは、Bluetoothデバイス名（ ``bleName`` ）、受信したテキストと最後のメッセージの時間を追跡するための変数、サービスとキャラクタリスティックのUUID、および``BLECharacteristic``オブジェクト（``pCharacteristic``）を定義します。
    
    .. code-block:: arduino

        // Bluetoothデバイス名の定義
        const char *bleName = "ESP32_Bluetooth";

        // 受信したテキストと最後のメッセージの時間の定義
        String receivedText = "";
        unsigned long lastMessageTime = 0;

        // サービスとキャラクタリスティックのUUIDの定義
        #define SERVICE_UUID           "your_service_uuid_here"
        #define CHARACTERISTIC_UUID_RX "your_rx_characteristic_uuid_here"
        #define CHARACTERISTIC_UUID_TX "your_tx_characteristic_uuid_here"

        // Bluetoothキャラクタリスティックの定義
        BLECharacteristic *pCharacteristic;

* **セットアップ**: ``setup()``関数では、シリアルポートが115200のボーレートで初期化され、Bluetooth BLEを設定するために``setupBLE()``関数が呼び出されます。

    .. code-block:: arduino
    
        void setup() {
            Serial.begin(115200);  // シリアルポートの初期化
            setupBLE();            // Bluetooth BLEの初期化
        }

* **メインループ**: ``loop()``関数では、BLEを介して文字列が受信され（``receivedText``が空でない場合）、最後のメッセージから少なくとも1秒が経過している場合、受信した文字列がシリアルモニタに表示され、キャラクタリスティックの値が受信した文字列に設定され、通知が送信され、受信した文字列がクリアされます。シリアルポートにデータがある場合は、改行文字が現れるまで文字列を読み取り、この文字列をキャラクタリスティックの値に設定し、通知を送信します。

    .. code-block:: arduino

        void loop() {
            // 受信したテキストが空でなく、最後のメッセージから1秒以上経過している場合
            // 通知を送信し、受信したテキストを表示する
            if (receivedText.length() > 0 && millis() - lastMessageTime > 1000) {
                Serial.print("Received message: ");
                Serial.println(receivedText);
                pCharacteristic->setValue(receivedText.c_str());
                pCharacteristic->notify();
                receivedText = "";
            }

            // シリアルポートからデータを読み取り、BLEキャラクタリスティックに送信する
            if (Serial.available() > 0) {
                String str = Serial.readStringUntil('\n');
                const char *newValue = str.c_str();
                pCharacteristic->setValue(newValue);
                pCharacteristic->notify();
            }
        }

* **コールバック**: Bluetooth通信に関連するイベントを処理するために、2つのコールバッククラス（``MyServerCallbacks``および``MyCharacteristicCallbacks``）が定義されています。``MyServerCallbacks``は、BLEサーバーの接続状態（接続または切断）に関連するイベントを処理するために使用されます。``MyCharacteristicCallbacks``は、BLEキャラクタリスティックに対する書き込みイベント、つまり接続されたデバイスがBLEを介してESP32に文字列を送信したときにキャプチャされ``receivedText``に保存され、現在の時間が``lastMessageTime``に記録されます。

    .. code-block:: arduino

        // BLEサーバーのコールバックを定義
        class MyServerCallbacks : public BLEServerCallbacks {
            // クライアントが接続されたときに接続メッセージを表示
            void onConnect(BLEServer *pServer) {
                Serial.println("Connected");
            }
            // クライアントが切断されたときに切断メッセージを表示
            void onDisconnect(BLEServer *pServer) {
                Serial.println("Disconnected");
            }
        };

        // BLEキャラクタリスティックのコールバックを定義
        class MyCharacteristicCallbacks : public BLECharacteristicCallbacks {
            void onWrite(BLECharacteristic *pCharacteristic) {
                // データが受信されたときにデータを取得し、receivedTextに保存し、時間を記録
                std::string value = pCharacteristic->getValue();
                receivedText = String(value.c_str());
                lastMessageTime = millis();
                Serial.print("Received: ");
                Serial.println(receivedText);
            }
        };

* **BLEのセットアップ**: ``setupBLE()``関数では、BLEデバイスとサーバーが初期化され、サーバーコールバックが設定され、定義されたUUIDを使用してBLEサービスが作成され、通知を送信するためのキャラクタリスティックとデータを受信するためのキャラクタリスティックが作成されてサービスに追加されます。最後に、サービスが開始され、サーバーがアドバタイズを開始します。

    .. code-block:: arduino

        // Initialize the Bluetooth BLE
        void setupBLE() {
            BLEDevice::init(bleName);                        // Initialize the BLE device
            BLEServer *pServer = BLEDevice::createServer();  // Create the BLE server
            // Print the error message if the BLE server creation fails
            if (pServer == nullptr) {
                Serial.println("Error creating BLE server");
                return;
            }
            pServer->setCallbacks(new MyServerCallbacks());  // Set the BLE server callbacks

            // Create the BLE service
            BLEService *pService = pServer->createService(SERVICE_UUID);
            // Print the error message if the BLE service creation fails
            if (pService == nullptr) {
                Serial.println("Error creating BLE service");
                return;
            }
            // Create the BLE characteristic for sending notifications
            pCharacteristic = pService->createCharacteristic(CHARACTERISTIC_UUID_TX, BLECharacteristic::PROPERTY_NOTIFY);
            pCharacteristic->addDecodeor(new BLE2902());  // Add the decodeor
            // Create the BLE characteristic for receiving data
            BLECharacteristic *pCharacteristicRX = pService->createCharacteristic(CHARACTERISTIC_UUID_RX, BLECharacteristic::PROPERTY_WRITE);
            pCharacteristicRX->setCallbacks(new MyCharacteristicCallbacks());  // Set the BLE characteristic callbacks
            pService->start();                                                 // Start the BLE service
            pServer->getAdvertising()->start();                                // Start advertising
            Serial.println("Waiting for a client connection...");              // Wait for a client connection
        }


このコードは双方向通信を可能にします - BLEを介してデータを送受信できます。しかし、特定のハードウェア
（例：LEDのオン/オフ切り替え）と連携するためには、受信した文字列を処理して適切に動作させるための追加
のコードが必要です。




