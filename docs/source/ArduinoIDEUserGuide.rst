Arduino IDE User Guide
======================

Arduino IDE is a simple and easy-to-use open-source development environment that supports a variety of microcontrollers, including the ESP32. Here, you can unleash your creativity and write your own programs.

1. Install Arduino IDE
----------------------

A. Click here to visit the `Arduino IDE <https://www.arduino.cc/en/software/>`_ website and download the installation package suitable for your computer version. This example will demonstrate using Windows.

.. image:: _static/arduino/1.install.png
   :width: 1000
   :align: center

----
B. Run the installer by double-clicking the downloaded ``arduino-ide_xxxx.exe`` file.  

C. Read and accept the **License Agreement**.  

.. image:: _static/arduino/2.install.png
   :width: 800
   :align: center

----

D. Select the desired installation options.

.. image:: _static/arduino/3.install.png
   :width: 800
   :align: center

----

E. Choose the installation path. It is recommended to install the software on a non-system drive.

.. image:: _static/arduino/4.install.png
   :width: 800
   :align: center

----

F. Click Install and wait for the process to complete. Finally, click Finish.

.. image:: _static/arduino/5.install.png
   :width: 800
   :align: center

----

2. Add Support For ESP32 Development Board
------------------------------------------

A. Open the Arduino IDE, click **File** → **Preferences** in the upper left corner, and copy and paste the following address into the Additional Board Manager URLs input box.

.. image:: _static/arduino/6.esp.png
   :width: 800
   :align: center
----

B. After entering the URL, click OK. **https://espressif.github.io/arduino-esp32/package_esp32_index.json**

.. image:: _static/arduino/7.esp.png
   :width: 800
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

.. image:: _static/arduino/8.esp.png
   :width: 800
   :align: center
----

C. After completing this step, close and reopen the Arduino IDE.

----
