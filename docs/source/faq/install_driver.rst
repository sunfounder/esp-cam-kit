 .. note::

    こんにちは、SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasts Community on Facebookへようこそ！Raspberry Pi、Arduino、ESP32の愛好者と共に、これらの技術を深く掘り下げましょう。

    **参加する理由**

    - **専門的なサポート**: コミュニティやチームの助けを借りて、販売後の問題や技術的な課題を解決します。
    - **学びと共有**: スキルを向上させるためのヒントやチュートリアルを交換します。
    - **独占プレビュー**: 新製品の発表や先行プレビューに早期アクセスできます。
    - **特別割引**: 最新製品の特別割引を楽しめます。
    - **フェスティバルプロモーションとプレゼント**: プレゼントやホリデープロモーションに参加できます。

    👉 私たちと一緒に探求し、創造する準備はできていますか？[|link_sf_facebook|]をクリックして、今すぐ参加してください！

.. _install_driver:

ESP32 用ドライバーの手動インストール
========================================

ESP32 ボードを USB でコンピューターに接続しても Arduino IDE や Thonny IDE にポートが表示されない（または **COM1** のみが表示される）場合は、ボードが認識されていない可能性があります。  
この場合は、USB ドライバーを手動でインストールする必要があります。

当社の ESP32 ボードには 2 種類あり、USB-シリアル変換チップが異なります：

- **CP2102**
- **CH340**

機能的には同じですが、必要な USB ドライバーが異なります。

.. image:: img/driver_ch340_cp2102.jpg
   :width: 400
   :align: center

* ESP32 ボードに **CH340** チップが搭載されている場合は、以下のガイドに従ってドライバーをインストールしてください：

  * :ref:`driver_ch340`

* ESP32 ボードに **CP2102** チップが搭載されている場合は、以下のガイドに従ってください：

  * :ref:`driver_cp2102`


.. _driver_ch340:

CH340 ドライバーのインストール方法
----------------------------------------

このチュートリアルでは、さまざまなオペレーティングシステムで CH340 ドライバーをインストールする手順を説明します。多くの場合、ドライバーは自動的にインストールされますが、システムのバージョンや更新状況によっては、最初の接続時に手動インストールが必要な場合があります。

ドライバー
^^^^^^^^^^^^

CH340 チップは WCH 社が製造しています。以下は WCH の公式サイトから各システム向けに提供されているドライバーのリンクです：

* `Windows (ZIP) <https://www.wch.cn/download/file?id=5>`_ -- バージョン v3.4（2024-10-16）
* `Windows (EXE) <https://www.wch.cn/download/file?id=65>`_ -- 実行形式インストーラー
* `Mac (ZIP) <https://www.wch.cn/download/file?id=178>`_ -- バージョン v1.5（2025-02-26）
* `Linux (ZIP) <https://www.wch.cn/download/file?id=177>`_ -- バージョン v1.5（2024-10-24）

最新ドライバーは WCH の中国語サイトにもあります：

* `WCH ドライバー ダウンロード <https://www.wch.cn/downloads/CH343SER_EXE.html>`_

Google Chrome を使っている場合、自動翻訳を利用できます。

次に、各システムごとのインストール手順を紹介します。

Windows 7/11
^^^^^^^^^^^^^^^^^^^^^

#. ドライバーをダウンロードします：

   * `Windows (ZIP) <https://www.wch.cn/download/file?id=5>`_
   * `Windows (EXE) <https://www.wch.cn/download/file?id=65>`_

#. `.exe` ファイルをダブルクリックします。ZIP をダウンロードした場合は解凍してから `.exe` を開いてください。

#. 最初に「Uninstall」をクリックして古いドライバーを削除し、その後「Install」をクリックしてインストールします。

   .. image:: img/driver_ch340_install.png

#. インストール完了後、「デバイスマネージャー」を開きます（スタートボタン → 「デバイスマネージャー」と入力）。

   .. image:: img/driver_ch340_device.png

#. 「ポート (COMとLPT)」を展開し、**USB-SERIAL CH340 (COM##)** のような表示があるか確認します。COM 番号は環境により異なります。

   .. image:: img/driver_ch340_com.png

macOS
^^^^^^^^^^^^

#. ドライバーパッケージをダウンロードして解凍します：

   * `Mac (ZIP) <https://www.wch.cn/download/file?id=178>`_

#. 解凍フォルダ内の `.pkg` ファイルをダブルクリックしてインストールを開始します。

   .. note::

      「システム機能拡張がブロックされました」や「未確認の開発元」のエラーが表示された場合は、**システム設定 > プライバシーとセキュリティ** に移動して、WCH 拡張機能を「許可」してください。  
      設定を変更するにはロック解除（パスワード入力）が必要です。変更後は再起動してください。

   .. image:: img/driver_ch340_install_mac.png
      :width: 500
      :align: center

#. インストール後、ターミナルを開き、以下のコマンドで認識されているか確認します：

   .. code-block::

      ls /dev/cu*

#. `/dev/cu.usbserial*****` のようなデバイス名が表示されれば、ドライバーは正常に動作しています。

   .. image:: img/driver_ch340_mac_port.png
      :width: 500
      :align: center

Linux
^^^^^^^^^^^

#. 多くの Linux ディストリビューションでは、CH340 ドライバーが標準で含まれています。通常は接続するだけで認識されます。

#. 認識されない場合は、以下のコマンドでシステムを更新してください：

   .. code-block::

      sudo apt-get update
      sudo apt-get upgrade

#. または、手動で CH340 ドライバーをインストールすることもできます：

   * `Linux (ZIP) <https://www.wch.cn/download/file?id=177>`_

#. ESP32 ボードを再接続し、ターミナルで次のコマンドを実行します：

   .. code-block::

      ls /dev/ttyUSB*

#. `/dev/ttyUSB0` のようなデバイスが表示されれば、ドライバーは正しく機能しています。

.. _driver_cp2102:

CH2102 ドライバーのインストール方法
-----------------------------------

このガイドでは、CH2102 USB-シリアルドライバーを各オペレーティングシステムにインストールする手順を説明します。  
多くの場合、ドライバーは OS によって自動的にインストールされますが、システムのバージョンや構成によっては、CH2102 デバイスを初めて接続する際に手動インストールが必要となる場合があります。

Windows
^^^^^^^^^^^^^

#. `Silicon Labs USB to UART Bridge VCP Drivers <https://www.silabs.com/developers/usb-to-uart-bridge-vcp-drivers?tab=downloads>`_ のページにアクセスし、**CP210x_Universal_Windows_Driver** をダウンロードします。

#. ZIP ファイルを解凍し、`.inf` ファイルを右クリックして **インストール** を選択します。画面の指示に従ってインストールを完了してください。

   .. image:: img/driver_cp2102_install.png

#. インストール後、CP2102 デバイスを USB ポートに接続します。**デバイスマネージャー** を開くには、⊞ Win + R を押し、「``devmgmt.msc``」と入力して Enter キーを押します。

#. **ポート (COMとLPT)** セクションを展開します。``Silicon Labs CP210x USB to UART Bridge (COM#)`` のような項目が表示されるはずです。

   .. image:: img/driver_cp2102_com.png

#. 警告アイコンが表示されていなければ、ドライバーは正しくインストールされ、正常に動作しています。

macOS
^^^^^^^^^^^^

CP2102 USB-to-UART ブリッジは Silicon Labs によって製造されています。最新の macOS バージョンでは基本的なサポートが含まれていることもありますが、完全な互換性と安定性のために公式ドライバーのインストールを推奨します。

#. `USB to UART Bridge VCP Drivers <https://www.silabs.com/developer-tools/usb-to-uart-bridge-vcp-drivers?tab=downloads>`_ のページから、お使いのシステム（Apple Silicon または Intel）に対応する **CP210x VCP Mac OSX Driver** をダウンロードします。

#. ダウンロードした ZIP ファイルを解凍し、同梱されている ``.dmg`` ファイルをダブルクリックしてマウントします。

#. マウントされたボリューム内の **Install CP210x VCP Driver.app** を開いて実行します。

#. 画面の指示に従ってインストールを完了させます。

   .. image:: img/driver_cp2102_mac_install.png
      :width: 500

#. macOS 10.13 以降では、ドライバーの拡張がブロックされる場合があります。以下の手順を行ってください：

   * **システム設定 > プライバシーとセキュリティ** に移動
   * Silicon Labs 拡張機能の横にある **許可** をクリック
   * 必要に応じてロックを解除（鍵アイコンをクリックしてパスワード入力）
   * Mac を再起動して変更を適用

#. インストール後、まだ行っていなければ **Mac を再起動** してください。

#. ドライバーが正しくインストールされたかを確認するには、ターミナルを開いて以下のコマンドを実行します：

   .. code-block::

      ls /dev/cu.*

#. 以下のようなデバイスが表示されれば、正常に動作しています：

   .. code-block::

      /dev/cu.SLAB_USBtoUART

Linux
^^^^^^^^^^^^^

#. ほとんどの Linux ディストリビューション（Ubuntu、Debian、Fedora など）は、CP2102 ドライバーのサポートを標準で備えています。通常、デバイスを接続するだけで使用可能です。

#. 認識されない場合は、次のコマンドでシステムを更新してみてください：

   .. code-block::

      sudo apt-get update
      sudo apt-get upgrade

#. 更新後、CP2102 デバイスを再接続し、以下のコマンドをターミナルで実行します：

   .. code-block::

      ls /dev/ttyUSB*

#. ドライバーが正常に動作していれば、次のような出力が得られます：

   .. code-block::

      /dev/ttyUSB0

#. また、カーネルログを確認して認識を確認することもできます：

   .. code-block::

      dmesg | grep ttyUSB
