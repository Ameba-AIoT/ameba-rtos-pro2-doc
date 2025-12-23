Introduction for Quick guide with ISP debug information
=======================================================

How to Build Wifi / SD Card Example Code
----------------------------------------

- Command

	- cmake .. -G"Unix Makefiles"-DCMAKE_TOOLCHAIN_FILE=../toolchain_ci.cmake -DVIDEO_EXAMPLE=ON

	- cmake --build . --target flash -j 4

- Entry point


	- Refer to project\\realtek_amebapro2_v0_example\\src\\mmfv2_video_example\\video_example_media_framework.c

	- example_mmf2_video_surport()

- Video example selection

	- Through Wifi

|image1|

	- Through Wifi & SD card recording

|image2|

	- Can choose only one of video example

Connect to Device through Wifi
------------------------------

- Connect by PC

	- Desktop: should use Ethernet connected to same AP router

	- Laptop: should use Wifi connected to same AP router

- ATWS (WIFI scan)

|image3|

- Set Wifi SSID

	- ATW0=Zako_Wifi

- Set Wifi Password

	- ATW1=12345678

- Connection

	- ATWC

- Dis-connection

	- ATWD

..

   |image4|

- Through VLC Player

|image5|

Debug information
-----------------

- Enable VOE log

|image6|

- Enable AT command

	- Refer to project\\realtek_amebapro2_v0_example\\inc\\ platform_opts.h

..

   |image7|

	- Enable CONFIG_ISP

- Command

	- Refer to component\\at_cmd\\atcmd_isp.c

- I2C

..

   • ATII=i2c,read,addr

   • ATII=i2c,write,addr,value

- ISP Control

..

   • ATIC=0,V4l2

   • ATIC=1,V4l2,value

- Memory access

..

   • ATIX=read32,addr,length

   • ATIX=writw,addr,length,value

- FPS

..

   • ATII=fps,show

   • ATII=fps,hide

|image8|

- MIPI RX information

	- ATII=info,mipi

..

   |image9|
   
+------------+-------------+---------+---------------------------------------+
| **Date**   | **Version** | **A     | **Release note**                      |
|            |             | uthor** |                                       |
+============+=============+=========+=======================================+
| 2024.09.02 | 1.0         | Zako Wu | Draft version for customer release    |
|            |             |         |                                       |
+------------+-------------+---------+---------------------------------------+


.. |image1| image:: ../../_static/user_manual/34_IQ_Advance_QuickdebugFlow/image1.png
   :width: 3.45549in
   :height: 0.4691in
.. |image2| image:: ../../_static/user_manual/34_IQ_Advance_QuickdebugFlow/image2.png
   :width: 4.58087in
   :height: 1.12224in
.. |image3| image:: ../../_static/user_manual/34_IQ_Advance_QuickdebugFlow/image3.png
   :width: 6.72329in
   :height: 2.85287in
.. |image4| image:: ../../_static/user_manual/34_IQ_Advance_QuickdebugFlow/image4.png
   :width: 6.74363in
   :height: 3.84089in
.. |image5| image:: ../../_static/user_manual/34_IQ_Advance_QuickdebugFlow/image5.png
   :width: 6.82831in
   :height: 3.79452in
.. |image6| image:: ../../_static/user_manual/34_IQ_Advance_QuickdebugFlow/image6.png
   :width: 6.51781in
   :height: 2.88215in
.. |image7| image:: ../../_static/user_manual/34_IQ_Advance_QuickdebugFlow/image7.png
   :width: 6.21538in
   :height: 1.14972in
.. |image8| image:: ../../_static/user_manual/34_IQ_Advance_QuickdebugFlow/image8.png
   :width: 7.01024in
   :height: 4.73489in
.. |image9| image:: ../../_static/user_manual/34_IQ_Advance_QuickdebugFlow/image9.png
   :width: 4.23958in
   :height: 2.26042in
