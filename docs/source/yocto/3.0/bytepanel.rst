#####################
bytePANEL (Yocto 3.0)
#####################

*****
Image
*****

Where do you get the SD card image?
===================================

.. list-table::
    :header-rows: 1

    * - Device
      - Yocto Version
      - Download
      - Checksum (SHA256)
    * - bytePANEL
      - Yocto 3.0
      - `bytesatwork-minimal-image-bytepanel-emmc.wic.gz <https://download.bytesatwork.io/transfer/bytesatwork/m2/3.0/bytesatwork-minimal-image-bytepanel-emmc.wic.gz>`_
        (`wic.bmap
        <https://download.bytesatwork.io/transfer/bytesatwork/m2/3.0/bytesatwork-minimal-image-bytepanel-emmc.wic.bmap>`__)
      - e3e166f28fb815b09c6372bbcae4b4c8fcd00f93e57e96084bdee90c255764d9

.. Hint:: Updating from an older image?
   You can update your older image by using: ``apt-get update`` and ``apt-get upgrade``.

   #. check for new version in the table above
   #. edit ``/etc/apt/sources.list`` and point to the new package feed
   #. run ``apt-get update; apt-get upgrade``

   As the yocto framework is based on several packages from various projects or suppliers, it is not guaranteed that
   an incremental upgrade by ``apt-get upgrade`` works automatically. Some manual adjustments might be needed.

----

How do you flash the image?
===========================

.. Attention::
  - You need a microSD card with **at least 8GB** capacity.
  - **All existing data** on the microSD card will be lost.
  - **Do not format** the microSD card before flashing.

Windows

   #. Unzip the file ``bytesatwork-minimal-image-bytepanel-emmc.wic.gz`` (e.g. with 7-zip)
   #. Write the resulting file to the microSD card with a tool like `Roadkils Disk Image <https://www.roadkil.net/program.php?ProgramID=12>`_

Linux

::

  gunzip -c bytesatwork-minimal-image-bytepanel-emmc.wic.gz | dd of=/dev/mmcblk<X> bs=8M conv=fdatasync status=progress

.. Hint:: To improve write performance, you could use bmap-tools under Linux:

  bmaptool copy bytesatwork-minimal-image-bytepanel-emmc.wic.gz /dev/mmcblk<X>

----

How do you build an image?
==========================

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

.. _get-toolchain-bytepanel-3.0:

Where do you get the toolchain?
===============================

.. list-table::
    :header-rows: 1

    * - Device
      - Yocto Version
      - Download
      - Checksum (SHA256)
    * - bytePANEL
      - Yocto 3.0
      - `poky-bytesatwork-glibc-x86_64-bytesatwork-minimal-image-armv7at2hf-neon-bytepanel-emmc-toolchain-3.0.2.sh <https://download.bytesatwork.io/transfer/bytesatwork/m2/3.0/poky-bytesatwork-glibc-x86_64-bytesatwork-minimal-image-armv7at2hf-neon-bytepanel-emmc-toolchain-3.0.2.sh>`_
      - a90763d7ff408e9e5f0556b051eccd3ea85c43406099c9a61d98a32e6a04e078

----

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

For additional information, please visit: https://www.yoctoproject.org/docs/3.0.3/overview-manual/overview-manual.html#cross-development-toolchain-generation


******
Kernel
******

.. _download-kernel-bytepanel-3.0:

Download the Linux Kernel
=========================

.. list-table::
    :header-rows: 1

    * - Device
      - Branch
      - git URL
    * - bytePANEL
      - baw-ti-linux-4.19.y
      - https://github.com/bytesatwork/ti-linux-kernel.git

----

Build the Linux Kernel
======================

For both targets, an ARM toolchain is necessary. You can use the
provided toolchain from :ref:`get-toolchain-bytepanel-3.0` or any compatible toolchain (e.g.
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
      * ``u-boot-tools``

#. Download kernel sources

   Download the appropriate kernel from :ref:`download-kernel-bytepanel-3.0`.

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
         - ``/dev/mmcblk0p1``
       * - ``arch/arm/boot/dts/bytepanel.dtb``
         - ``/boot/devtree.dtb``
         - ``/dev/mmcblk0p1``

.. This is the footer, don't edit after this
.. image:: ../../images/wiki_footer.jpg
   :align: center
   :target: https://www.bytesatwork.io
