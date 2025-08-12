###############################
byteDEVKIT-imx93 (Yocto 5.0)
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
    * - `bytesatwork-minimal-image-bytedevkit-imx93.rootfs.wic.gz <https://download.bytesatwork.io/transfer/bytesatwork/bytedevkit-imx93/5.0.11/bytesatwork-minimal-image-bytedevkit-imx93.rootfs.wic.gz>`_
      - 395c2119eb48ff9d4a1d50ec91f7cbe02eccdb2690bdb2e47809cea073e3ba3e
    * - `bytesatwork-minimal-image-bytedevkit-imx93.rootfs.wic.bmap <https://download.bytesatwork.io/transfer/bytesatwork/bytedevkit-imx93/5.0.11/bytesatwork-minimal-image-bytedevkit-imx93.rootfs.wic.bmap>`_
      - 60348771a88ff6b0b961bba6f4018baa3668572c2a50c98669cbe27589ec0696


.. _get-toolchain-bytedevkit-imx93-5.0:

Toolchain
=========

.. list-table::
    :header-rows: 1

    * - Download
      - Checksum (SHA256)
    * - `poky-bytesatwork-glibc-x86_64-bytesatwork-minimal-image-cortexa55-bytedevkit-imx93-toolchain-5.0.11.sh <https://download.bytesatwork.io/transfer/bytesatwork/bytedevkit-imx93/5.0.11/poky-bytesatwork-glibc-x86_64-bytesatwork-minimal-image-cortexa55-bytedevkit-imx93-toolchain-5.0.11.sh>`_
      - 5138c79b1e40429fa71b179ad5f5e374624ecaef42bbba70404d420fdd15b0a7


U-Boot
======

.. list-table::
     :header-rows: 1

     * - Description
       - Download
       - Checksum (SHA256)
     * - U-Boot (SD-card)
       - `imx-boot-bytedevkit-imx93-sd.bin-flash_singleboot <https://download.bytesatwork.io/transfer/bytesatwork/bytedevkit-imx93/5.0.11/imx-boot-bytedevkit-imx93-sd.bin-flash_singleboot>`_
       - 0eccf200c51e7b15efb87f34f69db7dc8426c06c51296e0374f51cf10f4271c6



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

   #. Unzip the file ``bytesatwork-minimal-image-bytedevkit-imx93.rootfs.wic.gz`` (e.g. with 7-zip)
   #. Write the resulting file to the microSD card with a tool like `Roadkils Disk Image <https://www.roadkil.net/program.php?ProgramID=12>`_

Linux

::

  gunzip -c bytesatwork-minimal-image-bytedevkit-imx93.rootfs.wic.gz | dd of=/dev/mmcblk<X> bs=8M conv=fsync status=progress

.. Hint:: To improve write performance, you could use bmap-tools under Linux:

  ``bmaptool copy bytesatwork-minimal-image-bytedevkit-imx93.rootfs.wic.gz /dev/mmcblk<X>``

----

How do you build an image?
==========================

Use ``repo`` to download all necessary repositories:

::

   $ mkdir -p ~/workdir/bytedevkit-imx93/5.0; cd ~/workdir/bytedevkit-imx93/5.0
   $ repo init -b scarthgap -u https://github.com/bytesatwork/bsp-platform-nxp.git
   $ repo sync

If those commands are completed successfully, the following command
will set up a Yocto Project environment for byteDEVKIT-imx93:

::

   $ cd ~/workdir/bytedevkit-imx93/5.0
   $ MACHINE=bytedevkit-imx93 DISTRO=poky-bytesatwork EULA=1 . setup-environment build

The final command builds the development image:

::

   $ cd $BUILDDIR
   $ bitbake bytesatwork-minimal-image

The output is found in:

::

   ~/workdir/bytedevkit-imx93/5.0/build/tmp/deploy/images/bytedevkit-imx93

.. Hint:: For additional information about yocto images and how to build them, please visit:
          https://docs.yoctoproject.org/5.0.11/brief-yoctoprojectqs/index.html#building-your-image.

How to modify the image
-----------------------

  The image recipes can be found in ``~/workdir/bytedevkit-imx93/5.0/sources/meta-bytesatwork/recipes-core/images``

  This is relative to where you started the ``repo`` command to fetch all the sources.

  Edit the minimal-image recipe ``bytesatwork-minimal-image.bb``

  Add the desired software-package to ``IMAGE_INSTALL`` variable, for example add ``net-tools`` to ``bytesatwork-minimal-image.bb``

  Rebuild the image by:

  ::

    $ cd ~/workdir/bytedevkit-imx93/5.0
    $ MACHINE=bytedevkit-imx93 DISTRO=poky-bytesatwork EULA=1 . setup-environment build
    $ bitbake bytesatwork-minimal-image


How to rename the image
-----------------------

If you want to rename or copy an image, simply rename or copy the image recipe by:

   ::

    $ cd ~/workdir/bytedevkit-imx93/5.0/sources/meta-bytesatwork/recipes-core/images
    $ cp bytesatwork-minimal-image.bb customer-example-image.bb


Troubleshooting
---------------

-  **Image size is too small**

   If you encounter that your image size is too small to install additional software,
   please have a look at the ``IMAGE_ROOTFS_SIZE`` variable under
   ``~/workdir/bytedevkit-imx93/5.0/sources/meta-bytesatwork/recipes-core/images/bytesatwork-minimal-image.bb``.
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

   source /opt/poky-bytesatwork/5.0.11/environment-setup-cortexa55-poky-linux

Check if Cross-compiler is available in environment:

::

   echo $CC

You should see the following output:

::

      aarch64-poky-linux-gcc -mcpu=cortex-a55+crypto -mbranch-protection=standard -fstack-protector-strong -O2 -D_FORTIFY_SOURCE=2 -Wformat -Wformat-security -Werror=format-security --sysroot=/opt/poky-bytesatwork/5.0.11/sysroots/cortexa55-poky-linux


Crosscompile the source code, e.g. by:

::

   $CC helloworld.c -o helloworld

Check generated binary:

::

   file helloworld

The output that is shown in prompt afterwards:

::

   helloworld: ELF 64-bit LSB pie executable, ARM aarch64, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-aarch64.so.1, BuildID[sha1]=2f109b4df123adb07897264729903d2b83cf32ab, for GNU/Linux 5.15.0, with debug_info, not stripped

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

   $ cd ~/workdir/bytedevkit-imx93/5.0
   $ repo init -b scarthgap -u https://github.com/bytesatwork/bsp-platform-nxp.git
   $ repo sync

If those commands are completed successfully, the following command
will set up a Yocto Project environment for byteDEVKIT-imx93:

::

   $ cd ~/workdir/bytedevkit-imx93/5.0
   $ MACHINE=bytedevkit-imx93 DISTRO=poky-bytesatwork EULA=1 . setup-environment build

The final command builds an installable toolchain:

::

   $ cd $BUILDDIR
   $ bitbake bytesatwork-minimal-image -c populate_sdk

The toolchain is located under:

::

   ~/workdir/bytedevkit-imx93/5.0/build/tmp/deploy/sdk

How to modify your toolchain
----------------------------

Currently the bytesatwork toolchain is generated out of the bytesatwork-minimal-image recipe. If you want to add additional libraries and development headers to customize the toolchain, you need to modify the bytesatwork-minimal-image recipe. It can be found under ``~/workdir/bytedevkit-imx93/5.0/sources/meta-bytesatwork/recipes-core/images``

For example: if you want to develop your own application utilizing CAN communication and need libsocketcan and the corresponding header files, edit the recipe ``bytesatwork-minimal-image.bb`` and add ``libsocketcan`` to the ``IMAGE_INSTALL`` variable.

This will provide the libsocketcan libraries and development headers in the toolchain. After adding additional software components, the toolchain needs to be rebuilt by:

::

$ cd ~/workdir/bytedevkit-imx93/5.0
$ MACHINE=bytedevkit-imx93 DISTRO=poky-bytesatwork EULA=1 . setup-environment build
$ bitbake bytesatwork-minimal-image -c populate_sdk

The newly generated toolchain will be available under:

::

~/workdir/bytedevkit-imx93/5.0/build/tmp/deploy/sdk

For additional information, please visit:
https://docs.yoctoproject.org/5.0.11/overview-manual/concepts.html#cross-development-toolchain-generation.


******
Kernel
******

.. _download-kernel-bytedevkit-imx93-5.0:

Download the Linux Kernel
=========================

.. list-table::
    :header-rows: 1

    * - Device
      - Branch
      - git URL
    * - bytedevkit-imx93
      - baw-lf-6.6.52-2.2.0
      - https://github.com/bytesatwork/linux-imx.git

----

Build the Linux Kernel
======================

For both targets, an ARM toolchain is necessary. You can use the
provided toolchain from :ref:`get-toolchain-bytedevkit-imx93-5.0` or any compatible toolchain (e.g.
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

   Download the appropriate kernel from :ref:`download-kernel-bytedevkit-imx93-5.0`.

#. Source toolchain

   ::

      source /opt/poky-bytesatwork/5.0.11/environment-setup-cortexa55-poky-linux

#. Create defconfig

   ::

      make bytedevkit_imx93_defconfig

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
         - ``/dev/mmcblk1p1``
       * - ``arch/arm64/boot/dts/freescale/imx93-bytedevkit.dtb``
         - ``/boot/imx93-bytedevkit.dtb``
         - ``/dev/mmcblk1p1``

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

       mkdir /tmp/bytedevkit-imx93
       make INSTALL_MOD_PATH=/tmp/bytedevkit-imx93 modules_install

   Now you can copy the content of the folder ``/tmp/bytedevkit-imx93`` into the
   target's root folder (``/``) which is partition ``/dev/mmcblk1p1``.

******
U-Boot
******

   Additional information can be found under
   https://www.nxp.com/docs/en/user-guide/IMX_LINUX_USERS_GUIDE.pdf and
   https://docs.u-boot.org/en/latest/board/nxp/index.html.

   .. Note::
      On i.MX 93, SPL and U-Boot are combined in a container file called
      ``flash.bin`` (Yocto: ``imx-boot-bytedevkit-imx93-sd.bin-flash_singleboot``).

   .. _download-uboot-source-bytedevkit-imx93-5.0:

Download U-Boot Source Code
===========================

   .. list-table::
        :header-rows: 1

        * - Device
          - Branch
          - git URL
        * - bytedevkit-imx93
          - baw-lf_v2023.04
          - https://github.com/bytesatwork/u-boot-imx

----

Install SPL and U-Boot
======================

   To use the newly created U-Boot, the necessary file needs to be installed
   on the SD card. This can be done either on the host or on the target.

   .. list-table::
        :header-rows: 1

        * - File
          - Target partition
          - Offset
        * - ``flash.bin``

            Yocto: ``imx-boot-bytedevkit-imx93-sd.bin-flash_singleboot``
          - ``/dev/mmcblk1`` (or ``/dev/sdX``)
          - 32 KiB

   You need to write the files to the respective "raw" partition, either on the host
   system or the target system:

   ::

      dd if=./u-boot-imx/flash.bin of=/dev/mmcblk1 bs=1K seek=32

   The next time the target is reset, it will start with the new U-Boot.


.. This is the footer, don't edit after this
.. image:: ../../images/wiki_footer.jpg
   :align: center
   :target: https://www.bytesatwork.io
