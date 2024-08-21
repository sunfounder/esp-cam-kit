
 .. note::

    Hello, welcome to the SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasts Community on Facebook! Dive deeper into Raspberry Pi, Arduino, and ESP32 with fellow enthusiasts.

    **Why Join?**

    - **Expert Support**: Solve post-sale issues and technical challenges with help from our community and team.
    - **Learn & Share**: Exchange tips and tutorials to enhance your skills.
    - **Exclusive Previews**: Get early access to new product announcements and sneak peeks.
    - **Special Discounts**: Enjoy exclusive discounts on our newest products.
    - **Festive Promotions and Giveaways**: Take part in giveaways and holiday promotions.

    👉 Ready to explore and create with us? Click [|link_sf_facebook|] and join today!

.. _iot_html_cam:


2.14 Custom Video Streaming Web Server
========================================

The Custom Video Streaming Web Server project offers an opportunity to learn how to create a web page from scratch and customize it to play video streams. Additionally, you can incorporate interactive buttons, such as ON and OFF, to control the LED's brightness.

By building this project, you will gain hands-on experience in web development, HTML, CSS, and JavaScript. You will learn how to create a responsive web page that can display video streams in real-time. Moreover, you will discover how to integrate interactive buttons to control the LED's state, providing a dynamic user experience.

**Required Components**

In this project, we need the following components. 



.. list-table::
    :widths: 30 20
    :header-rows: 1

    *   - COMPONENT INTRODUCTION
        - PURCHASE LINK

    *   - :ref:`cpn_esp32_wroom_32e`
        - |link_esp32_wroom_32e_buy|
    *   - :ref:`cpn_esp32_camera_extension`
        - \-
    *   - Breadboard
        - |link_breadboard_buy|
    *   - Several Jump Wires
        - |link_wires_buy|
    *   - Resistor
        - |link_resistor_buy|
    *   - LED
        - |link_led_buy|

**How to do?**

#. First plug in the camera.

    .. raw:: html

        <video loop autoplay muted style = "max-width:100%">
            <source src="../_static/video/plugin_camera.mp4" type="video/mp4">
            Your browser does not support the video tag.
        </video>

#. Build the circuit.

    .. image:: img/iot_3_html_led_bb.png

#. Then, connect ESP32-WROOM-32E to the computer using the USB cable.

    .. image:: img/plugin_esp32.png

#. Open the code.

    .. note::
        
        * :ref:`unknown_com_port`
 
    .. raw:: html

        <iframe src=https://create.arduino.cc/editor/sunfounder01/a5e33c30-63dc-4987-94c3-89bc6a599e24/preview?embed style="height:510px;width:100%;margin:10px 0" frameborder=0></iframe>

#. Locate the following lines and modify them with your ``SSID`` and ``PASSWORD``.

    .. code-block::  Arduino

        // Replace the next variables with your SSID/Password combination
        const char* ssid = "SSID";
        const char* password = "PASSWORD";

#. After selecting the correct board (ESP32 Dev Module) and port, click the **Upload** button.

#. You will see a successful WiFi connection message and the assigned IP address in the Serial Monitor.

    .. code-block:: 

        WiFi connected
        Camera Stream Ready! Go to: http://192.168.18.77

#. Enter the IP address in your web browser. You will be directed to the web page shown below, where you can use the customized ON and OFF buttons to control the LED.

    .. image:: img/sp230510_180503.png 

#. Insert a battery into the expansion board and remove the USB cable. Now you can place the device anywhere you desire within the Wi-Fi range.

    .. image:: img/plugin_battery.png
