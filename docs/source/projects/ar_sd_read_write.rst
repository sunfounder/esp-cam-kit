 .. note::

    こんにちは、SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasts CommunityのFacebookページへようこそ！Raspberry Pi、Arduino、ESP32の愛好者と共にさらに深く探求しましょう。

    **参加する理由**

    - **専門サポート**: コミュニティとチームの助けを借りて、購入後の問題や技術的な課題を解決します。
    - **学びと共有**: スキルを向上させるためのヒントやチュートリアルを交換します。
    - **限定プレビュー**: 新製品の発表や先行情報を早期に入手できます。
    - **特別割引**: 最新製品に対する特別割引をお楽しみいただけます。
    - **フェスティブプロモーションとプレゼント**: プレゼントやホリデープロモーションに参加できます。

    👉 私たちと一緒に探求し、創造する準備はできましたか？[|link_sf_facebook|]をクリックして、今日参加しましょう！

.. _ar_sd_write:

2.10 SDカードの書き込みと読み取り
==================================
このプロジェクトでは、ESP32マイクロコントローラを使用してSDカードを操作する基本機能を紹介します。 
SDカードのマウント、ファイルの作成、データの書き込み、ルートディレクトリ内のファイルリストの表示など、 
多くのデータロギングやストレージアプリケーションの基礎となる操作を示します。このプロジェクトは、ESP32の内蔵SDMMCホストペリフェラルを理解し、活用するための重要なステップです。

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


**操作手順**

#. USBケーブルを接続する前に、SDカードを拡張ボードのSDカードスロットに挿入します。

    .. image:: img/insert_sd.png

#. ESP32-WROOM-32EをUSBケーブルでコンピュータに接続します。

    .. image:: img/plugin_esp32.png

#. |link_download_this_code|、Arduino IDEに直接コピーします。Arduino IDEで適切なポートとボードを選択し、ESP32にコードをアップロードします。

    .. note::

        * :ref:`unknown_com_port`

    .. raw:: html

        <iframe src=https://create.arduino.cc/editor/sunfounder01/912df4b8-a7b6-43dc-95b5-8206801cc9c1/preview?embed style="height:510px;width:100%;margin:10px 0" frameborder=0></iframe>
        

#. コードが正常にアップロードされた後、ファイルの書き込みが成功したことを示すプロンプトと、SDカード内のすべてのファイル名とサイズのリストが表示されます。シリアルモニタを開いた後に何も表示されない場合は、EN（RST）ボタンを押してコードを再実行する必要があります。

    .. image:: img/sd_write_read.png

    .. note::

        次の情報が表示された場合。

        .. code-block::

            E (528) vfs_fat_sdmmc: mount_to_vfs failed (0xffffffff).
            Failed to mount SD card

        まず、SDカードが拡張ボードに正しく挿入されているか確認してください。

        正しく挿入されている場合、SDカードに問題がある可能性があります。消しゴムで金属接点を掃除してみてください。

        問題が解決しない場合は、SDカードをフォーマットすることをお勧めします。詳細は :ref:`format_sd_card` を参照してください。

**仕組みはどうなっていますか？**

このプロジェクトの目的は、ESP32ボードでSDカードを使用する方法を示すことです。ESP32の内蔵SDMMCホストペリフェラルを使用してSDカードに接続します。

プロジェクトはシリアル通信を初期化し、次にSDカードのマウントを試みることから始まります。SDカードのマウントに失敗した場合、プログラムはエラーメッセージを表示し、setup関数を終了します。

SDカードが正常にマウントされると、プログラムはSDカードのルートディレクトリに「test.txt」という名前のファイルを作成します。ファイルが書き込みモードで正常に開かれると、プログラムは「Hello, world!」という行をファイルに書き込みます。書き込み操作が成功した場合、プログラムは成功メッセージを表示し、そうでない場合はエラーメッセージを表示します。

書き込み操作が完了した後、プログラムはファイルを閉じ、次にSDカードのルートディレクトリを開きます。次にルートディレクトリ内のすべてのファイルをループし、見つかった各ファイルのファイル名とファイルサイズを表示します。

メインのloop関数では、操作は行いません。このプロジェクトは、SDカードのマウント、ファイルの作成、ファイルへの書き込み、ファイルディレクトリの読み取りなど、すべての操作がsetup関数内で実行されることに焦点を当てています。

このプロジェクトは、ESP32でSDカードを操作する基本を学ぶための有用な導入となります。データロギングやストレージが必要なアプリケーションにおいて重要です。


以下はコードの解析です。

#. ESP32の内蔵SDMMCホストペリフェラルを使用してSDカードを操作するために必要な ``SD_MMC`` ライブラリをインクルードします。

    .. code-block:: arduino

        #include "SD_MMC.h"

#. ``setup()`` 関数内で、次のタスクが実行されます。

    * **SDカードを初期化します**

    SDカードを初期化し、マウントします。SDカードのマウントに失敗した場合、シリアルモニタに「Failed to mount SD card」と表示し、実行を停止します。

    .. code-block:: arduino
        
        if(!SD_MMC.begin()) { // Attempt to mount the SD card
            Serial.println("Failed to mount card"); // If mount fails, print to serial and exit setup
            return;
        } 
      
    * **ファイルを開きます**

    SDカードのルートディレクトリにある ``"test.txt"`` という名前のファイルを書き込みモードで開きます。ファイルのオープンに失敗した場合、「Failed to open file for writing」と表示して戻ります。

    .. code-block:: arduino

        File file = SD_MMC.open("/test.txt", FILE_WRITE); 
        if (!file) {
            Serial.println("Failed to open file for writing"); // Print error message if file failed to open
            return;
        }

    * **データをファイルに書き込みます**

    ファイルに "Test file write" というテキストを書き込みます。
    書き込み操作が成功した場合、 "File write successful" と表示し、
    失敗した場合は "File write failed" と表示します。

    .. code-block:: arduino

        if(file.print("Test file write")) { // Write the message to the file
            Serial.println("File write success"); // If write succeeds, print to serial
        } else {
            Serial.println("File write failed"); // If write fails, print to serial
        } 

    * **ファイルを閉じます**
        
    開いたファイルを閉じます。これにより、バッファされたデータがファイルに書き込まれ、ファイルが正しく閉じられます。

    .. code-block:: arduino

        file.close(); // Close the file

    * **ルートディレクトリを開きます**

    SDカードのルートディレクトリを開きます。ディレクトリのオープンに失敗した場合、「Failed to open directory」と表示して戻ります。

    .. code-block:: arduino

        File root = SD_MMC.open("/"); // Open the root directory of SD card
        if (!root) {
            Serial.println("Failed to open directory"); // Print error message if directory failed to open
            return;
        }

    * **各ファイルの名前とサイズを表示します**
    
    ``while (File file = root.openNextFile())`` で始まるループは、ルートディレクトリ内のすべてのファイルをループし、
    各ファイルの名前とサイズをシリアルモニタに表示します。

    .. code-block:: arduino
    
        Serial.println("Files found in root directory:"); // Print the list of files found in the root directory
        while (File file = root.openNextFile()) { // Loop through all the files in the root directory
              Serial.print("  ");
              Serial.print(file.name()); // Print the filename
              Serial.print("\t");
              Serial.println(file.size()); // Print the filesize
              file.close(); // Close the file
        }

#. この ``loop()`` 関数は空のループで、現在のプログラムでは何もしません。ただし、通常のArduinoプログラムでは、この関数はコードを継続的にループして実行します。この場合、必要なタスクはすべてsetup関数で実行されたため、loop関数は不要です。

    .. code-block:: arduino

        void loop() {} // 空のループ関数、何もしない
