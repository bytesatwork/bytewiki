########################################
byteDEVKIT-STM32MP2x OSM (Debian Trixie)
########################################

*********
Downloads
*********


SD card image
=============

.. list-table::
    :header-rows: 1

    * - Download
      - Checksum (SHA256)
    * - `debian13_stm32mp2.img.zst <https://download.bytesatwork.io/transfer/bytesatwork/debian/debian13_stm32mp2.img.zst>`_
      - 4a03f6aadfe308167894c3f04cc7e121b0f445889a36a777e033ec86a95c195b



*****
Image
*****

.. Hint::
  - The provided image is only compatible with the byteDEVKIT OSM-S Rev 2.0 or higher.
  - If you have a different byteDEVKIT OSM-S Revision, contact us for support via `support@bytesatwork.ch <mailto:support@bytesatwork.ch>`_

How do you flash the image?
===========================

.. Attention::
  - You need a microSD card with **at least 8GB** capacity.
  - **All existing data** on the microSD card will be lost.
  - **Do not format** the microSD card before flashing.

Windows

   #. Unzip the file ``debian13_stm32mp2.img.zst`` (e.g. with 7-zip)
   #. Write the resulting file to the microSD card with a tool like `Roadkils Disk Image <https://www.roadkil.net/program.php?ProgramID=12>`_

Linux

::

  zstdcat -dc debian13_stm32mp2.img.zst | dd of=/dev/mmcblk<X> bs=8M conv=fsync status=progress


----

Default Credentials
===================

.. Note::
   The default user is baw with password baw



.. This is the footer, don't edit after this
.. image:: ../images/wiki_footer.jpg
   :align: center
   :target: https://www.bytesatwork.io
