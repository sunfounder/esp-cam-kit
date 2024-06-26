 .. note::

    こんにちは、SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasts Community on Facebookへようこそ！Raspberry Pi、Arduino、ESP32の愛好者と共に、これらの技術を深く掘り下げましょう。

    **参加する理由**

    - **専門的なサポート**: コミュニティやチームの助けを借りて、販売後の問題や技術的な課題を解決します。
    - **学びと共有**: スキルを向上させるためのヒントやチュートリアルを交換します。
    - **独占プレビュー**: 新製品の発表や先行プレビューに早期アクセスできます。
    - **特別割引**: 最新製品の特別割引を楽しめます。
    - **フェスティバルプロモーションとプレゼント**: プレゼントやホリデープロモーションに参加できます。

    👉 私たちと一緒に探求し、創造する準備はできていますか？[|link_sf_facebook|]をクリックして、今すぐ参加してください！

.. _unknown_com_port:

「Unknown COMxx」と表示される場合
-------------------------------------------

ESP32をコンピュータに接続すると、Arduino IDEはしばしば ``Unknown COMxx`` と表示します。なぜこのようなことが起こるのでしょうか？

.. image:: img/unknown_device.png

これは、ESP32用のUSBドライバが通常のArduinoボードとは異なるためです。Arduino IDEはこのボードを自動的に認識できません。

このような場合、以下の手順に従って正しいボードを手動で選択する必要があります：

#. **"Select the other board and port"** をクリックします。

    .. image:: img/unknown_select.png

#. 検索欄に **"esp32 dev module"** と入力し、表示されたボードを選択します。その後、正しいポートを選択して **OK** をクリックします。

    .. image:: img/unknown_board.png

#. これで、クイックビューウィンドウにボードとポートが表示されるようになります。

    .. image:: img/unknown_correct.png
