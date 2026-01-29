###############################
byteDEVKIT-imx93 (Debian Trixie)
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
    * - `debian13_m9.img.zst <https://download.bytesatwork.io/transfer/bytesatwork/debian/debian13_m9.img.zst>`_
      - 82e2f3f0d7d8d166d3b0cfec8d426ba4c36ba5015c2eb8c145cab75dfd7a1c1c



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

   #. Unzip the file ``debian13_m9.img.zst`` (e.g. with 7-zip)
   #. Write the resulting file to the microSD card with a tool like `Roadkils Disk Image <https://www.roadkil.net/program.php?ProgramID=12>`_

Linux

::

  zstd -d debian13_m9.img.zst | dd of=/dev/mmcblk<X> bs=8M conv=fsync status=progress


----

Default Credentials
===================

.. Note::
   The default user is baw with password baw



.. This is the footer, don't edit after this
.. image:: ../images/wiki_footer.jpg
   :align: center
   :target: https://www.bytesatwork.io
