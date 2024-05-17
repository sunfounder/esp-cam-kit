.. _ar_blink:

2.1 Digital Output
=======================================

Among the many microcontroller development boards, the ESP32 stands out for its high performance and versatility. This project will show you how to use the digital output pins of the ESP32 board to control an external device—in this case, lighting up an LED. This serves as a foundation for learning ESP32 programming and an entry point into exploring IoT applications.

**Available Pins**

Here is a list of available pins on the ESP32 board for this project.

.. list-table::
    :widths: 5 20 

    * - Available Pins
      - IO13, IO12, IO14, IO27, IO26, IO25, IO33, IO32, IO15, IO2, IO0, IO4, IO5, IO18, IO19, IO21, IO22, IO23



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

**Schematic**

.. image:: img/circuit_2.1_led.png

This circuit works on a simple principle, and the current direction is shown in the figure. The LED will light up after the 220ohm current limiting resistor when pin26 outputs high level. The LED will turn off when pin26 outputs low level.

**Wiring**

.. image:: img/2.1_hello_led_bb.png


**Upload Code**

#. Download this code or copy this code to the Arduino IDE directly.

    .. note::
        
        * :ref:`unknown_com_port`

    .. raw:: html

        <iframe src=https://create.arduino.cc/editor/sunfounder01/1bff2463-40ad-43c1-8815-9f448bab3735/preview?embed style="height:510px;width:100%;margin:10px 0" frameborder=0></iframe>
    
#. Then connect the ESP32 WROOM 32E to your computer using a Micro USB cable. 

    * :ref:`unknown_com_port`

    .. image:: img/plugin_esp32.png
        :width: 600
        :align: center

#. Select the board (ESP32 Dev Module) and the appropriate port.

    .. image:: img/choose_board.png

#. Now, click the **Upload** button to upload the code to the ESP32 board.
    
    .. image:: img/click_upload.png

#. After the code is uploaded successfully, you will see the LED blinking.

**How it works?**

#. Declare an integer constant named ``ledPin`` and assigns it the value 26. 

    .. code-block:: arduino

        const int ledPin = 26;  // The GPIO pin for the LED


#. Now, initialize the pin in the ``setup()`` function, where you need to initialize the pin to ``OUTPUT`` mode.

    .. code-block:: arduino

        void setup() {
            pinMode(ledPin, OUTPUT);
        }

    * ``void pinMode(uint8_t pin, uint8_t mode);``: This function is used to define the GPIO operation mode for a specific pin.

        * ``pin`` defines the GPIO pin number.
        * ``mode`` sets operation mode.

        The following modes are supported for the basic input and output:

        * ``INPUT`` sets the GPIO as input without pullup or pulldown (high impedance).
        * ``OUTPUT`` sets the GPIO as output/read mode.
        * ``INPUT_PULLDOWN`` sets the GPIO as input with the internal pulldown.
        * ``INPUT_PULLUP`` sets the GPIO as input with the internal pullup.

#. The ``loop()`` function contains the main logic of the program and runs continuously. It alternates between setting the pin high and low, with one-second intervals between the changes.

    .. code-block:: arduino

        void loop() {
            digitalWrite(ledPin, HIGH);   // turn the LED on (HIGH is the voltage level)
            delay(1000);                       // wait for a second
            digitalWrite(ledPin, LOW);    // turn the LED off by making the voltage LOW
            delay(1000);                       // wait for a second
        }

    * ``void digitalWrite(uint8_t pin, uint8_t val);``: This function sets the state of the selected GPIO to ``HIGH`` or ``LOW``. This function is only used if the ``pinMode`` was configured as ``OUTPUT``.
    
        * ``pin`` defines the GPIO pin number.
        * ``val`` set the output digital state to ``HIGH`` or ``LOW``.