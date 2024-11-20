###############################
byteDEVKIT-stm32mp1 (Yocto 5.0)
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
    * - `bytesatwork-minimal-image-bytedevkit-stm32mp1.rootfs.wic.gz <https://download.bytesatwork.io/transfer/bytesatwork/bytedevkit-stm32mp1/5.0.3/bytesatwork-minimal-image-bytedevkit-stm32mp1.rootfs.wic.gz>`_
      - 6956ec4fadc9d1168019c40d85bc4838140647d1f452b54c95596a59158d16de
    * - `bytesatwork-minimal-image-bytedevkit-stm32mp1.rootfs.wic.bmap <https://download.bytesatwork.io/transfer/bytesatwork/bytedevkit-stm32mp1/5.0.3/bytesatwork-minimal-image-bytedevkit-stm32mp1.rootfs.wic.bmap>`_
      - 5a63717c31002634f2348c78dd9be141da4a90f4361b3369a6feba14fae47836


.. _get-toolchain-bytedevkit-stm32mp1-5.0:

Toolchain
=========

.. list-table::
    :header-rows: 1

    * - Download
      - Checksum (SHA256)
    * - `poky-bytesatwork-glibc-x86_64-bytesatwork-minimal-image-cortexa7t2hf-neon-vfpv4-bytedevkit-stm32mp1-toolchain-5.0.3.sh <https://download.bytesatwork.io/transfer/bytesatwork/bytedevkit-stm32mp1/5.0.3/poky-bytesatwork-glibc-x86_64-bytesatwork-minimal-image-cortexa7t2hf-neon-vfpv4-bytedevkit-stm32mp1-toolchain-5.0.3.sh>`_
      - f2376fc463bdbd92c40fce5ce788732f0486e780cb11013f88dea74c96372a99




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

   #. Unzip the file ``bytesatwork-minimal-image-bytedevkit-stm32mp1.rootfs.wic.gz`` (e.g. with 7-zip)
   #. Write the resulting file to the microSD card with a tool like `Roadkils Disk Image <https://www.roadkil.net/program.php?ProgramID=12>`_

Linux

::

  gunzip -c bytesatwork-minimal-image-bytedevkit-stm32mp1.rootfs.wic.gz | dd of=/dev/mmcblk<X> bs=8M conv=fsync status=progress

.. Hint:: To improve write performance, you could use bmap-tools under Linux:

  ``bmaptool copy bytesatwork-minimal-image-bytedevkit-stm32mp1.rootfs.wic.gz /dev/mmcblk<X>``

----

How do you build an image?
==========================

Use ``repo`` to download all necessary repositories:

::

   $ mkdir -p ~/workdir/bytedevkit-stm32mp1/5.0; cd ~/workdir/bytedevkit-stm32mp1/5.0
   $ repo init -b scarthgap -u https://github.com/bytesatwork/bsp-platform-st.git
   $ repo sync

If those commands are completed successfully, the following command
will set up a Yocto Project environment for byteDEVKIT-stm32mp1:

::

   $ cd ~/workdir/bytedevkit-stm32mp1/5.0
   $ MACHINE=bytedevkit-stm32mp1 DISTRO=poky-bytesatwork EULA=1 . setup-environment build

The final command builds the development image:

::

   $ cd $BUILDDIR
   $ bitbake bytesatwork-minimal-image

The output is found in:

::

   ~/workdir/bytedevkit-stm32mp1/5.0/build/tmp/deploy/images/bytedevkit-stm32mp1

.. Hint:: For additional information about yocto images and how to build them, please visit:
          https://docs.yoctoproject.org/5.0.3/brief-yoctoprojectqs/index.html#building-your-image.

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

    $ cd ~/workdir/<machine name>/<yocto version>/sources/meta-bytesatwork/recipes-core/images
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

   source /opt/poky-bytesatwork/5.0.3/environment-setup-cortexa7t2hf-neon-vfpv4-poky-linux-gnueabi

Check if Cross-compiler is available in environment:

::

   echo $CC

You should see the following output:

::

   arm-poky-linux-gnueabi-gcc -mthumb -mfpu=neon-vfpv4 -mfloat-abi=hard -mcpu=cortex-a7 -fstack-protector-strong -O2 -D_FORTIFY_SOURCE=2 -Wformat -Wformat-security -Werror=format-security -D_TIME_BITS=64 -D_FILE_OFFSET_BITS=64 --sysroot=/opt/poky-bytesatwork/5.0.3/sysroots/cortexa7t2hf-neon-vfpv4-poky-linux-gnueabi


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

   $ cd ~/workdir/bytedevkit-stm32mp1/5.0
   $ repo init -b scarthgap -u https://github.com/bytesatwork/bsp-platform-st.git
   $ repo sync

If those commands are completed successfully, the following command
will set up a Yocto Project environment for byteDEVKIT-stm32mp1:

::

   $ cd ~/workdir/bytedevkit-stm32mp1/5.0
   $ MACHINE=bytedevkit-stm32mp1 DISTRO=poky-bytesatwork EULA=1 . setup-environment build

The final command builds an installable toolchain:

::

   $ cd $BUILDDIR
   $ bitbake bytesatwork-minimal-image -c populate_sdk

The toolchain is located under:

::

   ~/workdir/bytedevkit-stm32mp1/5.0/build/tmp/deploy/sdk

How to modify your toolchain
----------------------------

Currently the bytesatwork toolchain is generated out of the bytesatwork-minimal-image recipe. If you want to add additional libraries and development headers to customize the toolchain, you need to modify the bytesatwork-minimal-image recipe. It can be found under ``~/workdir/<machine name>/<yocto version>/sources/meta-bytesatwork/recipes-core/images``

For example: if you want to develop your own application utilizing CAN communication and need libsocketcan and the corresponding header files, edit the recipe ``bytesatwork-minimal-image.bb`` and add ``libsocketcan`` to the ``IMAGE_INSTALL`` variable.

This will provide the libsocketcan libraries and development headers in the toolchain. After adding additional software components, the toolchain needs to be rebuilt by:

::

$ cd ~/workdir/<machine name>/<yocto version>
$ MACHINE=<machine> DISTRO=poky-bytesatwork EULA=1 . setup-environment build
$ bitbake bytesatwork-minimal-image -c populate_sdk

The newly generated toolchain will be available under:

::

~/workdir/<machine name>/<yocto version>/build/tmp/deploy/sdk

For additional information, please visit:
https://docs.yoctoproject.org/5.0.3/overview-manual/concepts.html#cross-development-toolchain-generation.


******
Kernel
******

.. _download-kernel-bytedevkit-stm32mp1-5.0:

Download the Linux Kernel
=========================

.. list-table::
    :header-rows: 1

    * - Device
      - Branch
      - git URL
    * - bytedevkit-stm32mp1
      - baw-v6.1-stm32mp
      - https://github.com/bytesatwork/linux-stm32mp.git

----

Build the Linux Kernel
======================

For both targets, an ARM toolchain is necessary. You can use the
provided toolchain from :ref:`get-toolchain-bytedevkit-stm32mp1-5.0` or any compatible toolchain (e.g.
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

   Download the appropriate kernel from :ref:`download-kernel-bytedevkit-stm32mp1-5.0`.

#. Source toolchain

   ::

      source /opt/poky-bytesatwork/5.0.3/environment-setup-cortexa7t2hf-neon-vfpv4-poky-linux-gnueabi

#. Create defconfig

   ::

      make multi_v7_defconfig
      scripts/kconfig/merge_config.sh -m -r .config arch/arm/configs/fragment-*
      make olddefconfig

#. Build Linux kernel

   ::

      make LOADADDR=0xC2000040 -j `nproc` uImage stm32mp157c-bytedevkit-v1-3.dtb modules

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
         - ``/dev/mmcblk0p7``
       * - ``arch/arm/boot/dts/stm32mp157c-bytedevkit-v1-3.dtb``
         - ``/boot/stm32mp157c-bytedevkit-v1-3.dtb``
         - ``/dev/mmcblk0p7``

   .. Note::
      After installing a new kernel, it often fails to load modules, as the
      _signature_ of the kernel changed and it fails to find its corresponding modules
      folder. This issue can often be resolved with a symlink:

      ::

        ln -s /lib/modules/<EXISTING FOLDER> /lib/modules/`uname -r`

     Otherwise, please follow the instructions to copy the kernel modules

   .. Hint::
      If you have a byteDEVKIT V1.1, replace ``v1-3`` with ``v1-1`` in the file names above.

#.  Install kernel modules

    To copy all available modules to the target, it's best to deploy them
    locally first and then copy all modules to the target.

    ::

       mkdir /tmp/bytedevkit-stm32mp1
       make INSTALL_MOD_PATH=/tmp/bytedevkit-stm32mp1 modules_install

   Now you can copy the content of the folder ``/tmp/bytedevkit-stm32mp1`` into the
   target's root folder (``/``) which is partition ``/dev/mmcblk0p7``.


******
U-Boot
******

.. _download-uboot-source-bytedevkit-stm32mp1-5.0:

Download U-Boot Source Code
===========================

   .. list-table::
        :header-rows: 1

        * - Device
          - Branch
          - git URL
        * - bytedevkit-stm32mp1
          - baw-v2022.10-stm32mp
          - https://github.com/bytesatwork/u-boot-stm32mp

----

Build U-Boot
============

#. Download U-Boot sources

   Download the appropriate U-Boot from :ref:`download-uboot-source-bytedevkit-stm32mp1-5.0`.

#. Source toolchain

   ::

        source /opt/poky-bytesatwork/5.0.3/environment-setup-cortexa7t2hf-neon-vfpv4-poky-linux-gnueabi

#. Create defconfig

   ::

        make stm32mp157_bytedevkit_defconfig

#. Build U-Boot

   ::

        make -j `nproc`

#. Download ATF sources

   .. list-table::
        :header-rows: 1

        * - Name
          - Branch
          - git URL
        * - Arm-Trusted-Firmware
          - baw-v2.8-stm32mp
          - https://github.com/bytesatwork/arm-trusted-firmware-stm32mp

#. Build ATF

   ::

        unset CFLAGS
        unset LDFLAGS
        make -j $(nproc) ARM_ARCH_MAJOR=7 ARCH=aarch32 PLAT=stm32mp1 STM32MP_SDMMC=1 STM32MP15=1 DTB_FILE_NAME=stm32mp157c-bytedevkit.dtb
        make -j $(nproc) ARM_ARCH_MAJOR=7 ARCH=aarch32 PLAT=stm32mp1 AARCH32_SP=optee DTB_FILE_NAME=stm32mp157c-bytedevkit.dtb dtbs

#. Download OP-TEE sources

   .. list-table::
        :header-rows: 1

        * - Name
          - Branch
          - git URL
        * - Optee
          - baw-3.19.9-stm32mp
          - https://github.com/bytesatwork/optee-os-stm32mp


#. Build OP-TEE

   ::

        unset LDFLAGS
        unset CFLAGS
        make -j $(nproc) PLATFORM=stm32mp1 CFG_EMBED_DTB_SOURCE_FILE=stm32mp157c-bytedevkit-v1-3.dts CFG_TEE_CORE_LOG_LEVEL=2 O=build all


Install U-Boot on SD card
=========================

To use the newly created U-Boot, the following files need to be installed on the
SD card:

   * BL2: ``tf-a-stm32mp157c-bytedevkit.stm32``
   * BL31: ``fip-stm32mp157c-bytedevkit-v1-3-optee.bin``


   For detailed information about the boot and build process see:
   `ST TF-A overview <https://wiki.stmicroelectronics.cn/stm32mpu/wiki/TF-A_overview>`_

#. Copy ``tf-a-stm32mp157c-bytedevkit.stm32`` from ATF build

   ::

        cp arm-trusted-firmware-stm32mp/build/stm32mp1/release/tf-a-stm32mp157c-bytedevkit.stm32 .


#. Create ``fip-stm32mp157c-bytedevkit-v1-3-optee.bin``

   ::

        fiptool create \
           --tos-fw optee-os-stm32mp/build/core/tee-header_v2.bin \
           --tos-fw-extra1 optee-os-stm32mp/build/core/tee-pager_v2.bin \
           --tos-fw-extra2 optee-os-stm32mp/build/core/tee-pageable_v2.bin \
           --hw-config u-boot-stm32mp/u-boot.dtb \
           --fw-config arm-trusted-firmware-stm32mp/build/stm32mp1/release/fdts/stm32mp157c-bytedevkit-fw-config.dtb \
           --nt-fw u-boot-stm32mp/u-boot-nodtb.bin \
           fip-stm32mp157c-bytedevkit-v1-3-optee.bin

   .. Important::
           If an error occurs, check the paths used in the command. They need to point to
           the u-boot, ATF and OP-TEE folder.

   .. Note::
           The program fiptool is installed in the toolchain:
           ``/opt/poky-bytesatwork/5.0.3/sysroots/x86_64-pokysdk-linux/usr/bin/fiptool``

#. Copy to SD card

   ::

        sudo dd if=tf-a-stm32mp157c-bytedevkit.stm32 of=/dev/sdX1 conv=fdatasync
        sudo dd if=tf-a-stm32mp157c-bytedevkit.stm32 of=/dev/sdX2 conv=fdatasync
        sudo dd if=fip-stm32mp157c-bytedevkit-v1-3-optee.bin of=/dev/sdX5 conv=fdatasync
        sudo dd if=fip-stm32mp157c-bytedevkit-v1-3-optee.bin of=/dev/sdX6 conv=fdatasync

   .. Note::
           Replace ``/dev/sdX`` with correct target device.


.. This is the footer, don't edit after this
.. image:: ../../images/wiki_footer.jpg
   :align: center
   :target: https://www.bytesatwork.io
