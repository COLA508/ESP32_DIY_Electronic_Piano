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

2. Add ESP32 Board Manager
--------------------------

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

3. Add Libraries
----------------

Arduino libraries can significantly simplify the development process.

They encapsulate commonly used functions and hardware driver code, allowing users to simply call ready-made functions without writing complex low-level code from scratch.

A wealth of community-provided third-party libraries also allows for quick integration with various sensors and modules.

These library functions make it easy to interact with hardware and expand Arduino’s functionality.

A. We've compiled all the libraries necessary to run this suite. Please click the link below to download them and follow the instructions to complete the installation:  `Download libraries <https://www.dropbox.com/scl/fo/syf1zstu58f4xlcld2nss/ACJOi93PcIafo5yGabrprDA?rlkey=hoc2undykymrxac7z8nclpk9u&st=el86zaw9&dl=1>`_

----

B. Unzip the downloaded library file. The library file storage path is **Code and Libraries** → **Libraries** . Open it and confirm that it contains the library file shown in the figure below.

（插入库文件图片）

----

C. Open the Arduino IDE and click **Sketch** → **Include Library** → **Add .ZIP Library**.

.. image:: _static/arduino/9.lib.png
   :width: 800
   :align: center

----

D. In the pop-up window, locate the folder of the library you just downloaded and unzipped, select it, and click Open to complete the import.

.. image:: _static/arduino/10.lib.png
   :width: 800
   :align: center

----

E. If the library file is imported successfully, the Arduino IDE output window will display the message: **Library installed**.

.. image:: _static/arduino/11.lib.png
   :width: 800
   :align: center

----

.. note::

   - The Arduino IDE does not support importing multiple libraries at once; you must import one library at a time, and all library files included in the resources must be imported.
   - If a library file already exists, a prompt will appear asking whether to overwrite it. It is recommended to confirm overwrite to avoid program errors caused by different library versions.

.. image:: _static/arduino/12.lib.png
   :width: 800
   :align: center

----

F. Download Libraries Using Arduino IDE

You can also download required libraries directly using the Arduino IDE.

 - On the right side of the Arduino IDE interface, click the Library Manager icon.

 - Enter keywords in the search box to find the required library and click Install to download.

.. image:: _static/arduino/13.lib.png
   :width: 800
   :align: center

----