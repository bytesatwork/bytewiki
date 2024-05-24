######
Errata
######

.. contents:: Known issues
  :local:

*****************
byteDEVKIT < V1.2
*****************

.. _stm32mp1-ethernet:

STM32MP1 Ethernet
=================

| Due to a hardware issue at the ethernet PHY autonegotiation is disabled.
| Using the ethernet setting from Device Tree of 1 GbE will not work on ethernet switches < 1 GbE.
| As a workaround the ``ethtool`` could be used to set the speed manually.

Download it from `here
<http://packages.bytesatwork.io/yocto/3.1.11/bytedevkit-stm32mp1/cortexa7t2hf-neon-vfpv4/ethtool_5.4-r0_armhf.deb>`_,
copy it to the SD card and install it on the target with:

::

  dpkg -i ethtool_5.4-r0_armhf.deb

Set the desired speed manually:

::

  ethtool -s eth0 speed 100 duplex full
  # or even
  ethtool -s eth0 speed 10 duplex half

.. This is the footer, don't edit after this
.. image:: images/wiki_footer.jpg
   :align: center
   :target: https://www.bytesatwork.io
