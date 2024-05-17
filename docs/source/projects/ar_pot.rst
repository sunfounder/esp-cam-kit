.. _ar_potentiometer:

2.4 Analog Input
==========================

This lesson explores the use of a potentiometer as an analog input device to adjust the brightness of an LED. By simply turning the knob of the potentiometer, you can vary the light intensity of the LED, similar to the way you might adjust the brightness of a desk lamp. This straightforward setup demonstrates the direct impact of analog input on real-world applications, offering an intuitive understanding of how changes in input can control electronic components.


**Available Pins**

* **Available Pins**

    Here is a list of available pins on the ESP32 board for this project.

    .. list-table::
        :widths: 5 15

        *   - Available Pins
            - IO14, IO25, I35, I34, I39, I36

* **Strapping Pins**

    The following pins are strapping pins, which affect the startup process of the ESP32 during power on or reset. However, once the ESP32 is booted up successfully, they can be used as regular pins.

    .. list-table::
        :widths: 5 15

        *   - Strapping Pins
            - IO0, IO12


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
    *   - Potentiometer
        - |link_potentiometer_buy|



**Schematic**

.. image:: img/circuit_5.8_potentiometer.png

When you rotate the potentiometer, the value of I35 will change. By programming, you can use the value of I35 to control the brightness of the LED. Therefore, as you rotate the potentiometer, the brightness of the LED will also change accordingly.


**Wiring**

.. image:: img/5.8_potentiometer_bb.png

**Code**

Download this code or copy this code to the Arduino IDE directly.

.. note::

    * :ref:`unknown_com_port`
   
.. raw:: html
     
    <iframe src=https://create.arduino.cc/editor/sunfounder01/aadce2e7-fd5d-4608-a557-f1e4d07ba795/preview?embed style="height:510px;width:100%;margin:10px 0" frameborder=0></iframe>

After the code is uploaded successfully, rotate the potentiometer and you will see the brightness of the LED change accordingly. At the same time you can see the analog and voltage values of the potentiometer in the serial monitor.


**How it works?**

#. Define constants for pin connections and PWM settings.

    .. code-block:: arduino

        const int potPin = 14; // Potentiometer connected to GPIO14
        const int ledPin = 26; // LED connected to GPIO26

        // PWM settings
        const int freq = 5000; // PWM frequency
        const int resolution = 12; // PWM resolution (bits)
        const int channel = 0; // PWM channel

    Here the PWM resolution is set to 12 bits and the range is 0-4095.

#. Configure the system in the ``setup()`` function.

    .. code-block:: arduino

        void setup() {
            Serial.begin(115200);

            // Configure PWM
            ledcSetup(channel, freq, resolution);
            ledcAttachPin(ledPin, channel);
        }

    * In the ``setup()`` function, the Serial communication is started at a baud rate of 115200. 
    * The ``ledcSetup()`` function is called to set up the PWM channel with the specified frequency and resolution, and the ``ledcAttachPin()`` function is called to associate the specified LED pin with the PWM channel.

#. Main loop (executed repeatedly) in the loop() function.

    .. code-block:: arduino

        void loop() {

            int potValue = analogRead(potPin); // read the value of the potentiometer
            uint32_t voltage_mV = analogReadMilliVolts(potPin); // Read the voltage in millivolts
            
            ledcWrite(channel, potValue);
            
            Serial.print("Potentiometer Value: ");
            Serial.print(potValue);
            Serial.print(", Voltage: ");
            Serial.print(voltage_mV / 1000.0); // Convert millivolts to volts
            Serial.println(" V");
            
            delay(100);
        }

    * ``uint32_t analogReadMilliVolts(uint8_t pin);``: This function is used to get ADC value for a given pin/ADC channel in millivolts.

        * ``pin`` GPIO pin to read analog value.

    The potentiometer value is directly used as the PWM duty cycle for controlling the LED brightness via the ``ledcWrite()`` function, as the range of values is also from 0 to 4095.

