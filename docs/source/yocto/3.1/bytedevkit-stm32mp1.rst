###############################
byteDEVKIT-stm32mp1 (Yocto 3.1)
###############################

*********
Downloads
*********


SD card image
=============

.. list-table::
    :header-rows: 1

    * - Download
      - Checksum (SHA256)
    * - `bytesatwork-minimal-image-bytedevkit-stm32mp1.wic.gz <https://download.bytesatwork.io/transfer/bytesatwork/m5/3.1.11/bytesatwork-minimal-image-bytedevkit-stm32mp1.wic.gz>`_
        (`wic.bmap
        <https://download.bytesatwork.io/transfer/bytesatwork/m5/3.1.11/bytesatwork-minimal-image-bytedevkit-stm32mp1.wic.bmap>`__)
      - 6fa368ff5df6967480f3704c1a9e987f284fa0f8b78ec679c57be9f74e4520f7

.. Hint:: Updating from an older image?
   You can update your older image by using: ``apt-get update`` and ``apt-get upgrade``.

   #. check for new version in the table above
   #. edit ``/etc/apt/sources.list`` and point to the new package feed
   #. run ``apt-get update; apt-get upgrade``

   As the yocto framework is based on several packages from various projects or suppliers, it is not guaranteed that
   an incremental upgrade by ``apt-get upgrade`` works automatically. Some manual adjustments might be needed.


.. _get-toolchain-bytedevkit-stm32mp1-3.1:

Toolchain
=========

.. list-table::
    :header-rows: 1

    * - Download
      - Checksum (SHA256)
    * - `poky-bytesatwork-glibc-x86_64-bytesatwork-minimal-image-cortexa7t2hf-neon-vfpv4-bytedevkit-stm32mp1-toolchain-3.1.11.sh <https://download.bytesatwork.io/transfer/bytesatwork/m5/3.1.11/poky-bytesatwork-glibc-x86_64-bytesatwork-minimal-image-cortexa7t2hf-neon-vfpv4-bytedevkit-stm32mp1-toolchain-3.1.11.sh>`_
      - 41e304ec75a26d3bcac7d1f9f2cb72fc07e6002d97f7de45f65ef36baf71f450


U-Boot
======

.. Note::
        The images come with a preinstalled U-Boot that supports 512 MB of RAM.
        If you have a module with 1 GB of RAM, you will have to
        :ref:`install-spl-uboot-bytedevkit-stm32mp1-3.1` to unlock the full
        1 GB of RAM.



.. list-table::
     :header-rows: 1

     * - Description
       - Download
       - Checksum (SHA256)
     * - MLO (512 MB)
       - `u-boot-spl.stm32-stm32mp157c-bytedevkit-basic <https://download.bytesatwork.io/transfer/bytesatwork/m5/3.1.11/u-boot-spl.stm32-stm32mp157c-bytedevkit-basic>`_
       - ffc3c38e453f7b8760b4edfabd0e6aa0c55fb3e386d8a5a80b90e3a12d0e900d
     * - U-Boot (512 MB)
       - `u-boot-stm32mp157c-bytedevkit-basic.img <https://download.bytesatwork.io/transfer/bytesatwork/m5/3.1.11/u-boot-stm32mp157c-bytedevkit-basic.img>`_
       - c0fe5de015ceefa8b3e9a761007523b33fb0e0dddda9ee39d7c3d55382a13ccb
     * - MLO (1 GB)
       - `u-boot-spl.stm32-stm32mp157c-bytedevkit-1g_ram <https://download.bytesatwork.io/transfer/bytesatwork/m5/3.1.11/u-boot-spl.stm32-stm32mp157c-bytedevkit-1g_ram>`_
       - 99b88a246879e704f92a4f934a9641db8cf64262033e81dbc69b73b6bdba1d20
     * - U-Boot (1 GB)
       - `u-boot-stm32mp157c-bytedevkit-1g_ram.img <https://download.bytesatwork.io/transfer/bytesatwork/m5/3.1.11/u-boot-stm32mp157c-bytedevkit-1g_ram.img>`_
       - 8fa044532a61bfe82621bafad4b640710cb5406bc280f43e026a4709d269cb45



*****
Image
*****


How do you flash the image?
===========================

.. Attention::
  - You need a microSD card with **at least 8GB** capacity.
  - **All existing data** on the microSD card will be lost.
  - **Do not format** the microSD card before flashing.

Windows

   #. Unzip the file ``bytesatwork-minimal-image-bytedevkit-stm32mp1.wic.gz`` (e.g. with 7-zip)
   #. Write the resulting file to the microSD card with a tool like `Roadkils Disk Image <https://www.roadkil.net/program.php?ProgramID=12>`_

Linux

::

  gunzip -c bytesatwork-minimal-image-bytedevkit-stm32mp1.wic.gz | dd of=/dev/mmcblk<X> bs=8M conv=fdatasync status=progress

.. Hint:: To improve write performance, you could use bmap-tools under Linux:

  ``bmaptool copy bytesatwork-minimal-image-bytedevkit-stm32mp1.wic.gz /dev/mmcblk<X>``

----

How do you build an image?
==========================

Use ``repo`` to download all necessary repositories:

::

   $ mkdir -p ~/workdir/bytedevkit-stm32mp1/3.1; cd ~/workdir/bytedevkit-stm32mp1/3.1
   $ repo init -u https://github.com/bytesatwork/bsp-platform-st.git -b dunfell
   $ repo sync

If those commands are completed successfully, the following command
will set up a Yocto Project environment for byteDEVKIT-stm32mp1:

::

   $ cd ~/workdir/bytedevkit-stm32mp1/3.1
   $ MACHINE=bytedevkit-stm32mp1 DISTRO=poky-bytesatwork EULA=1 . setup-environment build

The final command builds the development image:

::

   $ cd $BUILDDIR
   $ bitbake bytesatwork-minimal-image

The output is found in:

::

   ~/workdir/bytedevkit-stm32mp1/3.1/build/tmp/deploy/images/bytedevkit-stm32mp1

.. Hint:: For additional information about yocto images and how to build them, please visit:
          https://docs.yoctoproject.org/3.1.11/brief-yoctoprojectqs/brief-yoctoprojectqs.html#building-your-image.

How to modify the image
-----------------------

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
-----------------------

If you want to rename or copy an image, simply rename or copy the image recipe by:

   ::

    $ cd ~/workdir/<machine name>/<yocto version>/build/tmp/deploy/images/<machine name>
    $ cp bytesatwork-minimal-image.bb customer-example-image.bb


Troubleshooting
---------------

-  **Image size is too small**

   If you encounter that your image size is too small to install additional software,
   please have a look at the ``IMAGE_ROOTFS_SIZE`` variable under
   ``~/workdir/<machine-name>/<yocto version>/sources/meta-bytesatwork/recipes-core/images/bytesatwork-minimal-image.bb``.
   Increase the size if necessary.

----

*********
Toolchain
*********


How do you install the toolchain?
=================================

Simply download the toolchain and execute the downloaded file, which is
a self-extracting shell script.

.. Hint:: If you encounter problems when trying to install the toolchain, make sure the downloaded toolchain is executable. Run ``chmod +x /<path>/<toolchain-file>.sh`` to make it executable.

.. Important::
   The following tools need to be installed on your development system:
      * ``xz`` (Debian package: ``xz-utils``)
      * ``python`` (any version)
      * ``gcc``

----

How do you use the toolchain?
=============================

Source the installed toolchain:

::

   source /opt/poky-bytesatwork/3.1.11/environment-setup-cortexa7t2hf-neon-vfpv4-poky-linux-gnueabi

Check if Cross-compiler is available in environment:

::

   echo $CC

You should see the following output:

::

   arm-poky-linux-gnueabi-gcc -mthumb -mfpu=neon-vfpv4 -mfloat-abi=hard -mcpu=cortex-a7 -fstack-protector-strong -D_FORTIFY_SOURCE=2 -Wformat -Wformat-security -Werror=format-security --sysroot=/opt/poky-bytesatwork/3.1.11/sysroots/cortexa7t2hf-neon-vfpv4-poky-linux-gnueabi

Crosscompile the source code, e.g. by:

::

   $CC helloworld.c -o helloworld

Check generated binary:

::

   file helloworld

The output that is shown in prompt afterwards:

::

   helloworld: ELF 32-bit LSB pie executable, ARM, EABI5 version 1

----

How to bring your binary to the target?
=======================================

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

----

How do you build a toolchain?
=============================

::

   $ cd ~/workdir/bytedevkit-stm32mp1/3.1
   $ repo init -u https://github.com/bytesatwork/bsp-platform-st.git -b dunfell
   $ repo sync

If those commands are completed successfully, the following command
will set up a Yocto Project environment for byteDEVKIT-stm32mp1:

::

   $ cd ~/workdir/bytedevkit-stm32mp1/3.1
   $ MACHINE=bytedevkit-stm32mp1 DISTRO=poky-bytesatwork EULA=1 . setup-environment build

The final command builds an installable toolchain:

::

   $ cd $BUILDDIR
   $ bitbake bytesatwork-minimal-image -c populate_sdk

The toolchain is located under:

::

   ~/workdir/bytedevkit-stm32mp1/3.1/build/tmp/deploy/sdk

How to modify your toolchain
----------------------------

Currently the bytesatwork toolchain is generated out of the bytesatwork-minimal-image recipe. If you want to add additional libraries and development headers to customize the toolchain, you need to modify the bytesatwork-minimal-image recipe. It can be found under ``~/workdir/<machine name>/<yocto version>/sources/meta-bytesatwork/recipes-core/images``

For example if you want to develop your own ftp client and you need libftp and the corresponding header files, edit the recipe ``bytesatwork-minimal-image.bb`` and add ``ftplib`` to the ``IMAGE_INSTALL`` variable.

This will provide the ftplib libraries and development headers in the toolchain. After adding additional software components, the toolchain needs to be rebuilt by:

::

$ cd ~/workdir/<machine name>/<yocto version>
$ MACHINE=<machine> DISTRO=poky-bytesatwork EULA=1 . setup-environment build
$ bitbake bytesatwork-minimal-image -c populate_sdk

The newly generated toolchain will be available under:

::

~/workdir/<machine name>/<yocto version>/build/tmp/deploy/sdk

For additional information, please visit:
https://docs.yoctoproject.org/3.1.11/overview-manual/overview-manual-concepts.html#cross-development-toolchain-generation.


******
Kernel
******

.. _download-kernel-bytedevkit-stm32mp1-3.1:

Download the Linux Kernel
=========================

.. list-table::
    :header-rows: 1

    * - Device
      - Branch
      - git URL
    * - bytedevkit-stm32mp1
      - baw-v5.10-stm32mp-r1
      - https://github.com/bytesatwork/linux-stm32mp.git

----

Build the Linux Kernel
======================

For both targets, an ARM toolchain is necessary. You can use the
provided toolchain from :ref:`get-toolchain-bytedevkit-stm32mp1-3.1` or any compatible toolchain (e.g.
from your distribution)

.. Important::
   The following tools need to be installed on your development system:
      * ``git``
      * ``make``
      * ``bc``

.. Note::
        The following instructions assume, you installed the provided toolchain
        for the respective target.

.. Important::
   The following tools need to be installed on your development system:
      * OpenSSL headers (Debian package: ``libssl-dev``)
      * ``depmod`` (Debian package: ``kmod``)

#. Download kernel sources

   Download the appropriate kernel from :ref:`download-kernel-bytedevkit-stm32mp1-3.1`.

#. Source toolchain

   ::

      source /opt/poky-bytesatwork/3.1.11/environment-setup-cortexa7t2hf-neon-vfpv4-poky-linux-gnueabi

#. Create defconfig

   ::

      make multi_v7_defconfig
      scripts/kconfig/merge_config.sh -m -r .config arch/arm/configs/fragment-*
      make olddefconfig

#. Build Linux kernel

   ::

      make LOADADDR=0xC2000040 -j `nproc` uImage stm32mp157c-bytedevkit.dtb modules

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
       * - ``arch/arm/boot/dts/stm32mp157c-bytedevkit.dtb``
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

       mkdir /tmp/bytedevkit-stm32mp1
       make INSTALL_MOD_PATH=/tmp/bytedevkit-stm32mp1 modules_install

   Now you can copy the content of the folder ``/tmp/bytedevkit-stm32mp1`` into the
   target's root folder (``/``) which is partition ``/dev/mmcblk0p5``.

******
U-Boot
******

   .. _download-uboot-bytedevkit-stm32mp1-3.1:

Download U-Boot
===============

   .. list-table::
        :header-rows: 1

        * - Device
          - Branch
          - git URL
        * - bytedevkit-stm32mp1
          - baw-v2020.01-stm32mp-r1
          - https://github.com/bytesatwork/u-boot-stm32mp

----

Build U-Boot
============

To compile U-Boot, an ARM toolchain is necessary. You can use the provided
toolchain from :ref:`get-toolchain-bytedevkit-stm32mp1-3.1` or any compatible
toolchain (e.g. from your distribution)

   .. Important::
        The following tools need to be installed on your development system:
         * ``git``
         * ``make``
         * ``bc``

   .. Note::
        The following instructions assume, you installed the provided toolchain
        for the respective target.

#. Download U-Boot sources

   Download the appropriate U-Boot from :ref:`download-uboot-bytedevkit-stm32mp1-3.1`.

#. Source toolchain

   ::

        source /opt/poky-bytesatwork/3.1.11/environment-setup-cortexa7t2hf-neon-vfpv4-poky-linux-gnueabi

#. Create defconfig

   ::

        make stm32mp157_bytedevkit_defconfig

   .. Note::
        For the 1 GB RAM variant, use ``make stm32mp157_bytedevkit_1g_defconfig`` instead.

#. Build U-Boot and SPL

   ::

        make -j `nproc`


.. _install-spl-uboot-bytedevkit-stm32mp1-3.1:

Install SPL and U-Boot
======================

   To use the newly created U-Boot, the necessary files need to be installed
   on the SD card. This can be done either on the host or on the target.

   .. list-table::
        :header-rows: 1

        * - File
          - Target partition
        * - ``u-boot-spl.stm32``
          - ``/dev/mmcblk0p1``
        * - ``u-boot-spl.stm32``
          - ``/dev/mmcblk0p2``
        * - ``u-boot.img``
          - ``/dev/mmcblk0p3``

   You need to write the to the respective "raw" partition, either on the host
   system or the target system:

   ::

        dd if=u-boot-spl.stm32 of=/dev/mmcblk0p1
        dd if=u-boot-spl.stm32 of=/dev/mmcblk0p2
        dd if=u-boot.img of=/dev/mmcblk0p3

   The next time the target is reset, it will start with the new U-Boot.

.. This is the footer, don't edit after this
.. image:: ../../images/wiki_footer.jpg
   :align: center
   :target: https://www.bytesatwork.io
