###############################
byteDEVKIT-imx8mm (Yocto 4.0)
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
    * - `bytesatwork-minimal-image-bytedevkit-imx8mm.wic.gz <https://download.bytesatwork.io/transfer/bytesatwork/bytedevkit-imx8mm/4.0.9/bytesatwork-minimal-image-bytedevkit-imx8mm.wic.gz>`_
      - 99ce54bf379fc97c11157bc48fa0a4fb91ac5f1776968e3bfe2a45471b878427
    * - `bytesatwork-minimal-image-bytedevkit-imx8mm.wic.bmap <https://download.bytesatwork.io/transfer/bytesatwork/bytedevkit-imx8mm/4.0.9/bytesatwork-minimal-image-bytedevkit-imx8mm.wic.bmap>`_
      - c94c9177bf80a56fb493acd79df8d677cc7b11d70ea6b7b97256647c161872b4

.. Hint:: Updating from an older image?
   You can update your older image by using: ``apt-get update`` and ``apt-get upgrade``.

   #. check for new version in the table above
   #. edit ``/etc/apt/sources.list`` and point to the new package feed
   #. run ``apt-get update; apt-get upgrade``

   As the yocto framework is based on several packages from various projects or suppliers, it is not guaranteed that
   an incremental upgrade by ``apt-get upgrade`` works automatically. Some manual adjustments might be needed.


.. _get-toolchain-bytedevkit-imx8mm-4.0:

Toolchain
=========

.. list-table::
    :header-rows: 1

    * - Download
      - Checksum (SHA256)
    * - `poky-bytesatwork-glibc-x86_64-bytesatwork-minimal-image-cortexa53-crypto-bytedevkit-imx8mm-toolchain-4.0.9.sh <https://download.bytesatwork.io/transfer/bytesatwork/bytedevkit-imx8mm/4.0.9/poky-bytesatwork-glibc-x86_64-bytesatwork-minimal-image-cortexa53-crypto-bytedevkit-imx8mm-toolchain-4.0.9.sh>`_
      - b558c84d3030628daa4d227ba122a3a4f5deccf476d291bd3584222b38c8427f


U-Boot
======

.. list-table::
     :header-rows: 1

     * - Description
       - Download
       - Checksum (SHA256)
     * - U-Boot (SD-card)
       - `imx-boot-bytedevkit-imx8mm-sd.bin-flash_evk <https://download.bytesatwork.io/transfer/bytesatwork/bytedevkit-imx8mm/4.0.9/imx-boot-bytedevkit-imx8mm-sd.bin-flash_evk>`_
       - ee2bddafa023d6c84b59474cd783b46fa3bfac7301ba8765d37486dd833b3d0a



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

   #. Unzip the file ``bytesatwork-minimal-image-bytedevkit-imx8mm.wic.gz`` (e.g. with 7-zip)
   #. Write the resulting file to the microSD card with a tool like `Roadkils Disk Image <https://www.roadkil.net/program.php?ProgramID=12>`_

Linux

::

  gunzip -c bytesatwork-minimal-image-bytedevkit-imx8mm.wic.gz | dd of=/dev/mmcblk<X> bs=8M conv=fsync status=progress

.. Hint:: To improve write performance, you could use bmap-tools under Linux:

  ``bmaptool copy bytesatwork-minimal-image-bytedevkit-imx8mm.wic.gz /dev/mmcblk<X>``

----

How do you build an image?
==========================

Use ``repo`` to download all necessary repositories:

::

   $ mkdir -p ~/workdir/bytedevkit-imx8mm/4.0; cd ~/workdir/bytedevkit-imx8mm/4.0
   $ repo init -b kirkstone -u https://github.com/bytesatwork/bsp-platform-nxp.git
   $ repo sync

If those commands are completed successfully, the following command
will set up a Yocto Project environment for byteDEVKIT-imx8mm:

::

   $ cd ~/workdir/bytedevkit-imx8mm/4.0
   $ MACHINE=bytedevkit-imx8mm DISTRO=poky-bytesatwork EULA=1 . setup-environment build

The final command builds the development image:

::

   $ cd $BUILDDIR
   $ bitbake bytesatwork-minimal-image

The output is found in:

::

   ~/workdir/bytedevkit-imx8mm/4.0/build/tmp/deploy/images/bytedevkit-imx8mm

.. Hint:: For additional information about yocto images and how to build them, please visit:
          https://docs.yoctoproject.org/4.0.9/brief-yoctoprojectqs/index.html#building-your-image.

How to modify the image
-----------------------

  The image recipes can be found in ``~/workdir/bytedevkit-imx8mm/4.0/sources/meta-bytesatwork/recipes-core/images``

  This is relative to where you started the ``repo`` command to fetch all the sources.

  Edit the minimal-image recipe ``bytesatwork-minimal-image.bb``

  Add the desired software-package to ``IMAGE_INSTALL`` variable, for example add ``net-tools`` to ``bytesatwork-minimal-image.bb``

  Rebuild the image by:

  ::

    $ cd ~/workdir/bytedevkit-imx8mm/4.0
    $ MACHINE=bytedevkit-imx8mm DISTRO=poky-bytesatwork EULA=1 . setup-environment build
    $ bitbake bytesatwork-minimal-image


How to rename the image
-----------------------

If you want to rename or copy an image, simply rename or copy the image recipe by:

   ::

    $ cd ~/workdir/bytedevkit-imx8mm/4.0/sources/meta-bytesatwork/recipes-core/images
    $ cp bytesatwork-minimal-image.bb customer-example-image.bb


Troubleshooting
---------------

-  **Image size is too small**

   If you encounter that your image size is too small to install additional software,
   please have a look at the ``IMAGE_ROOTFS_SIZE`` variable under
   ``~/workdir/bytedevkit-imx8mm/4.0/sources/meta-bytesatwork/recipes-core/images/bytesatwork-minimal-image.bb``.
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

   source /opt/poky-bytesatwork/4.0.9/environment-setup-cortexa53-crypto-poky-linux

Check if Cross-compiler is available in environment:

::

   echo $CC

You should see the following output:

::

   aarch64-poky-linux-gcc -mcpu=cortex-a53 -march=armv8-a+crc+crypto -fstack-protector-strong -O2 -D_FORTIFY_SOURCE=2 -Wformat -Wformat-security -Werror=format-security --sysroot=/opt/poky-bytesatwork/4.0.9_bytedevkit-imx8mm/sysroots/cortexa53-crypto-poky-linux

Crosscompile the source code, e.g. by:

::

   $CC helloworld.c -o helloworld

Check generated binary:

::

   file helloworld

The output that is shown in prompt afterwards:

::

   helloworld: ELF 64-bit LSB pie executable, ARM aarch64, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-aarch64.so.1, BuildID[sha1]=c4a368203085c7897b632728f24bfa60eec34771, for GNU/Linux 3.14.0, with debug_info, not stripped

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

   $ cd ~/workdir/bytedevkit-imx8mm/4.0
   $ repo init -b kirkstone -u https://github.com/bytesatwork/bsp-platform-nxp.git
   $ repo sync

If those commands are completed successfully, the following command
will set up a Yocto Project environment for byteDEVKIT-imx8mm:

::

   $ cd ~/workdir/bytedevkit-imx8mm/4.0
   $ MACHINE=bytedevkit-imx8mm DISTRO=poky-bytesatwork EULA=1 . setup-environment build

The final command builds an installable toolchain:

::

   $ cd $BUILDDIR
   $ bitbake bytesatwork-minimal-image -c populate_sdk

The toolchain is located under:

::

   ~/workdir/bytedevkit-imx8mm/4.0/build/tmp/deploy/sdk

How to modify your toolchain
----------------------------

Currently the bytesatwork toolchain is generated out of the bytesatwork-minimal-image recipe. If you want to add additional libraries and development headers to customize the toolchain, you need to modify the bytesatwork-minimal-image recipe. It can be found under ``~/workdir/bytedevkit-imx8mm/4.0/sources/meta-bytesatwork/recipes-core/images``

For example if you want to develop your own ftp client and you need libftp and the corresponding header files, edit the recipe ``bytesatwork-minimal-image.bb`` and add ``ftplib`` to the ``IMAGE_INSTALL`` variable.

This will provide the ftplib libraries and development headers in the toolchain. After adding additional software components, the toolchain needs to be rebuilt by:

::

$ cd ~/workdir/bytedevkit-imx8mm/4.0
$ MACHINE=bytedevkit-imx8mm DISTRO=poky-bytesatwork EULA=1 . setup-environment build
$ bitbake bytesatwork-minimal-image -c populate_sdk

The newly generated toolchain will be available under:

::

~/workdir/bytedevkit-imx8mm/4.0/build/tmp/deploy/sdk

For additional information, please visit:
https://docs.yoctoproject.org/4.0.9/overview-manual/concepts.html#cross-development-toolchain-generation.


******
Kernel
******

.. _download-kernel-bytedevkit-imx8mm-4.0:

Download the Linux Kernel
=========================

.. list-table::
    :header-rows: 1

    * - Device
      - Branch
      - git URL
    * - bytedevkit-imx8mm
      - baw-lf-5.15.y
      - https://github.com/bytesatwork/linux-imx.git

----

Build the Linux Kernel
======================

For both targets, an ARM toolchain is necessary. You can use the
provided toolchain from :ref:`get-toolchain-bytedevkit-imx8mm-4.0` or any compatible toolchain (e.g.
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

   Download the appropriate kernel from :ref:`download-kernel-bytedevkit-imx8mm-4.0`.

#. Source toolchain

   ::

      source /opt/poky-bytesatwork/4.0.9/environment-setup-cortexa53-crypto-poky-linux

#. Create defconfig

   ::

      make bytedevkit_imx8mm_defconfig

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
       * - ``arch/arm64/boot/dts/freescale/imx8mm-bytedevkit.dtb``
         - ``/boot/imx8mm-bytedevkit.dtb``
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

       mkdir /tmp/bytedevkit-imx8mm
       make INSTALL_MOD_PATH=/tmp/bytedevkit-imx8mm modules_install

   Now you can copy the content of the folder ``/tmp/bytedevkit-imx8mm`` into the
   target's root folder (``/``) which is partition ``/dev/mmcblk1p1``.

******
U-Boot
******

   Additional information can be found under
   https://www.nxp.com/docs/en/user-guide/IMX_LINUX_USERS_GUIDE.pdf and
   https://docs.u-boot.org/en/latest/board/nxp/index.html.

   .. Note::
      On i.MX 8M Mini, SPL and U-Boot are combined in a container file called
      ``flash.bin`` (Yocto: ``imx-boot-bytedevkit-imx8mm-sd.bin-flash_evk``).

   .. _download-uboot-source-bytedevkit-imx8mm-4.0:

Download U-Boot Source Code
===========================

   .. list-table::
        :header-rows: 1

        * - Device
          - Branch
          - git URL
        * - bytedevkit-imx8mm
          - baw-imx_v2020.04_5.4.24_2.1.0
          - https://github.com/bytesatwork/u-boot-imx

----

Build U-Boot
============

To compile U-Boot, an ARM toolchain is necessary. You can use the provided
toolchain from :ref:`get-toolchain-bytedevkit-imx8mm-4.0` or any compatible
toolchain (e.g. from your distribution)

   .. Important::

        A list of needed host tools can be found here
        https://docs.u-boot.org/en/latest/build/gcc.html#building-with-gcc,
        e.g.

        ::

            sudo apt install bc bison build-essential coccinelle \
            device-tree-compiler dfu-util efitools flex gdisk graphviz imagemagick \
            liblz4-tool libgnutls28-dev libguestfs-tools libncurses-dev \
            libpython3-dev libsdl2-dev libssl-dev lz4 lzma lzma-alone openssl \
            pkg-config python3 python3-asteval python3-coverage python3-filelock \
            python3-pkg-resources python3-pycryptodome python3-pyelftools \
            python3-pytest python3-pytest-xdist python3-sphinxcontrib.apidoc \
            python3-sphinx-rtd-theme python3-subunit python3-testtools \
            python3-virtualenv swig uuid-dev

        ``fspi_packer.sh`` additionally needs the package ``xxd`` to be
        installed on your host:

        ::

            sudo apt install xxd

   .. Note::
        The following instructions assume, you installed the provided toolchain
        for the respective target.

#. Download ARM-Trusted-Firmware sources

   .. list-table::
        :header-rows: 1

        * - Device
          - Branch
          - git URL
        * - bytedevkit-imx8mm
          - imx_5.4.24_2.1.0
          - https://github.com/nxp-imx/imx-atf

#. Build ARM-Trusted-Firmware

   ::

      cd imx-atf
      export CROSS_COMPILE=/opt/poky-bytesatwork/4.0.9/sysroots/x86_64-pokysdk-linux/usr/bin/aarch64-poky-linux/aarch64-poky-linux-
      make PLAT=imx8mm bl31
      cd ..

#. Download IMX Firmware

   ::

      wget https://www.nxp.com/lgfiles/NMG/MAD/YOCTO/firmware-imx-8.15.bin
      chmod +x firmware-imx-8.15.bin
      ./firmware-imx-8.15.bin

#. Download U-Boot sources

   Download the appropriate U-Boot from :ref:`download-uboot-source-bytedevkit-imx8mm-4.0`.

#. Source toolchain

   ::

        source /opt/poky-bytesatwork/4.0.9/environment-setup-cortexa53-crypto-poky-linux

#. Copy necessary files into U-Boot folder

   ::

      cp -pv ./firmware-imx-8.15/firmware/ddr/synopsys/lpddr4_pmu_train_* ./u-boot-imx/
      cp -pv ./imx-atf/build/imx8mm/release/bl31.bin ./u-boot-imx/

#. Build ``flash.bin``

   * SD Card

      ::

         cd u-boot-imx
         make distclean
         make bytedevkit_defconfig
         export ATF_LOAD_ADDR=0x920000
         make -j `nproc`
         make -j `nproc` flash.bin
         cd ..

   * SPI

      Building for SPI requires IMX mkimage tool

      ::

         git clone -b lf-5.15.5_1.0.0 https://github.com/nxp-imx/imx-mkimage.git

      ::

         cd u-boot-imx
         make distclean
         make bytedevkit_fspi_defconfig
         export ATF_LOAD_ADDR=0x920000
         make -j `nproc`
         make -j `nproc` flash.bin
         ../imx-mkimage/scripts/fspi_packer.sh ../imx-mkimage/scripts/fspi_header 0
         cd ..

   .. Important::

      The build command will overwrite the generated ``flash.bin``, so you
      can not build a binary for the SD Card and the SPI at the same time.

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

            Yocto: ``imx-boot-bytedevkit-imx8mm-sd.bin-flash_evk``
          - ``/dev/mmcblk1`` (or ``/dev/sdX``)
          - 33 KiB

   You need to write the files to the respective "raw" partition, either on the host
   system or the target system:

   ::

      dd if=./u-boot-imx/flash.bin of=/dev/mmcblk1 bs=1K seek=33

   The next time the target is reset, it will start with the new U-Boot.

   .. Note::

      Flash to SPI

      #. Copy flash.bin to first SD card partition (root partition)

      #. You need to boot into u-boot.

      #. In the u-boot shell: ``run update-spi``

      #. Or do it manually by

         ::

            sf probe; sf erase 0 0x200000; load mmc 1:1 ${loadaddr} flash.bin; sf write $loadaddr 0 $filesize


.. This is the footer, don't edit after this
.. image:: ../../images/wiki_footer.jpg
   :align: center
   :target: https://www.bytesatwork.io
