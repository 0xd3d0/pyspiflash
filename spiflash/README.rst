pyspiflash
==========

SPI data flash device drivers (pure Python)

SPI flash devices, also known as *DataFlash* are commonly found in embedded
products, to store firmware, microcode or configuration parameters.

PySpiFlash_ comes with several pure Python drivers for those flash devices, that
demonstrate use of SPI devices with PyFtdi_. It could also be useful to dump
flash contents or recover from a bricked devices.

.. _PySpiFlash : https://github.com/eblot/pyspiflash
.. _PyFtdi : https://github.com/eblot/pyftdi

Supported SPI flash devices
---------------------------

============= ======= ========== ======== ====== ======= ========== ========== ==========
Vendor        Atmel   Atmel      Macronix SST    Winbond Eon        Numonix     Micron
------------- ------- ---------- -------- ------ ------- ---------- ---------- ----------
DataFlash     AT45_   AT25_      MX25L_   SST25_ W25Q_   EN25Q      M25P       N25Q
============= ======= ========== ======== ====== ======= ========== ========== ==========
Status        Tested  Tested     Tested   Tested Tested  Not tested Not tested Tested
------------- ------- ---------- -------- ------ ------- ---------- ---------- ----------
Sizes (MiB)       2,4      2,4,8 2,4,8,16    2,4     2,4                       8
------------- ------- ---------- -------- ------ ------- ---------- ---------- ----------
Read (KiB/s)     1278       1279     1329    642    1252                       1315
------------- ------- ---------- -------- ------ ------- ---------- ---------- ----------
Write (KiB/s)      56         64       71      2      63                       107
------------- ------- ---------- -------- ------ ------- ---------- ---------- ----------
Erase (KiB/s)      60         63       31    500      60                       84
============= ======= ========== ======== ====== ======= ========== ========== ==========

Notes about performances
........................

* *Read* operation is synchronous with SPI bus clock: it therefore only depends
  on the achievable frequency on the SPI bus, which is bound to the highest
  supported frequency of the flash device.
* *Write* operation depends mostly on the flash device performance, whose upper
  limit comes mostly from the maximum write packet size of the device, as the
  device needs to be polled for completion after each packet: the shorter the
  packet, the higher traffic on the SPI and associated overhead.
* *Erase* operation depends mostly on the flash device performance, whose fully 
  depends on the flash device internal technology, as very few and short
  packets are exchanged over the SPI bus.

Supporting new flash devices of series '25'
...........................................
Many flash devices support a common subset to for read/write/erase operations.
Critical differences appear with lock and protection features, and with
security features. An NDA is often required to obtain details about the
advanced security features of these devices.

It should be nevertheless quite easy to add support for new flash device
variants:
 
* ``match`` method in the PyFtdi flash device API should be the first to look
  at to detect more compatible flash devices.

.. _AT45: http://www.adestotech.com/sites/default/files/datasheets/doc8784.pdf
.. _AT25: http://www.atmel.com/Images/doc8693.pdf
.. _SST25: http://ww1.microchip.com/downloads/en/DeviceDoc/25071A.pdf
.. _MX25L: http://www.mxic.com.tw/
.. _W25Q: http://www.nexflash.com/hq/enu/ProductAndSales/ProductLines/FlashMemory/SerialFlash/

Supported SPI flash commands
----------------------------

Identification
  The SPI device driver is automatically selected based on the detected SPI
  flash device

Read
  Read byte sequences of any size, starting at any location from the SPI
  flash device

Write
  Write arbitrary byte sequences of any size, starting at any location to the
  SPI flash device

Erase
  Erase SPI flash device blocks, whose size depend on the capabilities of the
  flash device, typically 4KiB and/or 64KiB.

Unlock
  Unlock any protected flash device sectors

Dependencies
------------

PySpiFlash_ relies on PyFtdi_ module to access the SPI flash device. The
internal API of PyFtdi_ has been changed with version 0.11.0.

==================== ===============
PySpiFlash_ version  PyFtdi_ version
-------------------- ---------------
0.2.*                0.9 .. 0.10
0.3.*                0.11+
==================== ===============
