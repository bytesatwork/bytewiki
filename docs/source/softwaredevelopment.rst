####################
Software Development
####################
The entire development life cycle is done in-house with transparent project management and customer involvement. We have proven experience in a wide range of industries, including industrial automation and custom solutions for consumer electronics. This section helps you step by step initiating the software development process:


*****
Image
*****

Where do you get the SD card image?
===============================================

.. list-table::
    :header-rows: 1

    * - Device
      - Yocto Version
      - Download
      - Checksum
    * - byteDEVKIT
      - Yocto 3.0
      - `bytesatwork-minimal-image-bytedevkit.wic.gz <https://download.bytesatwork.io/transfer/bytesatwork/m5/3.0/bytesatwork-minimal-image-bytedevkit.wic.gz>`_
        (`wic.bmap
        <https://download.bytesatwork.io/transfer/bytesatwork/m5/3.0/bytesatwork-minimal-image-bytedevkit.wic.bmap>`__)
      - 4ce5b056a78a0bfecae46ad6777a8b7bcfa0e5a679d4f53654969234c9a19282
    * - byteDEVKIT
      - Yocto 2.7
      - `flashlayout_bytesatwork-minimal-image_FlashLayout_sdcard_stm32mp157c-bytedevkit.raw.gz <https://download.bytesatwork.io/transfer/bytesatwork/m5/2.7/flashlayout_bytesatwork-minimal-image_FlashLayout_sdcard_stm32mp157c-bytedevkit.raw.gz>`_
      - 7e62644473c21d200603b52d0080894a0ccfd950dd4a2f3c7df2b14753566de8
    * - bytePANEL
      - Yocto 3.0
      - `bytesatwork-minimal-image-bytepanel-emmc.wic.gz <https://download.bytesatwork.io/transfer/bytesatwork/m2/3.0/bytesatwork-minimal-image-bytepanel-emmc.wic.gz>`_
        (`wic.bmap
        <https://download.bytesatwork.io/transfer/bytesatwork/m2/3.0/bytesatwork-minimal-image-bytepanel-emmc.wic.bmap>`__)
      - e3e166f28fb815b09c6372bbcae4b4c8fcd00f93e57e96084bdee90c255764d9
    * - bytePANEL
      - Yocto 2.7
      - `devbase-image-bytesatwork-bytepanel-emmc-20190729194430.sdimg.gz <https://download.bytesatwork.io/transfer/bytesatwork/m2/2.7/devbase-image-bytesatwork-bytepanel-emmc-20190729194430.sdimg.gz>`_
      - 3b3e51d83c68f68d6ebbc2983d6b41b9e21d4878c1c9570804e6949624d7a41e

---------------

How do you flash the image?
==============================

.. Attention::
  - You need a microSD card with **at least 8GB** capacity.
  - **All existing data** on the microSD card will be lost.
  - **Do not format** the microSD card before flashing.

byteDEVKIT
--------------

-  **Yocto 3.0**

   Windows

      #. Unzip the file ``bytesatwork-minimal-image-bytedevkit.wic.gz`` (e.g. with 7-zip)
      #. Write the resulting file to the microSD card with a tool like `Roadkils Disk Image <https://www.roadkil.net/program.php?ProgramID=12>`_

   Linux

   ::

     gunzip -c bytesatwork-minimal-image-bytedevkit.wic.gz | dd of=/dev/mmcblk<X> bs=8M conv=fdatasync status=progress

.. Hint:: To improve write performance, you could use bmap-tools under Linux:

  ``bmaptool copy bytesatwork-minimal-image-bytedevkit.wic.gz /dev/mmcblk<X>``

-  **Yocto 2.7**

   Windows

      #. Unzip the file ``flashlayout_bytesatwork-minimal-image_FlashLayout_sdcard_stm32mp157c-bytedevkit.raw.gz`` (e.g. with 7-zip)
      #. Write the resulting file to the microSD card with a tool like `Roadkils Disk Image <https://www.roadkil.net/program.php?ProgramID=12>`_

   Linux

   ::

     gunzip -c flashlayout_bytesatwork-minimal-image_FlashLayout_sdcard_stm32mp157c-bytedevkit.raw.gz | dd of=/dev/mmcblk<X> bs=8M conv=fdatasync status=progress

bytePANEL
-------------

-  **Yocto 3.0**

   Windows

      #. Unzip the file ``bytesatwork-minimal-image-bytepanel-emmc.wic.gz`` (e.g. with 7-zip)
      #. Write the resulting file to the microSD card with a tool like `Roadkils Disk Image <https://www.roadkil.net/program.php?ProgramID=12>`_


   Linux

   ::

     gunzip -c bytesatwork-minimal-image-bytepanel-emmc.wic.gz | dd of=/dev/mmcblk<X> bs=8M conv=fdatasync status=progress

.. Hint:: To improve write performance, you could use bmap-tools under Linux:

  bmaptool copy bytesatwork-minimal-image-bytepanel-emmc.wic.gz /dev/mmcblk<X>

-  **Yocto 2.7**

   Windows

      #. Unzip the file ``devbase-image-bytesatwork-bytepanel-emmc-20190729194430.sdimg.gz`` (e.g. with 7-zip)
      #. Write the resulting file to the microSD card with a tool like `Roadkils Disk Image <https://www.roadkil.net/program.php?ProgramID=12>`_


   Linux

   ::

     gunzip -c devbase-image-bytesatwork-bytepanel-emmc-20190729194430.sdimg.gz | dd of=/dev/mmcblk<X> bs=8M conv=fdatasync status=progress

---------------

How do you build an image?
=============================

byteDEVKIT
--------------

-  **Yocto 3.0**

   Use ``repo`` to download all necessary repositories:

   ::

      $ mkdir -p ~/workdir/bytedevkit/3.0; cd ~/workdir/bytedevkit/3.0
      $ repo init -u https://github.com/bytesatwork/bsp-platform-st.git -b zeus
      $ repo sync

   If those commands are completed successfully, the following command
   will set up a Yocto Project environment for byteDEVKIT:

   ::

      $ cd ~/workdir/bytedevkit/3.0
      $ MACHINE=bytedevkit DISTRO=poky-bytesatwork EULA=1 . setup-environment build

   The final command builds the development image:

   ::

      $ cd $BUILDDIR
      $ bitbake bytesatwork-minimal-image

   The output is found in:

   ::

      ~/workdir/bytedevkit/3.0/build/tmp/deploy/images/bytedevkit

.. Hint:: For additional information about yocto images and how to build them, please visit: https://www.yoctoproject.org/docs/3.0/mega-manual/mega-manual.html#brief-building-your-image

-  **Yocto 2.7**

   Use ``repo`` to download all necessary repositories:

   ::

      $ mkdir -p ~/workdir/bytedevkit/2.7; cd ~/workdir/bytedevkit/2.7
      $ repo init -u https://github.com/bytesatwork/bsp-platform-st.git -b warrior
      $ repo sync

   If those commands are completed successfully, the following command
   will set up a Yocto Project environment for byteDEVKIT:

   ::

      $ cd ~/workdir/bytedevkit/2.7
      $ MACHINE=bytedevkit DISTRO=poky-bytesatwork EULA=1 . setup-environment build

   The final command builds the development image:

   ::

      $ cd $BUILDDIR
      $ bitbake devbase-image-bytesatwork

   The output is found in:

   ::

      ~/workdir/bytedevkit/2.7/build/tmp/deploy/images/bytedevkit


bytePANEL
-------------

-  **Yocto 3.0**

   Use ``repo`` to download all necessary repositories:

   ::

      $ mkdir -p ~/workdir/bytepanel/3.0; cd ~/workdir/bytepanel/3.0
      $ repo init -u https://github.com/bytesatwork/bsp-platform-ti.git -b zeus
      $ repo sync

   If those commands are completed successfully, the following command
   will set up a Yocto Project environment for bytePANEL:

   ::

      $ cd ~/workdir/bytepanel/3.0
      $ MACHINE=bytepanel DISTRO=poky-bytesatwork EULA=1 . setup-environment build

   The final command builds the development image:

   ::

      $ cd $BUILDDIR
      $ bitbake bytesatwork-minimal-image

   The output is found in:

   ::

      ~/workdir/bytepanel/3.0/build/tmp/deploy/images/bytepanel

.. Hint:: For additional information about yocto images and how to build them, please visit: https://www.yoctoproject.org/docs/3.0/mega-manual/mega-manual.html#brief-building-your-image

-  **Yocto 2.7**

   Use ``repo`` to download all necessary repositories:

   ::

      $ mkdir -p ~/workdir/bytepanel/2.7; cd ~/workdir/bytepanel/2.7
      $ repo init -u https://github.com/bytesatwork/bsp-platform.git -b warrior
      $ repo sync

   If those commands are completed successfully, the following command
   will set up a Yocto Project environment for bytePANEL:

   ::

      $ cd ~/workdir/bytepanel/2.7
      $ MACHINE=bytepanel DISTRO=poky-bytesatwork EULA=1 . setup-environment build

   The final command builds the development image:

   ::

      $ cd $BUILDDIR
      $ bitbake devbase-image-bytesatwork

   The output is found in:

   ::

      ~/workdir/bytepanel/2.7/build/tmp/deploy/images/bytepanel


How to modify the image
---------------------------

  The image recipes can be found in ``~/workdir/<machine name>/<yocto version>/sources/meta-bytesatwork/recipes-core/images``

  This is relative to where you started the ``repo`` command to fetch all the sources.

  Edit the minimal-image recipe ``bytesatwork-minimal-image.bb``

  Add the desired software-package to ``IMAGE_INSTALL`` variable, for example add ``net-tools`` to ``bytesatwork-minimal-image.bb``

  Rebuild the image by:

  ::

    $ cd ~/workdir/<machine name>/<yocto version>
    $ MACHINE=<machine name> DISTRO=poky-bytesatwork EULA=1 . setup-environment build
    $ bitbake bytesatwork-minimal-image


How to rename the image
---------------------------

If you want to rename or copy an image, simply rename or copy the image recipe by:

   ::

    $ cd ~/workdir/<machine name>/<yocto version>/build/tmp/deploy/images/<machine name>
    $ cp bytesatwork-minimal-image.bb customer-example-image.bb


Troubleshooting
-------------------

-  **Image size is to small**

   If you encounter that your image size is to small to install additional software,
   please have a look at the ``IMAGE_ROOTFS_SIZE`` variable under
   ``~/workdir/<machine-name>/<yocto version>/sources/meta-bytesatwork/recipes-core/images/bytesatwork-minimal-image.bb``.
   Increase the size if necessary.

---------------

*********
Toolchain
*********

.. _get-toolchain:

Where do you get the toolchain?
===============================

.. list-table::
    :header-rows: 1

    * - Device
      - Yocto Version
      - Download
      - Checksum
    * - byteDEVKIT
      - Yocto 3.0
      - `poky-bytesatwork-glibc-x86_64-bytesatwork-minimal-image-cortexa7t2hf-neon-vfpv4-bytedevkit-toolchain-3.0.2.sh <https://download.bytesatwork.io/transfer/bytesatwork/m5/3.0/poky-bytesatwork-glibc-x86_64-bytesatwork-minimal-image-cortexa7t2hf-neon-vfpv4-bytedevkit-toolchain-3.0.2.sh>`_
      - 50ac1ed18dcbbf8ff37854f6752af52e1e01aed1a26815f41b3d9b965dcb5806
    * - byteDEVKIT
      - Yocto 2.7
      - `poky-bytesatwork-glibc-x86_64-devbase-image-bytesatwork-cortexa7t2hf-neon-vfpv4-bytedevkit-toolchain-2.7.1.sh <https://download.bytesatwork.io/transfer/bytesatwork/poky-bytesatwork-glibc-x86_64-devbase-image-bytesatwork-cortexa7t2hf-neon-vfpv4-bytedevkit-toolchain-2.7.1.sh>`_
      - 61896873ac7c75ac711a0b8e439ded6721d1a794deec26b4903178efbf51d307
    * - bytePANEL
      - Yocto 3.0
      - `poky-bytesatwork-glibc-x86_64-bytesatwork-minimal-image-armv7at2hf-neon-bytepanel-emmc-toolchain-3.0.2.sh <https://download.bytesatwork.io/transfer/bytesatwork/m2/3.0/poky-bytesatwork-glibc-x86_64-bytesatwork-minimal-image-armv7at2hf-neon-bytepanel-emmc-toolchain-3.0.2.sh>`_
      - a90763d7ff408e9e5f0556b051eccd3ea85c43406099c9a61d98a32e6a04e078
    * - bytePANEL
      - Yocto 2.7
      - `poky-bytesatwork-glibc-x86_64-devbase-image-bytesatwork-armv7at2hf-neon-bytepanel-toolchain-2.7.3.sh <https://download.bytesatwork.io/transfer/bytesatwork/poky-bytesatwork-glibc-x86_64-devbase-image-bytesatwork-armv7at2hf-neon-bytepanel-toolchain-2.7.3.sh>`_
      - b25e4a3f764eaf583ad0e6a3e0edcac9a1a9314ab6d1f4aad290c415afdbe0e7

---------------

How do you install the toolchain?
====================================

Simply download the toolchain and execute the downloaded file, which is
a self-extracting shell script.

.. Hint:: If you encounter problems when trying to install the toolchain, make sure the downloaded toolchain is executable. Run ``chmod +x /<path>/<toolchain-file>.sh`` to make it executable.

.. Important::
   The following tools need to be installed on your development system:
      * ``xz`` (Debian package: ``xz-utils``)
      * ``python`` (any version)
      * ``gcc``

---------------

How do you use the toolchain?
================================


byteENGINE STM32MP1x
------------------------

Source the installed toolchain:

::

   source /opt/poky-bytesatwork/3.0.2/environment-setup-cortexa7t2hf-neon-vfpv4-poky-linux-gnueabi

Check if Cross-compiler is available in environment:

::

   echo $CC

You should see the following output:

::

   arm-poky-linux-gnueabi-gcc -mthumb -mfpu=neon-vfpv4 -mfloat-abi=hard -mcpu=cortex-a7 -fstack-protector-strong -D_FORTIFY_SOURCE=2 -Wformat -Wformat-security -Werror=format-security --sysroot=/opt/poky-bytesatwork/3.0.2/sysroots/cortexa7t2hf-neon-vfpv4-poky-linux-gnueabi

Crosscompile the source code, e.g. by:

::

   $CC helloworld.c -o helloworld

Check generated binary:

::

   file helloworld

The output that is shown in prompt afterwards:

::

   helloworld: ELF 32-bit LSB pie executable, ARM, EABI5 version 1

byteENGINE AM335x
---------------------
Source the installed toolchain:

::

   source /opt/poky-bytesatwork/3.0.2/environment-setup-armv7at2hf-neon-poky-linux-gnueabi

Check if Cross-compiler is available in environment:

::

   echo $CC

You should see the following output:

::

   arm-poky-linux-gnueabi-gcc -march=armv7-a -mthumb -mfpu=neon -mfloat-abi=hard --sysroot=/opt/poky-bytesatwork/3.0.2/sysroots/armv7at2hf-neon-poky-linux-gnueabi


Cross-compile the source code, e.g. by:

::

   $CC helloworld.c -o helloworld

Check generated binary:

::

   file helloworld

The output that is shown in prompt afterwards:

::

   helloworld: ELF 32-bit LSB pie executable, ARM, EABI5 version 1

---------------

How to bring your binary to the target?
==========================================

1. Connect the embedded device's ethernet to your LAN
2. Determine the embedded target IP address by ``ip addr show``

.. image:: https://www.bytesatwork.io/wp-content/uploads/2020/05/ip_addr_show_28.png
   :scale: 100%
   :align: center

3. Copy your binary, e.g. ``helloworld`` to the target by ``scp helloworld root@<ip address of target>:/tmp``

.. image:: https://www.bytesatwork.io/wp-content/uploads/2020/05/scp2.png
   :scale: 100%
   :align: center

4. Run ``chmod +x`` on the target to make your binary executable: ``chmod +x /<path>/<binary name>``
5. Run your binary on the target: ``/<path>/<binary name>``

---------------

How do you build a toolchain?
================================

byteDEVKIT
--------------

-  **Yocto 3.0**

   ::

      $ cd ~/workdir/bytedevkit/3.0
      $ repo init -u https://github.com/bytesatwork/bsp-platform-st.git -b zeus
      $ repo sync

   If those commands are completed successfully, the following command
   will set up a Yocto Project environment for byteDEVKIT:

   ::

      $ cd ~/workdir/bytedevkit/3.0
      $ MACHINE=bytedevkit DISTRO=poky-bytesatwork EULA=1 . setup-environment build

   The final command builds an installable toolchain:

   ::

      $ cd $BUILDDIR
      $ bitbake bytesatwork-minimal-image -c populate_sdk

   The toolchain is located under:

   ::

      ~/workdir/bytedevkit/3.0/build/tmp/deploy/sdk

-  **Yocto 2.7**

   ::

      $ cd ~/workdir/bytedevkit/2.7
      $ repo init -u https://github.com/bytesatwork/bsp-platform-st.git -b warrior
      $ repo sync

   If those commands are completed successfully, the following command
   will set up a Yocto Project environment for byteDEVKIT:

   ::

      $ ~/workdir/bytedevkit/2.7
      $ MACHINE=bytedevkit DISTRO=poky-bytesatwork EULA=1 . setup-environment build

   The final command builds an installable toolchain:

   ::

      $ cd $BUILDDIR
      $ bitbake devbase-image-bytesatwork -c populate_sdk

   The toolchain is located under:

   ::

      ~/workdir/bytedevkit/2.7/build/tmp/deploy/sdk


bytePANEL
-------------

-  **Yocto 3.0**

   ::

      $ cd ~/workdir/bytepanel/3.0
      $ repo init -u https://github.com/bytesatwork/bsp-platform-ti.git -b zeus
      $ repo sync

   If those commands are completed successfully, the following command
   will set up a Yocto Project environment for bytePANEL:

   ::

      $ cd ~/workdir/bytepanel/3.0
      $ MACHINE=bytepanel DISTRO=poky-bytesatwork EULA=1 . setup-environment build

   The final command builds an installable toolchain:

   ::

      $ cd $BUILDDIR
      $ bitbake bytesatwork-minimal-image -c populate_sdk

   The toolchain is located under:

   ::

      ~/workdir/bytepanel/3.0/build/tmp/deploy/sdk

-  **Yocto 2.7**

   ::

      $ cd ~/workdir/bytepanel/2.7
      $ repo init -u https://github.com/bytesatwork/bsp-platform.git -b warrior
      $ repo sync

   If those commands are completed successfully, the following command
   will set up a Yocto Project environment for bytePANEL:

   ::

      $ cd ~/workdir/bytepanel/2.7
      $ MACHINE=bytepanel DISTRO=poky-bytesatwork EULA=1 . setup-environment build

   The final command builds an installable toolchain:

   ::

      $ cd $BUILDDIR
      $ bitbake devbase-image-bytesatwork -c populate_sdk

   The toolchain is located under:

   ::

      ~/workdir/bytepanel/2.7/build/tmp/deploy/sdk

How to modify your toolchain
--------------------------------

Currently the bytesatwork toolchain is generated out of the bytesatwork-minimal-image recipe. If you want to add additional libraries and development headers to customize the toolchain, you need to modify the bytesatwork-minimal-image recipe. It can be found under ``~/workdir/<machine name>/<yocto version>/sources/meta-bytesatwork/recipes-core/images``

For example if you want to develop your own ftp client and you need libftp and the corresponding header files, edit the recipe ``bytesatwork-minimal-image.bb`` and add ``ftplib`` to the ``IMAGE_INSTALL`` variable.

This will provide the ftplib libraries and development headers in the toolchain. After adding additional software components, the toolchain needs to be rebuilt by:

::

$ cd ~/workdir/<machine name>/<yocto version>
$ MACHINE=<machine> DISTRO=poky-bytesatwork EULA=1 . setup-environment build
$ bitbake bytesatwork-minimal-image -c populate_sdk

The newely generated toolchain will be available under:

::

~/workdir/<machine name>/<yocto version>/build/tmp/deploy/sdk

For additional information, please visit: https://www.yoctoproject.org/docs/3.0.2/overview-manual/overview-manual.html#cross-development-toolchain-generation


Troubleshooting
-------------------

-  **Errors when building the toolchain**

   If you get the error below, please revert commit: ``179c5cb7fd0f06970135187f1203507aa55d6bde`` in the poky repository (sources/poky). See also Bug 13338 https://bugzilla.yoctoproject.org/show_bug.cgi?id=13338.

.. code-block:: none
   :emphasize-lines: 11,12

   ERROR: bytesatwork-minimal-image-1.0-r0 do_populate_sdk: Unable to install packages. Command '/home/daniel/workspace/bytesatwork/yocto/ti-m2-zeus/build/tmp/work/bytepanel_emmc-poky-linux-gnueabi/bytesatwork-minimal-image/1.0-r0/recipe-sysroot-native/usr/bin/apt-get  install --force-yes --allow-unauthenticated openssh-ssh openssh-sshd apt dpkg coreutils base-passwd dhcp-client target-sdk-provides-dummy shadow openssh-scp packagegroup-core-standalone-sdk-target packagegroup-core-boot vim openssh-sftp-server run-postinsts' returned 100:
   Reading package lists...
   Building dependency tree...
   Reading state information...
   Some packages could not be installed. This may mean that you have
   requested an impossible situation or if you are using the unstable
   distribution that some required packages have not yet been created
   or been moved out of Incoming.
   The following information may help to resolve the situation:

   The following packages have unmet dependencies:
    target-sdk-provides-dummy : Conflicts: coreutils
   E: Unable to correct problems, you have held broken packages.

----

******
Kernel
******

.. _download-kernel:

Download the Linux Kernel
=========================

.. list-table::
    :header-rows: 1

    * - Device
      - Branch
      - git URL
    * - byteDEVKIT
      - baw-v4.19-stm32mp
      - https://github.com/bytesatwork/linux-stm32mp.git
    * - bytePANEL
      - baw-ti-linux-4.19.y
      - https://github.com/bytesatwork/ti-linux-kernel.git

----

Build the Linux Kernel
======================

For both targets, an ARM toolchain is necessary. You can use the
provided toolchain from :ref:`get-toolchain` or any compatible toolchain (e.g.
from your distribution)

.. Important::
   The following tools need to be installed on your development system:
      * ``git``
      * ``make``
      * ``bc``

.. Note::
        The following instructions assume, you installed the provided toolchain
        for the respective target.

byteDEVKIT
----------

.. Important::
   The following tools need to be installed on your development system:
      * OpenSSL headers (Debian package: ``libssl-dev``)
      * ``depmod`` (Debian package: ``kmod``)

#. Download kernel sources

   Download the appropriate kernel from :ref:`download-kernel`.

#. Source toolchain

   ::

      source /opt/poky-bytesatwork/3.0.2/environment-setup-cortexa7t2hf-neon-vfpv4-poky-linux-gnueabi

#. Create defconfig

   ::

      make multi_v7_defconfig
      scripts/kconfig/merge_config.sh -m -r .config arch/arm/configs/fragment-*
      make olddefconfig

#. Build Linux kernel

   ::

      make LOADADDR=0xC2000040 -j `nproc` uImage stm32mp157c-bytedevkit-v1-1.dtb modules

#. Install kernel and device tree

   To use the newly created kernel, device tree and/or module, the necessary
   files need to be installed on the target. This can be done either via
   Ethernet (e.g. ``scp``) or by copying the files to the SD card.

   .. Note::
      For scp installation: Don't forget to mount /boot on the target.

   .. list-table::
       :header-rows: 1

       * - File
         - Target path
         - Target partition
       * - ``arch/arm/boot/uImage``
         - ``/boot/uImage``
         - ``/dev/mmcblk0p4``
       * - ``arch/arm/boot/dts/stm32mp157c-bytedevkit-v1-1.dtb``
         - ``/boot/stm32mp157c-bytedevkit.dtb``
         - ``/dev/mmcblk0p4``

   .. Note::
      After installing a new kernel, it often fails to load modules, as the
      _signature_ of the kernel changed and it fails to find its corresponding modules
      folder. This issue can often be resolved with a symlink:

      ::

        ln -s /lib/modules/<EXISTING FOLDER> /lib/modules/`uname -r`

     Otherwise, please follow the instructions to copy the kernel modules

#.  Install kernel modules

    To copy all available modules to the target, it's best to deploy them
    locally first and then copy all modules to the target.

    ::

       mkdir /tmp/bytedevkit
       make INSTALL_MOD_PATH=/tmp/bytedevkit modules_install

   Now you can copy the content of the folder ``/tmp/bytedevkit`` into the
   target's root folder (``/``) which is partition ``/dev/mmcblk0p5``.

bytePANEL
---------

.. Important::
   The following tools need to be installed on your development system:
      * ``u-boot-tools``

#. Download kernel sources

   Download the appropriate kernel from :ref:`download-kernel`.

#. Source toolchain

   ::

      source /opt/poky-bytesatwork/3.0.2/environment-setup-armv7at2hf-neon-poky-linux-gnueabi

#. Create defconfig

   ::

      make bytepanel_defconfig

#. Build Linux kernel

   ::

      make LOADADDR=0x80008000 -j `nproc` uImage bytepanel.dtb

#. Install kernel and device tree

   To use the newly created kernel and device tree, the necessary
   files need to be installed on the target. This can be done either via
   Ethernet (e.g. ``scp``) or in copying the files to the SD card.

   .. Note::
      For scp installation: Don't forget to mount /boot on the target.

   .. list-table::
       :header-rows: 1

       * - File
         - Target path
         - Target partition
       * - ``arch/arm/boot/uImage``
         - ``/boot/uImage``
         - ``/dev/mmcblk0p1``
       * - ``arch/arm/boot/dts/bytepanel.dtb``
         - ``/boot/devtree.dtb``
         - ``/dev/mmcblk0p1``


.. This is the footer, don't edit after this
.. image:: https://www.bytesatwork.io/wp-content/uploads/2020/04/Bildschirmfoto-2020-04-20-um-19.41.44.jpg
   :scale: 100%
   :align: center

