RTK IQ Validation Training
==========================

**終端 Camera拍照主客觀測試標準**
---------------------------------


**Brief
introduction：**\ 本標準規定了終端公司Camera圖像效果測試規範，明確了各個測試項目的具體測試方法、測試標準；供公司內部決策參考，適用於Camera模組的規格制定、拍照產品設計和測試、及品質控制等方面。

**Key
words：**\ Camera、圖像效果、測試方法、測試標準、解析力、色彩還原、均勻性、幾何失真、白平衡、動態範圍、信噪比、視場角、自動曝光測試、閃光燈測試、AF測試、Flare測試、缺陷圖元、溫升性能測試

**Term & Definition：**

+---------+--------------+--------------------------------------------+
| **A     | **Full       | **Chinese explanation**                    |
| bbrevia | spelling**   |                                            |
| tions** |              |                                            |
+=========+==============+============================================+
| Res     | Resolution   | 解析力                                     |
| olution |              |                                            |
+---------+--------------+--------------------------------------------+
| 3A      | AE , AF ,    | 自動曝光，自動對焦，自動白平衡             |
|         | AWB          |                                            |
+---------+--------------+--------------------------------------------+
| FOV     | Field of     | 視場角                                     |
|         | View         |                                            |
+---------+--------------+--------------------------------------------+
| SNR     | Signal/Noise | 信噪比                                     |
|         | Ratio        |                                            |
+---------+--------------+--------------------------------------------+
| MTF     | Modulation   | 調製傳遞函數                               |
|         | Transfer     |                                            |
|         | Function     |                                            |
+---------+--------------+--------------------------------------------+
| HDR     | High-        | 高動態範圍                                 |
|         | DynamicRange |                                            |
+---------+--------------+--------------------------------------------+
| LSC     | Lens Shading | 鏡頭失光補正                               |
|         | Correction   |                                            |
+---------+--------------+--------------------------------------------+

**RTK IQ Function Overview**

+----------------+-------------------------------------------+--------+
| Quality        | Shading Correction (RI / Color)           | DUT    |
+----------------+-------------------------------------------+--------+
| 01             | [D65] Relative illumination ≥ 80%         | Pass   |
+----------------+-------------------------------------------+--------+
| 02             | [CWF] Relative illumination ≥ 80%         | Pass   |
+----------------+-------------------------------------------+--------+
| 03             | [A] Relative illumination ≥ 80%           | Pass   |
+----------------+-------------------------------------------+--------+

+----------------+-------------------------------------------+--------+
| Quality        | Auto White Balance                        | 　     |
+----------------+-------------------------------------------+--------+
| 01             | [D65] #20 ~ #23 Max ΔS ≤ 0.1              | Pass   |
+----------------+-------------------------------------------+--------+
| 02             | [CWF] #20 ~ #23 Max ΔS ≤ 0.1              | Pass   |
+----------------+-------------------------------------------+--------+
| 03             | [A] #20 ~ #23 Max ΔS ≤ 0.1                | Pass   |
+----------------+-------------------------------------------+--------+

+----------------+-------------------------------------------+--------+
| Quality        | Saturation / Color accuracy               | 　     |
+----------------+-------------------------------------------+--------+
| 01             | [D65] mean ΔC ≤ 10                        | Pass   |
+----------------+-------------------------------------------+--------+
| 02             | [D65] max ΔC ≤ 30                         | Pass   |
+----------------+-------------------------------------------+--------+
| 03             | [D65] mean ΔE ≤ 20                        | Pass   |
+----------------+-------------------------------------------+--------+
| 04             | [D65] max ΔE ≤ 30                         | Pass   |
+----------------+-------------------------------------------+--------+
| 05             | [D65] Saturation [100% ~ 125%]            | Pass   |
+----------------+-------------------------------------------+--------+
| 06             | [CWF] mean ΔC ≤ 10                        | Pass   |
+----------------+-------------------------------------------+--------+
| 07             | [CWF] max ΔC ≤ 30                         | Pass   |
+----------------+-------------------------------------------+--------+
| 08             | [CWF] mean ΔE ≤ 20                        | Pass   |
+----------------+-------------------------------------------+--------+
| 09             | [CWF] max ΔE ≤ 30                         | Pass   |
+----------------+-------------------------------------------+--------+
| 10             | [CWF] Saturation [100% ~ 125%]            | Pass   |
+----------------+-------------------------------------------+--------+
| 11             | [A] mean ΔC ≤ 12                          | Pass   |
+----------------+-------------------------------------------+--------+
| 12             | [A] max ΔC ≤ 30                           | Pass   |
+----------------+-------------------------------------------+--------+
| 13             | [A] mean ΔE ≤ 20                          | Pass   |
+----------------+-------------------------------------------+--------+
| 14             | [A] max ΔE ≤ 30                           | Pass   |
+----------------+-------------------------------------------+--------+
| 15             | [A] Saturation [90% ~ 110%]               | Pass   |
+----------------+-------------------------------------------+--------+

+----------------+-------------------------------------------+--------+
| Quality        | MTF                                       | 　     |
+----------------+-------------------------------------------+--------+
| 01             | 中心水平分辨率 ≥ 800                      | Pass   |
+----------------+-------------------------------------------+--------+
| 02             | 中心垂直分辨率 ≥ 800                      | Pass   |
+----------------+-------------------------------------------+--------+
| 03             | 四角水平分辨率 ≥ 400                      | Pass   |
+----------------+-------------------------------------------+--------+
| 04             | 四角垂直分辨率 ≥ 400                      | Pass   |
+----------------+-------------------------------------------+--------+

+----------------+-------------------------------------------+--------+
| Quality        | SNR                                       | 　     |
+----------------+-------------------------------------------+--------+
| 01             | [D65] [Standard] R/G/B/Y ≥ 39             | Pass   |
+----------------+-------------------------------------------+--------+
| 02             | [CWF] [Standard] R/G/B/Y ≥ 39             | Pass   |
+----------------+-------------------------------------------+--------+
| 03             | [A] [Standard] Y ≥ 39                     | Pass   |
+----------------+-------------------------------------------+--------+

+----------------+-------------------------------------------+--------+
| Quality        | Defect Pixel Correction                   | 　     |
+----------------+-------------------------------------------+--------+
| 01             | 全黑場景無Hot Pixel                       | Pass   |
+----------------+-------------------------------------------+--------+
| 02             | 全白場景無Dead Pixel                      | Pass   |
+----------------+-------------------------------------------+--------+

+----------------+-------------------------------------------+--------+
| Quality        | Dynamic Range                             | 　     |
+----------------+-------------------------------------------+--------+
| 01             | [D65] [Standard]灰階亮度最大值 ≥ 200      | Pass   |
+----------------+-------------------------------------------+--------+
| 02             | [D65] [Standard] 可分辨灰階數             | Pass   |
+----------------+-------------------------------------------+--------+

**Set up standard testing scene**

|image1|

**客觀測試標準-------MTF**
--------------------------

1.1 解析力和銳化測試Resolution Sharpness

1.1.1 測試目的

測試拍照系統的解析力，包含中心解析力和邊角解析力；解析力又稱解析度，是Camera測試主要參數之一。在

拍照系統同等設計條件下，若解析力指標不達標，拍出的照片將模糊不清。

1.1.2 測試設備

ISO12233
Chart（1x，2x，4x；標準型和增強型）、手機夾具、大小三腳架、均勻光照明設備、色溫照度計、

顯微鏡、擦鏡布、酒精。

1.1.3 測試軟體

HYRes3和Imatest。

1.1.4 測試環境

保證反射式測試圖的物體照度為1000
Lux±10%；保證ISO12233整個Chart表面的亮度值均勻性偏差小於10%

（最大不能超過20%）。測試Chart周圍區域具有低反射係數，測試區域要遮罩任何反射光，具有有效光譜中性。

1.1.5 測試步驟

1、 拍照設置

調節Camera拍照系統相關參數設置為恢復預設模式；保證鏡頭乾淨、無損傷。

2、 測試環境搭建

|image2|

|image3|

**客觀測試標準-------MTF 解析度測試**
-------------------------------------

1. 採用HYRes軟體進行解析力測試。

2. HYRes3.1軟體使用方法：

（1）測試設置
使用軟體打開解析度測試圖像，在Trim介面下，按照如下圖所示的紅框來框選測試區域，注意左右區域選擇邊框要將線外的白色和黑色部分包含進去一些。

（2）測試讀數

|image4|

+-----------------------------------------+-------------+-------------+
| Sensor像素                              | 解析力值    |             |
+-----------------------------------------+-------------+-------------+
|                                         | 中心        | 四角        |
+-----------------------------------------+-------------+-------------+
| VGA 0.3M（640*480）                     | 300         | 250         |
+-----------------------------------------+-------------+-------------+
| 1.3M（1280*960）                        | 600         | 500         |
+-----------------------------------------+-------------+-------------+
| 2M（1600*1200）                         | 800         | 600         |
+-----------------------------------------+-------------+-------------+
| 3M（2048*1536）                         | 1000        | 800         |
+-----------------------------------------+-------------+-------------+
| 5M（2592*1936）                         | 1250        | 1000        |
+-----------------------------------------+-------------+-------------+
| 8M（3264*2448）                         | 1600        | 1200        |
+-----------------------------------------+-------------+-------------+
| 13M（4224*3136）                        | 2000        | 1500        |
+-----------------------------------------+-------------+-------------+

**客觀測試標準-------SFR 測試**
-------------------------------

1. 採用Imatest軟體進行SFR測試。

2. 選取SFR測試，如下圖和所示框選測試Chart中心和邊緣區域斜邊

|image5|

|image6|

**客觀測試標準-------AWB**
--------------------------

|image7|

|image8|

**客觀測試標準-------SNR**
--------------------------

1. 採用Imatest軟體進行SNR測試。

2. 選取SNR測試，如下圖和所示框選24 Color Chart

3. 計算SNR信噪比資訊

|image9|

|image10|

**客觀測試標準-------Color Accuracy**
-------------------------------------

1. 採用Imatest軟體進行色彩分析準確度測試。

2. 選取24 color chart，框選對角框

3. 計算24 color chart 色彩分析

|Canon_S110_DAY|

|image11|

|image12|

**MSLync & Skype**
-------------------

- Microsoft Lync Devices Video Capture Specification 和 Skype Hardware
  Certification Specification 為目前業界針對NBCam,
  Webcam做影像量測最常使用的兩套規格.

- MSLync的規格分成: Basic, Standard and Premium,
  一般客戶通常只要求過Standard.

- Skype的規格分成: preferred level and required level (a subset of
  preferred level), 通常都是要過 preferred level.

- MTF和SNR為重點測試項目(通常這兩項測試要一直做tradeoff).

- 測試環境&設定比較:

|1|

測試環境

|image13|

**Test Items of MS Lync (ver.E)**

|MSLync_TestItems2|

**MS Lync: Image Resolution Quality (MTF, Oversharpening, Edge
Roughness) (I)**

- 此項測試目的為確保影像是否“夠”清晰.

- MTF30:
  即量測edge當MTF=30%時所量測到的頻率(cycles/pixel),頻率越高表示解像力越好.

- Sharpening (oversharpening or undersharpening): 即量測edge的銳利度;
  可藉由調整ISP端的sharpness值, 來加強/減弱edge的黑邊和白邊.

- Edge Roughness: 即量測edge的粗糙度; 在低亮度環境雜訊較大時,
  此量測值通常會偏高, 要適度的做denoise來減低roughness.

|image14|

|image15|

|image16|

**MS Lync: Spatial Noise (I)**
------------------------------

|image17|

|image18|

|image19|

|image20|

.. tip :: **拍照測試過程有關的策略原則**
 1、客觀測試拍照過程發生了異常問題，儘量拿去各種對應主觀場景進行測驗。
 2、客觀測試資料瀕臨測試標準邊緣和存在爭議的情況，需要進行主觀測試檢驗，必要的情況要進行評測。
 3、客觀測試與主觀測試結果衝突時，\ **主要遵守主觀測試結果**\ ，或者進行多方面評測。
 4、增加一些客觀測試項和附加用的測試內容是必要的；減少或者簡化測試，需要有充足的分析和理由，在必要的情況下以文檔備案

**主觀測試標準**
----------------

測試場景及測試項

|image21|

|image22|

|畫面剪輯|

|image23|

|image24|

**DXOMARK Camera V4 Protocol**
------------------------------

|image25|

**Photo Color Outdoor**

1、White Balance remain a challenge for every type of camera.

2、Color saturation for saturated colors is also important.

3、Skin tone colors.

|image26|

**Portrait and HDR**

1、In both portrait and landscape photography, a lot of scenes require
strong HDR capacities to preserve both dark and bright parts in the
image.

2、High dynamic scenes have been added to the protocol to make it more
representative of typical usages.

|image27|

**Photo Exposure Outdoor**

1、Target exposure is usually accurate.

2、Dynamic range is extended.

|image28|

**Photo Texture/Noise & HDR**

1、Texture for HDR scene can vary significantly.

2、Texture for portrait, and portrait with motion is still complicated
even for best devices..

3、Skin tone colors.

|image29|

**Portrait and lowlight**

1、Dimmer light conditions are tougher to handle, devices can struggle
providing a good exposure while preserving a decent texture-noise
trade-off. Lowlight conditions are more represented in this new protocol
version, with various use cases.

2、Skin tone colors.

|image30|

**Smart Capture test plan**

|image31|

|image32|

|image33|

**Subjective test plan**
|image38|

**Scene Map**

|image39|

|image40|

**RTK verification scene**
|image41|

|image42|

|image43|

|image44|

|image45|

|image46|

|image47|

|image48|

|image49|

|image50|

|image51|

|image52|

|image53|





.. |image1| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image1.png
   :width: 7.26806in
   :height: 3.41181in
.. |image2| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image2.png
   :width: 3.99446in
   :height: 2.42393in
.. |image3| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image3.png
   :width: 7.26806in
   :height: 2.46319in
.. |image4| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image4.png
   :width: 7.26806in
   :height: 3.07431in
.. |image5| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image5.png
   :width: 5.93253in
   :height: 3.79416in
.. |image6| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image6.png
   :width: 7.26806in
   :height: 3.87431in
.. |image7| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image7.png
   :width: 7.26806in
   :height: 2.32986in
.. |image8| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image8.png
   :width: 7.26806in
   :height: 2.31944in
.. |image9| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image9.png
   :width: 7.26806in
   :height: 2.36736in
.. |image10| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image10.png
   :width: 7.26806in
   :height: 2.84028in
.. |Canon_S110_DAY| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image11.jpeg
   :width: 4.03472in
   :height: 3.02951in
.. |image11| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image12.png
   :width: 7.26806in
   :height: 7.11389in
.. |image12| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image13.png
   :width: 7.26806in
   :height: 4.75625in
.. |1| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image14.jpeg
   :width: 6.60714in
   :height: 2.07633in
.. |image13| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image15.png
   :width: 7.26806in
   :height: 4.42222in
.. |MSLync_TestItems2| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image16.jpeg
   :width: 6.90278in
   :height: 6.22743in
.. |image14| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image17.png
   :width: 7.26806in
   :height: 5.90903in
.. |image15| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image18.png
   :width: 7.26806in
   :height: 5.27083in
.. |image16| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image19.png
   :width: 7.26806in
   :height: 4.26389in
.. |image17| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image20.png
   :width: 7.26806in
   :height: 6.92708in
.. |image18| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image21.png
   :width: 7.26806in
   :height: 8.03542in
.. |image19| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image22.png
   :width: 7.26806in
   :height: 6.91042in
.. |image20| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image23.png
   :width: 7.26806in
   :height: 6.30972in
.. |image21| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image24.png
   :width: 7.26806in
   :height: 3.56042in
.. |image22| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image25.png
   :width: 7.26806in
   :height: 3.14583in
.. |畫面剪輯| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image26.png
   :width: 7.26806in
   :height: 3.92708in
.. |image23| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image27.png
   :width: 7.26806in
   :height: 2.14722in
.. |image24| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image28.png
   :width: 7.26806in
   :height: 3.15972in
.. |image25| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image29.png
   :width: 7.26806in
   :height: 3.14167in
.. |image26| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image30.png
   :width: 7.26806in
   :height: 2.52222in
.. |image27| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image31.png
   :width: 7.26806in
   :height: 2.44167in
.. |image28| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image32.png
   :width: 7.26806in
   :height: 2.54375in
.. |image29| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image33.png
   :width: 7.26806in
   :height: 3.50486in
.. |image30| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image34.png
   :width: 7.26806in
   :height: 2.43611in
.. |image31| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image35.png
   :width: 7.26806in
   :height: 4.63264in
.. |image32| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image36.png
   :width: 7.26806in
   :height: 5.82014in
.. |image33| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image37.png
   :width: 7.26806in
   :height: 2.96181in
.. |image38| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image38.png
   :width: 7.56806in
   :height: 3.65972in
.. |image39| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image39.png
   :width: 7.56806in
   :height: 4.03333in
.. |image40| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image40.png
   :width: 7.56806in
   :height: 3.34792in
.. |image41| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image41.png
   :width: 7.56806in
   :height: 3.34792in   
.. |image42| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image42.png
   :width: 7.56806in
   :height: 3.04792in   
.. |image43| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image43.png
   :width: 7.56806in
   :height: 3.04792in
.. |image44| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image44.png
   :width: 7.56806in
   :height: 3.04792in   
.. |image45| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image45.png
   :width: 7.56806in
   :height: 3.04792in   
.. |image46| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image46.png
   :width: 7.56806in
   :height: 3.04792in   
.. |image47| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image47.png
   :width: 7.56806in
   :height: 3.04792in   
.. |image48| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image48.png
   :width: 7.56806in
   :height: 3.04792in   
.. |image49| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image49.png
   :width: 7.56806in
   :height: 3.54792in   
.. |image50| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image50.png
   :width: 7.56806in
   :height: 3.04792in      
.. |image51| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image51.png
   :width: 7.56806in
   :height: 3.54792in      
.. |image52| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image52.png
   :width: 7.56806in
   :height: 3.04792in   
.. |image53| image:: ../../_static/user_manual/31_RTK_IQ_Vlidation_training/image53.png
   :width: 7.56806in
   :height: 3.54792in               