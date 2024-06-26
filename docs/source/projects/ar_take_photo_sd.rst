.. note::

    こんにちは、SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasts Communityへようこそ！Facebookで一緒にRaspberry Pi、Arduino、ESP32の世界を深く探求しましょう。

    **参加する理由**

    - **専門サポート**: コミュニティとチームの助けを借りて、販売後の問題や技術的な課題を解決しましょう。
    - **学び＆共有**: スキルを向上させるためのヒントやチュートリアルを交換しましょう。
    - **限定プレビュー**: 新製品の発表や予告編をいち早く入手しましょう。
    - **特別割引**: 最新製品の限定割引をお楽しみください。
    - **フェスティブプロモーションとプレゼント**: プレゼントやホリデープロモーションに参加しましょう。

    👉 私たちと一緒に探求し、創造する準備はできましたか？[|link_sf_facebook|]をクリックして、今すぐ参加しましょう！

.. _ar_take_photo_sd:

2.12 SDカードへの写真撮影
============================

このドキュメントでは、ESP32-CAMを使用して写真を撮影し、SDカードに保存するプロジェクトについて説明します。
このプロジェクトの目的は、ESP32-CAMを使用して画像をキャプチャし、SDカードに保存する簡単なソリューションを提供することです。

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


**関連する注意事項**

ESP32-CAMを使用する際は、スケッチをアップロードするためにGPIO 0ピンをGNDに接続する必要があることに注意してください。
また、GPIO 0をGNDに接続した後、ESP32-CAMのオンボードリセットボタンを押してボードをフラッシュモードにする必要があります。
さらに、画像を保存する前にSDカードが正しくマウントされていることを確認することが重要です。



**操作手順**

#. カードリーダーを使用してSDカードをコンピューターに挿入し、フォーマットします。:ref:`format_sd_card` のチュートリアルを参照できます。

#. 次に、カードリーダーを取り外し、SDカードを拡張ボードに挿入します。

    .. image:: img/insert_sd.png

#. カメラを接続します。

    .. raw:: html

        <video loop autoplay muted style = "max-width:100%">
            <source src="../_static/video/plugin_camera.mp4" type="video/mp4">
            Your browser does not support the video tag.
        </video>

#. USBケーブルを使用してESP32-WROOM-32Eをコンピューターに接続します。

    .. image:: img/plugin_esp32.png

#. このコードをダウンロードするか、Arduino IDEに直接コピーします。

    .. note::

        * :ref:`unknown_com_port`

    .. raw:: html

        <iframe src=https://create.arduino.cc/editor/sunfounder01/4d698dc3-aef7-4aea-b8a3-7d143a4c7d3c/preview?embed style="height:510px;width:100%;margin:10px 0" frameborder=0></iframe>

#. **PSRAM** を有効にします。

    .. image:: img/sp230516_150554.png

#. パーティションスキームを **Huge APP (3MB No OTA/1MB SPIFFS)** に設定します。

    .. image:: img/sp230516_150840.png   

#. Arduino IDEで適切なポートとボードを選択し、ESP32にコードをアップロードします。

#. コードのアップロードが成功した後、 **リセット** ボタンを押して写真を撮影します。さらに、シリアルモニタで次の情報が表示され、キャプチャが成功したことを確認できます。

    .. code-block:: arduino

        Picture file name: /picture9.jpg
        Saved file to path: /picture9.jpg
        Going to sleep now

    .. image:: img/press_reset.png

#. 拡張ボードからSDカードを取り出し、コンピューターに挿入します。撮影した写真を確認できます。

    .. image:: img/take_photo1.png

**仕組みは？**

このコードは、AI Thinker ESP32-CAMを使って写真を撮影し、SDカードに保存し、その後ESP32-CAMをディープスリープ状態にするものです。以下に主要な部分の説明を示します：

* **ライブラリ**: コードは、ESP32-CAM、ファイルシステム（FS）、SDカード、EEPROM（電源サイクル間でデータを保存するために使用）に必要なライブラリをインクルードすることから始まります。

    .. code-block:: arduino

        #include "esp_camera.h"
        #include "Arduino.h"
        #include "FS.h"                // SD Card ESP32
        #include "SD_MMC.h"            // SD Card ESP32
        #include "soc/soc.h"           // Disable brownour problems
        #include "soc/rtc_cntl_reg.h"  // Disable brownour problems
        #include "driver/rtc_io.h"
        #include <EEPROM.h>  // read and write from flash memory

* **ピンの定義**: このセクションでは、カメラモジュールへのESP32-CAMのピン接続を表す定数を設定します。

    .. code-block:: arduino

        #define PWDN_GPIO_NUM 32
        #define RESET_GPIO_NUM -1
        #define XCLK_GPIO_NUM 0
        #define SIOD_GPIO_NUM 26
        #define SIOC_GPIO_NUM 27

        #define Y9_GPIO_NUM 35
        #define Y8_GPIO_NUM 34
        #define Y7_GPIO_NUM 39
        #define Y6_GPIO_NUM 36
        #define Y5_GPIO_NUM 21
        #define Y4_GPIO_NUM 19
        #define Y3_GPIO_NUM 18
        #define Y2_GPIO_NUM 5
        #define VSYNC_GPIO_NUM 25
        #define HREF_GPIO_NUM 23
        #define PCLK_GPIO_NUM 22

* **グローバル変数** : SDカードに保存される写真の数を追跡するために、グローバル変数``pictureNumber``が宣言されます。

    .. code-block:: arduino

        int pictureNumber = 0;

* **セットアップ関数** : ``setup()`` 関数では、以下のタスクが実行されます：

    * まず、ブラウンアウト検出器を無効にして、カメラが動作しているときの高電流消費時にESP32-CAMがリセットされるのを防ぎます。

        .. code-block:: arduino

            WRITE_PERI_REG(RTC_CNTL_BROWN_OUT_REG, 0);  //disable brownout detector

    * デバッグのためにシリアル通信を初期化します。

        .. code-block:: arduino

            Serial.begin(115200);

    * GPIOピン、XCLK周波数、ピクセルフォーマット、フレームサイズ、JPEG品質、およびフレームバッファ数を含む ``camera_config_t`` でカメラ設定を行います。

        .. code-block:: arduino

            camera_config_t config;
            config.ledc_channel = LEDC_CHANNEL_0;
            config.ledc_timer = LEDC_TIMER_0;
            config.pin_d0 = Y2_GPIO_NUM;
            config.pin_d1 = Y3_GPIO_NUM;
            config.pin_d2 = Y4_GPIO_NUM;
            config.pin_d3 = Y5_GPIO_NUM;
            config.pin_d4 = Y6_GPIO_NUM;
            config.pin_d5 = Y7_GPIO_NUM;
            config.pin_d6 = Y8_GPIO_NUM;
            config.pin_d7 = Y9_GPIO_NUM;
            config.pin_xclk = XCLK_GPIO_NUM;
            config.pin_pclk = PCLK_GPIO_NUM;
            config.pin_vsync = VSYNC_GPIO_NUM;
            config.pin_href = HREF_GPIO_NUM;
            config.pin_sscb_sda = SIOD_GPIO_NUM;
            config.pin_sscb_scl = SIOC_GPIO_NUM;
            config.pin_pwdn = PWDN_GPIO_NUM;
            config.pin_reset = RESET_GPIO_NUM;
            config.xclk_freq_hz = 20000000;
            config.pixel_format = PIXFORMAT_JPEG;

    * 設定を使用してカメラを初期化し、失敗した場合はエラーメッセージを表示します。

        .. code-block:: arduino

            esp_err_t err = esp_camera_init(&config);
            if (err != ESP_OK) {
                Serial.printf("Camera init failed with error 0x%x", err);
                return;
            }

    * SDカードを初期化し、失敗した場合はエラーメッセージを表示します。

        .. code-block:: arduino

            if (!SD_MMC.begin()) {
                Serial.println("SD Card Mount Failed");
                return;
            }   

            uint8_t cardType = SD_MMC.cardType();
            if (cardType == CARD_NONE) {
                Serial.println("No SD Card attached");
                return;
            }        

    * カメラで写真を撮影し、フレームバッファに保存します。

        .. code-block:: arduino

            fb = esp_camera_fb_get();
            if (!fb) {
                Serial.println("Camera capture failed");
                return;
            }

    * EEPROMを読み込み、最後の写真の番号を取得し、新しい写真のための番号をインクリメントします。

        .. code-block:: arduino

            EEPROM.begin(EEPROM_SIZE);
            pictureNumber = EEPROM.read(0) + 1;

    * SDカード上に新しい写真のパスを作成し、写真番号に対応するファイル名を付けます。

        .. code-block:: arduino

            String path = "/picture" + String(pictureNumber) + ".jpg";

            fs::FS &fs = SD_MMC;
            Serial.printf("Picture file name: %s\n", path.c_str());

    * 写真を保存した後、写真番号をEEPROMに再度保存し、次の電源サイクルで取り出せるようにします。

        .. code-block:: arduino

            File file = fs.open(path.c_str(), FILE_WRITE);
            if (!file) {
                Serial.println("Failed to open file in writing mode");
            } else {
                file.write(fb->buf, fb->len);  // payload (image), payload length
                Serial.printf("Saved file to path: %s\n", path.c_str());
                EEPROM.write(0, pictureNumber);
                EEPROM.commit();
            }
            file.close();
            esp_camera_fb_return(fb); 

    * 最後に、オンボードLED（フラッシュ）をオフにし、ESP32-CAMをディープスリープ状態にします。

        .. code-block:: arduino

            pinMode(4, OUTPUT);
            digitalWrite(4, LOW);
            rtc_gpio_hold_en(GPIO_NUM_4);

    * スリープモード: ESP32-CAMは各写真撮影後にディープスリープに入ります。これにより電力を節約します。リセットや特定のピンの信号で再起動できます。

        .. code-block:: arduino

            delay(2000);
            Serial.println("Going to sleep now");
            delay(2000);
            esp_deep_sleep_start();
            Serial.println("This will never be printed");

* ループ関数: ``loop()`` 関数は空のままであり、セットアッププロセスの後、ESP32-CAMはすぐにディープスリープに入ります。

このコードを機能させるためには、スケッチをアップロードする際にGPIO 0をGNDに接続し、オンボードリセットボタンを押してボードをフラッシュモードにする必要があることに注意してください。また、"/picture"を自身のファイル名に置き換えることを忘れないでください。EEPROMのサイズは1に設定されており、0から255までの値を保存できます。255枚以上の写真を撮影する予定がある場合、EEPROMのサイズを増やし、写真番号の保存および読み取り方法を調整する必要があります。
