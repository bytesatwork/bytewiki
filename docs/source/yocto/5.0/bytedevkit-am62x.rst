###############################
byteDEVKIT-am62x (Yocto 5.0.11)
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
    * - `bytesatwork-minimal-image-bytedevkit-am62x.rootfs.wic.gz <https://download.bytesatwork.io/transfer/bytesatwork/bytedevkit-am62x/5.0.11/bytesatwork-minimal-image-bytedevkit-am62x.rootfs.wic.gz>`_
      - 2af364645e97cd57b42cd079a3ccc116597f69e33cd403b98ae9286d8d5d1b02
    * - `bytesatwork-minimal-image-bytedevkit-am62x.rootfs.wic.bmap <https://download.bytesatwork.io/transfer/bytesatwork/bytedevkit-am62x/5.0.11/bytesatwork-minimal-image-bytedevkit-am62x.rootfs.wic.bmap>`_
      - ca399c52fd846eff93f9a0235d1931417ac96e9e8c581a2490e558fd866fa44e


.. _get-toolchain-bytedevkit-am62x-5.0:

Toolchain
=========

.. list-table::
    :header-rows: 1

    * - Download
      - Checksum (SHA256)
    * - `poky-bytesatwork-glibc-x86_64-bytesatwork-minimal-image-aarch64-bytedevkit-am62x-toolchain-5.0.11.sh <https://download.bytesatwork.io/transfer/bytesatwork/bytedevkit-am62x/5.0.11/poky-bytesatwork-glibc-x86_64-bytesatwork-minimal-image-aarch64-bytedevkit-am62x-toolchain-5.0.11.sh>`_
      - d6932a500cfe07a607e8f0559596c93dab5c9b82a09bada4fe73e794f17db7ab


U-Boot
======

.. list-table::
     :header-rows: 1

     * - Description
       - Download
       - Checksum (SHA256)
     * - SPL R5F
       - `tiboot3.bin <https://download.bytesatwork.io/transfer/bytesatwork/bytedevkit-am62x/5.0.11/tiboot3.bin>`_
       - 0306b6bea65472cdbb85d7462dea38ac0f19e95a5eae114c7adfb9eb5c5d8f5b
     * - SPL A53
       - `tispl.bin <https://download.bytesatwork.io/transfer/bytesatwork/bytedevkit-am62x/5.0.11/tispl.bin>`_
       - f3162bc35ecb9571e66496c74bd17e62dccb63bd45d30567e3330e7f5b8a63cd
     * - U-Boot A53
       - `u-boot.img <https://download.bytesatwork.io/transfer/bytesatwork/bytedevkit-am62x/5.0.11/u-boot.img>`_
       - c9e1787fb4f0087c404292109aa81ced8ff7d71356ede3c6b1c36206b1ccb786




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

   #. Unzip the file ``bytesatwork-minimal-image-bytedevkit-am62x.rootfs.wic.gz`` (e.g. with 7-zip)
   #. Write the resulting file to the microSD card with a tool like `Roadkils Disk Image <https://www.roadkil.net/program.php?ProgramID=12>`_

Linux

::

  gunzip -c bytesatwork-minimal-image-bytedevkit-am62x.rootfs.wic.gz | dd of=/dev/mmcblk<X> bs=8M conv=fsync status=progress

.. Hint:: To improve write performance, you could use bmap-tools under Linux:

  ``bmaptool copy bytesatwork-minimal-image-bytedevkit-am62x.rootfs.wic.gz /dev/mmcblk<X>``

----

How do you build an image?
==========================

Use ``repo`` to download all necessary repositories:

::

   $ mkdir -p ~/workdir/bytedevkit-am62x/5.0; cd ~/workdir/bytedevkit-am62x/5.0
   $ repo init -b scarthgap -u https://github.com/bytesatwork/bsp-platform-ti.git
   $ repo sync

If those commands are completed successfully, the following command
will set up a Yocto Project environment for bytedevkit-am62x:

::

   $ cd ~/workdir/bytedevkit-am62x/5.0
   $ MACHINE=bytedevkit-am62x DISTRO=poky-bytesatwork EULA=1 . setup-environment build

The final command builds the development image:

::

   $ cd $BUILDDIR
   $ bitbake bytesatwork-minimal-image

The output is found in:

::

   ~/workdir/bytedevkit-am62x/5.0/build/deploy-ti/images/bytedevkit-am62x

.. Hint:: For additional information about yocto images and how to build them, please visit:
          https://docs.yoctoproject.org/5.0.11/brief-yoctoprojectqs/index.html#building-your-image.

How to modify the image
-----------------------

  The image recipes can be found in ``~/workdir/bytedevkit-am62x/5.0/sources/meta-bytesatwork/recipes-core/images``

  This is relative to where you started the ``repo`` command to fetch all the sources.

  Edit the minimal-image recipe ``bytesatwork-minimal-image.bb``

  Add the desired software-package to ``IMAGE_INSTALL`` variable, for example add ``net-tools`` to ``bytesatwork-minimal-image.bb``

  Rebuild the image by:

  ::

    $ cd ~/workdir/bytedevkit-am62x/5.0
    $ MACHINE=bytedevkit-am62x DISTRO=poky-bytesatwork EULA=1 . setup-environment build
    $ bitbake bytesatwork-minimal-image


How to rename the image
-----------------------

If you want to rename or copy an image, simply rename or copy the image recipe by:

   ::

    $ cd ~/workdir/bytedevkit-am62x/5.0/sources/meta-bytesatwork/recipes-core/images
    $ cp bytesatwork-minimal-image.bb customer-example-image.bb


Troubleshooting
---------------

-  **Image size is too small**

   If you encounter that your image size is too small to install additional software,
   please have a look at the ``IMAGE_ROOTFS_SIZE`` variable under
   ``~/workdir/bytedevkit-am62x/5.0/sources/meta-bytesatwork/recipes-core/images/bytesatwork-minimal-image.bb``.
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

   source /opt/poky-bytesatwork/5.0.11/environment-setup-aarch64-poky-linux

Check if Cross-compiler is available in environment:

::

   echo $CC

You should see the following output:

::

      aarch64-poky-linux-gcc -mbranch-protection=standard -fstack-protector-strong -O2 -D_FORTIFY_SOURCE=2 -Wformat -Wformat-security -Werror=format-security --sysroot=/opt/poky-bytesatwork/5.0.11/sysroots/aarch64-poky-linux

Crosscompile the source code, e.g. by:

::

   $CC helloworld.c -o helloworld

Check generated binary:

::

   file helloworld

The output that is shown in prompt afterwards:

::

   helloworld: ELF 64-bit LSB pie executable, ARM aarch64, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-aarch64.so.1, BuildID[sha1]=b127b388d6abe4d5ad5638b6e01fc91dc6e86e1a, for GNU/Linux 5.15.0, with debug_info, not stripped
   
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

   $ cd ~/workdir/bytedevkit-am62x/5.0
   $ repo init -b scarthgap -u https://github.com/bytesatwork/bsp-platform-ti.git
   $ repo sync

If those commands are completed successfully, the following command
will set up a Yocto Project environment for bytedevkit-am62x:

::

   $ cd ~/workdir/bytedevkit-am62x/5.0
   $ MACHINE=bytedevkit-am62x DISTRO=poky-bytesatwork EULA=1 . setup-environment build

The final command builds an installable toolchain:

::

   $ cd $BUILDDIR
   $ bitbake bytesatwork-minimal-image -c populate_sdk

The toolchain is located under:

::

   ~/workdir/bytedevkit-am62x/5.0/build/deploy-ti/sdk

How to modify your toolchain
----------------------------

Currently the bytesatwork toolchain is generated out of the bytesatwork-minimal-image recipe. If you want to add additional libraries and development headers to customize the toolchain, you need to modify the bytesatwork-minimal-image recipe. It can be found under ``~/workdir/bytedevkit-am62x/5.0/sources/meta-bytesatwork/recipes-core/images``

For example: if you want to develop your own application utilizing CAN communication and need libsocketcan and the corresponding header files, edit the recipe ``bytesatwork-minimal-image.bb`` and add ``libsocketcan`` to the ``IMAGE_INSTALL`` variable.

This will provide the libsocketcan libraries and development headers in the toolchain. After adding additional software components, the toolchain needs to be rebuilt by:

::

$ cd ~/workdir/bytedevkit-am62x/5.0
$ MACHINE=bytedevkit-am62x DISTRO=poky-bytesatwork EULA=1 . setup-environment build
$ bitbake bytesatwork-minimal-image -c populate_sdk

The newly generated toolchain will be available under:

::

~/workdir/bytedevkit-am62x/5.0/build/deploy-ti/sdk

For additional information, please visit:
https://docs.yoctoproject.org/5.0.11/overview-manual/concepts.html#cross-development-toolchain-generation.


******
Kernel
******

.. _download-kernel-bytedevkit-am62x-5.0:

Download the Linux Kernel
=========================

.. list-table::
    :header-rows: 1

    * - Device
      - Branch
      - git URL
    * - bytedevkit-am62x
      - baw-ti-linux-6.12.y
      - https://github.com/bytesatwork/ti-linux-kernel

----

Build the Linux Kernel
======================

For both targets, an ARM toolchain is necessary. You can use the
provided toolchain from :ref:`get-toolchain-bytedevkit-am62x-5.0` or any compatible toolchain (e.g.
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

   Download the appropriate kernel from :ref:`download-kernel-bytedevkit-am62x-5.0`.

#. Source toolchain

   ::

      source /opt/poky-bytesatwork/5.0.11/environment-setup-aarch64-poky-linux

#. Create defconfig

   ::

      make bytedevkit_am62x_defconfig

#. Build Linux kernel

   ::

      make -j `nproc` Image dtbs modules

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
       * - ``arch/arm64/boot/Image``
         - ``/boot/Image``
         - ``/dev/mmcblk1p2``
       * - ``arch/arm64/boot/dts/ti/k3-am625-bytedevkit.dtb``
         - ``/boot/k3-am625-bytedevkit.dtb``
         - ``/dev/mmcblk1p2``

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

       mkdir /tmp/bytedevkit-am62x
       make INSTALL_MOD_PATH=/tmp/bytedevkit-am62x modules_install

   Now you can copy the content of the folder ``/tmp/bytedevkit-am62x`` into the
   target's root folder (``/``) which is partition ``/dev/mmcblk1p2``.

******
U-Boot
******

   .. _download-uboot-source-bytedevkit-am62x-5.0:

Download U-Boot Source Code
===========================

   .. list-table::
        :header-rows: 1

        * - Device
          - Branch
          - git URL
        * - bytedevkit-am62x
          - baw-ti-u-boot-2025.01
          - https://github.com/bytesatwork/u-boot-ti

----

Build U-Boot
======================

#. Install and get Dependencies

   - `Cross toolchain <https://software-dl.ti.com/processor-sdk-linux/esd/AM62X/10_01_10_04/exports/docs/linux/Overview/GCC_ToolChain.html#linux-devkit>`_
   - `TI-linux-firmware <https://software-dl.ti.com/processor-sdk-linux/esd/AM62X/10_01_10_04/exports/docs/devices/AM62X/linux/Release_Specific_Release_Notes.html#ti-linux-firmware>`_
   - `TF-A <https://software-dl.ti.com/processor-sdk-linux/esd/AM62X/10_01_10_04/exports/docs/devices/AM62X/linux/Release_Specific_Release_Notes.html#tf-a>`_
   - `OP-TEE <https://software-dl.ti.com/processor-sdk-linux/esd/AM62X/10_01_10_04/exports/docs/devices/AM62X/linux/Release_Specific_Release_Notes.html#op-tee>`_

   .. Hint::

      Probably some tools are missing on your host:

         - A list can be found here
           https://docs.u-boot.org/en/latest/build/gcc.html#building-with-gcc

         - A non-exhaustive list of (additional) necessary tools

           ::

            sudo apt install bison flex swig libssl-dev python3-setuptools \
            python-dev python3-dev python3-yaml python3-jsonschema

#. Build TF-A

   `TI TF-A build instructions <https://software-dl.ti.com/processor-sdk-linux/esd/AM62X/10_01_10_04/exports/docs/linux/Foundational_Components_ATF.html#arm-trusted-firmware-a>`_

#. Build OP-TEE

   `TI OP-TEE build instructions <https://software-dl.ti.com/processor-sdk-linux/esd/AM62X/10_01_10_04/exports/docs/linux/Foundational_Components_OPTEE.html#op-tee>`_

#. Build u-boot

   You should have downloaded TI-linux-firmware and built TF-A, OP-TEE OS already.

   `TI u-boot build instructions <https://software-dl.ti.com/processor-sdk-linux/esd/AM62X/10_01_10_04/exports/docs/linux/Foundational_Components/U-Boot/UG-General-Info.html#general-information>`_

   .. Important::
      Use ``am62x_bytedevkit_r5_defconfig`` and ``am62x_bytedevkit_a53_defconfig`` instead of the TI
      defconfigs.

   .. Note::
      Clean command: ``make ARCH=arm CROSS_COMPILE=aarch64-linux-gnu- O=<your_dir> distclean``

Install SPL and U-Boot
======================

SD Card
-------

   To use the newly created U-Boot, the necessary files need to be installed on
   the SD card. This can be done either on the host or on the target.

   .. list-table::
      :header-rows: 1

      * - File
        - Target partition
        - Target partition label
        - File system
      * - ``tiboot3.bin`` ``tispl.bin`` ``u-boot.img``
        - ``/dev/mmcblk1p1`` (or ``/dev/sdX``)
        - ``boot``
        - FAT32

   You need to copy the files to the boot partition. The example assumes that the boot partition is
   mounted on ``/media/${USER}/boot``:

   ::

      cp tiboot3.bin tispl.bin u-boot.img /media/${USER}/boot/


   The next time the target is reset, it will start with the new U-Boot.

   .. Hint::
      Copy the related files to SD card, see end of section
      `TI u-boot build instructions <https://software-dl.ti.com/processor-sdk-linux/esd/AM62X/10_01_10_04/exports/docs/linux/Foundational_Components/U-Boot/UG-General-Info.html#general-information>`_

eMMC via SD Card
----------------

   #. Copy the ``tiboot3.bin``, ``tispl.bin`` and ``u-boot.img`` to the SD Card rootfs partition.

   #. Program the ``tiboot3.bin``, ``tispl.bin`` and ``u-boot.img`` from the SD card to the eMMC.

      In the u-boot shell ``run update_emmc``

      Or manually by following commands

      ::

         mmc dev 0 1
         load mmc 1:2 ${loadaddr} tiboot3.bin
         mmc write ${loadaddr} 0x0 0x400
         load mmc 1:2 ${loadaddr} tispl.bin
         mmc write ${loadaddr} 0x400 0xC00
         load mmc 1:2 ${loadaddr} u-boot.img
         mmc write ${loadaddr} 0x1000 0x1000
         mmc dev 0 0

   .. Note::

      The bootloader needs to be stored in the boot0 hardware partition of the eMMC.
      The layout of boot0 is defined so that it fits within 4 MiB, defined in blocks
      of 512 Bytes:

      .. list-table::
         :header-rows: 1

         * - File
           - start
           - end
           - size
         * - ``tiboot3.bin``
           - 0x0000
           - 0x0400
           - 0x0400 512 KiB
         * - ``tispl.bin``
           - 0x0400
           - 0x1000
           - 0x0C00 1536 KiB
         * - ``u-boot.img``
           - 0x1000
           - 0x2000
           - 0x1000 2048 KiB

.. This is the footer, don't edit after this
.. image:: ../../images/wiki_footer.jpg
   :align: center
   :target: https://www.bytesatwork.io
