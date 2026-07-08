 .. note::

    Hello, welcome to the SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasts Community on Facebook! Dive deeper into Raspberry Pi, Arduino, and ESP32 with fellow enthusiasts.

    **Why Join?**

    - **Expert Support**: Solve post-sale issues and technical challenges with help from our community and team.
    - **Learn & Share**: Exchange tips and tutorials to enhance your skills.
    - **Exclusive Previews**: Get early access to new product announcements and sneak peeks.
    - **Special Discounts**: Enjoy exclusive discounts on our newest products.
    - **Festive Promotions and Giveaways**: Take part in giveaways and holiday promotions.

    👉 Ready to explore and create with us? Click [|link_sf_facebook|] and join today!

.. _ar_motor:

2.6 Drive a Motor
===========================

In this engaging project, we'll explore how to drive a motor using the L293D.

The L293D is a versatile integrated circuit (IC) commonly used for motor control in electronics and robotics projects. It can drive two motors in both forward and reverse directions, making it a popular choice for applications requiring precise motor control.

By the end of this captivating project, you will have gained a thorough understanding of how digital signals and PWM signals can effectively be utilized to control motors. This invaluable knowledge will prove to be a solid foundation for your future endeavors in robotics and mechatronics. Buckle up and get ready to dive into the exciting world of motor control with the L293D!

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
    *   - DC Motor
        - |link_motor_buy|
    *   - L293D Motor Driver
        - \-

**Available Pins**

Here is a list of available pins on the ESP32 board for this project.

.. list-table::
    :widths: 5 20 

    * - Available Pins
      - IO13, IO14, IO27, IO26, IO25, IO33, IO32, IO15, IO2, IO0, IO4, IO5, IO18, IO19, IO21, IO22, IO23


**Schematic**

.. image:: img/circuit_4.1_motor_l293d.png


    
**Wiring**

.. note:: 

  Since the motor requires a relatively high current, it is necessary to first insert the battery and then slide the switch on the expansion board to the ON position to activate the battery supply. 

.. image:: img/4.1_motor_l293d_bb.png



**Code**

#. |link_download_this_code| or copy this code to the Arduino IDE directly.

.. note::

    * :ref:`unknown_com_port`
    
    
.. code-block:: arduino

    #define motor1A 13
    #define motor2A 14

    // the setup function runs once when you press reset or power the board
    void setup() {
      // initialize digital pin as an output.
      pinMode(motor1A, OUTPUT);
      pinMode(motor2A, OUTPUT);
    }

    // the loop function runs over and over again forever
    void loop() {

      // Rotate
      digitalWrite(motor1A, HIGH);
      digitalWrite(motor2A, LOW);
      delay(2000);

      // Rotate in the opposite direction
      digitalWrite(motor1A, LOW);
      digitalWrite(motor2A, HIGH);
      delay(2000);

      // Stop
      digitalWrite(motor1A, LOW);
      digitalWrite(motor2A, LOW);
      delay(3000);
    }



Once the code is successfully uploaded, you will observe the motor rotating clockwise for one second, then counter-clockwise for one second, followed by a two-second pause. This sequence of actions will continue in an endless loop.

