.. note::

    こんにちは、SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasts CommunityのFacebookページへようこそ！Raspberry Pi、Arduino、ESP32の愛好家と共にさらに深く探求しましょう。

    **なぜ参加するのか？**

    - **専門サポート**: コミュニティとチームの助けを借りて、購入後の問題や技術的な課題を解決します。
    - **学びと共有**: スキルを向上させるためのヒントやチュートリアルを交換します。
    - **限定プレビュー**: 新製品の発表や先行情報を早期に入手できます。
    - **特別割引**: 最新製品に対する特別割引をお楽しみいただけます。
    - **フェスティブプロモーションとプレゼント**: プレゼントやホリデープロモーションに参加できます。

    👉 私たちと一緒に探求し、創造する準備はできましたか？[|link_sf_facebook|]をクリックして、今日参加しましょう！

.. _ar_mp3_player_sd:

2.11 MP3プレーヤーとSDカードサポート
==============================================

ESP32で音楽の世界へようこそ！このプロジェクトは、オーディオ処理の力を手元にもたらし、ESP32を計算のための素晴らしいマイクロコントローラーだけでなく、個人用の音楽プレーヤーにも変えます。部屋に入ると、この小さなデバイスからお気に入りの曲が流れることを想像してください。今日はその力を手に入れましょう。

**必要なコンポーネント**

このプロジェクトでは、以下のコンポーネントが必要です。

.. list-table::
    :widths: 30 20
    :header-rows: 1

    *   - コンポーネントの紹介
        - 購入リンク

    *   - :ref:`cpn_esp32_wroom_32e`
        - |link_esp32_wroom_32e_buy|
    *   - :ref:`cpn_esp32_camera_extension`
        - \-
    *   - ブレッドボード
        - |link_breadboard_buy|
    *   - いくつかのジャンプワイヤー
        - |link_wires_buy|
    *   - 抵抗
        - |link_resistor_buy|
    *   - スピーカー
        - \-

**操作手順**

#. カードリーダーを使用してSDカードをコンピュータに挿入し、フォーマットします。チュートリアルは :ref:`format_sd_card` を参照してください。

#. お気に入りのMP3ファイルをSDカードにコピーします。

    .. image:: img/mp3_music.png

#. SDカードを拡張ボードのSDカードスロットに挿入します。

    .. image:: img/insert_sd.png

#. 回路を組み立てます。

    このモノアンプでは、IO25をオーディオアンプモジュールのLまたはRピンに接続できます。

    10K抵抗は高周波ノイズを減少させ、音量を下げるために使用されます。これは、DACとオーディオアンプの寄生容量とともにRCローパスフィルタを形成します。このフィルタは高周波信号の振幅を減少させ、高周波ノイズを効果的に減少させます。したがって、10K抵抗を追加することで音楽が柔らかくなり、不要な高周波ノイズが除去されます。

    SDカードの音楽がすでに柔らかい場合は、抵抗を取り除くか、より小さな値に置き換えることができます。

    .. image:: img/7.3_bluetooth_audio_player_bb.png

#. ESP32-WROOM-32EをUSBケーブルでコンピュータに接続します。

    .. image:: img/plugin_esp32.png

#. このコードをダウンロードするか、直接Arduino IDEにコピーします。

    コード行 ``file = new AudioFileSourceSD_MMC("/To Alice.mp3")`` を変更して、ファイル名とパスを反映させます。

    .. note::

        * :ref:`unknown_com_port`
        * ``ESP8266Audio`` ライブラリを使用しています。インストール方法は :ref:`install_lib_man` を参照してください。
        * :download:`ESP8266Audio </_static/zip/ESP8266Audio.zip>`



    .. warning::

        ESP32開発ボードのバージョン3.0.0以上を使用している場合、コンパイルプロセス中にエラーが発生することがあります。
        この問題は、ボードの新しいバージョンが「ESP8266Audio」ライブラリをサポートしなくなったためです。
        この例を正しく実行するには、ESP32ボードのファームウェアバージョンを2.0.17にダウングレードすることをお勧めします。
        この例を完了した後、最新バージョンに再度アップグレードしてください。

        .. image:: ../faq/img/version_2.0.17.png

    .. raw:: html

        <iframe src=https://create.arduino.cc/editor/sunfounder01/13f5c757-9622-4735-aa1a-fdbe6fc46273/preview?embed style="height:510px;width:100%;margin:10px 0" frameborder=0></iframe>
        
#. Arduino IDEで適切なポートとボードを選択し、コードをESP32にアップロードします。

#. コードが正常にアップロードされた後、お気に入りの音楽が再生されます。


**動作の仕組み**

* コードは ``ESP8266Audio`` ライブラリのいくつかのクラスを使用して、SDカードからI2Sを通してMP3ファイルを再生します：

    .. code-block:: arduino

        #include "AudioFileSourceSD_MMC.h"
        #include "AudioOutputI2S.h"
        #include "AudioGeneratorMP3.h"
        #include "SD_MMC.h"
        #include "FS.h"

    * ``AudioGeneratorMP3`` はMP3オーディオをデコードするクラスです。
    * ``AudioFileSourceSD_MMC`` はSDカードからオーディオデータを読み取るクラスです。
    * ``AudioOutputI2S`` はオーディオデータをI2Sインターフェースに送信するクラスです。

* ``setup()`` 関数では、SDカードを初期化し、SDカードからMP3ファイルを開き、ESP32の内部DACにI2S出力を設定し、出力をモノラルに設定し、MP3ジェネレータを開始します。

    .. code-block:: arduino

        void setup() {
            // シリアル通信を開始します。
            Serial.begin(115200);
            delay(1000);

            // SDカードを初期化します。失敗した場合はエラーメッセージを表示します。
            if (!SD_MMC.begin()) {
                Serial.println("SD card mount failed!");
            }

            // SDカードからMP3ファイルを開きます。"/To Alice.mp3"を自分のMP3ファイル名に置き換えてください。
            file = new AudioFileSourceSD_MMC("/To Alice.mp3");
            
            // ESP32の内部DACにI2S出力を設定します。
            out = new AudioOutputI2S(0, 1);
            
            // 出力をモノラルに設定します。
            out->SetOutputModeMono(true);

            // ファイルと出力でMP3ジェネレータを初期化します。
            mp3 = new AudioGeneratorMP3();
            mp3->begin(file, out);
        }


* ``loop()`` 関数では、MP3ジェネレータが動作しているかどうかをチェックします。動作していればループを続け、そうでなければ停止し、シリアルモニタに「MP3 done」と表示します。

    .. code-block:: arduino

        void loop() {
            // MP3が動作している場合はループします。そうでない場合は停止します。
            if (mp3->isRunning()) {
                if (!mp3->loop()) mp3->stop();
            } 
            // MP3が動作していない場合は、メッセージを表示し、1秒待機します。
            else {
                Serial.println("MP3完了");
                delay(1000);
            }
        }


