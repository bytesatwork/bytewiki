################################
byteDEVKIT-am62x (Debian Trixie)
################################

*********
Downloads
*********


SD card image
=============

.. list-table::
    :header-rows: 1

    * - Download
      - Checksum (SHA256)
    * - `debian13_am62x.img.zst <https://download.bytesatwork.io/transfer/bytesatwork/debian/debian13_am62x.img.zst>`_
      - 55e22e1b03268429dab0c048efd152a1fab08338fdd9f6ceeccf93fc82d5d889



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

   #. Unzip the file ``debian13_am62x.img.zst`` (e.g. with 7-zip)
   #. Write the resulting file to the microSD card with a tool like `Roadkils Disk Image <https://www.roadkil.net/program.php?ProgramID=12>`_

Linux

::

  zstdcat -dc debian13_am62x.img.zst | dd of=/dev/mmcblk<X> bs=8M conv=fsync status=progress


----

Default Credentials
===================

.. Note::
   The default user is baw with password baw



.. This is the footer, don't edit after this
.. image:: ../images/wiki_footer.jpg
   :align: center
   :target: https://www.bytesatwork.io
