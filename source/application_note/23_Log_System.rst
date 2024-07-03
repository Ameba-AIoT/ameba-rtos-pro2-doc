Log System
==========

.. contents::
  :local:
  :depth: 2

Log redirection
---------------

SDK provide several mechanisms to build up log system. Default log comes
out from UART, and by integrating other features, the log could be
deliver to only one port, ex UART, or two ports concurrently, ex: UART +
CDC.

To check how to hook lout out to call back, check sample code here:

.. code-block:: c

    extern void console_stdio_init(void *read_cb, void *write_cb);  //default uart or cdc
    extern void remote_stdio_init(void *read_cb, void *write_cb);   //default telnet or ssh

    unsigned user_read_buffer(unsigned fd, void *buf, unsigned len)
    {
        //read the cdc or uart data to buf
    }

    unsigned user_write_buffer(unsigned fd, const void *buf, unsigned len)
    {
        //write the buf to cdc or uart
    }

    void user_log_init(void)
    {
        console_stdio_init(user_read_buffer, user_write_buffer);    //Register the log system
    }


The application side needs to complete the read, write callbacks, and
register to the log system through the above sample code. Currently,
there are related examples of implementation for UART LOG, USB CDC, and
TELNET. For detailed methods, please refer to each example. If you need
to support the output of two log services at the same time, you need to
register the remote and console separately at the same time.


|

Fault message redirection
-------------------------

The initial configuration of the fault log was set to output
exclusively through the UART port, which presented a limitation as it
didn't allow the logs to be saved to alternative storage mediums. To
enhance functionality, the provided example enables users to save the
fault log directly to flash memory, which can be particularly useful for
post-event debugging.

The example code can be found in the file main_faultlog.c, located
within the directory project/realtek_amebapro2_v0_exmaple/src. The
function fault_message_log_init is designed to initialize the logging
process, and it utilizes two callback functions, fault_log and bt_log,
to manage the saving of log information.

fault_log callback: This function is tasked with handling the fault
event log messages, which includes the register and stack memory
dumps.

bt_log callback: This function is responsible for recording the stack
backtrace log messages.

Once the system has been rebooted, the function read_last_fault_log
can be called to retrieve the most recent fault log from the flash
memory.

For detailed implementation guidance, users should refer to the
main_faultlog.c file, which serves as a practical example of how to
integrate these logging features into their projects.


.. note :: If a bus fault occurs, peripheral access and log saving through the bus may fail.

.. code-block:: c

    void main(void)
    {
        // read last fault log, setup fault and backtrace saving, enable backtrace
        read_last_fault_log();
        fault_message_log_init(fault_log, bt_log);
        sys_backtrace_enable();

        mpu_rodata_protect_init();
        console_init();

        voe_t2ff_prealloc();

        setup();

        /* Execute application example */
        run_app_example();

        /* set tick count initial value before start scheduler */
        set_initial_tick_count();
        vTaskStartScheduler();
        while (1);
    }

