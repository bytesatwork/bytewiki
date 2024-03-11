###############################
byteDEVKIT-am62x (Yocto 4.0)
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
    * - `bytesatwork-minimal-image-bytedevkit-am62x.wic.gz <https://download.bytesatwork.io/transfer/bytesatwork/bytedevkit-am62x/4.0.9/bytesatwork-minimal-image-bytedevkit-am62x.wic.gz>`_
      - 0747dfb463edad01cd3bf7985bed602e717b1dfa2f09258ed6860c37b57c67cb
    * - `bytesatwork-minimal-image-bytedevkit-am62x.wic.bmap <https://download.bytesatwork.io/transfer/bytesatwork/bytedevkit-am62x/4.0.9/bytesatwork-minimal-image-bytedevkit-am62x.wic.bmap>`_
      - 3577b6bc71600903fcba120629a50f5595e25f9ceb63d6301efb3f46d3848115

.. Hint:: Updating from an older image?
   You can update your older image by using: ``apt-get update`` and ``apt-get upgrade``.

   #. check for new version in the table above
   #. edit ``/etc/apt/sources.list`` and point to the new package feed
   #. run ``apt-get update; apt-get upgrade``

   As the yocto framework is based on several packages from various projects or suppliers, it is not guaranteed that
   an incremental upgrade by ``apt-get upgrade`` works automatically. Some manual adjustments might be needed.


.. _get-toolchain-bytedevkit-am62x-4.0:

Toolchain
=========

.. list-table::
    :header-rows: 1

    * - Download
      - Checksum (SHA256)
    * - `poky-bytesatwork-glibc-x86_64-bytesatwork-minimal-image-aarch64-bytedevkit-am62x-toolchain-4.0.9.sh <https://download.bytesatwork.io/transfer/bytesatwork/bytedevkit-am62x/4.0.9/poky-bytesatwork-glibc-x86_64-bytesatwork-minimal-image-aarch64-bytedevkit-am62x-toolchain-4.0.9.sh>`_
      - a5e9e6706cbff94fb3e31b41e948cbe1665cabca457e1bf337c59d45d6616c82


U-Boot
======

.. list-table::
     :header-rows: 1

     * - Description
       - Download
       - Checksum (SHA256)
     * - SPL R5F
       - `tiboot3.bin <https://download.bytesatwork.io/transfer/bytesatwork/bytedevkit-am62x/4.0.9/tiboot3.bin>`_
       - 53481b110634d711c43c47db40b2cfbce8b993cc6b63892d204d6563f35ea690
     * - SPL A53
       - `tispl.bin <https://download.bytesatwork.io/transfer/bytesatwork/bytedevkit-am62x/4.0.9/tispl.bin>`_
       - ee581879fba5a58dc872395eda734e5fe4d5bfdc4a4eb48b7e09b21991827908
     * - U-Boot A53
       - `u-boot.img <https://download.bytesatwork.io/transfer/bytesatwork/bytedevkit-am62x/4.0.9/u-boot.img>`_
       - 7c14d88c61772c3bb36d4d1441eee46f3d64f4d5d5abbb1b0ba2a264247a20aa



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

   #. Unzip the file ``bytesatwork-minimal-image-bytedevkit-am62x.wic.gz`` (e.g. with 7-zip)
   #. Write the resulting file to the microSD card with a tool like `Roadkils Disk Image <https://www.roadkil.net/program.php?ProgramID=12>`_

Linux

::

  gunzip -c bytesatwork-minimal-image-bytedevkit-am62x.wic.gz | dd of=/dev/mmcblk<X> bs=8M conv=fsync status=progress

.. Hint:: To improve write performance, you could use bmap-tools under Linux:

  ``bmaptool copy bytesatwork-minimal-image-bytedevkit-am62x.wic.gz /dev/mmcblk<X>``

----

How do you build an image?
==========================

Use ``repo`` to download all necessary repositories:

::

   $ mkdir -p ~/workdir/bytedevkit-am62x/4.0; cd ~/workdir/bytedevkit-am62x/4.0
   $ repo init -b kirkstone -u https://github.com/bytesatwork/bsp-platform-ti.git
   $ repo sync

If those commands are completed successfully, the following command
will set up a Yocto Project environment for byteDEVKIT-am62x:

::

   $ cd ~/workdir/bytedevkit-am62x/4.0
   $ MACHINE=bytedevkit-am62x DISTRO=poky-bytesatwork EULA=1 . setup-environment build

The final command builds the development image:

::

   $ cd $BUILDDIR
   $ bitbake bytesatwork-minimal-image

The output is found in:

::

   ~/workdir/bytedevkit-am62x/4.0/build/tmp/deploy/images/bytedevkit-am62x

.. Hint:: For additional information about yocto images and how to build them, please visit:
          https://docs.yoctoproject.org/4.0.9/brief-yoctoprojectqs/index.html#building-your-image.

How to modify the image
-----------------------

  The image recipes can be found in ``~/workdir/bytedevkit-am62x/4.0/sources/meta-bytesatwork/recipes-core/images``

  This is relative to where you started the ``repo`` command to fetch all the sources.

  Edit the minimal-image recipe ``bytesatwork-minimal-image.bb``

  Add the desired software-package to ``IMAGE_INSTALL`` variable, for example add ``net-tools`` to ``bytesatwork-minimal-image.bb``

  Rebuild the image by:

  ::

    $ cd ~/workdir/bytedevkit-am62x/4.0
    $ MACHINE=bytedevkit-am62x DISTRO=poky-bytesatwork EULA=1 . setup-environment build
    $ bitbake bytesatwork-minimal-image


How to rename the image
-----------------------

If you want to rename or copy an image, simply rename or copy the image recipe by:

   ::

    $ cd ~/workdir/bytedevkit-am62x/4.0/sources/meta-bytesatwork/recipes-core/images
    $ cp bytesatwork-minimal-image.bb customer-example-image.bb


Troubleshooting
---------------

-  **Image size is too small**

   If you encounter that your image size is too small to install additional software,
   please have a look at the ``IMAGE_ROOTFS_SIZE`` variable under
   ``~/workdir/bytedevkit-am62x/4.0/sources/meta-bytesatwork/recipes-core/images/bytesatwork-minimal-image.bb``.
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

   source /opt/poky-bytesatwork/4.0.9/environment-setup-aarch64-poky-linux

Check if Cross-compiler is available in environment:

::

   echo $CC

You should see the following output:

::

   aarch64-poky-linux-gcc -fstack-protector-strong -O2 -D_FORTIFY_SOURCE=2 -Wformat -Wformat-security -Werror=format-security --sysroot=/opt/poky-bytesatwork/4.0.9_bytedevkit-am62x/sysroots/aarch64-poky-linux

Crosscompile the source code, e.g. by:

::

   $CC helloworld.c -o helloworld

Check generated binary:

::

   file helloworld

The output that is shown in prompt afterwards:

::

   helloworld: ELF 64-bit LSB pie executable, ARM aarch64, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-aarch64.so.1, BuildID[sha1]=257792938c3ed4fbf6b15d071c60973ab51b2f37, for GNU/Linux 3.14.0, with debug_info, not stripped

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

   $ cd ~/workdir/bytedevkit-am62x/4.0
   $ repo init -b kirkstone -u https://github.com/bytesatwork/bsp-platform-ti.git
   $ repo sync

If those commands are completed successfully, the following command
will set up a Yocto Project environment for byteDEVKIT-am62x:

::

   $ cd ~/workdir/bytedevkit-am62x/4.0
   $ MACHINE=bytedevkit-am62x DISTRO=poky-bytesatwork EULA=1 . setup-environment build

The final command builds an installable toolchain:

::

   $ cd $BUILDDIR
   $ bitbake bytesatwork-minimal-image -c populate_sdk

The toolchain is located under:

::

   ~/workdir/bytedevkit-am62x/4.0/build/tmp/deploy/sdk

How to modify your toolchain
----------------------------

Currently the bytesatwork toolchain is generated out of the bytesatwork-minimal-image recipe. If you want to add additional libraries and development headers to customize the toolchain, you need to modify the bytesatwork-minimal-image recipe. It can be found under ``~/workdir/bytedevkit-am62x/4.0/sources/meta-bytesatwork/recipes-core/images``

For example if you want to develop your own ftp client and you need libftp and the corresponding header files, edit the recipe ``bytesatwork-minimal-image.bb`` and add ``ftplib`` to the ``IMAGE_INSTALL`` variable.

This will provide the ftplib libraries and development headers in the toolchain. After adding additional software components, the toolchain needs to be rebuilt by:

::

$ cd ~/workdir/bytedevkit-am62x/4.0
$ MACHINE=bytedevkit-am62x DISTRO=poky-bytesatwork EULA=1 . setup-environment build
$ bitbake bytesatwork-minimal-image -c populate_sdk

The newly generated toolchain will be available under:

::

~/workdir/bytedevkit-am62x/4.0/build/tmp/deploy/sdk

For additional information, please visit:
https://docs.yoctoproject.org/4.0.9/overview-manual/concepts.html#cross-development-toolchain-generation.


******
Kernel
******

.. _download-kernel-bytedevkit-am62x-4.0:

Download the Linux Kernel
=========================

.. list-table::
    :header-rows: 1

    * - Device
      - Branch
      - git URL
    * - bytedevkit-am62x
      - baw-ti-linux-6.1.y
      - https://github.com/bytesatwork/ti-linux-kernel

----

Build the Linux Kernel
======================

For both targets, an ARM toolchain is necessary. You can use the
provided toolchain from :ref:`get-toolchain-bytedevkit-am62x-4.0` or any compatible toolchain (e.g.
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

   Download the appropriate kernel from :ref:`download-kernel-bytedevkit-am62x-4.0`.

#. Source toolchain

   ::

      source /opt/poky-bytesatwork/4.0.9/environment-setup-aarch64-poky-linux

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
         - ``/boot/k3-am62x-bytedevkit.dtb``
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

   .. _download-uboot-source-bytedevkit-am62x-4.0:

Download U-Boot Source Code
===========================

   .. list-table::
        :header-rows: 1

        * - Device
          - Branch
          - git URL
        * - bytedevkit-am62x
          - baw-ti-u-boot-2023.04
          - https://github.com/bytesatwork/u-boot-ti

----

Install SPL and U-Boot
======================

   To use the newly created U-Boot, the necessary files need to be installed
   on the SD card. This can be done either on the host or on the target.

   .. list-table::
        :header-rows: 1

        * - File
          - Target partition
          - Target partition label
          - File system
        * - ``tiboot3.bin`` ``tispl.bin`` ``u-boot.img``
          - ``/dev/mmcblk1p1``
          - ``boot``
          - FAT32

   You need to copy the files to the boot partition. The example assumes that the boot partition is
   mounted on ``/media/user/boot``:

   ::

        cp tiboot3.bin tispl.bin u-boot.img /media/user/boot/


   The next time the target is reset, it will start with the new U-Boot.

.. This is the footer, don't edit after this
.. image:: https://www.bytesatwork.io/wp-content/uploads/2020/04/Bildschirmfoto-2020-04-20-um-19.41.44.jpg
   :scale: 100%
   :align: center
