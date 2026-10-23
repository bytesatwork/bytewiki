####################################
byteDEVKIT-imx93 OSM (Debian Trixie)
####################################

*********
Downloads
*********


SD card image
=============

.. list-table::
    :header-rows: 1

    * - Download
      - Checksum (SHA256)
    * - `debian13_imx93.img.zst <https://download.bytesatwork.io/transfer/bytesatwork/debian/debian13_imx93.img.zst>`_
      - 44f233087a17828449dacdf7cb7f37d8d0c764c13ddaf624d797e5888b8c9c2f



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

   #. Unzip the file ``debian13_imx93.img.zst`` (e.g. with 7-zip)
   #. Write the resulting file to the microSD card with a tool like `Roadkils Disk Image <https://www.roadkil.net/program.php?ProgramID=12>`_

Linux

::

  zstd -dc debian13_imx93.img.zst | dd of=/dev/mmcblk<X> bs=8M conv=fsync status=progress


----

Default Credentials
===================

.. Note::
   The default user is baw with password baw



.. This is the footer, don't edit after this
.. image:: ../images/wiki_footer.jpg
   :align: center
   :target: https://www.bytesatwork.io
