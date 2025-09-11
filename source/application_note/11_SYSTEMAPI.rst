System API
==========

.. contents::
  :local:
  :depth: 2

System reset
------------

Reset the system

.. code-block:: c

    /**
      * @brief  system software reset.
      * @retval none
      */
    void sys_reset(void)


Get boot select
---------------

Identify the system is boot from NAND or NOR flash

.. code-block:: c

    /**
      * @brief  Get boot select function.
      * @retval boot select device
      * @note
      *  BootFromNORFlash            = 0,
      *  BootFromNANDFlash           = 1,
      *  BootFromUART                = 2
      */
    uint8_t sys_get_boot_sel(void)


JTAG/SWD disable
----------------

JTAG/SWD is enabled by default. Turn off JTAG/SWD to release more pins
to the application

.. code-block:: c

    /**
      * @brief  Turn off the JTAG/SWD function.
      * @retval none
      */
    void sys_jtag_off(void)


MPU read only section protect
-----------------------------

Using MPU read only section protection may help to find the issue that
may break the read only section like .text or .rodata. The data in those
sections should be unchangeable, and protecting those area may help to
find the issue that may damage read only sections. To enable this
function, please uncomment the following code in main.c.

.. code-block:: c

   /* for debug, protect rodata*/
   mpu_rodata_protect_init();



Get DDR type & size
-------------------

Identify the DDR type of the system.

.. code-block:: c

    /**
      * @brief  Get currently DRAM type.
      * @retval dram byte
      * @note
      *  DRAM_TYP_DDR2 = 0,
      *  DRAM_TYP_DDR3 = 1,
      *  DRAM_TYP_UNDEFINE = 0xFF
      */
    uint8_t sys_get_dram_type(void)


Identify the DDR size of the system.

.. code-block:: c

    /**
      * @brief  Get currently DRAM density.
      * @retval dram size
      * @note
      *  32MB = 0,
      *  64MB = 1,
      *  128MB = 2,
      *  256MB = 3,
      *  512MB = 4,
      *  1024MB = 5,
      *  2048MB = 6,
      *  UNDEFINE = 0xFF
      */
    uint8_t sys_get_dram_size(void)


R/W otp logical map
-------------------

Read otp content on logical map. size must be an integer multiple of 2.

.. code-block:: c

    /**
      * @brief  Read otp content on logical map
      * @param  laddr: address on logical map
      * @param  size: size of wanted data
      * @param  pbuf: buffer of read data
      * @retval : return number of used bytes
      */
    int otp_logical_read(u16 laddr, u16 size, u8 *pbuf);


Write otp content on logical map. size must be an integer multiple of 2.
OTP has a limit on the number of writes, and it is necessary to avoid
repeated writing.

.. code-block:: c

    /**
      * @brief  Write user's content to otp on logical map
      * @param  addr: address on logical map
      * @param  cnts: how many bytes of data
      * @param  data: data need to be written
      * @retval 0: success <0: failure
      */
    int otp_logical_write(u16 addr, u16 cnts, u8 *data);


Write otp to disable log uart
-----------------------------

Write logical otp to disable log uart

.. code-block:: c

    int otp_rom_log_message_disable(void)

