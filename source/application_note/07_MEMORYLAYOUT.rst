Memory Layout
=============

.. contents::
  :local:
  :depth: 2

Programming Space
-----------------

================= ======== ================= ===============
**Start Address** **Size** **Cache Support** **IP Function**
================= ======== ================= ===============
0x0000_0000       32 KB    -                 ITCM ROM
0x0001_0000       128 KB   -                 ITCM RAM
================= ======== ================= ===============

Data Space
----------

================= ======== ================= ===============
**Start Address** **Size** **Cache Support** **IP Function**
================= ======== ================= ===============
0x2000_0000       16 KB    -                 DTCM RAM
0x2001_0000       80 KB    -                 DTCM ROM
================= ======== ================= ===============

Extension Memory Space
----------------------

======== ======================= ======== =====================
**Name** **Physical Address**    **Size** **IP Function**
======== ======================= ======== =====================
Flash    0x0800_0000~0x0FFF_FFFF 128 MB   External flash memory
DRAM     0x0700_0000~0x7FFF_FFFF 128 MB   Extended DRAM memory
======== ======================= ======== =====================

.. note :: The external flash memory address base is from 0x0800_0000 to 0x0FFF_FFFF

.. note :: The extended DRAM address base is from 0x7000_0000 to 0x7FFF_FFFF

.. note :: Both of flash and DRAM access are cacheable design

SRAM Retention 
---------------

================= ======== ================= ===============
**Start Address** **Size** **Cache Support** **IP Function**
================= ======== ================= ===============
0x2012_0000       128 KB   -                 SRAM
================= ======== ================= ===============
