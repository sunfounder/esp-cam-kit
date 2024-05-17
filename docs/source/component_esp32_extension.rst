.. _cpn_esp32_camera_extension:

ESP32 Camera Extension
=======================

We have designed an expansion board that enables you to fully utilize the camera and SD card functionalities of the ESP32 WROOM 32E. By combining the OV2640 camera, Micro SD, and ESP32 WROOM 32E, you get an all-in-one expansion board.

The board provides two types of GPIO headers - one with female headers, perfect for quick prototyping projects. The other type features screw terminals, ensuring stable wire connections and making it suitable for IoT projects.

Additionally, you can power your project using a single 3.7V 18650 battery. If the battery runs low, you can conveniently charge it by simply plugging in a 5V Micro USB cable. This makes it a great tool for outdoor projects and remote applications.

.. image:: img/esp32_camera_extension.jpg
    :width: 600
    :align: center

Interface Introduction
----------------------

.. image:: img/esp32_camera_extension_pinout.jpg
    :width: 800
    :align: center

* **Power Switch**
    * Controls the battery's power supply, toggling it on and off.

* **Charging Port**
    * Upon connecting a 5V Micro USB cable, the battery can be charged.

* **Battery Port**
    * Features a PH2.0-2P interface, compatible with 3.7V 18650 lithium batterry.
    * Provides power to both the ESP32 WROOM 32E and ESP32 Camera Extension.

* **ESP32 Pin Headers**
    * Intended for the ESP32 WROOM 32E module. Pay close attention to its orientation; ensure both Micro USB ports face the same side to avoid incorrect placement.

* **GPIO Headers**
    * **Female Headers**: Used to connect various components to the ESP32, perfect for quick prototyping projects.
    * **Screw Terminal**: 3.5mm pitch 14pin screw terminal, ensuring stable wire connections and making it suitable for IoT projects.

* **Indicator Lights**
    * **PWR**: Lights up when the battery is powered or when a Micro USB is directly plugged into the ESP32.
    * **CHG**: Illuminates upon connecting a Micro USB to the board's charging port, signifying charging onset. It will turn off once the battery is fully charged.

* **Micro SD Connector**
    * Spring-loaded slot for the easy insertion and ejection of Micro SD card.

* **24-pin 0.5mm FFC / FPC connector**
    * Designed for the OV2640 camera, making it suitable for various vision-related projects.


ESP32 Camera Extension Pinout
--------------------------------

The ESP32 WROOM 32E's pinout diagram can be found in :ref:`esp32_pinout`. 

However, when the ESP32 WROOM 32E is inserted into the extension board, some of its pins may also be used to drive the Micro SD card or a camera. 

Consequently, pull-up or pull-down resistors have be added to these pins. If you're using these pins as inputs, it's crucial to account for these resistors as they can affect input levels.

Here's the pinout table for the right-side pins:

    .. image:: img/esp32_extension_pinout1.jpg
        :width: 100%
        :align: center

Here's the pinout table for the left-side pins:

    .. image:: img/esp32_extension_pinout2.jpg
        :width: 100%
        :align: center

    .. note::

        There are some specific constraints:

        * **IO33** is connected to a 4.7K pull-up resistor and a filtering capacitor, which prevents it from driving the WS2812 RGB Strip.

Interface Insertion Guide
-------------------------------

**Upload Code**

    When you need to upload code to the ESP32 WROOM 32E, connect it to your computer using a Micro USB cable.

    .. image:: img/plugin_esp32.png
        :width: 600
        :align: center

**Inserting the Micro SD Card**

    Gently push in the Micro SD card to secure it in place. Pushing it again will eject it.

    .. image:: img/insert_sd.png
        :width: 600
        :align: center

**Attaching the Camera**

    When connecting the camera, ensure the black stripe of the FPC cable is facing upwards and is fully inserted 
    into the connector.

    .. raw:: html

        <video loop autoplay muted style = "max-width:100%">
            <source src="_static/video/plugin_camera.mp4" type="video/mp4">
            Your browser does not support the video tag.
        </video>

**Battery Power and Charging**

    Carefully insert the battery cable into the battery port, avoiding applying too much force to prevent pushing up the battery terminal. If the terminal is pushed up, it's okay as long as the pins are not broken; you can simply press it back into position.

    .. image:: img/plugin_battery.png
        :width: 500
        :align: center

    If the battery is drained, plug in a 5V Micro USB to charge it.

    .. image:: img/battery_charge.png
        :width: 500
        :align: center
