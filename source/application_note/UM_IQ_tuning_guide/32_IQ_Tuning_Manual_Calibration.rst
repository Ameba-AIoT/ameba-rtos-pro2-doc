IQ Manual Calibration
=====================



Preview video stream via RealCam
--------------------------------

- RealCam does not support Windows 7, please use Windows 10.

- NV16toYUY2 and NV12toYUY2 need to be registered first.

|image1|

**Basic Calibration Module**

- BLC

  - BLC calibration

  - BLC IQ set

- LSC calibration

- AWB calibration

- CCM calibration

Basic Calibration Module_BLC calibration
----------------------------------------

- BLC calibration

  - Make sure the lens module is light-tight. Cover the lens to block
    out light. Use a lens cap or opaque black cloth, and place it in a
    light-free environment. If it's a sensor PCB board, it's suggested
    to use black tape to stick onto the backplate and prevent light
    leakage.

|image2|

**Basic Calibration Module_BLC calibration**

|image3|

**Basic Calibration Module_Fast BLC IQ set**

|image4|

**Basic Calibration Module_Fast BLC IQ set**

- Once saved as a bin file, the BLC settings are completed. You can load
  the bin file to check if it matches the set parameters.

|image5|

**Basic Calibration Module_Manual BLC IQ set**

|image6|

**Need to uncheck Global Dynamic Control first**

**Basic Calibration Module_Manual BLC IQ set**

|image7|

**Basic Calibration Module_Manual BLC IQ set**

- Once saved as a bin file, the BLC settings are completed. You can load
  the bin file to check if it matches the set parameters.

|image8|

**Basic Calibration Module_LSC calibration**

- To calibrate LSC, use integral sphere, DNP lightbox, optical diffuser,
  or milky acrylic panel as a diffuser to cover the lens and align with
  the light source in a uniform environment. The purpose is to ensure
  the uniform. The recommended light source color temperature is D65 or
  D50.

|image9|

**Basic Calibration Module_LSC calibration**

|image10|

**Need to uncheck Global Dynamic Control first**

**Basic Calibration Module_LSC calibration**

- Turn on **Lens Shading Analyze** to observe. The better the RGB curve
  overlap, the better the color cast correction.

|image11|

**Basic Calibration Module_LSC calibration**

|image12|

**Basic Calibration Module_LSC calibration**

|image13|

**Basic Calibration Module_LSC calibration**

|image14|

- Attenuation：The degree of correction for Lens Shading. If set to
  100%, it means that the brightness around the image after correction
  will be the same as the center brightness. To suppress noise at the
  corners, it is recommended to choose an Attenuation of 70% to 90%.

- NLSC 【Get Curve】MLSC 【Get Curve】: If the lens quality is good,
  just do NLSC calibration.

**Basic Calibration Module_LSC calibration**

- Once saved as a bin file, the LSC settings are completed. You can load
  the bin file to check if it matches the set parameters.

|image15|

**AWB calibration**

- It is recommended to prepare a multi-color temperature **lightbox**
  and a **gray card** (gray points will be concentrated in one place)
  for AWB calibration. The suggested calibration color temperature is as
  follows: D75、D65 、D50 、CWF 、U35 、A(U30)

- Because the image presented by realcam is the result of the final
  image processing, it is recommended

  - Do not multiply R and B gains in AWB Gain Adjust, keep it at the
    default value of 256 (1x).

  - Set CCM to the unit matrix.

  - Reset any UV effects to default.

  - In Video property, set saturation to 64(1x)

.. tip :: **In the above functions, make sure to uncheck Global Dynamic
  Control first before making modifications. To change to default
  values, press "write" and "update" (must be done before switching to
  the function page). Save it as a bin file to avoid redoing in case of
  a system crash.**

**AWB calibration**

Do not add R B gain in AWB gain Adjust, the default is 256 (1x)

|image16|

Set the CCM as the identity matrix

|image17|

UV color tune set back to default

|image18|

In the Video property, set the saturation to 64 (1x)

|image19|

**AWB calibration**

- After confirming the above parameters, you can load the saved bin file
  and check the **Global Dynamic Control** mode to begin AWB
  calibration.

|image20|

**AWB calibration**

|image21|

- Decide how many sets of light sources need to be calibrated, and
  modify the **Count of CT Points** column in the ct_setting window.

- Switch the lightbox switch to the light source that needs calibration,
  modify the CT (K) field value in the ct_setting window to match the
  color temperature value of the light source.

**AWB calibration**

- You can modify the X/Y coordinate values in the ct_setting window or
  utilize the real-time display of the mouse cursor position in the
  lower left corner of the AWB Analyze window to obtain the coordinates
  of the current red dot and fill them in as reference values.

|image22|

Ex: color temperature 4020k(In this case, **all the gray dots are
outside the white and gray areas, and the red dot remains fixed at its
default value**. So, you can start by moving the color temperature point
to the place where the gray points are concentrated.Once the red dot is
in a normal position, you can make fine adjustments.)

**AWB calibration**

|image23|

**AWB calibration**

- Switching to Param will automatically calculate the white area (pink)
  and gray area (blue) area, and it can also be modified manually (next
  page)

|image24|

**AWB calibration**

|image25|

**AWB calibration**

- Once saved as a bin file, the AWB settings are completed. You can load
  the bin file to check if it matches the set parameters.

|image26|

**CCM calibration**

- Prepare multi-color temperature light boxes and 24 color charts, and
  do CCM calibration.

- Because the image presented by realcam is the result of the final
  image processing, it is recommended

  - UV color tune set back to default

  - In the Video property, set the saturation to 64 (1x)

- **Tip : In the above functions, make sure to uncheck Global Dynamic
  Control first before making modifications. To change to default
  values, press "write" and "update" (must be done before switching to
  the function page). Save it as a bin file to avoid redoing in case of
  a system crash.**

**CCM calibration**

UV color tune set back to default

|image27|

In the Video property, set the saturation to 64 (1x)

|image28|

**CCM calibration**

- Determine the number of calibration light sources and modify the CCM
  column in the Index Manager window.

|image29|

**CCM calibration**

- Place the Color Checker in the light box, select the color temperature
  light source to be corrected, and **uncheck Global Dynamic Control
  first.**

|image30|

**CCM calibration**

- UVC_AIQ 🡪 UvcAIQ_Init 🡪 Set Patch Position

|image31|

**CCM calibration**

|image32|

**CCM calibration**

|image33|

**CCM calibration**

- Read the CCM value calibrated by AIQ, and after updating, the CCM
  calibration of this color temperature will be completed.

..

   |image34|

**CCM calibration**

- Once saved as a bin file, the CCM settings are completed. You can load
  the bin file to check if it matches the set parameters.

- Load bin file and check the Global Dynamic Control mode , switch to
  the next color temperature light source, and get the next Index color
  temperature value, repeat the above steps, and calibrate the CCM of
  each color temperature once.

|image35|

Added IQ parameter range
------------------------

- Purpose: When a certain scene needs to be adjusted accurately, the
  color temperature and gain range can be added for this scene, and the
  parameters between other scenes will be formed by static switching and
  dynamic interpolation.

- category:

- Add color temperature range

- Add exposure ratio range

- New gain range

**Added IQ parameter range**

- Operation method:

|image36|

**Add color temperature range**

|image37|

**Added color temperature range - color temperature threshold**

- IN: Displays the threshold value of entering the color temperature
  range **(color temperature up)**

- OUT: Displays the threshold value for leaving the color temperature
  range **(color temperature descending)**

|image38|

**Add exposure ratio range**

|image39|

**Added exposure ratio interval-exposure ratio threshold**

- vHDR sensor will start.

- Magnification is 2^𝑛: 2, 4, 8, 16, 32, 64.

- If the exposure ratio falls between the two sets of Indexes, the
  parameters of the previous set of indexes will be maintained.

..

   |image40|

|image41|

**New gain range**

|image42|

.. |image1| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image1.png
   :width: 7.56806in
   :height: 2.56944in
.. |image2| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image2.png
   :width: 5.76806in
   :height: 4.23472in
.. |image3| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image3.png
   :width: 7.56806in
   :height: 3.13889in
.. |image4| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image4.png
   :width: 7.56806in
   :height: 3.15in
.. |image5| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image5.png
   :width: 5.76806in
   :height: 4.26944in
.. |image6| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image6.png
   :width: 7.56806in
   :height: 2.50833in
.. |image7| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image7.png
   :width: 7.56806in
   :height: 3.38056in
.. |image8| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image8.png
   :width: 5.26806in
   :height: 4.33819in
.. |image9| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image9.png
   :width: 7.56806in
   :height: 2.76458in
.. |image10| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image10.png
   :width: 7.56806in
   :height: 2.69583in
.. |image11| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image11.png
   :width: 7.56806in
   :height: 2.86111in
.. |image12| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image12.png
   :width: 7.56806in
   :height: 2.90833in
.. |image13| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image13.png
   :width: 7.56806in
   :height: 3.2806in
.. |image14| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image14.png
   :width: 7.56806in
   :height: 2.38472in
.. |image15| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image15.png
   :width: 5.36806in
   :height: 4.25694in
.. |image16| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image16.png
   :width: 7.56806in
   :height: 0.88611in
.. |image17| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image17.png
   :width: 5.76806in
   :height: 1.76319in
.. |image18| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image18.png
   :width: 5.76806in
   :height: 2.37014in
.. |image19| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image19.png
   :width: 7.56806in
   :height: 1.18056in
.. |image20| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image20.png
   :width: 7.56806in
   :height: 2.39097in
.. |image21| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image21.png
   :width: 7.56806in
   :height: 2.11597in
.. |image22| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image22.png
   :width: 7.56806in
   :height: 2.61181in
.. |image23| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image23.png
   :width: 7.56806in
   :height: 2.86319in
.. |image24| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image24.png
   :width: 7.56806in
   :height: 4.70139in
.. |image25| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image25.png
   :width: 7.56806in
   :height: 3.33403in
.. |image26| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image26.png
   :width: 7.56806in
   :height: 3.27708in
.. |image27| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image27.png
   :width: 5in
   :height: 1.9685in
.. |image28| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image28.png
   :width: 5.76806in
   :height: 0.88681in
.. |image29| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image29.png
   :width: 7.56806in
   :height: 2.92014in
.. |image30| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image30.png
   :width: 5.76806in
   :height: 2.87083in
.. |image31| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image31.png
   :width: 7.56806in
   :height: 2.74167in
.. |image32| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image32.png
   :width: 7.56806in
   :height: 3in
.. |image33| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image33.png
   :width: 7.56806in
   :height: 3.14514in
.. |image34| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image34.png
   :width: 7.56806in
   :height: 2.87639in
.. |image35| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image35.png
   :width: 5.56806in
   :height: 4.39444in
.. |image36| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image36.png
   :width: 7.56806in
   :height: 3.11597in
.. |image37| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image37.png
   :width: 7.56806in
   :height: 3.08056in
.. |image38| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image38.png
   :width: 7.56806in
   :height: 3.22569in
.. |image39| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image39.png
   :width: 7.56806in
   :height: 3.10208in
.. |image40| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image40.png
   :width: 5.42079in
   :height: 2.86964in
.. |image41| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image41.png
   :width: 7.15374in
   :height: 5.81176in
.. |image42| image:: ../../_static/user_manual/32_IQ_Tuning_Manual_Calibration/image42.png
   :width: 7.56806in
   :height: 3.01528in
