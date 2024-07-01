System Resource Evaluation
==========================

.. contents::
  :local:
  :depth: 2


This section will explain how to calculate used system resource in GCC
project.

Memory Section
--------------

We can find the memory configuration in “rtl8735b_ram.ld”:

-  .ram.code_rodata : This section is read-only data in SRAM

-  .ram.data : This section is read-write data in SRAM

-  .ram.bss : This section is data has no initial values in SRAM

-  .ddr.rodata : This section is read-only data in DDR Memory

-  .ddr.data : This section is read-write data in DDR Memory

-  .ddr.bss : This section is data has no initial values in DDR Memory

Memory Size
-----------

User can refer “application.ntz.map” to observe them after project
build. This file can be found in
“project/realtek_amebapro2_v0_example/GCC-RELEASE/build/application”
folder.

Memory Size in SRAM
~~~~~~~~~~~~~~~~~~~

There are several sections in SRAM, including “.ram.code_text”,
“.ram.data”, “.ram.code_rodata”, “.ram.bss” and “.non_secure.bss”. We
can sum up these sections:

“.ram.code_text” has size 0xdef0:

.. code-block:: bash

    .ram.code_text  0x0000000020100b00               0xdef0
                    0x0000000020100b00                . = ALIGN (0x4)
                    0x0000000020100b00                __etext2 = .
                    0x0000000020100b00                . = ALIGN (0x20)
                    0x0000000020100b00                __ram_entry_text_start__ = .



“.ram.data” has size 0x9b4:

.. code-block:: bash

    .ram.data       0x000000002010e9f0                0x9b4
                    0x000000002010e9f0                __fw_img_start__ = .
                    0x000000002010e9f0                __etext = .
                    0x000000002010e9f0                __data_start__ = .



“.ram.code_rodata” has size 0x5d8:

.. code-block:: bash

    .ram.code_rodata
                    0x000000002010f3a8                0x5d8
                    0x000000002010f3a8                . = ALIGN (0x4)
                    0x000000002010f3a8                __ram_code_rodata_start__ = .



“.ram.bss” has size 0x139fc:

.. code-block:: bash

    .ram.bss        0x000000002010f980                0x139fc
                    0x000000002010f980                . = ALIGN (0x4)
                    0x000000002010f980                __bss_start__ = .



“.non_secure.bss” has size 0x4:

.. code-block:: bash

    .non_secure.bss
                    0x000000002012337c                0x4
                    0x0000000020123380                . = ALIGN (0x10)



So user totally use 0xdef0 + 0x9b4 + 0x5d8 + 0x139fc + 0x4 = 0x2287c =
138KB memory in SRAM.

And the total SRAM space is defined at
project\\realtek_amebapro2_v0_example\\GCC-RELEASE\\application\\rtl8735b_ram.ld:

.. code-block:: bash

   RAM (rwx) : ORIGIN = 0x20100B00, LENGTH = 0x20177B00 - 0x20100B00 /* 476KB */

For this case, the SRAM total size is 476KB. So user still have free
SRAM space about 476KB - 138KB = 338KB.

Memory Size in SRAM Retention
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

For memory used in SRAM retention, we should check “.ram.retention.data”.

“.ram.retention.data” has size 0xd70:

.. code-block:: bash

    .ram.retention.data
                    0x0000000020120000      0xd70
                    0x0000000020120000                __retention_start__ = .
     *(.retention.table*)
     *(.retention.data*)
     .retention.data
                    0x0000000020120000      0xd70 libwlan.a(hal_wowlan_8735b.c.obj)
                    0x0000000020120000                g_wlan_resume_parm
                    0x0000000020120d70                __retention_end__ = .


And the total SRAM retention space is defined at
project\\realtek_amebapro2_v0_example\\GCC-RELEASE\\application\\rtl8735b_ram.ld:

.. code-block:: bash

   RAM_RETENTION (rwx) : ORIGIN = 0x20120000, LENGTH = 0x20140000 - 0x20120000 /* 128KB retention data */

For this case, the SRAM retention total size is 128KB. So user still have free SRAM retention space about 128KB - 3KB = 125KB.

Memory Size in DDR Memory (ERAM)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

There are several sections in DDR Memory, “.ddr.bss”, “.ddr.text”, “.ddr.data” and “.ddr.rodata”. We can sum up these sections:

“.ddr.bss” has size 0x1424d8:

.. code-block:: bash

    .ddr.bss        0x0000000070100000                0x1424d8
                    0x0000000070100000                . = ALIGN (0x4)
                    0x0000000070100000                __eram_bss_start__ = .


“.ddr.text” has size 0x7d278:

.. code-block:: bash

    .ddr.text       0x00000000702424e0               0x7d278
                    0x00000000702424e0                . = ALIGN (0x4)
                    0x00000000702424e0                __eram_text_start__ = .


“.ddr.data” has size 0x4e08:

.. code-block:: bash

    .ddr.data        0x00000000702bf79c              0x4e08
                     0x00000000702bf79c              . = ALIGN (0x4)
                     0x00000000702bf79c              __eram_data_start__ = .


“.ddr.rodata” has size 0x512a:

.. code-block:: bash

    .ddr.rodata     0x00000000702c45a4               0x512af
                    0x00000000702c45a4               . = ALIGN (0x4)
                    0x00000000702c45a4               __eram_rodata_start__ = .


we cannot calculate the heap usage now. However, we can get its value
after running an application on AmebaPro2.

DDR memory total size = .ddr.bss + .ddr.text + .ddr.data + .ddr.rodata +
heap usage + heap available

We can get the heap available size by entering AT command – “ATW?” in
uart console:

.. code-block:: bash

   [MEM] After do cmd, available heap 55318304

For this example, the heap available size is 54893248 = 53606 KB = 52 MB

And the total ERAM space is defined at
project\\realtek_amebapro2_v0_example\\GCC-RELEASE\\application\\rtl8735b_ram.ld:

.. code-block:: bash

   DDR (rwx) : ORIGIN = 0x70100000, LENGTH = 0x73900000 - 0x70100000 /* 56MB */

For this case, the DDR memory total size is 0x73900000 - 0x70100000 = 0x3800000 = 56MB.

So we can calculate the heap usage now:

heap usage = DDR memory total size - .ddr.bss - .ddr.text - .ddr.data -
.ddr.rodata - heap available = 0x3800000(56MB) - 0x1424d8 - 0x7d278 -
0x4e08 - 0x512af - 54893248 = 1,641,785 B = 1.6 MB

Code Size
---------

The size of flash_ntz.bin:

.. code-block:: bash

   $ ls -al GCC-RELEASE/build/flash_ntz.bin
   -rw-r--r-- 1 user Domain Users 4599808 May 27 16:11 flash_ntz.bin

For this case, the code size is about 4.4MB

CPU Utilization
---------------

CPU utilization can be evaluated by AT command - “ATSS” in uart console.

It will show the amount of time each task has spent in the Running state
(how much CPU time each task has consumed).

.. code-block:: bash

   [ATSS]: _AT_SYSTEM_CPU_STATS_
   log_service 161 <1%
   IDLE 4983981 99%
   Tmr Svc 0 <1%
   TCP_IP 0 <1%
   cmd_thread 1246 <1%
   rtw_interru 0 <1%
   rtw_recv_ta 0 <1%
   rtw_xmit_ta 0 <1%
