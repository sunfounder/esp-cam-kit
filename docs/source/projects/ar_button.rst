 .. note::

    Hello, welcome to the SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasts Community on Facebook! Dive deeper into Raspberry Pi, Arduino, and ESP32 with fellow enthusiasts.

    **Why Join?**

    - **Expert Support**: Solve post-sale issues and technical challenges with help from our community and team.
    - **Learn & Share**: Exchange tips and tutorials to enhance your skills.
    - **Exclusive Previews**: Get early access to new product announcements and sneak peeks.
    - **Special Discounts**: Enjoy exclusive discounts on our newest products.
    - **Festive Promotions and Giveaways**: Take part in giveaways and holiday promotions.

    👉 Ready to explore and create with us? Click [|link_sf_facebook|] and join today!

.. _ar_button:

2.3 Digital Input
=========================

In this interactive project, we explore the use of digital input through button controls to manipulate an LED.The operation is simple but powerful. We will monitor the state of a button; when pressed, it will register a high voltage level, known as a 'high state'. This change in state will act as a trigger for an LED to illuminate.By learning to read this digital input, you will gain a fundamental understanding of how microcontrollers can interact with external devices. This project not only introduces basic electronic concepts but also sets the stage for more complex control systems involving multiple inputs and outputs.

**Available Pins**

* **Available Pins**

    Here is a list of available pins on the ESP32 board for this project.

    .. list-table::
        :widths: 5 20

        *   - For Input
            - IO14, IO25, I35, I34, I39, I36, IO18, IO19, IO21, IO22, IO23
        *   - For Output
            - IO13, IO12, IO14, IO27, IO26, IO25, IO33, IO32, IO15, IO2, IO0, IO4, IO5, IO18, IO19, IO21, IO22, IO23
    
* **Conditional Usage Pins (Input)**

    The following pins have built-in pull-up or pull-down resistors, so external resistors are not required when **using them as input pins**:


    .. list-table::
        :widths: 5 15
        :header-rows: 1

        *   - Conditional Usage Pins
            - Description
        *   - IO13, IO15, IO2, IO4
            - Pulling up with a 47K resistor defaults the value to high.
        *   - IO27, IO26, IO33
            - Pulling up with a 4.7K resistor defaults the value to high.
        *   - IO32
            - Pulling down with a 1K resistor defaults the value to low.

* **Strapping Pins (Input)**

    Strapping pins are a special set of pins that are used to determine specific boot modes during device startup 
    (i.e., power-on reset).
     
    .. list-table::
        :widths: 5 15

        *   - Strapping Pins
            - IO5, IO0, IO2, IO12, IO15 
    
    Generally, it is **not recommended to use them as input pins**. If you wish to use these pins, consider the potential impact on the booting process. For more details, please refer to the :ref:`esp32_strapping` section.


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
    *   - Button
        - |link_button_buy|


**Schematic**

.. image:: img/circuit_5.1_button.png

To ensure proper functionality, connect one side of the button pin to 3.3V and the other side to IO14. When the button is pressed, IO14 will be set to high, causing the LED to light up. When the button is released, IO14 will return to its suspended state, which may be either high or low. To ensure a stable low level when the button is not pressed, IO14 should be connected to GND through a 10K pull-down resistor.

**Wiring**

.. image:: img/5.1_button_bb.png

.. note::
    
    A four-pin button is designed in an H shape. When the button is not pressed, the left and right pins are disconnected, and current cannot flow between them. However, when the button is pressed, the left and right pins are connected, creating a pathway for current to flow.

**Code**

Download this code or copy this code to the Arduino IDE directly.
    
.. note::
    
    * :ref:`unknown_com_port`
 
.. raw:: html

    <iframe src=https://create.arduino.cc/editor/sunfounder01/702c5a70-78e7-4a8b-a0c7-10c0acebfc12/preview?embed style="height:510px;width:100%;margin:10px 0" frameborder=0></iframe>


Remember to Set the serial communication baud rate to 115200.

Once the code is uploaded successfully, the LED lights up when you press the button and goes off when you release it.

At the same time you can open the Serial Monitor in the upper right corner to observe the value of the button, when the button is pressed, "1" will be printed, otherwise "0" will be printed.

.. image:: img/button_serial.png


**How it works**

The previous projects all involved outputting signals, either in the form of digital or PWM signals.

This project involves receiving input signals from external component to the ESP32 board. You can view the input signal through the Serial Monitor in Arduino IDE.


#. In the ``setup()`` function, the button pin is initialized as an ``input`` and the LED pin is initialized as an ``output``. The Serial communication is also initiated with a baud rate of 115200.

    .. code-block:: arduino

        void setup() {
            Serial.begin(115200);
            // initialize the button pin as an input
            pinMode(buttonPin, INPUT);
            // initialize the LED pin as an output
            pinMode(ledPin, OUTPUT);
        }
    
    * ``Serial.begin(speed)``: Sets the data rate in bits per second (baud) for serial data transmission.

        * ``speed``: in bits per second (baud). Allowed data types: ``long``.

#. In the ``loop()`` function, the state of the button is read and stored in the variable ``buttonState``. The value of ``buttonState`` is printed to the Serial Monitor using ``Serial.println()``.

    .. code-block:: arduino

        void loop() {
            // read the state of the button value
            buttonState = digitalRead(buttonPin);
            Serial.println(buttonState);
            delay(100);
            // if the button is pressed, the buttonState is HIGH
            if (buttonState == HIGH) {
                // turn LED on
                digitalWrite(ledPin, HIGH);

            } else {
                // turn LED off
                digitalWrite(ledPin, LOW);
            }
        }

    If the button is pressed and the ``buttonState`` is HIGH, the LED is turned on by setting the ``ledPin`` to ``HIGH``. Else, turn the LED off.

    * ``int digitalRead(uint8_t pin);``: To read the state of a given pin configured as INPUT, the function digitalRead is used. This function will return the logical state of the selected pin as ``HIGH`` or ``LOW``.

        * ``pin`` select GPIO

    * ``Serial.println()``: Prints data to the serial port as human-readable ASCII text followed by a carriage return character (ASCII 13, or '\r') and a newline character (ASCII 10, or '\n').









