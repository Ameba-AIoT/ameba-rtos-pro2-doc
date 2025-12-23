Tuning Guide ENG
================

.. contents::
  :local:
  :depth: 2


|49e5b7885d682849526623f041bcf74f|

Document Version: v1.0

Release Date: 2025/05/12

COPYRIGHT
---------

©2021 **Realtek Semiconductor Corp**. All rights reserved. No part of
this document may be reproduced, transmitted, transcribed, stored in a
retrieval system, or translated into any language in any form or by any
means without the written permission of **Realtek Semiconductor Corp**.
**DISCLAIMER** provides this document "as is", without warranty of any
kind, neither expressed nor implied, including, but not limited to, the
particular purpose. Realtek may make improvements and/or changes in this
document or in the product described in this document at any time. This
document could include technical inaccuracies or typographical errors.
**TRADEMARKS** Realtek is a trademark, of **Realtek Semiconductor
Corporation**\ . Other names mentioned in this document are
trademarks/registered trademarks of their respective owners.This document is intended for the hardware and firmware
engineer's general information on the Realtek IP Camera IC. Though every
effort has been made to ensure that this document is current and
accurate, more information may have become available subsequent to the
production of this guide. In that event, please contact your Realtek
representative for additional information that may help in the
development process.

|0b0824664be62141aead42032b360397|


Revision History
----------------

+-----------+--------+---------------------------------------+---------+
| Revision  | Release|       Description                     | Author  |
|           |        |                                       |         |
|           | Date   |                                       |         |
+===========+========+=======================================+=========+
|           |        |                                       |         |
+-----------+--------+---------------------------------------+---------+

1.Tool Overview
---------------

This chapter introduces the basic information and activation steps for
the **RealCam Pro** image debugging tool, including software
installation and activation procedures. Through this chapter, users can
successfully execute **RealCam Pro** and begin the image debugging
process.\ 

1.1 Hardware and Software Version Description
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**RealCam Pro is an auxiliary tool specifically developed for image
debugging. After successfully connecting to the prototype, it will
automatically identify the connected chip. If identified as the Amebapro
2 chip, it will display ISP Tuning Pro, indicating successful chip
identification. Users can directly select this option to open the image
debugging window.**

|1e15d66adef047853a10f9f7c50073a6|\ 

< Amebapro 2 IC chip>

1.1.1  UVC FW Burning Method
"""""""""""""""""""""""""""""

The following diagram shows the EVB development board and mini82
development board provided by RTK.

|image1|


a. On the AmebaPro2 EVB, use the jumper to connect the J27 Pin and press
   the reset button to enter the burning mode

b. |image2|



c. Use the PG tool to burn FW

d. |image3|



e. Type “uartfwburn.exe -p comXX -f .\\filename.bin -b 1000000 –U”

f. The "XX" part is the corresponding com port and the name of the file
   to be burned\ |image4|

When it finish,it the download success will be showen.

1.1.2 RealCam Pro software install and enable
"""""""""""""""""""""""""""""""""""""""""""""

**RealCam Pro** There is no need for additional installation procedures,
and when you get the delivered software, you can store it in the
computer folder and directly execute the software execution file
RealCam.exe。

When you run **RealCam Pro** for the first time, a
Realcam_activation.bin will be generated, please submit this file to our
technician window, and wait for our company to reply to the
corresponding SW Key, paste the software activation window to complete
the software enablement, this enabling program only needs to be executed
once on each computer.

2. Interface functions
----------------------

This section will introduce the **RealCam Pro** interface, basic
functions, and correction procedures



Section 2.1 is an introduction to **RealCam Pro**. It includes the
function introduction of each block on the main interface, the process
of the standard connection prototype, and how to open the image
debugging window

Section 2.2 is an introduction to capture the Raw、JPG、BMP files and
UVC Streaming、Raw Streaming.

Section 2.3 is about image debugging parameter import. When the hardware
specifications of the product are similar to the existing debugging
parameters, you can start adjusting based on the existing set of
parameters by importing the existing parameters, which can complete the
image adjustment process more quickly

Section 2.4 explains the export of image debugging parameters, the
export function is to export the debugged parameters from **RealCam
Pro**, in addition to the version control of different debugging
parameters, the exported parameter file is placed into the prototype, so
that the debugged image parameters do not actually act on the prototype
through **RealCam Pro**, so as to complete the image debugging

Section 2.5 describes the calibration process for each module. Hardware
differences between different camera modules will result in different
image characteristics. Therefore, the image correction process must be
carried out before starting image debugging. **RealCam Pro** calculates
the image information of the module based on the image signal, and
imports the corresponding image information according to the process to
complete the correction process. The calibration module includes: BLC,
RNR, LSC, AWB, CCM and other modules

2.1 Primer UI
^^^^^^^^^^^^^

First of all, follow **1.1.1 RealCam**\ **、RealCam Pro** Software
Installation、Enable to execute **RealCam Pro** Take a quick start
like the one below。

The **RealCam Pro** execution home screen is divided into three main
areas, except for the name of the title bar and the version number

1) Toolbar

2) The function button area will only be displayed when streaming

3) Streaming/Image display area

|0c063f10b6bdb12aa4342616f824f4bb|

< The UI of RealCam >

After connecting the prototype, click Vendor\\ISP Tuning Pro on the
toolbar of the main screen as follows to enter the image parameter group
switching, as well as the image tuning window, see **Section 3 Image
tuning steps** are performed

|18952795efc4249f391397c7719970cc|

< Parameters Switch selection >

|64800c5acd3149a8d9b80b5c68afc40d|

< Tuning Page >

|image5|

2.2 Connect the prototype for streaming image capture and IQ tuning
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

RealCam supports UVC streaming prototypes, which can be connected for IQ
debugging, IQ parameter export and import, image analysis through
real-time streaming, and can still be connected for IQ tuning, parameter
export and import without real-time streaming.

UVC Streaming
"""""""""""""

   Compiler the UVC FW for IQ tuning

   Please refer to RTK document **AmebaPro2_UVC_FW_compile.pdf**

Using **RealCam Pro** ，If it is already connected Amebapro 2 demo
kit，A live stream of the mockup is automatically displayed, see the
toolbar / Device Confirm that it is correctly recognized Amebapro 2 demo
kit，There will be when properly identified **USB UVC CLASS** showing.

|5cb482a58d6f7ee75aeed3b1e839c016|\

< Connect Amebapro2 / Device >

RAW Streaming
"""""""""""""

RAW Streaming is Sensor Bayer Data output 、Pass ISP the handler flows
directly，It can be used to capture RAW Data judge by Sensor noise or
BLC correction。

It is work when UVC streaming, tap the UI Vendor / RNR Calibration ，UVC
streaming switches automatically Raw streaming

< UVC Streaming and click RNR Calibration >

|72bef7d82bd7eb50a315c617d134efa1|

On the correction page，it appears in the **RealCam Pro** Stream/Image
Display Area:

|ebeb861081840756ede36237f82dcf21|

< RAW Streaming display >



Capture BMP or JPG
""""""""""""""""""

Tap **RealCam Pro** Button area icon to set the file storage
path、format，For example:

< Capture BMP or JPG >

Click on the wrench graphic in the lower toolbar to enter video
recording and image capture setting

Note if you want to record video，Video type to select the H264.

|image6|

2.3 Import image parameters (Burned IQ Table into demo kit)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

At **Amebapro2** chips，IQ parameters to an independent one
bin file.

Complete parameter profile ( Completed IQ ) It contains parameters for
tuning different exposure modes, for example Linear and HDR, Each mode
is fixed by two modes. For example, day and night mode。

The IQ parameters in each mode can also be a separate profile ( Partial
IQ )，Therefore, the import parameter profile can be divided into
sections ( Partial IQ ), the Complete IQ contains more than one Partial
IQ and can be made by SW API to switch, the Linear mode is called by
default in daylight mode at Group 0 Day，Night mode will switch to Group
0 Night. The HDR mode The daylight preset is invoked Group 1  Day
，Night mode is Group 1 Night 。

.. note :: **AmebaPro2's IQ.BIN exists as shown below. It can contain up to 6 sets of IQ Tables, corresponding to Linear and HDR mode. Each mode contains 3 sets of IQ Tables corresponding to Day, Night, and other modes. The SDK requires Complete IQ (Full IQ) instead of a single Partial IQ. Please note that the complete Full IQ must be added to the header (recording file size information).**

|image7|

Import the Complete IQ 
""""""""""""""""""""""

|image8| 

Please refer to the following figure for step-by-step
instructions.

Import the Partial IQ
"""""""""""""""""""""

|image9| 
Please refer to the following figure for step-by-step
instructions.

2.4 Export image parameters
^^^^^^^^^^^^^^^^^^^^^^^^^^^

After the image parameter tuning is complete and updated the IQ Table.
The parameters need to be export by **RealCam Pro**, It will be imported
into the corresponding storage location in the demo kit, and will take
effect on the prototype immediately after completion.

Using ISP Tuning Pro windows，Click the【Vendor】->【ISP Tuning Pro】in
**RealCam Pro**.


|image10| 

Import the Partial IQ


Merge Partial IQ into Complete IQ (IQPackTool)
""""""""""""""""""""""""""""""""""""""""""""""

Click the executable file and the following interface will pop up. This
tool can merge independent Partial IQ into a Complete IQ, or disassemble
Complete IQ into independent Partial IQ.\ 

|image11|

Disassembling Complete IQ and merging Partial IQ
""""""""""""""""""""""""""""""""""""""""""""""""

|image12| 

Click the executable file and the following interface will
pop up. This tool can merge independent Partial IQ into a Complete IQ,
or disassemble Complete IQ into independent Partial IQ.


Conversely, the Partial IQ can also be merged into Complete
IQ through the tool. The steps are as follows:

|image13| 

.. warning:: **Please note that if the merged IQ.BIN is put into the SDK for compilation, the header information must be added, which is the step (5) in the above figure.**

**Complete IQ imported through RealCam does not need to add a header.**

2.5 Calibration
^^^^^^^^^^^^^^^

It is required to calibration by **RealCam Pro**, Please follow the
1.1.1 **RealCam Pro** software installation and enablement before
proceeding below。

BLC calibration


LSC calibration


AWB calibration


CCM calibration


Please refer to RTK document **AmebaPro2 IQ Tuning
Manual_Calibration_Ver0.0.2_230712.pdf**

.. _section-14:

.. _section-15:

3. Image tuning steps
---------------------

Click the Vendor\\ISP Tuning Pro of **RealCam Pro**, The main
interface of the image tuning window is as follows and can be divided
into several areas:

|9360d58e2904730c338c9e463c6f4bdf|\ 

<tuning UI page> 

The interface
display could be different by different version

Tuning Directory Area: A tree-like display of the main tuning items,
each of which roughly corresponds to the ISP operation module

Tuning Ribbon: Displays parameter readout, writes, connection demo kit,
updates and extractions of IQ Table... and other functions

After the parameters are tuned for the current color temperature and
brightness, you need to update the color temperature and gain range
corresponding to the IQ Table, and then export the final parameters to a
.bin file

The color temperature function in the left half of the debug ribbon is
described below

. Current: Displays the real-time color temperature estimate of the AWB
when connected, and does not update when the AWB is set to manual

. Index: Color temperature range, which set of color temperature ranges
the current color temperature is located in, so that the debugging
results in the parameter area can be updated to the corresponding color
temperature range

. IN: Displays the threshold for entering the color temperature range

. OUT: Displays the threshold for leaving the color temperature range

.. note :: Color temperature Index corresponding IN / OUT setting can be adjust by Index Manager

The Gain function in the left half of the debug ribbon is described
below

. TH Based on: Selection of the criterion for the gain setting，you can
choose Gain or ETGain ,compared with Gain，ETGain is more consideration
of exposure time, Gain unit is ( 1x )，ETGain unit is 0.1 millisecond
Multiply 1x gain ( 0.1 ms \* 1x )

. Current: Displays the real-time gain gain or ETGain gain value of the
AE when connected, and does not update when the AE is set to manual

. Index: Gain interval shows which set of gain intervals the current
gain is in, so that the tuning results of the parameter area can be
updated to the corresponding gain interval

. OUT: The cut-off point corresponding to the gain interval

The Gain/ETGain adjustment IN / OUT setting can be adjust by Index
Manager

Please refer to RTK document **AmebaPro2 IQ Tuning
Manual_Calibration_Ver0.0.2_230712.pdf**

3.1 BLC
^^^^^^^

BLC: Black Level Correction

The sensor is affected by temperature and offset voltage, resulting in
so-called dark currents, resulting in the generation of pixel value even
when there is no light sensitivity, reducing the contrast of the overall
image quality and dynamic range. BLC is to correct the dark current
cause the Black Level, mitigate the effects of contrast and dynamic
range in images.

The parameters of this module can be set according to the color
temperature / gain conditions

BLC \\ General 
""""""""""""""

|2730cf2ea4e6104a16101ee575c3809f|\ 

< BLC\\ General >

1. BLC Enable Long Exposure Path

Enable Linear mode or HDR mode BLC for long exposure frames.

2. BLC Enable Short Exposure Path

Enable Enable HDR mode BLC for short exposure frames. Only for HDR mode.

BLC \\ Long Exposure Path (Main Path) 
"""""""""""""""""""""""""""""""""""""

|e5af1c4eaaa552e5b90cafed7d23cb25|\ 

< Long Exposure Path (Main Path) >

If Sensor setting is RAW 10 bits, the correction value is multiplied by
4. For example, if you want to deduct 16 BLC from 10 bits , you need to
fill in 64 (16x4)

If Sensor setting is RAW 12 bits, this correction is relatively
intuitive for 1x. For example, if you want to deduct 16 BLC from 12 bits
, you need to fill in 16.

.. warning:: **The BLC completed in section 2.5.1 BLC Correction will be the value that can already be directly entered.**

(5) ~ (8) BLC Gain R、Gr、Gb、B: Bayer Domain, the Re-Scale gain of
R、Gr、Gb、B Channel.

BLC \\ Short Exposure Path 
""""""""""""""""""""""""""

|acff250b296320877b5214ddef94d0f6|

< BLC \\ Short Exposure Path >

Same as described in section **3.1.2 above**

 The circuit of the sensor itself will have `a dark
current <https://zhida.zhihu.com/search?content_id=131290295&content_type=Article&match_order=1&q=%E6%9A%97%E7%94%B5%E6%B5%81&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NDExMzk5MDMsInEiOiLmmpfnlLXmtYEiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoxMzEyOTAyOTUsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.BS9ZnYMa8KjwnZgnxZxqUZINI_iPsUYWuvZ9RmxwAAI&zhida_source=entity>`__,
As a result, when there is no light, the pixel unit also has a certain
output voltage, and the dark current is related to the exposure time and
gain, the different locations are also different values. Therefore, when
the gain increases, the gain of the circuit increases, and the dark
current will also increase, so many ISP’s will choose to subtract
different bl values under different gains.

For example, the black level value attached to the raw data output of
sensors needs to be cleaned at the front end of the ISP。If it is not
cleaned, the interference information will affect the processing of each
module of the back-end ISP. In particular, it will cause AWB to be
inaccurate, and the overall picture will be greenish or reddish as a
whole.

In the example below, the BLC corrects the comparison before and after

|image14|

3.2 AE
^^^^^^

AE: Automatic Exposure

According to the set target brightness and related parameter settings,
the appropriate exposure settings are obtained through the algorithm. AE
enables the image output to maintain a consistent level of brightness
even under the application of scene transitions or ambient brightness
changes.

The object we see is actually the light reflected from the object and
enters our eyes and is imaged in our eyes. In the same way, in order for
a camera to take a picture, it also needs to receive this reflected
light.T he human eye can automatically adjust the amount of light
through the pupil, and the camera needs to rely on our special module to
control the amount of light in the camera, through the measurement of
the ambient light of the shooting picture, adjust the camera parameters
to make the picture correctly exposed, and then the brightness of the
captured image is appropriate, to avoid overexposure or underexposure.

The purpose of automatic exposure is to obtain the appropriate exposure
amount for the image sensor. Exposure is the intensity of the light
multiplied by the time it takes for the light to reach the sensor.
Exposure is expressed in E, the unit is Lux. Auto exposure is essential
for camera capture and is one of the important processes of camera
capture.

AE Schematic Diagram (overexposed, correctly exposed and
underexposed)\ 

|image15|

|image17|\ |image18|\ |image16| 

Understand the difference in brightness information between the above
three images through the histograms maps to determine the correct
exposure.


AE Workflow Diagram

|image19|

AE \\ General & Manual Exposure 
"""""""""""""""""""""""""""""""

|76669ae4dec3e3860b28d19559f587f2|\ 

< AE \\ General & Manual Exposure>

(1) Bypass: ISP Auto Exposure control

Enable : **Disable** ISP AE, and AE Auto/Manual setting Manual Mode.

Disable : **Enable** ISP AE, Using this setting in normal AE.

(2) Auto/Manual: Auto / manual exposure switching

Auto: automatic exposure, and this setting is used in normal auto
exposure

Manual : manual exposure, when manual exposure is selected, the Exposure
and Gain value in the field can be set.

(3) Exposure:Manual exposure setting (**μs** )

(4) Gain:Manual gain setting (1x)

AE \\AE Attribute
"""""""""""""""""

< AE \\ AE Attribute - 1/2>

|image20|



(1) AE Weight: The image is split into 16x16 blocks，AE Weight is used
    to set the weight setting value for a total of 256 blocks，The
    weighted calculation of the corresponding area is done to produce
    the average brightness value Y Mean

..

   There are 3 common AE metering modes:

a. Average

b. Central weight

c. Spot

..

       |image23| \ |image22| \ |image21| 

           (a)       (b)        (c)

    (1) y_mean_target: The target of y_mean .

    (2) y_mean_target_l: The lowest y_mean boundery.

    (3) y_mean_target_h: The highest y_mean boundery.

..

   The AE Target relationship between the changes is shown in the figure
   below

   |image24|

(4) total_gain_max: The maximum gain could be used, 16 means 1x gain.

(5) ae_enter_stable_th: In the unstable state, when AE step persist
    within this threshold for a certain period of time and enter a
    stable state.

(6) ae_exit_stable_th: AE Search Trigger Gate, in the stable state, when
    the current frame Y Mean with the target value step is greater than
    this parameter setting, it leaves the stable state.

(7) dyn_fps_min: The minimum frame rate.

(8) dyn_fps_setting: Dynamic frame downgrading function，TH is the
    threshold for Gain, only this field can be adjusted, **The unit is a
    1x Gain**. When the gain is raised to the TH setting, FPS will be
    reduced from the value corresponding to the previous Index to the
    value corresponding to the current Index.

(9) dyn_target_etgain: According to ETGain dynamic reduce y_mean_target,
    The use scenario is usually reduced y_mean_target in low-light
    environments.

..

   **src_th is the threshold of ETGain,. The values here are the
   dyn_target_etgain that is not enabled, After enable it, the ETGain
   that the Tool sees has been recalculated.**
   
|image25|

<AE \\ Detail Adjustment> 
"""""""""""""""""""""""""


|image26|

|image27|

3.3. Over Exposed Protection
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Over Exposed Protection: Overexposed area overflow protection mechanism.

This module belongs to: Texture. Different settings can be given
according to different Gain setting.

Over Exposed Protection\\ General 
"""""""""""""""""""""""""""""""""

< Over Exposed Protection \\ General >
""""""""""""""""""""""""""""""""""""""
|image28|


Enable: Over Exposed Protection Function switch

Enable Over Exposed Protection. Inhibits over-exposure area color
overflow. Disable means disable Over Exposed Protection function, It
does not inhibit the overflow of overexposed areas.

(1) Clip Mode: Clip the image data to the setting of the bit area.
    Non-Clipped: Do not clip the data.

..

.. warning:: **Please use the default setting 14 bit.**

(2) Thd0: Defines non-overexposure values. It means that the brightness
    of the pixel begins to suppress the increase in brightness values

(3) Thd1: Define the brightness value of the overexposure, The value of
    the brighter pixel starts to increase from the brightness of the
    pixel.

|image29|

|image30|

3.4 LSC
^^^^^^^

LSC means lens shading correction.Lens Shading refers to the vignetting corners formed by the four corners
of the picture due to insufficient incident light, at the same time, due
to the difference in the refractive index of light at different
frequencies, it is caused color shading. So, LSC solved luma shading and
color shading.

The figure below is shown LSC of the adjustment page, The upper half of
the area is the automatic correction of the LSC , described in Section
2.5.2, the lower part is for the parameters that have been corrected,
and then manually adjusted. This section focuses on the use of the
second half.

Comparison before and after correction

|image31|

This Adjust Rate of function The parameters can be set differently
according to different color temperature and gain.

|cc8e7b1577d5befa95f94fee03cbc274|

(1) The R、G、B curve of NLSC. It can be modified manually NLSC curve.

The diagram on the right side of the page shows these three curve. The
horizontal axis is the distance from the center of the picture, The
vertical axis is curve value.

(2) Manual modify the parameters of MLSC .

Check the Block Enable, The screen of RealCam will be shown MLSC block
grid is displayed.

|a8f821b7c179490ca1a9e75d424e8b41|


|129461df9568b2a7418a1f0e1dc90de0|

Click on the block you want to modify

|efc6841aeec272db02a3e93a10bb56cd|

Press the button in the upper right corner of the
interface【Read】，Read the MLSC parameter value of the selected block
and modify the parameter value.

Press the button in the upper right corner of the
interface【Write】，Make the parameter take effect。You can see the
effect in the screen.

|d9f83eb0063f3d39998143dad826419d|

1. The accuracy of Adjust Rate is 1/32，32 means Rate for 1. It won't
   change the calibration NLSC curve, however, it will add the amplitude
   of the NLSC.

The Adjust Rate is smaller, the lower the brightness around the
image；if the Adjust Rate is larger, the brighter the image is around.

3.5 Gamma
^^^^^^^^^

For Image Sensor, the relationship between Input and Output
is linear. However, the perception of the dark part is more sensitive to
the human eye than that of the light part, in other words, the change of
the dark part is more easily detected by the human eye. Gamma is used to
process the conversion relationship between the sensor signal and the
human eye perception, so that the output image is more consistent with
the human eye perception.

Gamma calibrations are used in image display devices, such as monitors,
projectors, etc. Since the perception of different brightness by the
human eye is nonlinear, by adjusting the gamma value, the perceptual
characteristics of the human eye can be better simulated and the visual
effect of the image can be improved. In addition, Gamma correction is
widely used in image processing software, cameras, and image processing
algorithms to improve image quality.

|c3cd105481b998c720c80eea74b482f9|\ 

< Gamma >

(1) Graphical display: According to the choice of Gamma Type will be
    shown between Input and Output.

..

(2) Parameter: The table lists all the parameters of all Gamma modules.

(3) Gamma Type: Those are RGB Gamma / Y Gamma / Y Global Curve modules
    can be adjusted.

The main ones that will be used are: RGB Gamma and Y Global Curve. So
additionally there is RGB Gamma + YGC / RGB Gamma + System Gamma /Y
Global Curve +System Gamma 3 combination modes to be choose.

(4) Adjust Mode: Curve / Point / Gamma Value 3 adjustment modes are
    available.

|c71ffcd001bc80405b5720a96187dc80|

Curve: There will be 4
control points on the left graphical display interface that can be
dragged and dropped, and all parameters will be adjusted in an
approximate line as you drag.

|79971deeefa89ff0483b0cef213ba6fc|\ 

Point: On the graphical display
interface on the left, the control points of all parameters can be
dragged and dropped, and only a single parameter can be adjusted when
dragging.

|d04c108d241c1c8c7278952d73825f64|\ 

Gamma Value: Gamma
(Output = Input ^ Coef) to generate curves, the parameters Slope
function is control the linear change of the dark area, can avoid the
dark place Noise increases too much. If you don't need to use it, you
will set the Slope = -1.

(5) Undo: Revert to the last adjustment

(6) Redo: Revert to the state before Undo

(7) Restore: Revert to the original state

(8) History: Parameter combinations are memorized for switching

There are two different sets of settings below of Gamma, Left (high
contrast), Right (low contrast)

|image32|\ |image33|

|image34|\ |image35|

3.6 DRC & DRE
^^^^^^^^^^^^^

DRC: Dynamic Range Compression; DRE: Dark Range Enhancement

DRC & DRE\\General
""""""""""""""""""

< DRC&DRE\\General >

|image36|


(1) DRC Enable: Enable Dynamic Range Compression; Disable:Off.

(2) Blend Ori Level: The function of blending the scale with the
    original image can be automatically calculated or set manually.

(3) Mixed proportions；The closer to Ori represents the lower the
    proportion of the mixture, and the more ingredients are used.

.. note :: **In different segments (gain conditions) should be designed in the
  same direction as much as possible (gradually increasing or decreasing
  with the increase of gain) to avoid instantaneous over-brightness or
  over-dimming when the brightness changes.**

|image37|

|image38|

3.7 WDR & Dehaze
^^^^^^^^^^^^^^^^

WDR: Wide Dynamic Range

Wide dynamic range is processed to increase the brightness of dark areas
in high-contrast scenes.

WDR of AmebaPro2 allows the image to maintain excellent image quality
even in high-contrast light and shadow. Traditional cameras can't
capture the entire brightness data in one image. But When the WDR
function outputs a photo, the ISP will create two photos that are
brighter and darker than the original environment, and then combine the
two photos to make the overly bright and overly dark areas can be seen
more clearly than the original.

< WDR & Dehaze >

|image39|

   WDR Level: 0~100. The higher the value, the brighter the dark area
   increases.

   For example, The intensity of WDR Level can increase the brightness
   value of the dark parts of the picture.

   |image40|

3.8 GbGr
^^^^^^^^

GbGr: GbGr Balance

Because some features of Lens and Sensor do not match, or the module is defective（Common problems such as photodiode arrangement,
non-uniformity of CFA, lens
coating,amplifier problems, etc）,Causes large differences between adjacent Gr
and Gb pixels; so the strength of the original Gr and Gb signal may be
inconsistent in 在 Bayer Pattern. Eventually, a pattern of grids or
horizontal stripes will be formed on the screen, GbGr Balance module
that is intended to mitigate this phenomenon.

|image41|

This module belongs to Texture, Different settings can be given
according to different Gains.

< GbGr Long Exposure (Linear) - 1/2>

|image42|

(1) GbGr Balance Enable: Function switch Enable/Disable.

(2) GbGr Balance Strength: GbGr Strength control. Larger number, the
    greater correction.

..

   **The threshold and sensitivity of GbGr are fixed, and the user can
   only determine the strength.**

   |image43|

3.9 AWB 
^^^^^^^

AWB: Auto White Balance

With the auto white balance algorithm parameter settings, the image
output can still maintain the same tone as the original object under the
effect of scene transformation or ambient light source change.

AWB works by measuring the color temperature of light sources in a
scene. It uses the camera's sensor to detect the main light source in
the scene and adjust the color of the image based on this information.

Here's how AWB works:

Light source detection: The camera's sensor measures the dominant light
source in the scene and determines its color temperature. This can be
done by analyzing the spectrum emitted by the light source.

Color temperature correction: The camera determines the color
temperature of the light source, it adjusts the color of the image to
make the white look more neutral. This typically involves adjusting the
gain of the red, green, and blue channels to eliminate color temperature
deviations.

Image Processing: The camera renders the corrected image to ensure that
whites appear white in different lighting conditions and that other
colors remain accurate.

|image44|

**AWB is an important part of ISP's color processing.**

Color accuracy: AWB ensures that the colors in the photos accurately
reflect the actual scene. If the color temperature is incorrect, the
photo may appear unnatural in color, affecting the viewer's perception.

Time-saving: Using AWB saves photographers a lot of time because they
don't need to constantly manually adjust camera settings in different
lighting conditions.

Get creative: AWB enables photographers to focus more on the composition
and creative aspects of the shot without having to worry about color
correction.

**Color reference at different color temperatures**

|image45|

**Color performance at different color temperatures**

|image46|

AWB \\ General & Manual WB Attribute 
""""""""""""""""""""""""""""""""""""

< AWB \\ General & Manual WB Attribute >

|image47|


(1) Bypass: AWB Module switches, Enable or Disable.

(2) Auto/Manual: Auto/manual white balance switching, Auto: automatic
    white balance；Manual: manual white balance, When manual white
    balance is selected, R Gain / G Gain / B Gain settings in the field
    are used as white balance parameters.

..

(3) R Gain: R gain is set in 1/256x for manual white balance.

(4) G Gain: G gain is set in 1/256x for manual white balance.

(5) B Gain: B gain is set in 1/256x for manual white balance.

.. note :: **G Gain: In most cases, the value should be maintained at 256, and R Gain/B Gain should be preferred for adjustment.**

(6) Detail Adjustment: Click【Detail Adjustment】button to pop up a AWB_Analyse window, in this window, you can provide correction values for multiple sets of standard color temperature points ct_point (as shown in the green square dots shown below). The white zone of the white balance action zone white_area_point(The purple box on the inner left side of the figure below is shown below), gray area of the white balance action zone gray_area_point (As shown in the blue box
on the left outer left side of the figure below). The location of the
ISP in the figure below after numerical analysis of the image needs
to be selected by the white area and gray area boxes to be included
in the AWB triggering and convergence calculation, especially since
the numerical weight in the white area is greater than that in the
gray area.

|064a8a50e5ae0100238309d750b2e242|\

<AWB_Analyse >

. AWB_Analyse window is displayed at the top right is【Start 】、 【Stop
】button, click

【Start 】to display the statistical information of each block of AWB
updated in real time, click【Stop 】is not renewed.

. The first table on the right side of the AWB_Analyse window shows the
calculated values of AWB, and the main information is color_temperature
result_gain and the estimated color temperature value.

. The table at the bottom right of the AWB_Analyse window is the table
for adjusting the parameters of the white area and the gray area, and
there is a parameter setting method selection above the table. Clicking
on **IQ Data** means using the settings displayed after the current
dragging, and clicking on **Param** means using the parameters in the
table as settings.

**Using the Param mode to adjust the parameters in the table for the
first adjustment.**

intp_param: It may be insert between two ct_point that are far apart
white_area_point/ gray_area_point, Smaller value, the denser the
area_point inserted, and the corresponding blocks will be increase.

(7) ct_setting: Standard color temperature point (ct_point).

Depending on the type of ambient light source, you can adjust the number
of standard color temperature points to be used by modifying the value
of the **Count of CT Points**.

(8)  ct_block_num: The number of blocks in the color temperature region;
     using(1) Detail Adjustment Param mode -> intp_param to generate.

(9)  a: (ct_curve_fit_line), Read-only.

(10) b: (ct_curve_fit_line) , Read-only.

(11) c: (ct_curve_fit_line) , Read-only.

(12) white_area_up: Internal color temperature area (white_area) of the
     upper boundary points corresponds to the boundary points from left
     to right on the coordinate plane in the **Detail Adjustmen**\ t.

(13) white_area_down: Internal color temperature area (white_area) of
     the upper boundary points corresponds to the boundary points from
     left to right on the coordinate plane in the **Detail Adjustment.**

(14) gray_area_up: Exterior color temperature area (gray_area) of the
     upper boundary points corresponds to the boundary points from left
     to right on the coordinate plane in the **Detail Adjustment**.

(15) gray_area_down: Exterior color temperature area (gray_area) of the
     upper boundary points corresponds to the boundary points from left
     to right on the coordinate plane in the **Detail Adjustment.**

(16) normal_weight: the weighting of AWB blocks, w_table and g_table are
     the weighting of internal block (white_area)and exterior block
     (gray_area).

**For example, in the case of low color temperature, because the weight
of low color temperature is 0, AWB will not refer to the low color
temperature data to calculate the R and B gain, and AWB correction will
not be performed in the low color temperature scenario.**

   |image48|\ |image49|

(17) rg_bg_num_th: One of the thresholds to determine whether AWB enters
     the **HOLD** state; If the number of statistical points in the
     color temperature region of the AWB instance is less than this
     threshold, there are not enough white or near-white areas in the
     screen, and the AWB instance enters the stable state and no AWB is
     updated.

(18) min_bright_th: One of the thresholds to determine whether AWB
     enters the **HOLD** state; If the brightness of the AE image is
     less than this threshold, the ambient brightness is low, and the
     AWB statistics may change significantly, and the AWB will enter the
     stable state and the AWB will not be updated.

3.10 AWB Gain Adjust
^^^^^^^^^^^^^^^^^^^^

AWB Gain Adjust

Based on the white balance gain calculated by the white balance
algorithm, you can adjust the color tone of the picture according to
your preference. This module belongs to Texture, Different settings can
be given according to different Gains/Color temperature.

|6f6b1721129b0d03b2c622bd124351ea|\ 

< AWB Gain Adjust >

(1) R Gain Adjust:R channel settings.

The adjustment will increase the R channel, and the picture is reddish;
On the contrary, if the tone is small, the R component will decrease,
and the picture will appear greenish.

(2) B Gain Adjust: B channel settings.

The adjustment will increase the B channel, and the picture will appear
blue; On the contrary, if the tone is small, the B component will
decrease, and the picture will appear yellowish

In the example below, increase the B channel value and add the blue
component to the AWB base to make the color tone look more natural.

|image50|

|image51|

AWB adjustment mostly involves the accumulation of concepts and
experience and the customer's personal preference and other factors,
there is no absolute right or wrong, it is recommended to refer to the
various scenes of the digital camera AWB settings to find out the most
suitable parameters and settings for customers, and a large number of
field testing.

3.11 FCR & MCR & UVS
^^^^^^^^^^^^^^^^^^^^

This module belongs to Texture, Different settings can be given
according to different Gains.

3.11.1 FCR
""""""""""

FCR: False Color Reduction.

|2ea11651d66edccdb27ecbe769f97c31|\ 

< FCR >

(1) EEH Reduction Enable: Enable for EEH False Color Reduction.

(2) EEH Reduction Strength: The closer you get to **Strong** indicates
    the stronger the color suppression, acting on the edges of the image
    texture.

|image52|

(3) INTP Log Enable: Enable for INTP False Color Reduction.

(4) INTP FCR Texture: The closer you get to **Strong** indicates the
    stronger the color suppression, acting on the edges of the image
    texture.

(5) INTP FCR Flat: The closer you get to **Strong** indicates the
    stronger the color suppression, acting on the flats of the image.

3.11.2 UVS
""""""""""

UVS：UV Suppress

For less saturated Color Noise, it can be used with UVS and can avoid
affecting areas with high saturation.

< UVS >

   |b8965bee784140d35c9b14229b007729|

(1) UVS Rate: The higher value, the stronger the intensity; when set to
    the maximum, the effect is grayscale.

(2) UVS thd0: The threshold for the first turning point, smaller than
    this value affect the intensity of the UVS Rate ；This corresponds
    to the red frame in the inner circle in the image below (UV Color
    Tune page).

(3) UVS thd1: The threshold for the second turning point, larger than
    this value do not work UVS, This corresponds to the red frame in the
    outside circle in the image below (UV Color Tune page).

(4) UVS Slope: read only.

|image53|

- UVS thd0 larger, the more affected the area，more false color be
  suppressed, But more chroma is affected, such as the upper left petal
  and the upper right box.

- UVS thd0 is too large, the image will become a grayscale image.

|image54|

3.12 UV Color Tune
^^^^^^^^^^^^^^^^^^

UV Color Tune

Individually adjusting preferences for a single color system can avoid
affecting the expression of other colors.

This module belongs to Texture, Different settings can be given
according to different Gains/Color temperature.

< UV Color Tune >

|image55|

Because there are many parameters，\ **UV Color Tune** presented as a
graphical interface.

**UV Color Tune** will make the UV plane divided into 16 areas, with a
total of 16 points (white points) that can be controlled, and the white
points can be dragged and moved directly on the graph.

Dragging the white dot outwards increases the color saturation of the
area.

When the angle of the white point changes, the color hue of the area
changes.

Reset: Revert to the UV Color Tune without adjustment without any color
changes.

The process is described below

|image56|

Get ROI/Release ROI: When you press **Get ROI** button, A draggable
purple box will appear on the screen, and you can drag/adjust the box
onto the color area you want to adjust.

When you press **Get ROI** button, the text display will be **Release
ROI**, when the adjustment is complete, press **Release ROI** will reply
**Get ROI**. You can adjust the next color.

|image57|

- Matting: When **Release ROI**\ (press **Get ROI**), Matting could be
  press, after Matting, the selected ROI area is displayed in the Source
  Image area. The color position of the current ROI is marked with a
  blue dot on the UV plane.

- Matting function is for simulation display, press "Write" button will
  actually take effect.

|image58|\ |image59|

Before-and-after comparison of differences

.. note :: **This function can do individual color correction for a single color,but the forced change of hue will have a destructive correction of the image,it is recommended to try to change the color in CCM or AWB and other ways to change the color for color correction, UV Color Tune as an aid.**

3.13 DPC 
^^^^^^^^

DPC: Defect Pixel Compensation

The purpose of dead pixel compensation is to deal with the pixel defects
of the image sensor, and to strengthen the pixels that are abnormally
bright and abnormally dark through the operation of the algorithm, and
to consider the degree of preservation of textures and details to
achieve the integrity of the image content.

This module belongs to Texture, Different settings can be given
according to different Gains.

DPC \\ Long Exposure Path (Main Path) 
"""""""""""""""""""""""""""""""""""""

|image60|

< DPC \\ Long Exposure Path (Main Path) >



(1) DPC Enable: Function switch.

(2) DPC Type: Dead pixel type, Single means a single dead pixel
    compensation mode,、Multiple means a multiple dead pixel
    compensation mode.

(3) Process Type: **Normal** is Indicates the general processing mode,
    which can detect most of the dead pixels. **Keep Detail** is strong
    detail retention ability, but some dead pixel distributions cannot
    be detected.

(4) Write Back Mode: Enable will be a strong reduction effect.

..

   **Bright Defect Strength: The brightness and bad pixel compensation
   parameter group can distinguish the intensity settings in the dark
   area of the picture and the bright area.**

(5)  Dark Tone: Stronger means that the easier it is to judge the bad
     pixels, the stronger the strength of the compensation.

(6)  Bright Tone: Stronger means that the easier it is to judge the bad
     pixels, the stronger the strength of the compensation.

(7)  Reserve Texture: Keep the texture and details of the compensation
     area, closer to **As Less** means the stronger the strength of the
     compensation.

(8)  Dark Defect Strength: The dark and dead pixel compensation
     parameter group can distinguish the intensity settings in the dark
     area of the picture and the bright area.

(9)  Dark Tone: Stronger means that the easier it is to judge the bad
     pixels, the stronger the strength of the compensation.

(10) Bright Tone: Stronger means that the easier it is to judge the bad
     pixels, the stronger the strength of the compensation.

(11) Reserve Texture: Keep the texture and details of the compensation
     area, closer to **As Less** means the stronger the strength of the
     compensation.

..

   Dark/Bright Tone Boundary: The value that determines the dark and
   light areas.

The DPC types of **AmebaPro2** are Single Defect & Cluster Defect.

The Single Defect type can handle the following types of bad pixels:

|image61|

Cluster Defect can handle the following types of bad pixels (the tuning
tool uses Multi Defect for processing):

|image62|

The other types are not processed. It is recommended to use the DPC
function of the sensor to process them, and ISP is used as an auxiliary.

Using Multi Defect will lose a lot of details

**AmebaPro2 can set up different parameters in different brightness
scenes. It is recommended to turn on Multi Defect in extremely low light
environment.**

|image63|

Take the above figure as an example to compare the details before and
after DPC(left: Disable DPC，right: Enable DPC)

|image64|

|image65|

|image66|

3.14 INTP
^^^^^^^^^

INTP: Color Interpolation; Demosaic

When the image sensor is taken, the INTP module can reconstruct the
full-color image, and its function process is to judge and classify
various features of the image, and then interpolate the corresponding
colors according to different categories to achieve full-color images
with texture retention and noise suppression.

This module belongs to Texture, Different settings can be given
according to different Gains.

INTP\\Denoise
"""""""""""""

INTP\\Interpolation will denoise after matching interpolation according
to the various features detected.

< INTP\\Denoise >

|83f4fc1907b691b1473634fe51025d4d|

(1) Denoise Enable: Function switch.

(2) Denoise Debug Mode: Enable, the denoising intensity can be
    displayed, and the darker is stronger denoise.

.. warning:: **INTP\\Denoise The maximum denoising intensity is fixed, and the area of denoising is determined by the following three parameters (3) ~ (5).**

(3) Blend LPF ConditionTH0: The first turning point in denoising
    intensity, closer Strong indicates that the more areas can be
    denoised; closer Weak indicates that the more areas can be keep
    original.

(4) Blend LPF ConditionTH1: The second turning point in denoising
    intensity, closer Strong indicates that the more areas can be
    denoised; closer Weak indicates that the more areas can be keep
    original.

(5) Blend LPF Condition Slope: The slope of the denoising intensity,
    read only.

..

   **TH0 and TH1 determines the area of action of Denoise, using debug
   mode to understand denoise area, closer to strong, the stronger NR
   (black area).**

   |image67|

3.15 Noise Reduction
^^^^^^^^^^^^^^^^^^^^

NR: Noise Reduction

The denoising function type has TNR (Temporal Noise Reduction) and SNR
(Spatial Noise Reduction) . The scope is generally divided into static
and dynamic areas in the image. Static areas can be applied with TNR and
SNR, dynamic regions is applied with SNR.

This module belongs to Texture, Different settings can be given
according to different Gains.

Before NR and After NR

|image68|

Noise Reduction\\General 
""""""""""""""""""""""""

The denoising function is divided according to the parameter attributes
General, Motion Detection and Detail Extraction. In the 2) bookmark of
the image tuning window, you can click on the parameter category to
switch the parameter page.

|image69|

< NR\\ General >



The following is a description of the parameters in the General category
of the Denoising Module .

(1) Static Scene TNR Strength: TNR strength, closer to Strong means the
denoising is stronger.

   |image92|\ (2)~(5) Static Scene SNR Strength: SNR strength, closer to
   Strong means the denoising is stronger.

**The higher all thresholds, the more texture/noise falls into the
denoising zone.**

(6) ~ (9) Motion Scene SNR Strength: Motion SNR strength, closer to
Strong means the denoising is stronger.

**The higher all thresholds, the more texture/noise falls into the
denoising zone.**

   (10) 3DNR Strength: In 3DNR mode. The ratio of the output of the
   static and dynamic regions after denoising, closer to Strong means
   the denoising is stronger.

   Comparison of different intensity parameters.

   |image70|

Noise Reduction\\Motion Detection 
"""""""""""""""""""""""""""""""""

The following is the parameter page of the Motion category of the
denoising module, as well as the instructions for using each parameter.

|6920467a6525b8d28109c78c0aa42d15| 

< NR\\ Motion >

   (1) Debug Mode: It can help to understand which areas of the image
   are judged to be static or dynamic. Use overall、LP_all、HP_all is
   better.

   (2)~ (3) Low Freq Difference G_TH0, G_TH1: Thresholds to determine
   low-frequency differences in static and dynamic regions, Bayer G
   components were used for analysis. Closer to High , more TNR.

   (4) ~ (5) Low Freq Difference RB_TH0, RB_TH1: Thresholds to determine
   low-frequency differences in static and dynamic regions, Bayer R,B
   components were used for analysis. Closer to **High** , more TNR.

   (6) ~ (7) Low Freq Difference Y_TH0, Y_TH1: Thresholds to determine
   low-frequency differences in static and dynamic regions, Y components
   were used for analysis. Closer to **High** , more TNR.

   (8) ~ (9) High Freq Difference G_TH0, G_TH1: Thresholds to determine
   high-frequency differences in static and dynamic regions, Bayer G
   components were used for analysis. Closer to **High** , more TNR.

MD Comparison

|image71|

**Reducing the area of action and NR intensity of MD will alleviate the
unnatural motion blur phenomenon. Try to make balance between noise and
motion blur，This step requires constant fine tune and fine tune. It is
relatively time-consuming in the NR module.**

NR\\Detail Extraction 
"""""""""""""""""""""

3. The following is the parameter page of the Detail Extraction category
   of the denoising module, as well as the instructions for using each
   parameter.

< NR\\Detail Extraction >

|image72|


(1) Debug Mode: Provides in-frame texture information to help understand
    which areas in the image are judged to be flat, textured, and edged.

(2) Detail Extraction: After clicking, the debugging window will pop up,
    and you can set the threshold of texture information, The lower
    Detail value, the stronger Static Scene SNR will be applicate.

|0797ab1491624b98c3f63f1be00a2a7a|

   (3)~(6) Spatial Frequency X0~X3: Sets the threshold for texture
   information. Less than X0 is defined as a flat area. Regions greater
   than X1 and less than X2 are defined as texture areas, and edges
   greater than X3 are defined as edge areas.

   (7)~(8) Detail Extraction: Y0~Y3 are correspondence to X0~X4 Detail
   values, closer to Smooth means apply the stronger Static Scene SNR.

Definition diagram, convenient for users to define the range from flat
area to edge area. |image73| |image74|

3.16 Edge Enhance
^^^^^^^^^^^^^^^^^

The purpose of texture enhancement is to extract the intensity of the
texture in the image to achieve a sharp, clear visual effect. Its
principle of action is to judge the flat and textured areas in the
image, and to do different degrees of processing for different features.
It can be divided into functions according to: **General、Y Sharp、and
Overshoot/Undershoot**.

This module belongs to Texture, Different settings can be given
according to different Gains.

Before EE and After EE

|image75|

Edge Enhance\\General 
"""""""""""""""""""""

Here are the texture enhancement modules General category and the
instructions for using each parameter:

|image76|

< EEH\\ General >

(1) Edge Enhance Enable: Function switch.

(2) Y Mean Filter : The first threshold, Noise is removed before the
    edges of the texture are extracted. The more gradual the coefficient
    change, the more stable the texture extraction effect, on the
    contrary, the effect of extraction texture is more sensitive.

(3) Y Mean Filter : The second threshold

(4) Y Laplacian Filter : Texture features can be extracted at both
    horizontal and vertical edges, The more gradual the coefficient
    change, the more stable the texture extraction effect, on the
    contrary, the effect of extraction texture is more sensitive.

Edge Enhance\\Y Sharp
"""""""""""""""""""""

Y Sharp can extract textures are strengthened to different level by Edge
Detection, it can adjust the intensity based on the brightness of the
texture area.

|5691a8470f9d35c9019ee29f8b985a72|\ 

< EEH\\ Y Sharp >

(1) Sharp Edge Map Enable: When Enable, it can display the intensity of
    texture sharpening and the area of action. The streaming/image
    display area in the main window is displayed in grayscale. The
    whiter, the stronger Sharp Effect.

..

   (2)~(10) Yxx_th0: The threshold for the first turning point is
   enhanced by the texture. These ten parameters represent the first
   turning point threshold at different brightnesses.

(11) offset (th1 = th0 + offset): Textures enhance the offset between
     the first and second inflections.

(12) rate0: The eigenvalue is less than the sharpening intensity of the
     first turning point.

(13) rate1: The eigenvalue is greater than the sharpening intensity of
     the second turning point. The strength between the turning points
     is calculated by interpolation.

(14) Y Sharp Draw Location Effect: It can display the Distance Effect
     range.

(15) Dist_thd0: The distance from the center of the image is less than
     this value, means that the value of **Y Sharp Effect th0** is not
     heightened.

(16) Dist_thd1: The distance from the center of the image is greater
     than this value, means that the value of the **Dist_Max** is used
     to elevate **Y Sharp th0**.

(17) Dist_Max: Sets the maximum value for lowering Y Sharp th0 (Lowering
     sharp).

(18) NR Sharp Strength: read only.

(19) Sharp Strength: Strength control.

(20) Y Sharp Effect Button: Pop on Sharp Effect tuning window.

|image77|

Use Debug mode to define the EE scope. Make the area of action more
visible.

|image78|

How can we make an image look sharper? The easiest way to do this is to
make the "lines" in the image more visible. In an image, the lines
represent an area with a sudden change in color, so one of the ways to
make the lines more visible is to enhance the contrast of colors on both
sides of the line: make the deep one darker and the light one lighter.

**In addition, there are a few important points to keep in mind when
using sharpening:
\*Sharpen a small amount of time several times, starting with a minimum
amount of blurring in our photo, then color correction and other work,
then creative sharpening, and finally sharpening according to the way
you output.
\*If there is a lot of noise in the image, the noise is processed first,
and then sharpened to prevent sharpening and making the noise more
noticeable.**

Over/Under Shoot
""""""""""""""""

< EEH\\Over Shoot/Under Shoot >

|bff0cd25c27d3b80c2747652207eefc7|

(1) Suppress Strength: Suppresses effects such as over-enhanced aliasing
    of textures, closer Strong means the greater the suppression.

(2) Suppress Strength Button: Click on it to pop up an adjustment window as follows:

|9fc1ff8de3d3a3e6187bfd4de9b7eeeb| 

Before overshoot suppress and After overshoot suppress

|image79|

3.17 Video Property
^^^^^^^^^^^^^^^^^^^

Brightness /Contrast/Saturation adjustments.

This module belongs to Texture, Different settings can be given according to different Gains.

|8ccfac62b9cb9604db636eba900862b5|\ 

< Video Property >

(1) Brightness : Brightness adjustment.

0 means no adjustment, > 0 represents brightened image; < 0 indicates
reduced image brightness.

(2) Contrast\\Level: Image contrast adjustment.

32 means no adjustment, > 32 for increased image contrast; <32
represents reduced image contrast.

(3) Contrast\\Mean: Contrast to adjust the brightness baseline.

.. note :: **It should be noted that when the Contrast\\Level is set to 32, there will be no change in the image effect when adjusting the brightness baseline.**

(4) Saturation: Image saturation adjustment.

64 represents non-adjustment, > 64 represents increased image saturation
and < 64 is reduced the image saturation.

Example of brightness variation

|image80|

Contrasting examples of change

|image81|

Example of chroma variation

|image82|

3.18 CCM\ **（Color Correction Matrix）**
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In order to make the color displayed in the image close to the human
eye, the color correction module is required to restore various colors,
and transform the color from the sensor RGB space to the RGB space of
the human eye, so that the effect of the image conforms to the
subjective feeling of the person.

Before CCM and After CCM

   |image83|

 The actual color information and expected values of the first 18 color
patches in the 24-color card scene captured by the image sensor,
calculate the CCM matrix for 3x3. The smaller the difference between the
input color and its expected value from the CCM , and the good for CCM.

Please refer to RTK documentation **AmebaPro2 IQ Tuning
Manual_Calibration_Ver0.0.2_230712.pdf** The calibration data obtained
at each color temperature is displayed in the adjustment window.

|image84|


**The CCM produced by this calibration process is used for normal
brightness, and the saturation and hue can be further adjusted based on
the correction results under different gain conditions.**

(1) Click【Vendor】->【ISP Tuning Pro】to enter the image tuning page,
    than click【Dynamic Control】，to disable【Global Dynamic Control】.

(2) Switch to【CCM】, the checkbox beside CCM【Base/Tuned】set to
    Unchecked, you can see the

..

   【CCM】matrix becomes the same as【Final Result】.

(3) Adjust Downward【Saturation】interface, You can see the
    corresponding changes in the matrix.

(4) You can check the【Sync】to make U、V channel synchronous tuning, or
    you can uncheck let U、V channel Adjust according to what you needs.

**Advanced CCM tuning**

| Since the sensor's sensitive dynamic range is different from the
  standard color of the 24-color card, there may also be a difference in
  the average brightness of the image. We make brightness adjustments
  (multiplication) and dynamic range adjustments (contrast adjustments)
  for standard colors to match the average brightness and dynamic range
  of the current image as much as possible. In other words, the 6 black
  and white blocks below the standard 24 colors should be as bright as
  possible to the current output image.
| According to the CCM correction steps of RTK document, the CCM
  coefficient will be obtained according to the minimum color error ΔC,
  when the search range is set unreasonably and the image saturation is
  low, the calculated ΔC will be very small, but the actual effect is
  ugly.
| There is also a large difference in the number of CCMs in different
  color temperatures. Therefore, there are generally multiple sets of
  CCM coefficients in ISPs. In the actual use process, it is necessary
  to calculate the current color temperature through AWB, and then
  select the appropriate CCM coefficient, or a linear combination of
  several sets of CCM coefficients.

When the low light noise is large, the absolute value of the CCM
coefficient is sometimes reduced to reduce the noise. AWB solves the
influence of the sensitivity difference and color temperature of R, G
and B of the sensor, while CCM corrects the mutual interference between
R, G and B sensitivity, so that the image is closer to the visual effect
of the human eye.

And between different customers, there is a question of personal
preference. Therefore, the CCM adaptation process is highly customized.
Even the same Sensor + Lens combination will have different parameters
for different customers.

Among them, many large-scale customer groups will have their own
subjective and objective test standard processes. In the color
reproduction section, it is sometimes necessary to manually fine-tune
the CCM coefficient to obtain a more suitable image effect.

The picture below shows the standard 24 color blocks, and we need to
optimize the 13th color block (blue) to make it closer to the standard
color block. Through the graphical analysis tool, we can know that the
RGB value is (52,65,254), and the standard value in the standard color
card is (39,63,147), and we can adjust its coefficient through the CCM
tool to make it close to the standard color card effect.

|image86|\ |image87|


Patch 13 standard RGB value

|image85|

Open the tuning tools page, after uncheck the dynamic setting, the CCM
parameters can be adjusted. It can be observed when the Base/Tuned block
is unchecked.

【CCM】matrix same as【Final Result】.

Reduce the red channel to obtain a new CCM matrix as follows.

|image88|

**RealCam** provides a CCM fool-proof mechanism, and the right side of
the tool shows whether the sum of the three sets of coefficients is 256,
which is used by the adjuster to determine whether the current neutral
color has a color cast.

|image89|\ |image90|

The RGB value of Patch13 is shown in the image above，R channel 52 🡪 43

Red overall saturation decreases.

|image91|

4. Advanced features
--------------------

This section introduces the advanced features of **RealCam Pro**, which
are mainly auxiliary functions in addition to basic image adjustment,
and are mostly used for debugging or special applications.

4.1 section is register reading and writing, which can read and write
the register data of the chip or sensor in real time, which is a common
means of debugging. Scratchpad details are not publicly available and
must be assisted through the support window.

4.2 section describes how to manage the different parameter intervals.
In order to adapt to various light sources and scenes, the chip provides
a dynamic image parameter mechanism, which can distinguish different
image debugging parameters according to the illuminance and temperature
slicing of the scene. When parameters in different scenarios cannot be
split independently, you can use the guidelines in this section to
appropriately increase the range of different parameters.

   4.3 section introduces how to capture Raw files by **RealCam Pro**.
   When you need to capture Raw files for analysis and debugging, you
   can use the instructions in this section.

4.1 Register writing and reading
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Real-time settings of chip operation can be understood, it
is used for further analysis and judgment. Usually the register address
is unknown and not publicly available, When there is a problem to
clarify, if necessary, it will be done according to the address provided
by the support window.

(1) Click **RealCam Pro** toolbar Analyze.

(2) Analyze Drop-down menu Reg Set Tool to open the register Read and
    Write window.

|image92|


< Register read and write window >

|531fbdc6660886eeb34eca4b438ef395|

The above register access window can be clicked Sensor 、 Controller to
switch the read and write targets is Sensor or ISP .

4.2 Add the IQ parameters in different Lux Index
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Please read the section 3.0 Image tuning steps to understand how IQ
parameters work in the color temperature and gain ranges.

The new parameter interval can increase the accuracy of the adaptive
parameters. When a scene needs to be tuned accurately, you can add color
temperature and gain intervals for the scene, and the parameters between
other scenes will be statically switched and dynamically interpolated.

|62fe57561ab6d407485b97b4bb2fec81|\ 

In ISP Tuning Pro tuning page to
click【Index Manager】to open Index Manager window.

4.3 Added color temperature range


(1) Click Color Temp./Gain.

(2) Drop-down to select the ISP module, for example BLC

(3) Click the adjacent section to which you want to add a section.

(4) Click 【Add】, after that, a new set of intervals will be copied
    with adjacent intervals. New interdistrict IN/OUT Thresholds and
    parameters are first generated by replication or interpolation.

(5) Click 【Save】to save the new lux/CT index.

|ad77fa7370b07fa4c49cbeb33a7e41c3|

(6) Added section IN/OUT threshold can be modified according to the
    scene.

(7) 【Save】to save the interval deployment after the addition is
    completed.

|263b8a463b2fd7d0323762d3eccaa4ca|

Added lux index range


(1) Click Color Temp./Gain

(2) Drop-down to select the ISP module, for example BLC

(3) First, select which set of color temperature ranges the gain
    interval you want to add, and then click the adjacent interval of
    the new range.

(4) Drop-down select the threshold to switch the base unit with Gain or
    ETGain.

(5) Click 【Add】, after that, a new set of intervals will be copied
    with adjacent intervals. New interdistrict IN/OUT Thresholds and
    parameters are first generated by replication or interpolation.

(6) 【Save】to save the interval deployment after the
    addition is completed.

|d0da14c5a463e55414792dffe49b10f3|

(7) Added section IN/OUT threshold can be modified according to the
    scene.

(8) 【Save】to save the interval deployment after the addition is
    completed.

|ae54c50398bc8ffc18d9e2646e0ebff7|

4.3 Capture Raw file
^^^^^^^^^^^^^^^^^^^^

This chapter will introduce how to obtain Raw Data through **RealCam
Pro**.

Click【\ **Analyze**\ 】【\ **Color Distribution**\ 】 on the tool page
to pop up the color analysis window.

Click the【\ **CaptureRaw**\ 】 button to capture the Raw Data in the
current environment.

**RealCam** **allows users to obtain Raw Data of**\ 【 **Before AE**\ 】
**and** 【\ **Before INTP**\ 】 **nodes.**\ |image94|

The Raw files will be saved in the root directory of the tool folder in
\*.cap format.\ 

If you need to view the Raw file, please download the free software
**ImageJ**. Here is how to open the **.cap** Raw file.

Open ImageJ, File\\Import\\RAW... Open the .cap Raw file, fill in
16bits, width and height, 128 offset to first image, and select
Little-endian byte order, then click 【OK】to open the corresponding Raw
file.\
|image95|

|image96|


5. An example for tuning
------------------------

This section summarizes image adaptation examples and serves as a
reference guide for initial tuning.

The section 5.1 is the overall image debugging process, which can complete the module correction and perform the basic subjective and
objective image debugging in the recommended order according to the
process.


The overall image tuning process


There are many dependencies on image tuning, These chapters are
organized in the order in which they follow 1~12 The survey model block
is for reference, so as not to be inaccurate enough or repeatedly
repetitive.

5.1 Block diagram of the image tuning process
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

|c5cde072ff14d0aefb3ee7a1527f05d9|

(1) Process overview

(2) Description of the process sequence

+------+---------------------------------------------------------------+
| mo   | instructions                                                  |
| dule |                                                               |
+======+===============================================================+
| BLC  | The purpose is to correct the base value of the image sensor, |
|      | which needs to be recalibrated depending on the sensor and    |
|      | PCB, and the calibrated value may also vary depending on the  |
|      | gain.                                                         |
+------+---------------------------------------------------------------+
| RNR  | Based on the BLC correction results, the noise characteristic |
|      | distribution of the sensor at each gain value is analyzed.    |
|      | Depending on the sensor, PCB revision, etc....All conditions  |
|      | need to be recalibrated.                                      |
+------+---------------------------------------------------------------+

+------+---------------------------------------------------------------+
| LSC  | If the sensor receives light unevenly due to the optical      |
|      | characteristics of the lens, the compensation signal will     |
|      | enter the AE statistics, which needs to be completed before   |
|      | the AE tuning. It needs to be recalibrated depending on the   |
|      | lens.                                                         |
+======+===============================================================+
| AE   | BLC and LSC will affect AEC statistics, and the above         |
|      | correction modules need to be completed before AEC tuning.    |
+------+---------------------------------------------------------------+
| I    | After AE tuning, the convergence gain in each illumination    |
| ndex | environment is determined, and the gain interval is set based |
|      | on this, so that the back-end ISP module can dynamically      |
|      | configure the parameters according to the interval position.  |
+------+---------------------------------------------------------------+
| G    | Based on the AE brightness, the brightness is adjusted in     |
| amma | each gain range to make the picture brightness close to the   |
|      | human eye. After this step, the basic adjustment of image     |
|      | brightness is completed.                                      |
+------+---------------------------------------------------------------+
| AWB  | Calibrate the white balance statistical area and ambient      |
|      | color temperature judgment curve according to the             |
|      | characteristics of the module, and start after the brightness |
|      | module.                                                       |
+------+---------------------------------------------------------------+
| CT   | After AWB correction, the color temperature value under each  |
|      | light source is estimated, and the color temperature range is |
|      | set based on this, so that the back-end ISP module can        |
|      | dynamically configure the parameters according to the         |
|      | interval range.                                               |
+------+---------------------------------------------------------------+
| CCM  | Different parameters are configured based on the color        |
|      | temperature range, so that the color development at each      |
|      | color temperature can be independently and accurately         |
|      | corresponded.                                                 |
+------+---------------------------------------------------------------+
| UV   | Basic tuning is not recommended.                              |
| tune |                                                               |
+------+---------------------------------------------------------------+
| Enco | The encoding settings will affect the rendering of the        |
| ding | detailed texture of the image, so you need to set it before   |
|      | you start debugging the texture.                              |
+------+---------------------------------------------------------------+
| Tex  | For image texture detail tuning, it is recommended to start   |
| ture | based on the results of image brightness and color basic      |
|      | tuning.                                                       |
+------+---------------------------------------------------------------+

(3) Before Image tuning

Preparation of laboratory equipment

- 24-Patch Color checker

|image97|

- **Lux /CT meter** - It is used to measure the actual color temperature
  (K) and illuminance value (Lux) of the ambient light source.

|image98|

- |28a6fd7d955e0b6b2874a0a9a2f891be|\ |5c7315124c64d0b806d29f1f389f3788|\ LSB
  or diffuser - LSC Correction use

- **Lightbox** – The color temperature of the light source needs to be
  adjusted

|image99|

- **Standard scene** – It is recommended to use a D65 or D50 light
  source with adjustable brightness, and the light is evenly
  distributed. Standard scene content recommendations include:

  - Light and dark contrast: light area, dark area

  - Textures: wool, floor, turf, text...

  - Resolution test chart: star chart、TV line chart

  - Moving object: A moving object with a fixed trajectory (as shown in
    the figure below, a standard scene, a humanoid mannequin and a slide
    rail design)

|image100|

6. IQ/ISP Q & A
---------------

1. Dynamic gain switching of AE module

   - How does AE’s ETGain and Gain switching logic affect exposure
     stability under different lighting conditions? How to avoid screen
     flickering caused by frequent switching in actual applications?

   - In extreme contrast environments (such as backlight), how does AE
     dynamically allocate statistical weights for each brightness zone?

.. tip :: Most of the IQ parameters will be linearly interpolated, so there will be no discontinuity problem when switching. However, some parameters are directly discretely switched without linear interpolation, so the settings between parameters cannot be too abrupt.Backlit scenes cannot be solved using statistical weights unless the backlight area is fixed. Usually only the center weight is strengthened (theoretically the key point area of ​​the image).

   Backlit scenes usually need to be solved by histogram AE, WDR, or
   HDR.

   Design and configure AE weights according to the customer's
   application scenarios (such as garages, doorbells, surveillance,
   etc.)

2. AWB Correction of Color Temperature Range

   - How to adjust the IN/OUT threshold dynamically of the color
     temperature range according to the actual ambient light source? How
     to improve AWB stability under mixed light sources?

   - In low-light scenes, the Auto Exposure (AE) tends to increase the
     gain, but this may cause the Auto White Balance (AWB) to misjudge
     the color temperature. How to develop a joint optimization strategy
     for gain/color temperature?


.. tip :: If all mixed light sources fall within the CT box, you can only make a choice and adjust the weight of the color temperature block to make the result lean towards the preferred color temperature.AWB misjudgment in low light conditions is usually caused by sensor non-linearity issues, which can generally be improved by fine-tuning the BLC.

3.  Light source uniformity requirements for LSC correction

- The document recommends using a DNP light box or diffuser board, but
  how much influence do different light source color temperatures (such
  as D50 and D65) have on the calibration results? How to verify whether
  the uniformity meets the standard?



.. tip :: - RealCam can make LSC correction for different color temperatures, and linear interpolation will be performed for different color temperature ranges based on the correction results.

..

   Please refer to Section 2.5 and our company documents for details.

   **AmebaPro2 IQ Tuning Manual_Calibration_Ver0.0.2_230712.pdf**

- LSC verification uniformity depends on the specifications of your
  customers.

4. The Impact of ambient temperature on ISP stability.

- Will the sensor dark current and ISP processing drift in high or low
  temperature environments? Is a dynamic compensation mechanism required
  (such as temperature sensing feedback to adjust the BLC)?



.. tip :: - In a high temperature working environment, the hardware will indeed heat up and generate thermal noise. Therefore, the BLC module can also dynamically adjust the BLC according to different gain ranges, but it is limited to the configured gain range. The thermal noise generated by the sensor due to heating cannot be detected and processed by the ISP. The ISP only designs noise reduction parameters for the corresponding exposure range, unless it can be confirmed that the exposure parameters of the sensor in high and low temperature environments are fixed.

5. SNR degradation in low-light environments.

- How to balance the parameters of Sensor gain (Analog/Digital Gain) and
  multi-frame noise reduction (3DNR) in extremely low light to avoid
  chroma noise or motion blur?



.. tip :: - Noise Reduction – This module is the main noise reduction module of Amebapro2. It performs image processing based on RNR correction. For debugging instructions, please refer to Section 3.16 Noise Reduction.

  This module consists of SNR and 3DNR. 3DNR has the characteristics of
  multi-frame superposition and can fully restore the details. However,
  if there is a relatively moving object in the picture, the position
  difference between frames will cause incorrect superposition and cause
  moving object smear.

  The module has a Motion Detection function, which processes the blocks
  identified as moving objects with SNR. Through Debug Mode, you can
  separate the colors of moving and static objects to determine whether
  the threshold setting is appropriate. Refer to Section 3.16 Noise
  Reduction of this document.

6. Detail retention and noise reduction conflict in high-frequency
   texture areas.

- In NR processing, how to distinguish random noise from real textures
  (such as hair, fabric) to avoid over-smoothing?



.. tip :: By enable the Denoise Debug Mode in INTP, it is found that some dots in the flat area are judged as green (non-flat area processing).Appropriately increase TH0 / TH1 to make the flat area fall into black as much as possible (flat area processing).
..

   Enable the Debug Mode in INTP.

|image101| 

|image102|

- Refer to NR detail extraction in section 3.16.3 of this document to
  define the edge texture flat area and configure the appropriate
  intensity.

|image103|

Configure the appropriate X0~X3 parameters to define flat, textured, and
edge areas, and configure the SNR strength to be applied accordingly.

According to different brightness ranges, the SNR intensity and the
configuration of the effective area are defined respectively.

7. The tuning tool suddenly cannot detect UVCD streaming, even after
   restarting the device. What should I do?

ANS

.. tip :: |image104|\ RTK will provide a small program to clear the cache of RealCam .

   Click Delete button.

   |image105|\ Finish will be shown.

8. Use the bin generated by IQPackTool, build it into FW, and it cannot connect to the UVC camera after burning.

.. tip :: Please refer to AmebaPro2 FW_Tool_230105.pdf. Please note the IQ.bin file released by Realtek to customers has a header, while the one used by RealCam tool is headerless.

9. How to set 50Hz or 60Hz anti-flicker?

.. tip :: AT Command : ATIC=1,0x0018, value.For AT Command penetration method and command list, please contact your FAE window












.. _section-16:

.. |49e5b7885d682849526623f041bcf74f| image:: ../../_static/user_manual/24_tunungguide_CHT/image1.jpg
   :width: 6.83333in
   :height: 1.43439in
.. |0b0824664be62141aead42032b360397| image:: ../../_static/user_manual/24_tunungguide_CHT/image2.jpg
   :width: 5.83333in
   :height: 1.0072in
.. |1e15d66adef047853a10f9f7c50073a6| image:: ../../_static/user_manual/24_tunungguide_CHT/image3.png
   :width: 4.91232in
   :height: 3.1224in
.. |image1| image:: ../../_static/user_manual/24_tunungguide_CHT/image4.png
   :width: 6.93333in
   :height: 2.68103in
.. |image2| image:: ../../_static/user_manual/24_tunungguide_CHT/image05eng.png
   :width: 6.07787in
   :height: 1.79647in
.. |image3| image:: ../../_static/user_manual/24_tunungguide_CHT/image06eng.png
   :width: 6.54094in
   :height: 1.95618in
.. |image4| image:: ../../_static/user_manual/24_tunungguide_CHT/image07eng.png
   :width: 6.73028in
   :height: 3.01498in
.. |0c063f10b6bdb12aa4342616f824f4bb| image:: ../../_static/user_manual/24_tunungguide_CHT/image8.png
   :width: 6.92722in
   :height: 4.03958in
.. |18952795efc4249f391397c7719970cc| image:: ../../_static/user_manual/24_tunungguide_CHT/image3.png
   :width: 5.91232in
   :height: 3.5224in
.. |64800c5acd3149a8d9b80b5c68afc40d| image:: ../../_static/user_manual/24_tunungguide_CHT/image9.png
   :width: 3.99596in
   :height: 1.96344in
.. |image5| image:: ../../_static/user_manual/24_tunungguide_CHT/image10.png
   :width: 6.61045in
   :height: 2.83374in
.. |5cb482a58d6f7ee75aeed3b1e839c016| image:: ../../_static/user_manual/24_tunungguide_CHT/image11.jpg
   :width: 4.87139in
   :height: 3.26562in
.. |72bef7d82bd7eb50a315c617d134efa1| image:: ../../_static/user_manual/24_tunungguide_CHT/image12.jpg
   :width: 4.01761in
   :height: 2.68875in
.. |ebeb861081840756ede36237f82dcf21| image:: ../../_static/user_manual/24_tunungguide_CHT/image13.jpg
   :width: 4.8799in
   :height: 3.87583in
.. |image6| image:: ../../_static/user_manual/24_tunungguide_CHT/image14.png
   :width: 5.58611in
   :height: 3.46458in
.. |image7| image:: ../../_static/user_manual/24_tunungguide_CHT/image15.png
   :width: 5.48901in
   :height: 2.86823in
.. |image8| image:: ../../_static/user_manual/24_tunungguide_CHT/image016eng.png
   :width: 6.83333in
   :height: 3.54873in
.. |image9| image:: ../../_static/user_manual/24_tunungguide_CHT/image017eng.png
   :width: 6.83333in
   :height: 2.81302in
.. |image10| image:: ../../_static/user_manual/24_tunungguide_CHT/image018eng.png
   :width: 6.83333in
   :height: 3.20898in
.. |image11| image:: ../../_static/user_manual/24_tunungguide_CHT/image19.png
   :width: 6.83333in
   :height: 2.89968in
.. |image12| image:: ../../_static/user_manual/24_tunungguide_CHT/image020eng.png
   :width: 6.83333in
   :height: 3.20168in
.. |image13| image:: ../../_static/user_manual/24_tunungguide_CHT/image021eng.png
   :width: 6.83333in
   :height: 3.44214in
.. |9360d58e2904730c338c9e463c6f4bdf| image:: ../../_static/user_manual/24_tunungguide_CHT/image22.jpg
   :width: 6.80625in
   :height: 3.24653in
.. |2730cf2ea4e6104a16101ee575c3809f| image:: ../../_static/user_manual/24_tunungguide_CHT/image23.png
   :width: 6.19718in
   :height: 0.84in
.. |e5af1c4eaaa552e5b90cafed7d23cb25| image:: ../../_static/user_manual/24_tunungguide_CHT/image24.png
   :width: 6.62913in
   :height: 1.70229in
.. |acff250b296320877b5214ddef94d0f6| image:: ../../_static/user_manual/24_tunungguide_CHT/image25.png
   :width: 6.6325in
   :height: 1.71833in
.. |image14| image:: ../../_static/user_manual/24_tunungguide_CHT/image26.png
   :width: 6.83333in
   :height: 2.80324in
.. |image15| image:: ../../_static/user_manual/24_tunungguide_CHT/image27.png
   :width: 5.83333in
   :height: 1.37448in
.. |image17| image:: ../../_static/user_manual/24_tunungguide_CHT/image28.png
   :width: 2.06181in
   :height: 0.82083in
.. |image18| image:: ../../_static/user_manual/24_tunungguide_CHT/image29.png
   :width: 2.0625in
   :height: 0.79167in
.. |image16| image:: ../../_static/user_manual/24_tunungguide_CHT/image30.png
   :width: 2.22428in
   :height: 0.83819in
.. |image19| image:: ../../_static/user_manual/24_tunungguide_CHT/image31.png
   :width: 2.59691in
   :height: 2.51447in
.. |76669ae4dec3e3860b28d19559f587f2| image:: ../../_static/user_manual/24_tunungguide_CHT/image32.png
   :width: 5.85019in
   :height: 1.40208in
.. |image20| image:: ../../_static/user_manual/24_tunungguide_CHT/image33.png
   :width: 5.85417in
   :height: 3.20833in
.. |image23| image:: ../../_static/user_manual/24_tunungguide_CHT/image34.png
   :width: 2.00069in
   :height: 1.72361in
.. |image22| image:: ../../_static/user_manual/24_tunungguide_CHT/image35.png
   :width: 1.90278in
   :height: 1.73819in
.. |image21| image:: ../../_static/user_manual/24_tunungguide_CHT/image36.png
   :width: 1.88056in
   :height: 1.73819in
.. |image24| image:: ../../_static/user_manual/24_tunungguide_CHT/image37.png
   :width: 6.83333in
   :height: 2.29739in
.. |image25| image:: ../../_static/user_manual/24_tunungguide_CHT/image38.png
   :width: 5.84306in
   :height: 4in
.. |image26| image:: ../../_static/user_manual/24_tunungguide_CHT/image039eng.png
   :width: 6.83333in
   :height: 4.49447in
.. |image27| image:: ../../_static/user_manual/24_tunungguide_CHT/image040eng.png
   :width: 6.83333in
   :height: 3.61806in
.. |image28| image:: ../../_static/user_manual/24_tunungguide_CHT/image41.png
   :width: 6.68983in
   :height: 1.33799in
.. |image29| image:: ../../_static/user_manual/24_tunungguide_CHT/image42.png
   :width: 6.83333in
   :height: 3.22231in
.. |image30| image:: ../../_static/user_manual/24_tunungguide_CHT/image43.png
   :width: 6.83333in
   :height: 3.22381in
.. |image31| image:: ../../_static/user_manual/24_tunungguide_CHT/image44.png
   :width: 6.83333in
   :height: 2.59128in
.. |cc8e7b1577d5befa95f94fee03cbc274| image:: ../../_static/user_manual/24_tunungguide_CHT/image45.jpg
   :width: 5.88025in
   :height: 2.43958in
.. |a8f821b7c179490ca1a9e75d424e8b41| image:: ../../_static/user_manual/24_tunungguide_CHT/image46.png
   :width: 4.04069in
   :height: 0.50531in
.. |129461df9568b2a7418a1f0e1dc90de0| image:: ../../_static/user_manual/24_tunungguide_CHT/image47.jpg
   :width: 4.26092in
   :height: 2.84846in
.. |efc6841aeec272db02a3e93a10bb56cd| image:: ../../_static/user_manual/24_tunungguide_CHT/image48.jpg
   :width: 3.81382in
   :height: 0.54562in
.. |d9f83eb0063f3d39998143dad826419d| image:: ../../_static/user_manual/24_tunungguide_CHT/image49.jpg
   :width: 3.11775in
   :height: 2.73377in
.. |c3cd105481b998c720c80eea74b482f9| image:: ../../_static/user_manual/24_tunungguide_CHT/image50.jpg
   :width: 6.77383in
   :height: 5.03406in
.. |c71ffcd001bc80405b5720a96187dc80| image:: ../../_static/user_manual/24_tunungguide_CHT/image51.jpg
   :width: 5.70377in
   :height: 3.07241in
.. |79971deeefa89ff0483b0cef213ba6fc| image:: ../../_static/user_manual/24_tunungguide_CHT/image52.jpg
   :width: 5.73282in
   :height: 3.13244in
.. |d04c108d241c1c8c7278952d73825f64| image:: ../../_static/user_manual/24_tunungguide_CHT/image53.jpg
   :width: 5.69552in
   :height: 3.30056in
.. |image32| image:: ../../_static/user_manual/24_tunungguide_CHT/image54.png
   :width: 3.07048in
   :height: 2.13286in
.. |image33| image:: ../../_static/user_manual/24_tunungguide_CHT/image55.png
   :width: 2.95203in
   :height: 2.16514in
.. |image34| image:: ../../_static/user_manual/24_tunungguide_CHT/image56.png
   :width: 2.95117in
   :height: 1.61402in
.. |image35| image:: ../../_static/user_manual/24_tunungguide_CHT/image57.png
   :width: 3.02089in
   :height: 1.62317in
.. |image36| image:: ../../_static/user_manual/24_tunungguide_CHT/image58.png
   :width: 5.83333in
   :height: 1.01111in
.. |image37| image:: ../../_static/user_manual/24_tunungguide_CHT/image59.png
   :width: 6.83333in
   :height: 1.97591in
.. |image38| image:: ../../_static/user_manual/24_tunungguide_CHT/image60.png
   :width: 6.94886in
   :height: 2.38687in
.. |image39| image:: ../../_static/user_manual/24_tunungguide_CHT/image61.png
   :width: 5.05606in
   :height: 0.3279in
.. |image40| image:: ../../_static/user_manual/24_tunungguide_CHT/image62.png
   :width: 5.61685in
   :height: 3.14375in
.. |image41| image:: ../../_static/user_manual/24_tunungguide_CHT/image63.png
   :width: 2.21839in
   :height: 1.9574in
.. |image42| image:: ../../_static/user_manual/24_tunungguide_CHT/image64.png
   :width: 6.21896in
   :height: 0.92159in
.. |image43| image:: ../../_static/user_manual/24_tunungguide_CHT/image65.png
   :width: 6.83333in
   :height: 2.99848in
.. |image44| image:: ../../_static/user_manual/24_tunungguide_CHT/image66.png
   :width: 6.83333in
   :height: 1.68426in
.. |image45| image:: ../../_static/user_manual/24_tunungguide_CHT/image67.png
   :width: 4.64988in
   :height: 2.8443in
.. |image46| image:: ../../_static/user_manual/24_tunungguide_CHT/image68.png
   :width: 6.53333in
   :height: 2.02271in
.. |image47| image:: ../../_static/user_manual/24_tunungguide_CHT/image69.png
   :width: 6.83333in
   :height: 4.80639in
.. |064a8a50e5ae0100238309d750b2e242| image:: ../../_static/user_manual/24_tunungguide_CHT/image70.jpg
   :width: 5.06671in
   :height: 3.17324in
.. |image48| image:: ../../_static/user_manual/24_tunungguide_CHT/image71.png
   :width: 5.13517in
   :height: 4.63142in
.. |image49| image:: ../../_static/user_manual/24_tunungguide_CHT/image72.png
   :width: 5.12817in
   :height: 4.27472in
.. |6f6b1721129b0d03b2c622bd124351ea| image:: ../../_static/user_manual/24_tunungguide_CHT/image73.png
   :width: 5.96538in
   :height: 0.59501in
.. |image50| image:: ../../_static/user_manual/24_tunungguide_CHT/image74.png
   :width: 3.69553in
   :height: 3.25323in
.. |image51| image:: ../../_static/user_manual/24_tunungguide_CHT/image75.png
   :width: 6.83333in
   :height: 4.11222in
.. |2ea11651d66edccdb27ecbe769f97c31| image:: ../../_static/user_manual/24_tunungguide_CHT/image76.jpg
   :width: 5.85687in
   :height: 0.93981in
.. |image52| image:: ../../_static/user_manual/24_tunungguide_CHT/image77.png
   :width: 6.83131in
   :height: 3.14151in
.. |b8965bee784140d35c9b14229b007729| image:: ../../_static/user_manual/24_tunungguide_CHT/image78.png
   :width: 5.91753in
   :height: 1.36125in
.. |image53| image:: ../../_static/user_manual/24_tunungguide_CHT/image79.png
   :width: 6.82837in
   :height: 3.50005in
.. |image54| image:: ../../_static/user_manual/24_tunungguide_CHT/image80.png
   :width: 6.83333in
   :height: 3.75542in
.. |image55| image:: ../../_static/user_manual/24_tunungguide_CHT/image81.png
   :width: 5.19068in
   :height: 3.02666in
.. |image56| image:: ../../_static/user_manual/24_tunungguide_CHT/image82.png
   :width: 6.83333in
   :height: 3.57045in
.. |image57| image:: ../../_static/user_manual/24_tunungguide_CHT/image83.png
   :width: 6.83333in
   :height: 2.46412in
.. |image58| image:: ../../_static/user_manual/24_tunungguide_CHT/image84.png
   :width: 3.5844in
   :height: 2.17998in
.. |image59| image:: ../../_static/user_manual/24_tunungguide_CHT/image85.png
   :width: 3.5844in
   :height: 2.17998in
  
.. |image60| image:: ../../_static/user_manual/24_tunungguide_CHT/image86.png
   :width: 6.8in
   :height: 2.21458in
.. |image61| image:: ../../_static/user_manual/24_tunungguide_CHT/image87.png
   :width: 5.83333in
   :height: 1.15419in
.. |image62| image:: ../../_static/user_manual/24_tunungguide_CHT/image88.png
   :width: 2.82292in
   :height: 1.17708in
.. |image63| image:: ../../_static/user_manual/24_tunungguide_CHT/image89.png
   :width: 6.83333in
   :height: 3.58184in
.. |image64| image:: ../../_static/user_manual/24_tunungguide_CHT/image90.png
   :width: 5.50579in
   :height: 2.28367in
.. |image65| image:: ../../_static/user_manual/24_tunungguide_CHT/image91.png
   :width: 5.50016in
   :height: 2.28365in
.. |image66| image:: ../../_static/user_manual/24_tunungguide_CHT/image92.png
   :width: 5.50924in
   :height: 2.28052in
.. |83f4fc1907b691b1473634fe51025d4d| image:: ../../_static/user_manual/24_tunungguide_CHT/image93.png
   :width: 5.83333in
   :height: 1.2963in
.. |image67| image:: ../../_static/user_manual/24_tunungguide_CHT/image94.png
   :width: 6.83333in
   :height: 2.80381in
.. |image68| image:: ../../_static/user_manual/24_tunungguide_CHT/image95.jpg
   :width: 6.83333in
   :height: 3.00524in
.. |image69| image:: ../../_static/user_manual/24_tunungguide_CHT/image96.png
   :width: 6.83333in
   :height: 2.06389in
.. |image70| image:: ../../_static/user_manual/24_tunungguide_CHT/image97.png
   :width: 6.83333in
   :height: 2.94728in
.. |6920467a6525b8d28109c78c0aa42d15| image:: ../../_static/user_manual/24_tunungguide_CHT/image98.jpg
   :width: 6.832in
   :height: 2.01782in
.. |image71| image:: ../../_static/user_manual/24_tunungguide_CHT/image99.png
   :width: 6.83333in
   :height: 3.01942in
.. |image72| image:: ../../_static/user_manual/24_tunungguide_CHT/image100.png
   :width: 6.83333in
   :height: 1.61458in
.. |0797ab1491624b98c3f63f1be00a2a7a| image:: ../../_static/user_manual/24_tunungguide_CHT/image101.jpg
   :width: 6.81111in
   :height: 1.21389in
.. |image73| image:: ../../_static/user_manual/24_tunungguide_CHT/image102.png
   :width: 1.78819in
   :height: 2.2125in
.. |image74| image:: ../../_static/user_manual/24_tunungguide_CHT/image103.png
   :width: 3.8961in
   :height: 2.71717in
.. |image75| image:: ../../_static/user_manual/24_tunungguide_CHT/image104.jpg
   :width: 6.74in
   :height: 3.03347in
.. |image76| image:: ../../_static/user_manual/24_tunungguide_CHT/image105.png
   :width: 5.83333in
   :height: 1.39583in
.. |5691a8470f9d35c9019ee29f8b985a72| image:: ../../_static/user_manual/24_tunungguide_CHT/image106.png
   :width: 6.83333in
   :height: 3.95799in
.. |image77| image:: ../../_static/user_manual/24_tunungguide_CHT/image107.png
   :width: 5.97681in
   :height: 3.73071in
.. |image78| image:: ../../_static/user_manual/24_tunungguide_CHT/image108.png
   :width: 5.83333in
   :height: 3.22533in
.. |bff0cd25c27d3b80c2747652207eefc7| image:: ../../_static/user_manual/24_tunungguide_CHT/image109.png
   :width: 5.42222in
   :height: 0.57083in
.. |9fc1ff8de3d3a3e6187bfd4de9b7eeeb| image:: ../../_static/user_manual/24_tunungguide_CHT/image110.png
   :width: 5.88264in
   :height: 1.51181in
.. |image79| image:: ../../_static/user_manual/24_tunungguide_CHT/image111.png
   :width: 6.83333in
   :height: 1.79736in
.. |8ccfac62b9cb9604db636eba900862b5| image:: ../../_static/user_manual/24_tunungguide_CHT/image112.png
   :width: 5.93542in
   :height: 1.06153in
.. |image80| image:: ../../_static/user_manual/24_tunungguide_CHT/image113.png
   :width: 6.83333in
   :height: 3.04208in
.. |image81| image:: ../../_static/user_manual/24_tunungguide_CHT/image114.png
   :width: 6.83333in
   :height: 1.56111in
.. |image82| image:: ../../_static/user_manual/24_tunungguide_CHT/image115.png
   :width: 6.83333in
   :height: 1.58451in
.. |image83| image:: ../../_static/user_manual/24_tunungguide_CHT/image116.png
   :width: 5.02316in
   :height: 1.49669in
.. |image84| image:: ../../_static/user_manual/24_tunungguide_CHT/image117.png
   :width: 6.82083in
   :height: 3.18056in
.. |image86| image:: ../../_static/user_manual/24_tunungguide_CHT/image118.png
   :width: 0.62639in
   :height: 0.8875in
.. |image87| image:: ../../_static/user_manual/24_tunungguide_CHT/image119.jpg
   :width: 4.67108in
   :height: 2.35051in
.. |image85| image:: ../../_static/user_manual/24_tunungguide_CHT/image120.png
   :width: 0.75168in
   :height: 1.05235in
.. |image88| image:: ../../_static/user_manual/24_tunungguide_CHT/image121.png
   :width: 5.41319in
   :height: 1.35972in
.. |image89| image:: ../../_static/user_manual/24_tunungguide_CHT/image122.png
   :width: 3.03625in
   :height: 1.75208in
.. |image90| image:: ../../_static/user_manual/24_tunungguide_CHT/image123.png
   :width: 3.03194in
   :height: 1.74653in
.. |image91| image:: ../../_static/user_manual/24_tunungguide_CHT/image124.jpg
   :width: 5.83333in
   :height: 1.63314in
.. |image92| image:: ../../_static/user_manual/24_tunungguide_CHT/image125.png
   :width: 6.80556in
   :height: 3.01319in
.. |531fbdc6660886eeb34eca4b438ef395| image:: ../../_static/user_manual/24_tunungguide_CHT/image126.jpg
   :width: 5.3753in
   :height: 3.23125in
.. |62fe57561ab6d407485b97b4bb2fec81| image:: ../../_static/user_manual/24_tunungguide_CHT/image127.jpg
   :width: 5.86319in
   :height: 1.22778in
.. |ad77fa7370b07fa4c49cbeb33a7e41c3| image:: ../../_static/user_manual/24_tunungguide_CHT/image128.png
   :width: 4.62497in
   :height: 3.37208in
.. |263b8a463b2fd7d0323762d3eccaa4ca| image:: ../../_static/user_manual/24_tunungguide_CHT/image129.png
   :width: 4.62027in
   :height: 3.37208in
.. |d0da14c5a463e55414792dffe49b10f3| image:: ../../_static/user_manual/24_tunungguide_CHT/image130.png
   :width: 4.62027in
   :height: 3.37208in
.. |ae54c50398bc8ffc18d9e2646e0ebff7| image:: ../../_static/user_manual/24_tunungguide_CHT/image131.png
   :width: 4.69952in
   :height: 3.38378in
.. |image93| image:: ../../_static/user_manual/24_tunungguide_CHT/image132.png
   :width: 6.13308in
   :height: 4.06914in
.. |image94| image:: ../../_static/user_manual/24_tunungguide_CHT/image133.png
   :width: 4.04667in
   :height: 2.12751in
.. |image95| image:: ../../_static/user_manual/24_tunungguide_CHT/image134.png
   :width: 3.26667in
   :height: 3.69438in
.. |image96| image:: ../../_static/user_manual/24_tunungguide_CHT/image135.png
   :width: 3.22667in
   :height: 4.23045in
.. |c5cde072ff14d0aefb3ee7a1527f05d9| image:: ../../_static/user_manual/24_tunungguide_CHT/image136.jpg
   :width: 6.83333in
   :height: 3.92169in
.. |image97| image:: ../../_static/user_manual/24_tunungguide_CHT/image137.png
   :width: 2.375in
   :height: 1.65625in
.. |image98| image:: ../../_static/user_manual/24_tunungguide_CHT/image138.png
   :width: 0.82478in
   :height: 1.79176in
.. |28a6fd7d955e0b6b2874a0a9a2f891be| image:: ../../_static/user_manual/24_tunungguide_CHT/image139.jpg
   :width: 2.21245in
   :height: 1.92712in
.. |5c7315124c64d0b806d29f1f389f3788| image:: ../../_static/user_manual/24_tunungguide_CHT/image140.jpg
   :width: 2.38705in
   :height: 1.9306in
.. |image99| image:: ../../_static/user_manual/24_tunungguide_CHT/image141.png
   :width: 2.80565in
   :height: 2.35646in
.. |image100| image:: ../../_static/user_manual/24_tunungguide_CHT/image142.png
   :width: 6.5859in
   :height: 3.73227in
.. |image101| image:: ../../_static/user_manual/24_tunungguide_CHT/image143.png
   :width: 3.83815in
   :height: 2.1323in
.. |image102| image:: ../../_static/user_manual/24_tunungguide_CHT/image144.png
   :width: 3.81443in
   :height: 2.05335in
.. |image103| image:: ../../_static/user_manual/24_tunungguide_CHT/image145.png
   :width: 3.972in
   :height: 2.33383in
.. |image104| image:: ../../_static/user_manual/24_tunungguide_CHT/image146.png
   :width: 3.29064in
   :height: 0.86398in
.. |image105| image:: ../../_static/user_manual/24_tunungguide_CHT/image147.png
   :width: 3.44127in
   :height: 1.03463in
