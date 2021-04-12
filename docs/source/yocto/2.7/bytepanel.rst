#####################
bytePANEL (Yocto 2.7)
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
      - Yocto 2.7
      - `devbase-image-bytesatwork-bytepanel-emmc-20190729194430.sdimg.gz <https://download.bytesatwork.io/transfer/bytesatwork/m2/2.7/devbase-image-bytesatwork-bytepanel-emmc-20190729194430.sdimg.gz>`_
      - 3b3e51d83c68f68d6ebbc2983d6b41b9e21d4878c1c9570804e6949624d7a41e

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

   #. Unzip the file ``devbase-image-bytesatwork-bytepanel-emmc-20190729194430.sdimg.gz`` (e.g. with 7-zip)
   #. Write the resulting file to the microSD card with a tool like `Roadkils Disk Image <https://www.roadkil.net/program.php?ProgramID=12>`_

Linux

::

  gunzip -c devbase-image-bytesatwork-bytepanel-emmc-20190729194430.sdimg.gz | dd of=/dev/mmcblk<X> bs=8M conv=fdatasync status=progress

----

How do you build an image?
==========================

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

-  **Image size is to small**

   If you encounter that your image size is to small to install additional software,
   please have a look at the ``IMAGE_ROOTFS_SIZE`` variable under
   ``~/workdir/<machine-name>/<yocto version>/sources/meta-bytesatwork/recipes-core/images/bytesatwork-minimal-image.bb``.
   Increase the size if necessary.

----

*********
Toolchain
*********

.. _get-toolchain-bytepanel-2.7:

Where do you get the toolchain?
===============================

.. list-table::
    :header-rows: 1

    * - Device
      - Yocto Version
      - Download
      - Checksum (SHA256)
    * - bytePANEL
      - Yocto 2.7
      - `poky-bytesatwork-glibc-x86_64-devbase-image-bytesatwork-armv7at2hf-neon-bytepanel-toolchain-2.7.3.sh <https://download.bytesatwork.io/transfer/bytesatwork/poky-bytesatwork-glibc-x86_64-devbase-image-bytesatwork-armv7at2hf-neon-bytepanel-toolchain-2.7.3.sh>`_
      - b25e4a3f764eaf583ad0e6a3e0edcac9a1a9314ab6d1f4aad290c415afdbe0e7

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
----------------------------

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

For additional information, please visit: https://www.yoctoproject.org/docs/2.7.4/overview-manual/overview-manual.html#cross-development-toolchain-generation


******
Kernel
******

.. _download-kernel-bytepanel-2.7:

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
provided toolchain from :ref:`get-toolchain-bytepanel-2.7` or any compatible toolchain (e.g.
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

   Download the appropriate kernel from :ref:`download-kernel-bytepanel-2.7`.

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
.. image:: https://www.bytesatwork.io/wp-content/uploads/2020/04/Bildschirmfoto-2020-04-20-um-19.41.44.jpg
   :scale: 100%
   :align: center
