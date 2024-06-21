SDK Architecture
================

.. contents::
  :local:
  :depth: 2

In AmebaPro2 sdk, it mainly contains four folders. The folder
“component” store the main component source and the folder “project”
contains the project makefile, compile flag and some examples. The
folder “doc” and “tool” provide the document and tools for assisting you
to set up the project.

Component
---------

+------------+-------------+-------------------------------------------------------------------+
| File name  | Purpose     | Comment                                                           |
+============+=============+===================================================================+
| component  | at-cmd      | AT-command                                                        |
|            +-------------+-------------------------------------------------------------------+
|            | audio       | ASP algorithm api                                                 |
|            |             | audio codec                                                       |
|            +-------------+-------------------------------------------------------------------+
|            | bluetooth   | bluetooth driver                                                  |
|            +-------------+-------------------------------------------------------------------+
|            | example     | amazon releated examples                                          |
|            |             | audio releated examples                                           |
|            |             | fatfs releated examples                                           |
|            |             | mmf releated examples                                             |
|            |             | socket releated examples                                          |
|            |             | ...                                                               |
|            +-------------+-------------------------------------------------------------------+
|            | file system | Fatfs and Littlefs                                                |
|            +-------------+-------------------------------------------------------------------+
|            | lwip        | lwip API source code                                              |
|            +-------------+-------------------------------------------------------------------+
|            | mbed        | multi-media framework modules                                     |
|            |             | muxer and demuer                                                  |
|            |             | rtp codec for media                                               |
|            +-------------+-------------------------------------------------------------------+
|            | network     | cJSON                                                             |
|            |             | coap                                                              |
|            |             | dhcp                                                              |
|            |             | httpc and httpd                                                   |
|            |             | iperf                                                             |
|            |             | mDNS                                                              |
|            |             | mqtt                                                              |
|            |             | ping                                                              |
|            |             | rtsp                                                              |
|            |             | sntp                                                              |
|            |             | tftp                                                              |
|            |             | websockect                                                        |
|            +-------------+-------------------------------------------------------------------+
|            | os          | freertos: freertos source code                                    |
|            |             | os_dep: Realtek encapsulating interface for FreeRTOS, ram usage…  |
|            +-------------+-------------------------------------------------------------------+
|            | soc         | app: monitor and shell                                            |
|            |             | cmsis: cmsis style header file and startup file                   |
|            |             | fwlib: hal drivers and nn api                                     |
|            |             | mbed-drivers: mbed API source code                                |
|            |             | misc: driver and utilities                                        |
|            +-------------+-------------------------------------------------------------------+
|            | ssl         | ssl stub function and ram map source code                         |
|            +-------------+-------------------------------------------------------------------+
|            | stdlib      | stdlib header files                                               |
|            +-------------+-------------------------------------------------------------------+
|            | usb         | usb and uvc header files                                          |
|            +-------------+-------------------------------------------------------------------+
|            | video       | ISP and video related api                                         |
|            +-------------+-------------------------------------------------------------------+
|            | wifi        | wifi api and wifi config related source code and header files     |
+------------+-------------+-------------------------------------------------------------------+


Project
-------

+------------+---------------------------+----------------------------------------------------------------------------------+
| Folder     | Sub-folder                | Description                                                                      |
+============+===========================+==================================================================================+
| Project/*  | example_sources           | examples for peripherals                                                         |
|            +---------------------------+----------------------------------------------------------------------------------+
|            | GCC-RELEASE               | GCC cmake projects                                                               |
|            +---------------------------+----------------------------------------------------------------------------------+
|            | GCC-RELEASE/application   | libraries for (non-trust zone) GCC project                                       |
|            +---------------------------+----------------------------------------------------------------------------------+
|            | GCC-RELEASE/bootloader    | bootloader project                                                               |
|            +---------------------------+----------------------------------------------------------------------------------+
|            | GCC-RELEASE/build         | pre-build image files (boot.bin) and json files                                  |
|            |                           | place for building cmake projects and generate flash image file (flash_ntz.bin)  |
|            +---------------------------+----------------------------------------------------------------------------------+
|            | GCC-RELEASE/mp            | for mp                                                                           |
|            +---------------------------+----------------------------------------------------------------------------------+
|            | GCC-RELEASE/ROM           | ROM code libraries                                                               |
|            +---------------------------+----------------------------------------------------------------------------------+
|            | inc                       | the header files for setting the project compile flag                            |
|            +---------------------------+----------------------------------------------------------------------------------+
|            | src                       | the main file source code for the project                                        |
+------------+---------------------------+----------------------------------------------------------------------------------+


Doc and tools
-------------

+------------+---------------------------+----------------------------------------------------------------------------------+
| Folder     | Sub-folder                | Description                                                                      |
+============+===========================+==================================================================================+
| tools      |                           | PGTool: for downloading image files to AmebaPro2                                 |
|            |                           | msys64: for building the environment of AmebaPro2 project                        |
+------------+---------------------------+----------------------------------------------------------------------------------+
| doc        |                           | document for AmebaPro2                                                           |
+------------+---------------------------+----------------------------------------------------------------------------------+


Binary files
------------

+----------------------+--------------------------------+--------------+-------------------------------------------------------------+----------------------+
| File name            | Location in flash layout(\*1)  | OTA After MP | Purpose                                                     | Comment              |
+======================+================================+==============+=============================================================+======================+
| certable.bin         | Key Certificate Table          | N            | Used to set the size and position of the certificate.bin    |                      | 
+----------------------+--------------------------------+--------------+-------------------------------------------------------------+----------------------+
| partition.bin        | Partition Table                | N            | Used to set the size and position of each bin               |                      |
+----------------------+--------------------------------+--------------+-------------------------------------------------------------+----------------------+
| certificate.bin      | Key Certificate 1              | N            | Certificate bin file is used by secure boot. There is only  | certificate_ota.bin  |
|                      |                                |              | one copy                                                    | (for ota in MP)      |
+----------------------+--------------------------------+--------------+-------------------------------------------------------------+----------------------+
| boot.bin             | Boot Image Primary             | N            | bootloader                                                  | boot_ota.bin         |
|                      |                                |              |                                                             | (for ota in MP)      |
+----------------------+--------------------------------+--------------+-------------------------------------------------------------+----------------------+
| firmware_isp_iq.bin  | ISP_IQ Data                    | N            | Combination of fcs_data.bin & sensor.bin & iq.bin           | isp_iq_ota.bin       |
|                      |                                |              |                                                             | (for ota in MP)      |
|                      |                                |              | fcs_data.bin : load fcs data in rom code and initialize     |                      |
|                      |                                |              | the sensor                                                  |                      |
|                      |                                |              |                                                             |                      |
|                      |                                |              | sensor.bin : initialize the sensor in normal boot           |                      |
|                      |                                |              |                                                             |                      |
|                      |                                |              | iq.bin : IQ parameter adjustment                            |                      |
+----------------------+--------------------------------+--------------+-------------------------------------------------------------+----------------------+
| firmware.bin         | Firmware 1 or 2                | Y            | Firmware image contains firmware_isp_iq.bin                 | ota.bin (for ota)    |
|                      |                                |              | (sensor.bin & iq.bin )                                      |                      |
+----------------------+--------------------------------+--------------+-------------------------------------------------------------+----------------------+
| nn_model.bin         | NN Model Data                  | Y            | NN Model                                                    | nn_model_ota.bin     |
|                      |                                |              |                                                             | (for ota)            |
+----------------------+--------------------------------+--------------+-------------------------------------------------------------+----------------------+
| boot_fcs.bin         | FCS Data                       | Y            | fcs video parameters                                        | Update parameters    | 
|                      |                                |              |                                                             | through FWFS (\*2)   |
+----------------------+--------------------------------+--------------+-------------------------------------------------------------+----------------------+

\*1: please refer to figure in chapter **FLASHLAYOUT**

\*2: please refer to chapter **FCS_MULTI**

certificate.bin、boot.bin、 firmware_isp_iq.bin can be OTA in MP image.



SDK and lib version
-------------------

For more easily management prebuilt libraries version in SDK, each
library has its own “get version” API and the output version string
follows a specific format.

API in each library

.. code-block:: c
	  
  char * lib<lib_name>_get_version(void);


Output version string format

.. code-block:: c

  lib<lib_name>:YEAR.MON.DAY.HOUR.MIN.SEC_b<branch_name>_<branch SHA1>

YEAR, MON, DAY, HOUR, MIN and SEC are the building date and time of this
prebuilt lib.

For example

If user want to obtain the version information for a specific library,
such as the “wlan” lib, user could implement the code like following
code piece.

.. code-block:: bash

  extern char * libwlan_get_version(void);  
  printf(“%s\n\r”, libwlan_get_version());


Device should output string like

.. code-block:: c

  libwlan:2023.05.01.12.20.00_b9.5_0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f


For the version of “video” lib,

.. code-block:: bash

  extern char* libvideo_get_version(void);
  printf(“%s\n\r”, libvideo_get_version());


Device should output string like

.. code-block:: bash

   libvideo:2023.05.01.12.22.00_b9.5_0e0e0e0e0e0e0e0e0e0e0e0e0e0e0e0e0e0e0e0e


Additionally, user could directly search for the version string by
opening the prebuilt library binary file using a hex editor and
searching for the string, for example "**libvidoe:**" or
"**libwlan:**".

This version information is also useful for user for solving issues with
factory support.

The API is only accessible when the libraries linked into caller
application. Cannot get libraries’ version linked into non-secure
application from secure application, or vice versa.

Video version
~~~~~~~~~~~~~

If you need to get the version information of the video, please enable
the streaming first and use **video_get_version()** to get the
information.

The version information about the video has the following items

-  VOE

-  SENSOR DRIVER

-  SENSOR TIMESTAMP

-  FCS

-  IQ TIMESTAMP

-  IQ VERSION

The above information are all displayed in

.. code-block:: bash

   void video_get_version()

Taking sensor gc4653 with FCS under voe 1.4.2.1 as example, after
calling video_get_version(), you can see the content as follows:

.. code-block:: bash

   voe_ver: 1.4.2.1
   sensor_voe_ver: 1.4.2.1
   sensor_timestamp: 2023/04/20
   fcs_version: 0x5306
   iq_timestamp: 2023/05/12 16:04:30
   iq_cus_ver: 0x01


Below is more description for above information,

   **voe_ver** is an abbreviation for VOE version, where VOE is another
   core for ISP.

   **sensor_voe_ver** means sensor VOE version, which is the sensor
   version on VOE.

   **sensor_timestamp** is the release information of sensor driver.

   **fcs_version** is the version of FCS driver.

   **iq_timestamp** and **iq_cus_ver** are the release information of iq
   table.

-  iq_timestamp record the date/time information of the IQ release day.

-  iq_cus_ver record the version for specific IQ of an ongoing project.

At the same time, you can also use AT Command to display the version
information of the video, the command is as follows:

.. code-block:: bash

   ATII=version

If the user needs to show the respective version information on the
application side, please refer to the content of video_get_version() for
coding.

.. note :: Difference of libvideo_get_versonion() and video_get_version()

libvideo_get_versonion() is the version information in video driver
running on main core v8m.

video_get_version() retrieve all the other version information from
video offload engine, including VOE(bin file for Video offload engine),
Sensor Driver, FCS, IQ Timestamp and IQ version.

Bootloader version
~~~~~~~~~~~~~~~~~~

At offset 0x2B0 of bootloader image, the version is a 32bytes value in
little endian order. The definition of version is explained in the
“Version and Timestamp” section of OTA chapter. The version can be
configured in ‘amebapro2_bootloader.json’ under
‘project\\realtek_amebapro2_v0_example\\GCC-RELEASE\\mp’.

.. code-block:: bash

   "MANIFEST":{
   "label":"RTL8735B",
   "vrf_alg": "NA_VRF_CHECK",
   "tlv":[
   {"type":"PK", "length":384, "value":"auto"},
   {"type":"TYPE_ID", "length":2, "value":"IMG_BL"},
   {"type":"VERSION",
   "length":32,
   "value":"FEFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF"},
   {"type":"TIMST", "length":8, "value":"auto"},


For a bootloader image file in file system, file API, such as fopen()
and fread() can be used to open the file and read bootloader version at
the offset 0x2B0 of bootloader image file. For bootloader partition in
flash, FWFS API, such as pfw_open() and pfw_read() can be used to open
the partition and read bootloader version at the offset 0x2B0 of
bootloader partition.
