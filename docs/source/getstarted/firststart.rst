*******************************
First start byteDEVKIT STM32MP1
*******************************

**This guide helps with the first start of the byteDEVKIT STM32MP1:**

Connecting the Hardware and first Booting
-----------------------------------------
-  Prepare the USB serial cable for connection
-  Locate the black cable of the serial connector.

.. image:: https://www.bytesatwork.io/wp-content/uploads/2020/04/wiring_2kl.jpg
   :scale: 20%
   :align: center

------------

.. Caution:: Connect the serial cable to the byteDEVKIT STM32MP1 as shown. The **black cable** must point towards the USB OTG connector.

.. image:: https://www.bytesatwork.io/wp-content/uploads/2020/04/wiring_3kl.jpg
   :scale: 20%
   :align: center
   
------------

-  Connect the USB connector with USB port of your computer or laptop.
-  Connect the ethernet RJ45 with the byteDEVKIT STM32MP1.

.. image:: https://www.bytesatwork.io/wp-content/uploads/2020/04/wiring_5kl.jpg
   :scale: 20%
   :align: center
   
------------

-  Plug in the power socket.
-  Connect the power supply cable to the power slot of the byteDEVKIT STM32MP1.

.. image:: https://www.bytesatwork.io/wp-content/uploads/2020/04/wiring_7kl.jpg
   :scale: 20%
   :align: center
   
------------

-  A green LED on the backside of the byteDEVKIT STM32MP1 indicates the status of the power supply.

.. Attention:: Your byteDEVKIT STM32MP1 is powered up, when the green LED lights up. If the LED doesn´t light up, check the connection of the power socket.

.. image:: https://www.bytesatwork.io/wp-content/uploads/2020/04/wiring_8kl.jpg
   :scale: 20%
   :align: center
   
------------

-  The 5-inch touchscreen display shows the bytes at work-logo when booting.

.. Hint:: The booting procedure will take a few seconds.

.. image:: https://www.bytesatwork.io/wp-content/uploads/2020/04/wiring_9kl.jpg
   :scale: 20%
   :align: center
   
------------

-  Now you can access the byteDEVKIT STM32MP1 with your laptop.

.. Hint:: For further information refer to: "Bring-up_byteDEVKIT_STM32MP1_".

.. _Bring-up_byteDEVKIT_STM32MP1: https://jf-bytewiki.readthedocs.io/en/latest/firststart.html#id1

.. image:: https://www.bytesatwork.io/wp-content/uploads/2020/04/wiring_10kl.jpg
   :scale: 20%
   :align: center
   
------------


****************************
Bring-up byteDEVKIT STM32MP1
****************************

How do I connect to byteDEVKIT using the serial console?
--------------------------------------------------------

-  **Use the serial port to connect the byteDEVKIT STM32MP1:**
    • Connect the debug cable with the byteDEVKIT STM32MP1 and your computer/laptop
    • Start a serial communication program on your computer/laptop (‹putty›, ‹minicom› or something else)
    • Set to 115200, 8N1, no flow control
    • login with: **user: "root"** and **password: "rootme"**

=====
LINUX
=====

• Start PuTTY
  
.. image:: https://www.bytesatwork.io/wp-content/uploads/2020/04/Putty_1.png
   :scale: 100%
   :align: center

------------  

  • Click "Serial"
  • Change "Serial line" to "/dev/ttyUSB0"
  • Change "Speed" to 115200
  • Navigate to "Serial" in the menu "Connection"
  
  .. Hint::  make sure you have Data bits set to 8, Stop bits set to 1, Parity to None, Flow control to None
  
  • Click "Open"
  
----------- 

  • Power up the byteDEVKIT STM32MP1
  
.. image:: https://www.bytesatwork.io/wp-content/uploads/2020/04/Putty_2.png
   :scale: 100%
   :align: center

------------

  • Once the login prompt appears, login with user "root" and password "rootme"
  
.. image:: https://www.bytesatwork.io/wp-content/uploads/2020/04/Putty_3.png
   :scale: 100%
   :align: center

.. Note::  You are now succesfully connected to the byteDEVKIT STM32MP1

------------ 

=======
WINDOWS
=======

  • Connect the USB serial adapter to the computer
  • Windows installs the driver automatically (if the windows doesn´t install the driver reconnect the serial adapter cable)
  • Open device manager and navigate to "Ports (COM & LPT)"
  • The serial adapter shows up in the device tree: "Prolific USB-to-Serial Comm Port (COM7)"
  • "COM7" is your serial port
  • Install a serial terminal application, e.g. PuTTY (version 0.59 and newer) :guilabel:`https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html`
  
  • Start PuTTY
  
.. image:: https://www.bytesatwork.io/wp-content/uploads/2020/04/Putty_4.png
   :scale: 100%
   :align: center

------------  

  • Click "Serial"
  • Change "Serial line" to serial port you found in device manager
  • Change "Speed" to 115200
  • Navigate to "Serial" in the menu "Connection"
  
  .. Hint::  make sure you have Data bits set to 8, Stop bits set to 1, Parity to None, Flow control to None
  
  • Click "Open"
  
----------- 

  Power up the byteDEVKIT STM32MP1
  
.. image:: https://www.bytesatwork.io/wp-content/uploads/2020/04/Putty_5.png
   :scale: 100%
   :align: center

------------

  Once the login prompt appears, login with user "root" and password "rootme"
  
.. image:: https://www.bytesatwork.io/wp-content/uploads/2020/04/Putty_6.png
   :scale: 100%
   :align: center

.. Note::  You are now successfully connected to the byteDEVKIT STM32MP1

------------  

  

How to install additional software using apt
--------------------------------------------

.. Hint::  Follow the link for additional information about "apt": https://help.ubuntu.com/community/AptGet/Howto

.. Note:: byteDEVKIT < V1.2: If you are using a LAN switch (hub) with no 1 GbE support see :ref:`stm32mp1-ethernet`.

1. Connect the embedded device's ethernet to your LAN
2. Run: :guilabel:`apt-get update`
3. Run: :guilabel:`apt-cache search <software component>` to search for available packages
   e.g.: :guilabel:`apt-cache search nodejs`

.. image:: https://www.bytesatwork.io/wp-content/uploads/2020/05/apt-cache_nodejs.png
   :scale: 100%
   :align: center
   
------------

5. Run: :guilabel:`apt-get install <software component>` to install additional software
   e.g.: :guilabel:`apt-get install nodejs`
   
.. image:: https://www.bytesatwork.io/wp-content/uploads/2020/05/apt-get_install_wide.png
   :scale: 100%
   :align: center



.. This is the footer, don't edit after this
.. image:: ../images/wiki_footer.jpg
   :align: center
   :target: https://www.bytesatwork.io