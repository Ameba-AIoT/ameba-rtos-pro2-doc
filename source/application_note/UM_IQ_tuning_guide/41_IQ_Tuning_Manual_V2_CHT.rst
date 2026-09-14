IQ Tuning Manual V2
===================

.. contents::
  :local:
  :depth: 2

|image76|

Document Version: V 2.0

Release Date: 2023/06/01

COPYRIGHT
---------

©2021 **Realtek Semiconductor Corp**. All rights reserved. No part of
this document may be reproduced, transmitted, transcribed, stored in a
retrieval system, or translated into any language in any form or by any
means without the written permission of **Realtek Semiconductor Corp**.

**DISCLAIMER** Realtek provides this document "as is", without warranty
of any kind, neither expressed nor implied, including, but not limited
to, the particular purpose. Realtek may make improvements and/or changes
in this document or in the product described in this document at any
time. This document could include technical inaccuracies or typographical
errors.

**TRADEMARKS** Realtek is a trademark, of **Realtek Semiconductor
Corporation**. Other names mentioned in this document are
trademarks/registered trademarks of their respective owners.

**USING THIS DOCUMENT** This document is intended for the hardware and
firmware engineer's general information on the Realtek IP Camera IC.
Though every effort has been made to ensure that this document is current
and accurate, more information may have become available subsequent to
the production of this guide. In that event, please contact your Realtek
representative for additional information that may help in the
development process.

|image77|

1. 工具概述
-----------

**RealCam Pro** 是 Realtek 提供的圖像調試工具，用於 **Amebapro 2** 芯片的影像畫質調整與校正。透過此工具，您可以即時瀏覽樣機的影像串流、調整 ISP 各模組的參數，並將調試完成的參數導入至樣機內生效。

本章將引導您完成工具的安裝與啟用，並確認軟硬體環境已準備就緒，可開始進行圖像調試作業。

1.1 軟硬件版本說明
^^^^^^^^^^^^^^^^^^

**RealCam Pro** 支援 **Amebapro 2** 系列的圖像芯片。樣機透過 USB 連接至電腦後，工具會自動判讀所連接的芯片型號。

若成功識別為 **Amebapro 2**，工具列上會顯示 **ISP Tuning Pro** 選項，如下圖所示，代表芯片識別成功。

< Amebapro 2 的芯片版本 >

|image78|

點選 **ISP Tuning Pro** 即可開啟圖像調試視窗，開始進行參數調整。

.. _realcam-install:

1.1.1 RealCam Pro 軟件安裝與啟用
""""""""""""""""""""""""""""""""""

**RealCam Pro** 無需額外的安裝程序。將交付軟件存放於電腦後，直接執行 **RealCam.exe** 即可啟動。

第一次執行 **RealCam Pro** 時，系統會自動彈出註冊視窗，要求輸入 SW Key 以完成啟用：

**步驟 1.** 於視窗中輸入Realtek技術支援人員的 Email Address

**步驟 2.** 點選【Get Key】即可取得 SW Key，系統會將 Key 寄送至您的信箱；收到 Key 後貼上於啟用視窗，點選【Activate】即可完成啟用。此流程每台電腦僅需執行一次。

若遇網路連線問題，可改用離線方式完成啟用：

**步驟 1.** 點選【OK】即可跳過錯誤訊息

**步驟 2.** 點選【Dump】即可產生 SW Key 檔案，將該檔案提供給Realtek支援人員；收到 SW Key 後貼上於啟用視窗，點選【Activate】即可完成啟用。此流程每台電腦僅需執行一次。

< RealCam Pro 註冊視窗 >

|image79|

2. 界面功能介紹
---------------

本章將介紹 **RealCam Pro** 的主要操作介面與功能，包含樣機連線、串流取像、IQ 參數的上傳、下載、載入與儲存，以及各影像模組的校正流程。

**名詞對照說明**

RealCam Pro 的 IQ 參數操作涉及多種方向與工具，以下表格說明本章所使用的術語對應關係，務必參考下表的 **資料流向** 進行對照：

.. list-table::
   :header-rows: 1
   :widths: 12 18 20 50

   * - 工具介面（UI）
     - 使用工具/位置
     - 資料流向 (Source -> Destination)
     - 功能說明
   * - **Upload**
     - FW Manager
     - PC -> 板端 (Flash)
     - 將 ``.bin`` 參數檔\ **燒錄**\ 至樣機，變更永久生效（重開機仍有效）。
   * - **Download**
     - FW Manager
     - 板端 (Flash) -> PC
     - 從樣機韌體中\ **讀回 (Dump)**\ 現有 ``.bin`` 參數檔，用於備份或比對。
   * - **Load IQ Table**
     - ISP Tuning Pro 選單
     - 本地檔案 -> 工具 (RAM)
     - 將電腦上的 ``.bin`` 參數檔開啟並將當前的參數替換掉，以便繼續調試或比對。
   * - **Save IQ Table**
     - ISP Tuning Pro 選單
     - 工具 (RAM) -> 本地檔案
     - 將樣機中目前的 IQ 調試結果另存為本地端的 ``.bin`` 參數檔案。
   * - **Write**
     - ISP Tuning Pro 右上角
     - 工具 -> 板端 (暫存 RAM)
     - 即時將參數套用至連線中的樣機，供動態觀察效果（\ **重開機後失效**\ ）。
   * - **Read**
     - ISP Tuning Pro 右上角
     - 板端 (暫存 RAM) -> 工具
     - 即時自樣機讀取目前生效的暫存參數值至 RealCam Pro，用以確認狀態。

.. note::
   **開發者提示 (Developer Note)：**
   
   * ``Upload`` 實際物理動作為 **Program / Flash** (寫入板端 Flash)
   * ``Download`` 實際物理動作為 **Read back / Dump** (自板端讀回 PC)
   * 偵錯時請特別留意資料是寫入 **Flash**\ （永久）還是 **RAM**\ （暫存）
   
   

首先於 :ref:`2.1 快速入門 <section-2-1>` 認識主介面的基本佈局與功能區塊，並了解如何開啟圖像調試視窗。

:ref:`2.2 連線樣機 <section-2-2>` 說明樣機的連線方式，以及 UVC Streaming、Raw Streaming 等串流模式的切換操作，並提供單幀影像（RAW、JPG、BMP）的擷取步驟。

:ref:`2.3 上傳與下載 IQ 參數 <upload-download>` 說明如何透過 **FW Manager** 的 Upload 操作將 ``.bin`` 參數檔燒入至樣機，以及如何透過 Download 操作從樣機備份現有 IQ 參數。

:ref:`2.4 儲存 IQ 參數 <save-iq>` 說明如何透過 **ISP Tuning Pro** 的 Save IQ Table 功能，將調試完成的參數儲存為 ``.bin`` 檔案，供版本管控與後續上傳使用。

:ref:`2.5 校正模組 <section-2-5>` 說明各影像模組的校正流程。不同相機模組因硬體差異而有不同的圖像特性，因此在調試前須先執行校正。校正模組包含 BLC、RNR、LSC、AWB、CCM 等。

.. _section-2-1:

2.1 快速入門
^^^^^^^^^^^^

完成 :ref:`1.1.1 RealCam Pro 軟件安裝與啟用 <realcam-install>` 後，即可啟動 **RealCam Pro** 開始快速入門。

開啟後的主畫面如下圖所示。除標題列顯示的名稱與版本序號外，畫面主要分為三個區域：

1) 工具列
2) 功能按鈕區（連線出圖時才會顯示）
3) 串流/圖像顯示區

< 圖像調試工具 RealCam Pro 的主畫面 >

|image80|

連線樣機後，點選主畫面工具列上的 \ **Vendor\\ISP Tuning Pro**\ 即可開啟圖像參數群組切換視窗與圖像調試視窗，可參閱 :ref:`3. 圖像調試步驟 <image-tuning-steps>` 進行。

< 參數群組切換視窗 >

|image81|

< 圖像調試視窗 >

|image82|

.. _section-2-2:

2.2 連線樣機 進行串流取像與 IQ 調試
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**RealCam Pro** 可搭配具備 UVC 串流功能的樣機使用。連線後可透過即時串流進行 IQ 調試、參數儲存與上傳，以及圖像分析。即使不啟用即時串流，仍可連線進行 IQ 調試與參數管理。

.. _uvc-streaming:

2.2.1 UVC Streaming
""""""""""""""""""""

啟動 **RealCam Pro** 時，若已連接 **Amebapro 2** 樣機，系統會自動顯示即時串流。可透過工具列的 **Device** 選單確認樣機是否被正確識別；正確識別時會顯示 **USB UVC CLASS** 字樣。

< 連接 Amebapro 2 時的工具列 / Device 頁面 >

|image83|

.. _raw-streaming:

2.2.2 RAW Streaming
""""""""""""""""""""

RAW Streaming 是將 Sensor 的 Bayer Data 直接輸出，略過 ISP 處理程序，可用來抓拍 RAW Data，藉此判斷 Sensor 的雜訊程度，或作為 BLC 校正的依據。

操作方式是在 UVC 串流下，點選 **Vendor / RNR Calibration** 即可將 UVC streaming 切換為 Raw streaming。

< UVC Streaming 點選 RNR Calibration 功能 >

|image84|

進入校正頁面後，**RealCam Pro** 串流/圖像顯示區將出現 RAW Streaming 即時畫面：

< RAW Streaming 串流/圖像顯示 >

|image85|

2.2.3 Capture RAW
""""""""""""""""""

完成以上 :ref:`2.2.2 RAW Streaming <raw-streaming>` 操作後，點選功能按鈕區圖示即可設定檔案儲存路徑與檔案格式。

< Capture RAW 操作 >

|image86|

以上設置完成後，可在 RAW Streaming 狀態下點選 **F6** 進行抓拍。抓拍下來的 RAW 檔以附檔名 **.cap** 存在於指定路徑。

RAW Data 檔案的資料除了 128 Bytes headers 以外，其餘即為 pure RAW Data 的 little-endian 格式儲存，詳細可另外參閱 :ref:`4.2 Realtek RAW Data 格式介紹 <raw-data-format>`\ 。

2.2.4 Capture BMP or JPG
"""""""""""""""""""""""""

完成 :ref:`2.2.1 UVC Streaming <uvc-streaming>` 操作後，點選功能按鈕區圖示即可設定檔案儲存路徑與檔案格式，操作步驟如下：

< Capture BMP or JPG 操作 >

|image87|

以上設置完成後，可在 UVC Streaming 狀態下點選 **F6** 進行抓拍。

.. _upload-download:

2.3 IQ 參數的燒錄與讀回（Upload / Download）
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

在 **Amebapro 2** 芯片中，IQ 參數以獨立的 ``.bin`` 檔案形式存在。

完整的參數檔案（Complete IQ）包含多種曝光模式的調試參數，例如 Linear 與 HDR。每種曝光模式下固定由兩個模式（Mode）組成，例如白天與夜視。

每種模式的 IQ 參數也可獨立成一個檔案（Partial IQ）。燒錄時可選擇完整的 Complete IQ，或僅針對特定模式的 Partial IQ。

Complete IQ 包含多個 Partial IQ，可透過 SW API 切換。以預設設定為例：

- Linear 曝光下，白天調用 **Group 0 Day**，夜視切換為 **Group 0 Night**
- HDR 曝光下，白天調用 **Group 1 Day**，夜視切換為 **Group 1 Night**

.. note::
   Complete IQ 的結構如圖所示：至多包含 6 組 IQ Table，分別對應 Linear 與 HDR mode；每種模式下有 3 組 IQ Table，分別對應 Day、Night、Other mode。SDK 所需格式為 Complete IQ（Full IQ），而非單一的 Partial IQ。若需將 ``IQ.bin`` 放入 SDK 編譯，**必須在檔案前端加入 header**\ （記錄檔案大小資訊），詳見 :ref:`7.1.2 <iq-bin-upload-fail>`\ 。

.. _fig_iq_structure:

.. figure:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image96.png
   :alt: IQ Table 結構示意圖
   :align: center

   IQ Table 結構示意圖

.. _burnin-complete-iq:

2.3.1 燒錄完整的參數檔案（Complete IQ）
"""""""""""""""""""""""""""""""""""""""""

將 Complete IQ 的 ``.bin`` 參數檔燒錄至樣機，步驟如下：

(1) 使用 **RealCam Pro** 連線樣機後，點選 \ **Vendor\\ISP Tuning Pro**\ 即可開啟圖像調試視窗；於視窗的功能表列點選 **FW\FW Manager**\ 即可開啟 **FW Manager** 視窗。

< Amebapro 2 的 IQ Table 導入步驟 - 1 >

|image88|

|image89|

(2) 在 **FW Manager** 視窗中：

    - (2.1) 設定更新對象是 **SoC**
    - (2.2) Operation 選擇 **Upload**\ （上傳至樣機）
    - (2.3) 指定電腦上存放的 Complete IQ ``.bin`` 參數檔路徑
    - (2.4) Content 標示為 **Complete IQ Table**
(3) 點選【Run】即可開始燒錄。

< Amebapro 2 的 IQ Table 上傳步驟 - 2 >

|image90|

最後點選【OK】即可完成燒錄，重開機後參數將立即於樣機生效。

|image91|

.. _burnin-partial-iq:

2.3.2 燒錄單一模式的參數檔案（Partial IQ）
""""""""""""""""""""""""""""""""""""""""""""

燒錄 Partial IQ 的操作方式與 :ref:`2.3.1 燒錄完整的參數檔案 <burnin-complete-iq>` 相同，差別在於 **FW Manager** 中的以下選項：

(A) 指定欲燒錄的 Partial IQ ``.bin`` 檔案路徑
(B) Content 標示為 **Partial IQ Table**
(C) Group：根據曝光模式指定目標 **Group**
(D) Mode：指定目標 **Mode**
(E) IQ Version：填入版本號以利後續管控
(F) 點選時鐘按鈕可更新版本時間，時間戳將同步記錄於參數檔

< Amebapro 2 的 IQ Table 上傳步驟 - Partial IQ >

|image92|

最後點選【OK】即可完成上傳，參數將立即於樣機生效。

|image91|

重新開啟 **ISP Tuning Pro** 後，剛才設定的 IQ Version 與 Time Stamp 將顯示在標題列上以供識別，如下圖所示：

|image93|

.. _download-iq:

2.3.3 從樣機讀回備份 IQ 參數（Download）
""""""""""""""""""""""""""""""""""""""""""

當需要備份樣機上目前生效的 IQ 參數，或比對韌體中的參數版本時，可使用 FW Manager 的 **Download** 操作，將樣機內的 ``iq.bin`` 讀回電腦。

操作方式與 :ref:`2.3.1 <burnin-complete-iq>` 相同，差別在於：

- Operation 選擇 **Download**\ ，系統將自樣機韌體中\ **讀回（Dump）**\ 完整的 IQ 參數資料。
- File Path 指定欲儲存至電腦的路徑與檔名

點選【Run】後，樣機內的 IQ 參數將以 ``iq.bin`` 格式儲存至指定路徑。

.. _save-iq:

2.4 儲存 IQ 參數（Save IQ Table）
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

調試完成後，需透過 **ISP Tuning Pro** 的 **File \\ Save IQ Table** 功能，將目前的調試結果儲存為 ``.bin`` 檔案，供版本管控與後續燒錄至樣機使用。

- ``.bin``：可透過 :ref:`2.3 <upload-download>` 操作燒錄至樣機生效。

介面操作均使用 **ISP Tuning Pro** 圖像調試視窗，請在 **RealCam Pro** 主畫面上點選 \ **Vendor\\ISP Tuning Pro**\ 即可開啟。

|image94|

.. _save-partial-iq:

2.4.1 儲存 Partial IQ 參數（Save IQ Table by Mode）
"""""""""""""""""""""""""""""""""""""""""""""""""""""

將 RealCam Pro 中目前的調試結果儲存為 Partial IQ ``IQTable.bin`` 檔案，供後續燒錄至樣機使用。

< 圖像調試視窗：儲存 IQ Table 的操作 >


|image95|

指定好存檔路徑與檔案名稱即可。

.. _iqpacktool-merge:

2.4.2 使用 IQPackTool 合併 Complete IQ
"""""""""""""""""""""""""""""""""""""""

調試完成後，可使用離線工具 **IQPackTool** 六組 Partial IQ（``partialIQ_0`` ～ ``partialIQ_5``）更新並合併為一份 Complete IQ，再燒錄至板端。

< IQPackTool 主介面 >

|image378|

**合併步驟：**

(1) 開啟 IQPackTool 執行檔，點選【Open Full IQ】，選取現有 Complete IQ 檔案以載入其檔頭資訊。

(2) 工具會列出六個 Group / Index 欄位，依序點選各欄位旁的【...】，指定對應的調試好的 ``PartialIQ_xx.bin`` 檔案路徑。

(3) 點選【Get】更新版本時間，時間戳將同步寫入參數檔以利版本管控。

(4) 點選【Merge Full IQ】，於彈出視窗中指定輸出檔名。

< 指定輸出檔名 >

|image382|

(5) 點選【確定】，合併後的 Complete IQ 即輸出至指定路徑。

.. warning::
   合併完成的 ``IQ.bin`` 若需放入 SDK 編譯，**必須加入 header**\ （記錄檔案大小資訊）。``header`` 的加入方式請參閱 :ref:`7.1.2 <iq-bin-upload-fail>`\ 。

.. _iqpacktool-split:

2.4.3 使用 IQPackTool 拆解 Complete IQ
"""""""""""""""""""""""""""""""""""""""

當取得一份 Complete IQ（例如 ``iq_gc2053.bin``），可透過 **IQPackTool** 將其拆解為六組獨立的 ``PartialIQ_xx.bin`` ，再分別匯入 RealCam Pro 進行比對或調試。

**拆解步驟：**

< IQPackTool 主介面 >

|image383|

(1) 開啟 IQPackTool，點選【Open Full IQ】，選取 Complete IQ 所在的檔案。

< 選取 Complete IQ >

|image380|

(2) 工具下方將顯示該檔案的檔頭資訊（版本號、時間戳等）。

< 顯示檔頭資訊 >

|image381|

(3) 點選【Save Partial IQ】，於彈出視窗確認輸出路徑。

< Save Partial IQ 彈窗 >

|image379|

(4) 點選【確定】，六組 Partial IQ（``partialIQ_0`` ～ ``partialIQ_5``）將自動存放於相同目錄下。

< 拆解完成的六組 Partial IQ >

|image384|



.. _section-2-5:

2.5 校正模組
^^^^^^^^^^^^

進行校正程序時需要使用 **RealCam Pro**，請先依照 :ref:`1.1.1 RealCam Pro 軟件安裝與啟用 <realcam-install>` 完成操作後，再繼續以下流程。

.. _blc-calibration:

2.5.1 BLC 校正
"""""""""""""""

1) 遮住鏡頭使其不透光。若為 Sensor PCB 裸板，建議使用黑膠帶將背面貼緊，避免漏光。

2) 將輸出切換為 Raw Data：

點選 **Vendor / RNR Calibration** 即可將 UVC streaming 切換為 Raw streaming。

< UVC Streaming 點選 RNR Calibration 功能 >

|image99|

3) 在 **RealCam Pro** 主畫面點選 **Vendor\RNR Calibration**，開啟 RNR Calibration 校正視窗後，點選【BLC Calibration】。

< RNR Calibration 校正視窗 >

|image100|

4) 在 BLC Calibration 視窗中設定以下參數，完成後按下【Execute】即可開始執行 BLC 校正：

   |image101|

   **(1) BLC Region**

   |image102|

   - **Full Image**：將整張影像納入統計，適用於均勻光源環境。
   - **Central Region**：僅統計畫面中心區域，可避免邊緣暗角或雜訊干擾。以 Central Region 100 為例，表示取樣畫面中心 200x200 像素的範圍進行統計。

   **(2) BLC Source**

   可選取 **Live Streaming** 即時串流，或選取預先拍攝好的 Raw Data Cap 檔案進行校正：

   - **Live Streaming（即時校正）**：可選擇以單張畫面（Single Image）或多張畫面平均（Multi Image）來作校正。若選 Multi Image，可在 Image Count 設定平均張數。建議採用多張平均（例如 50 張）可獲得更穩定的結果。
   - **Raw Data Cap（離線校正）**：將 BLC Source 切換為 **Raw** 後，視窗右上方會顯示 Raw Data 相關的設定欄位，包含 Raw 檔案資料夾路徑、Cap 是否有 Header（With Header / Without Header），以及影像解析度（Image Width / Image Height）。

   **(3) Gain 與 Shutter 設定**

   在視窗下方的表格中，依序在各 Index 內設定以下參數：

   - **gainValue**：填入欲校正的增益值（1 表示增益 1x）
   - **Shutter**：填入曝光時間（1 表示曝光 1 us）

   並勾選要校正的 Index。為配合後續 Noise 特性校正，請對應校正 1x, 2x, 4x, 6x, 8x, 10x, 12x, 14x, 16x 共九組增益值下的 BLC。

   **(4) 執行校正**

   按下【Execute】即可開始執行 BLC 校正。

5) 點選【Save Text】，即可將 BLC 校正結果存檔。

6) 存檔後，各增益值對應的 R、Gr、Gb、B Mean 數值即為後續手動填入 BLC 調適模組的參考值。

7) 參考 :ref:`3.1 BLC <blc-module>`\ ，將 R、Gr、Gb、B Mean 分別填入對應的 Offset 欄位使其生效。接著依據 :ref:`3. 圖像調試步驟 <image-tuning-steps>`\ ，分別在不同增益值環境下執行更新（Update）。

8) 參考 :ref:`2.4.1 <save-partial-iq>`\ ，將調整完成的 BLC 設定**儲存**（Save IQ Table）；再透過 :ref:`2.3.2 上傳流程 <burnin-partial-iq>`\ ，即可將設定寫入開機 IQ 設定中。

.. _rnr-calibration:

2.5.2 RNR & 2DNR 校正
""""""""""""""""""""""

1) 在燈箱中放置 Colorchecker，使用 D65 環境光源
   （色溫不是重點，但燈箱光源盡可能維持 300~400 Lux 左右）。

2) 將 **RealCam Pro** 輸出設置為 Raw Data 輸出。
   點選 **Vendor / RNR Calibration** 即可將 UVC streaming 切換為 Raw streaming。

   < UVC Streaming 點選 RNR Calibration 功能 >

   |image99|

3) 於 **General** 區塊配置 RNR 校正相關的設定：

   - **On/Offline 設定**：選擇 **Online** 可執行即時校正程序（含 raw capture 及 Noise 校正）；**Offline** 則直接以現有的 Raw data 執行校正。
   - **Calibration Mode Select**：唯讀。根據目前感測器曝光模式（Linear mode 或 HDR mode）自動顯示。
   - **Calibration Type**：選擇 Noise 校正流程為 **RNR calibration** 或 **2DNR calibration**。Linear mode 下僅可選擇 RNR calibration。
   - **2DNR Source**：當 Calibration Type 為 2DNR 時，可選擇 Raw data 來源為 **short exposure** 或 **long exposure**。理想情況使用 short exposure，但其曝光上限較小；若環境照度不足、無法取得完整直方圖資訊，可改用 long exposure。

   |image104|

4) RNR Calibration 視窗中有 **Online** 與 **Offline** 兩個功能區塊，會根據 General 區塊的設定執行不同校正模式：

   - **Online**：直接使用目前連線的機台進行拍攝與校正。請執行步驟 5) ~ 12)。
   - **Offline**：針對已拍攝好的 Raw Data 進行校正。請直接執行步驟 13)。

   < RNR Calibration 視窗 >

   |image105|

5) 若尚未校正過 BLC，請先參考 :ref:`2.5.1 BLC 校正 <blc-calibration>` 完成校正。
   若已校正過 BLC，則可直接進行 RNR Calibration 的 ROI 標選作業。

   - **標定 ROI**：點選【Select ROI】開啟 ROI 標定視窗。

     |image106|

     先移動 Color Checker，使其中心點與綠色十字線重合
     （目前僅能重複執行 Select ROI 來檢視更新畫面）。

     |image107|

     可使用以下方式標定 ROI：

     - **ColorChecker 方式**：使用滑鼠框選 Color Checker 的 24 色塊標定區。可重複拉大拉小，或於視窗右下方色塊清單勾選是否納入該色塊。
     - **Single ROI 方式**：逐一標定單一色塊。先點選右下方色塊資料區選定色塊，再逐一拖拉或重新框選。

     無論使用何種方式，ROI 只框選平坦區，避免將邊界、紋理納入 ROI 範圍。

   - **儲存 ROI**：點選【Save ROI】即可儲存 ROI 標定資訊供後續處理參考，關閉 ROI Select 視窗。

     |image108|

6) 點選【Set Parameter】即可設定欲拍攝的 gain、shutter 數值。

   |image109|

7) 設定要拍攝的 Gain 與 Shutter 組數：

   - **Gain 組數**：拍攝 1x, 2x, 4x, 6x, 8x, 10x, 12x, 14x, 16x 共 9 組
   - **Shutter 組數**：每組 gain 至少拍攝 2 個 shutter Count，以取得 Raw Data 整個 dynamic range 上的統計資料。若 2 組 shutter 不足以涵蓋整個 dynamic range，請增加組數
   - **拍攝張數**：每個 shutter 建議至少拍攝 30 張以上

   |image110|

   設定完成後按下【Set】即可（以 9 組 gain、4 個 shutter 為例）。

8) 根據 :ref:`2.5.1 <blc-calibration>` 校正出來的 BLC，填入相對應 gain 的 BLC 數值
   （若 BLC 四個 channel（R、Gr、Gb、B）數值不一致，請選用其中最大值）。

   |image111|

9) 點選第一組 #1，填寫 shutter Val (us) 數值。
   按下【Simulate】後，Tool 會自動計算在 ROI 中、有勾選使用 Shutter 的情況下，Raw Data 的亮度數值分布圖。

   |image112|

   重複模擬，調整出合適的 shutter 值，使 Brightness Histogram 的每個 bin 都有足夠多的 Data（超過紅線）。

   |image113|

10) 第一組 1x 完成後，按下【Init shutters by 1x Gain】，
    讓後面八組 Gain 值的 shutter 經計算產出初始值。
    使用者可基於這些初始值，依序完成 2x ~ 16x 共九組 gain 的 shutter 設定。

    |image114|

11) 填完後按下【Save Text】，關閉 Parameter 視窗。

    |image115|

12) 設定存檔路徑與拍攝參數：

    - **存檔路徑**：點選欲儲存的資料夾位置
    - **拍攝張數**：設定每組 shutter 欲拍攝的張數（以 200 張 10 bits Sensor 為例）
    - **Save Raw Files**：勾選表示將校正過程中的 Raw 資料一併存檔

    按下【Run】，點選步驟 11) 所儲存的 ``RNR_Parameter.txt``\ ，即可開始自動校正。
    校正完成後，參數會儲存在 Raw Data Folder
    （目標資料夾名稱不能含有中文字元）。

    < Offline 校正設定 >

    |image116|

13) 若已事先拍攝好 Raw Data，可使用 **Offline** 區塊進行校正。
    設定好以下參數後按下【Run】即可開始自動校正：

    - **Raw Data Folder**：Raw Data 的存放路徑
    - **With Header / Without Header**：選擇有 128 byte header 的 cap 檔，或無 header 的 raw 檔
    - **Image Size**：Raw Data 影像解析度
    - **Sensor Bit**：Sensor 設定的 bit 數
    - **Gain Num**：Gain 的總組數
    - **shutterPerGain**：每組 Gain 的 shutter 組數
    - **PicNumInFile**：每組 shutter 的張數
    - **ROI Determine**：對 Raw 手動框選 ROI；若先前已儲存 ROI 參數，可直接使用 ROI Text 讀取
    - **ROI Text**：選擇事先決定好的 ROI 標定位置資訊 txt 檔
    - **Set Table**：點開後填選對應的 BLC 數值。``Lambda`` 可先使用預設值（後續可依據校正結果再調整），設置完點選【Save】後關閉

    |image117|

    按下【Run】即可開始自動校正，校正完成後會出現以下視窗，
    參數檔 ``RNR_Results.txt`` 將儲存在 **Raw Data Folder** 指定的路徑中。

    |image118|

14) RNR 校正完畢後，**RealCam Pro** 會產出 noise characteristic curves 圖檔。
    若操作正確，其圖形應為平滑連續的曲線，且標準差應隨著 Gain 值上升而變大。
    若否，則需檢查步驟 8) 的設定值或 Raw 檔的資料是否有錯誤。

    |image119|

15) 將結果更新至 FW 中。先開啟 **Vendor** 中的 **ISP Tuning Pro**，將動態功能關閉。

    |image120|

16) 將參數更新至 FW 中。切換至 **Noise Reduction**，點選【Load Noise Curve Txt】。

    |image121|

    選擇 ``RNR_Result.txt``\ 。

    |image122|

    |image123|

    按下【Write】，再按下【Update】，即可將 Noise Model 更新至 FW 中。

    |image124|

**2DNR 校正**

2DNR 校正參數的儲存與上傳方式與 RNR 基本相同。

17) 2DNR 校正完畢後，**RealCam Pro** 會在 Raw data 路徑下產生 ``2DNR_Result.txt``\ 。

18) 將 2DNR 結果更新至 FW 中。先開啟 **Vendor** 中的 **ISP Tuning Pro**，將動態功能關閉。

    |image125|

19) 將參數更新至 FW 中。切換至 **2DNR**，點選【Load Noise Curve Txt】。

    |image126|

    選擇 ``2DNR_Result.txt``\ 。

    |image127|

    |image128|

    按下【Write】，再按下【Update】，即可將 Noise Model 更新至 FW 中。

    |image129|

20) 參考 :ref:`2.4.1 <save-partial-iq>`\ ，將調整完成的 Noise Model 設定**儲存**（Save IQ Table）；再透過 :ref:`2.3.2 上傳流程 <burnin-partial-iq>`\ ，即可將設定寫入開機 IQ 設定中。

.. _lsc-calibration:

2.5.3 LSC 校正
"""""""""""""""

1) LSC 校正需在均勻光源的環境下進行：

   - **光源均勻方式**：可使用 DNP 光源燈箱、Optical Diffuser、或乳白色壓克力板作為 Diffuser 覆蓋鏡頭，並對準光源以確保均勻度
   - **光源色溫**：建議使用 D65 或 D50

2) 使用 **RealCam Pro** 連線樣機出圖後，點選 **Vendor** 中的 **ISP Tuning Pro** 開啟圖像調試視窗。

   |image130|

3) 在 **Dynamic Control** 頁面，將動態功能關閉，防止其影響 LSC 校正效果。

   |image131|

4) 切換到 **LSC** 頁面，依以下步驟進行校正。

   LSC 頁面上半部為參數設定區（Data Source、Center mode、Attenuation 等），下半部為 NLSC / MLSC 的校正與存檔操作區（Get Curve、Write、Save）。

   |image132|

   |image133|

   - **(1) Reset — 初始化參數**

     點選【Reset】，將 NLSC / MLSC 參數恢復預設值，避免受舊有 IQ 設定干擾。

   - **(2) LSC Data Source — 選擇校正資料來源**

     - **Video**：使用目前連線的 Streaming 即時圖像進行校正（一般使用此方式）
     - **Raw File**：使用預先拍攝好的 Raw Data 檔案進行校正

   - **(3) LSC Center mode — 中心點計算模式**

     - **以亮度資訊找出一個中心點**：全畫面共用單一中心點（一般使用此方式）
     - **RGB 各 channel 各有中心**：紅、綠、藍三通道分別計算獨立中心點

   - **(4) Find Lens Shading Center Method — 中心點搜尋方法**

     - **最小的對稱性差異為中心點**：以畫面對稱性最佳的位置作為中心（一般使用此方式）
     - **亮度最大值為中心點**：以畫面最亮的位置作為中心

   - **(5) Attenuation — 校正幅度設定**

     設定 Lens Shading 的校正強度：

     - 100% 表示圖像四周亮度經校正後與中心完全一致
     - 建議設定為 **70% ~ 90%**，可在改善暗角的同時抑制邊角雜訊
     - 此比例為完整補償曲線的相對比例，非最終中心與邊緣的實際亮度比例

   - **(6) 關閉 LSC — 確認未補償狀態**

     取消勾選 **NLSC Enable** 與 **MLSC Enable**，確認 LSC 處於未補償狀態後再開始校正。

   - **(7) NLSC 校正 — 同心圓校正**

     勾選 **NLSC Enable** 後，點選 NLSC 的【Get Curve】開始校正。
     NLSC 以中心點為基準，按同心圓方式逐圈校正 Lens Shading。
     若鏡頭品質較好，通常僅做 NLSC 校正即可達到理想效果。

   - **(8) MLSC 校正 — 網格校正**

     若鏡頭透光性不均勻（例如左邊較右邊亮），僅做 NLSC 效果不理想時，可再加上 MLSC 校正。
     MLSC 以網格方式校正 Lens Shading，可處理非對稱性的透光不均現象。

     勾選 **MLSC Enable** 後，點選 MLSC 的【Get Curve】開始校正，
     系統會彈出視窗供選擇 block 大小：

     - block 越小，補償越細緻，一般建議選擇最小值
     - 無法勾選的 size 表示目前 sensor 解析度不支援該尺寸

     |image134|

   - **(9) Write — 寫入樣機**

     點選【Write】，使校正參數立即生效於當前連線的樣機，可即時觀察校正後的畫面效果。

   - **(10) Save — 儲存參數**

     點選【Save】，將校正完成的參數匯出並儲存至 **RealCam Pro** 的資料夾中。

5) 點選頁面最上方的【Update】，將參數更新至 FW 中。

   |image135|

6) 參考 :ref:`2.4.1 <save-partial-iq>`\ ，將調整完成的 LSC 設定**儲存**（Save IQ Table）；再透過 :ref:`2.3.2 上傳流程 <burnin-partial-iq>`\ ，即可將設定寫入開機 IQ 設定中。

.. _awb-calibration:

2.5.4 AWB 校正
"""""""""""""""

1) 準備多色燈箱與灰卡，並使用 **RealCam Pro** 連線樣機，對著灰卡出圖後，點選【Vendor】中的【ISP Tuning Pro】開啟圖像調試視窗。

   |image136|

2) 在圖像調試視窗中切換至【AWB】頁面，分別點開 AWB Attribute 下的【Detail Adjustment】與 ``ct_setting`` 的【Detail Adjust】，此時將開啟兩個子視窗，供後續操作使用。

   |image137|

3) 校正各色溫定位點的操作步驟如下：

   - **(1) 設定色溫點數量**

     依據需要校正的燈源組數，修改 ``ct_setting`` 視窗中的 **Count of CT Points** 欄位。

   |image138|

   - **(2) 切換燈源 — 設定色溫值**

     切換燈箱至目標燈源，並修改 ``ct_setting`` 視窗中的 **CT (K)** 欄位，使色溫值與燈源匹配。
   
   - **(3) 填入色溫座標 X / Y**

     修改 ``ct_setting`` 視窗中的 **X / Y** 欄位座標值。
     可利用 AWB Analyze 視窗左下角的即時游標座標顯示功能，取得當前紅點的座標，作為 X / Y 欄位的填入參考值。

   |image140|

   |image141|

     .. note::
       若統計灰點未落在統計範圍內，可點擊 AWB Analyze 視窗中的【Para】，透過拖曳方式將草綠色方塊移動至統計灰點群附近，寫入後再確認紅點位置是否符合預期。

   - **(4) 寫入色溫座標**

     完成定位點色溫座標的填入後，分別按下 ``ct_setting`` 視窗中的【Write】及【AWB】頁面中的【Write】，使色溫點座標生效。

   |image139|

   |image142|
      
   - **(5) 更新 AWB Analyze 視窗**

     在 AWB Analyze 視窗中按下【Read】，更新 AWB 數值分析圖。

   - **(6) 確認定位點位置**

     確認對應色溫的草綠色方塊已移動至當前紅點附近。

   - **(7) 寫入設定**

     分別按下 AWB Analyze 視窗中的【Write】及【AWB】頁面中的【Write】，使設定生效。

   重複步驟 **(2) ～ (7)**，依序完成所有色溫點的校正。

   以燈箱光源為 **D50（5000K）** 為例：將 ``illum_ct`` 的 index 4 修改為 5000K，再修改 ct Detail Adjust 中 index 3 的 x、y 值，使 AWB Analyze 視窗中對應 5000K 的草綠色方塊移動至紅點附近。

   |image143|

   |image144|

4) 完成所有色溫點的校正後，確認各色溫點的分布位置符合預期。

5) 參考 :ref:`2.4.1 <save-partial-iq>`\ ，將調整完成的 AWB 設定**儲存**（Save IQ Table）；再透過 :ref:`2.3.2 上傳流程 <burnin-partial-iq>`\ ，即可將設定寫入開機 IQ 設定中。

.. _ccm-calibration:

2.5.5 CCM 校正
"""""""""""""""

1) 開始校正 CCM 前，先點選【Vendor】內的【ISP Tuning Pro】進入圖像調試視窗，確認【UV Color Tune】內各色溫、Gain 值下的校色參數是否已回到原點；若否，則點選【Reset】，再依序按下【Write】及【Update】，將參數還原至初始狀態。

   |image145|

2) 參考 :ref:`2.4.1 <save-partial-iq>`\ ，將初始化的 UV Color Tune 設定**儲存**（Save IQ Table）；再透過 :ref:`2.3.2 上傳流程 <burnin-partial-iq>` 將設定寫入開機 IQ 設定中，以避免在後續 CCM 校正流程中被還原為非初始的參數。

3) 在燈箱內放置 Color Checker，選擇欲校正的色溫燈源。

4) 使用 **RealCam Pro** 連線樣機，對著 24 色卡出圖後，點選【Vendor】中的【UVC AIQ】開啟 UvcAIQ 視窗。點選視窗中的【UvcAIQ_Init】，確認提示狀態後點選【確定】，進入 UvcAIQ 操作介面。

   |image146|

   |image147|

   |image148|

5) 在 UvcAIQ 視窗中點擊【Set Patch Position】，開啟 Set Patch Position 視窗。在圖像區拖曳左上角與右下角的紅色定位點，使各色塊的取樣點確實落在 24 色塊範圍內，完成後點擊【Set Position】並關閉視窗。

   < UvcAIQ 視窗 >

   |image149|

   < Set Patch Position 視窗 >

   |image150|

6) 在 UvcAIQ 視窗中點擊【Gamma Read】，載入連線樣機當前的 Gamma 參數。

   < UvcAIQ 視窗 >

   |image151|

7) 在 UvcAIQ 視窗中設定 CCM 搜索條件。

   < UvcAIQ 視窗 >

   |image152|

   (1) **Mode**：選擇顏色差異的計算標準。若選擇 ΔC and ΔE 系列，則需進一步設定所需的 Max、Mean 條件。

   (2) **Step**：CCM 搜索的步長。步長越小，搜索精度越高，但所需時間也會隨之增加。

   (3) **ΔC Max**：24 個色塊中 ΔC 的最大允許值。

   (4) **ΔC Mean**：24 個色塊 ΔC 的平均允許值。

   (5) **ΔE Max**：24 個色塊中 ΔE 的最大允許值。

   (6) **ΔE Mean**：24 個色塊 ΔE 的平均允許值。

   (7) 色框內的參數用於限定 CCM 搜索範圍。例如，CCM2\\8 Min 與 Max 代表 CCM 矩陣中第 2 個與第 8 個參數的搜索下限與上限。

8) 在 UvcAIQ 視窗中點擊【CCM Search】，在彈出的 CCM Search 視窗中設定色卡的目標值：

   - 點擊【Default】可載入 x-rite 官方標準色彩目標值
   - 點擊【Update Target】可載入自訂的 bmp 圖檔，或手動調整目標值
   - 視窗左下方的 24 個 Weights 分別對應 24 色卡各色塊的計算權重；提高特定色塊的權重可改善該色塊的顏色準確性，但可能影響整體 24 色塊的平均準確性

   < UvcAIQ 視窗 >

   |image153|

   < CCM Search 視窗 >

   |image154|

9) 確認上述設定完成後，在 CCM Search 視窗中點擊【Get CCM】，開始搜索 CCM 參數。

   - **Progress**：顯示搜索進度，達到 100% 表示搜索完成。
   - **Total Group**：搜索過程中滿足條件（Delta Max、Delta Mean）的 CCM 參數組數。

   若 Total Group 為 0，表示未找到符合條件的 CCM 參數，可適當放寬條件或擴大搜索範圍後重試；若 Total Group 不為 0，工具將自動選出 ΔC Mean 最佳的一組參數。

   關閉 CCM Search 視窗後，所選的 CCM 參數將顯示在 UvcAIQ 視窗中。確認無誤後，點擊【CCM Write】將參數寫入樣機。

   |image155|

10) 在 **RealCam Pro** 主畫面上點選【Vendor】中的【ISP Tuning Pro】進入圖像調試視窗，切換至【CCM】頁面，按下【Read】讀取剛才寫入的 CCM 校正數值，再按下【Update】更新至 IQ Table。

    |image156|

    |image157|

11) 切換至下一個色溫燈源，重複步驟 1) ～ 9)，依序完成各色溫的 CCM 校正。

12) 參考 :ref:`2.4.1 <save-partial-iq>`\ ，將調整完成的 CCM 設定**儲存**（Save IQ Table）；再透過 :ref:`2.3.2 上傳流程 <burnin-partial-iq>`\ ，即可將設定寫入開機 IQ 設定中。

13) 上述校正流程所產出的 CCM 適用於正常亮度環境。在不同增益條件下，可基於校正結果進一步調整飽和度與色相：

    (1) 點選【Vendor】內的【ISP Tuning Pro】進入圖像調試視窗，點選【Dynamic Control】，關閉【Global Dynamic Control】選項。

    (2) 切換至【CCM】頁面，取消勾選 CCM 旁的【Base/Tuned】，此時【CCM】矩陣的數值將與【Final Result】一致。

    (3) 調整下方的【Hue】與【Saturation】，矩陣數值將隨之連動更新。

    (4) 可勾選【Sync】使 U、V channel 同步調整，或取消勾選後分別對 U、V channel 依需求個別調整。

    |image158|

.. _image-tuning-steps:

3. 圖像調試步驟
---------------

點選 **RealCam Pro** 工具列上的 **Vendor** \\ **ISP Tuning Pro** 可進入圖像調試視窗。主界面如下，可區分為以下幾個區域：

< 圖像調試視窗的主界面 >

|image159|

1) **調試目錄區**

   以樹狀結構列出各主要調試項目，每個項目大致對應一個 ISP 運作模組。

2) **調試功能區**

   提供參數讀出、寫入連線樣機、更新與提取暫存區 IQ Table 等功能。當前色溫與亮度的參數調試完成後，需更新至 IQ Table 對應的色溫、增益區間，以及 vHDR 模式下的曝光比區間，以供最後的參數導出（.bin 檔案）。

   **Color Temperature（色溫）**

   調試功能區左半部的色溫相關功能說明如下：

   - **Current**：顯示 AWB 的即時色溫估算值；當 AWB 設為手動模式時不更新。
   - **Index**：色溫區間。顯示當前色溫位於哪一組色溫區間，使參數調試結果能更新至對應的區間。
   - **IN**：進入該色溫區間的閥值。
   - **OUT**：離開該色溫區間的閥值。

   .. note:: 色溫 Index 對應的 IN / OUT 設定可透過 **Index Manager** 調整。

   |image160|

   **Gain（增益）**

   調試功能區左半部的增益相關功能說明如下：

   - **TH Based on**：增益判斷基準，可選擇 **Gain** 或 **ETGain** 其中之一。相較於 Gain，ETGain 額外納入了曝光時間（Exposure Time）的考量。Gain 的單位為一倍增益（1x），ETGain 的單位為 0.1 ms × 1x。
   - **Current**：顯示 AE 的即時增益數值（Gain 或 ETGain）；當 AE 設為手動模式時不更新。
   - **Index**：增益區間。顯示當前增益位於哪一組增益區間，使參數調試結果能更新至對應的區間。
   - **OUT**：增益區間對應的分界點。

   .. note:: 增益的判斷基準及 Index 對應的 OUT 設定可透過 **Index Manager** 調整。

   |image161|

   **Exposure Ratio（曝光比）**

   調試功能區左半部的曝光比相關功能僅在 vHDR 模式下可供使用，說明如下：

   - **Current**：顯示 AE 即時計算的曝光比；當曝光比手動設為固定值時不更新。Amebapro 2 的曝光比為 2 的冪次方：2、4、8、16、32、64。
   - **Index**：曝光比區間。顯示當前曝光比位於哪一組曝光比區間，使參數調試結果能更新至對應的區間。
   - **OUT**：曝光比區間對應的分界點。

   |image162|

   .. note::
     與增益區間不同，若曝光比落在兩組 Index 之間，其參數不會進行內插，而是沿用前一組 Index 的參數；待曝光比超過下一組 Index 的分界點時，參數才進行離散切換。

   **Index Manager**

   調試功能區左半部的 **Index Manager** 可調整所有動態模組的區間設定，詳細說明請參閱「4.3 新增 IQ 參數區間」章節。

   **Load / Update**

   調試功能區左半部的 **Load** 與 **Update** 依據色溫與增益的 Index，對暫存區 IQ Table 進行提取或更新：

   - **Load**：根據色溫與增益的 Index，將暫存區 IQ Table 的參數提取至調試參數區。
   - **Update**：將調試參數區的參數更新至暫存區 IQ Table 對應的色溫與增益區間，以供最後的參數導出。

   **Update All / Extrapolate**

   調試功能區中間部分提供 Texture 相關頁面的更新選項，作用於 DRC&DRE、WDR&Dehaze、GbGr、CAC、FCR&MCR&UVS、DPC、INTP、Noise Reduction、Edge Enhance、Video Property、LDC 等頁面：

   - **Update All**：勾選後，在上述任一 Texture 頁面按下【Update】時，將同步更新所有 Texture 相關頁面的參數至暫存區 IQ Table；未勾選時，僅更新當前頁面的參數。
   - **Extrapolate**：勾選後，在上述 Texture 頁面按下【Update】時，支援動態插值的參數將以當前數值進行插值計算後更新至暫存區 IQ Table；未勾選時，直接以當前數值更新。

   **Write / Read / History**

   調試功能區右半部提供連線樣機的參數讀寫功能：

   - **Write**：將調試參數區的參數寫入連線樣機並立即生效。請注意，此操作不會同時更新暫存區 IQ Table（AE、AWB、WDR、DayNight 除外）。確認調試結果後，仍需點擊【Update】才會將參數更新至暫存區 IQ Table，以供最後的參數導出。
   - **Read**：將連線樣機的當前參數讀出至調試參數區。請注意，此操作讀取的是樣機當前執行的參數，而非暫存區 IQ Table 的內容。
   - **History**：參數群組記憶功能，可儲存並切換不同的參數狀態。

3) **調試參數區**

   依主要調試項目分類顯示可調試的參數。

   .. warning::
     調試參數前，請務必先關閉動態更新功能，以避免 PC Host（RealCam）與樣機端的 Embedded Host 之間互相覆寫參數。

   關閉動態功能的步驟如下：

   |image163|

   (1) 點選調試目錄區的 **Dynamic Control** 選項。
   (2) 點選 **Disable**。
   (3) 點選【Write】，使設定生效。

4) **vHDR 模式下的調試參數區**

   目前各模組中僅有部分參數支援依曝光比進行獨立配置。在 vHDR 模式下，調試功能區的 **Exposure Ratio** 欄位將以藍色字體標示；調試參數區中同樣以藍色字體顯示的參數，即代表該參數可依不同曝光比分別配置。

   |image164|

   (1) vHDR 模式下，調試功能區的 **Exposure Ratio** 以藍色字體顯示。
   (2) 以 WDR 頁面的 **WDR Level** 為例，由於此參數支援依曝光比配置，因此也以藍色字體標示。

   目前開放可依曝光比切換的 ISP 參數如下表所示：

   .. list-table::
      :header-rows: 1
      :widths: 30 70

      * - 模組
        - 參數
      * - HDR Fusion
        - | Fusion Rate Max
          | Fusion Rate Min
      * - DRC
        - Blend Ori Level
      * - WDR
        - WDR Level
      * - INTP
        - | Flat Prior to Texture - TH
          | Blend LPF Condition - thd0 / thd1
      * - Noise Reduction
        - Static Scene TNR Strength
      * - 2DNR
        - | Noise Reduction - thd0 / thd1 / nr rate / G Delta Clip
          | Sharpness - thd0 / thd1 / sharpness Rate
      * - EEH
        - | Y-Sharp - rate0 / rate1
          | Suppress Strength
      * - Video Property
        - | Brightness
          | Contrast Level / Mean
          | Saturation
 
.. _blc-module:

3.1 BLC
-------

BLC（Black Level Correction；暗電流校正）是 ISP 影像處理流程中的基礎校正模組，負責消除感測器固有的暗電流偏置，確保後續影像處理的色彩與對比度正確性。

**功能定義**

影像感測器（Sensor）在完全無光的環境下，受暗電流（Dark Current）與電路雜訊影響，輸出的原始像素值（Raw Data）通常不為零。BLC 模組精確扣除這些硬體固有的基礎偏置值（Black Offset），將全黑環境下的亮度定義回「0」，確保影像在後續處理（如白平衡、色彩校正、Gamma 曲線）時色彩與對比度不失真，避免畫面整體發灰或暗部泛紫/泛綠。

**調校說明**

本模組的相關參數可依據不同的色溫與增益條件（Gain）進行獨立設定。

3.1.1 BLC\\General 參數說明
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

< 圖像調試視窗的 BLC \\ General 頁面 >

|image165|

(1) **Initialize BLC Setting**：初始化 BLC 設定。載入 :ref:`2.5.1 BLC 校正 <blc-calibration>` 中儲存的校正檔案，並將相關參數寫入 IQ 設定。

   .. note::
     - 初始化操作將清除原有的 BLC IQ 設定。
     - 載入後的動態設定僅依照 2.5.1 BLC 校正所使用的增益條件進行配置，不包含依色溫的動態設定。
     - 若需加入依色溫的動態設定，請參閱「4.3.1 新增色溫區間」章節。

(2) **BLC Enable Long Exposure Path**：Linear 模式或 HDR 模式長曝幀的暗電流校正開關。

   - **Enable**：開啟暗電流校正。
   - **Disable**：關閉暗電流校正。

(3) **BLC Enable Short Exposure Path**：HDR 模式短曝幀的暗電流校正開關，僅在 HDR 模式下有效。

   - **Enable**：開啟暗電流校正。
   - **Disable**：關閉暗電流校正。

下圖以實拍場景對比說明 BLC 的校正效果：右圖（校正前）因暗電流偏置的影響，影像整體出現色偏；左圖（BLC 啟用後）偏置消除，影像色彩得以正確還原。

< BLC 校正效果對比（右：校正前；左：校正後） >

|image168|

3.1.2 BLC\\Long Exposure Path (Main Path) 參數說明
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

< 圖像調試視窗的 BLC \\ Long Exposure Path (Main Path) 頁面 >

|image166|

本頁面適用於 **HDR 模式長曝幀**\ 及 **Linear 模式**\ 。

(1) ～ (4) **BLC Offset R、Gr、Gb、B**：Bayer Domain 中 R、Gr、Gb、B 各通道的 Black Level 校正數值。

   填入數值的換算方式依感測器輸出位元深度而異：

   - **RAW 10-bit**：校正數值須乘以 4 後填入。例如：欲扣除 16 LSB，填入 64（16 × 4）。
   - **RAW 12-bit**：校正數值直接填入，無需換算。例如：欲扣除 16 LSB，填入 16。

   .. note:: :ref:`2.5.1 BLC 校正 <blc-calibration>` 所產出的校正數值已換算為可直接填入的格式。

(5) ～ (8) **BLC Gain R、Gr、Gb、B**：Bayer Domain 中 R、Gr、Gb、B 各通道的 Re-Scale 增益，由 RealCam 依據 BLC Offset 設定自動計算，用於補償 BLC Offset 所造成的動態範圍損失。

3.1.3 BLC\\Short Exposure Path 參數說明
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

< 圖像調試視窗的 BLC \\ Short Exposure Path 頁面 >

|image167|

本頁面適用於 **HDR 模式短曝幀**\ ，僅在 HDR 模式下有效。

(1) ～ (4) **BLC Offset R、Gr、Gb、B**：Bayer Domain 中 R、Gr、Gb、B 各通道的 Black Level 校正數值。

   填入數值的換算方式依感測器輸出位元深度而異：

   - **RAW 10-bit**：校正數值須乘以 4 後填入。例如：欲扣除 16 LSB，填入 64（16 × 4）。
   - **RAW 12-bit**：校正數值直接填入，無需換算。例如：欲扣除 16 LSB，填入 16。

   .. note:: :ref:`2.5.1 BLC 校正 <blc-calibration>` 所產出的校正數值已換算為可直接填入的格式。

(5) ～ (8) **BLC Gain R、Gr、Gb、B**：Bayer Domain 中 R、Gr、Gb、B 各通道的 Re-Scale 增益，由 RealCam 依據 BLC Offset 設定自動計算，用於補償 BLC Offset 所造成的動態範圍損失。

.. _ae-module:

3.2 AE
------

AE（Auto Exposure；自動曝光）是 ISP 影像處理流程中的核心功能模組，負責根據場景環境光自動調整曝光設定，使輸出影像在各種光線條件下均能維持合適的亮度水準。

**功能定義**

感測器所接收的影像來自被攝物體的反射光。曝光量（E）定義為光線強度與曝光時間的乘積，單位為 Lux·s。AE 模組透過持續量測畫面亮度，自動計算並調整曝光時間（Exposure Time）與增益（Gain）等參數，確保畫面以目標亮度正確曝光，避免過曝或曝光不足。

下圖為三種曝光狀態的影像示意，以及對應的亮度分佈圖，可作為判斷曝光是否正確的參考依據。

< AE 曝光效果示意：過曝、正確曝光、曝光不足 >

|image19|

< 三種曝光狀態對應的亮度分佈圖 >

|image22|\ |image20|\ |image21|

< AE 工作流程示意圖 >

|image23|

**調校說明**

本模組的相關參數可依據不同的增益條件（Gain）進行獨立設定。

.. _ae-general:

3.2.1 AE\\General & ManualExposure 參數說明
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

|image1|

< 圖像調試視窗的 AE \\ General & Manual Exposure 參數頁面 >

(1) Bypass: ISP Auto Exposure 數位增益的模組控制

    - Enable 表示關閉 ISP AE 模組、不使用 ISP 增益，同時也會將 AE Auto/Manual強制設置為 Manual Mode
    - Disable 表示使用 ISP AE 模組、包含 ISP 增益，於正常自動曝光下皆使用此設置（留意 AE Auto/Manual 是否正確設置）

(2) Auto/Manual: 自動 / 手動曝光的切換功能

    - Auto 表示自動曝光，於正常自動曝光下皆使用此設置
    - Manual 表示手動曝光，選擇手動曝光時，下方的 Exposure 跟 Gain 欄位中的數值才能設置

(3) Exposure: 手動曝光時的曝光時間設定，單位為微秒（μs）

(4) Gain: 手動曝光時的增益設定，單位為倍 (1x)

.. _ae-attribute:

3.2.2 AE\\AE Attribute 參數說明
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

< 圖像調試視窗的 AE \\ AE Attribute 參數頁面 - 1/2 >

|image2|

(1) **AE Weight**：演算法內計算 ``y_mean`` 的加權表格。

  AE 將畫面切分為 16×16 共 256 個區塊，AE Weight 設定各區塊的計算權重，加權後產出亮度平均值 Y Mean。

  |image3|

  常見之 AE 測光模式如下三種：

  .. list-table::
     :widths: 33 33 33

     * - |image24|
       - |image25|
       - |image26|
     * - (a) 點測光
       - (b) 中央加權測光
       - (c) 平均測光

(2) ``y_mean_target``：Y Mean 收斂的最終目標值。

(3) ``y_mean_target_l``：Y Mean 目標區間的下限值。

(4) ``y_mean_target_h``：Y Mean 目標區間的上限值。

  Y Mean 會先收斂進入 ``y_mean_target_l`` 與 ``y_mean_target_h`` 之間，再依設定條件進一步調整。

.. note::
  AE Target 的變化關係如下圖所示

  |image30|

(5) ``total_gain_max``：AE 收斂時允許使用的最大增益，16 表示 1 倍增益。

(6) ``same_block_en``：景物相似度偵測的功能開關。啟用後，系統將畫面切分為 16×16 共 256 個區塊，持續比較各區塊的亮度變化，只有當足夠多的區塊偵測到亮度改變時，才觸發 AE 更新曝光與增益設定。

(7) ``y_mean_same_block_diff_th``：單一區塊是否發生變化的亮度差閾值。區塊亮度均值的變化量超過此閾值，才視為該區塊景物有所改變。

(8) ``same_block_num_stable_th``：AE 進入穩定狀態的靜止區塊數閾值。靜止（景物未改變）的區塊數量超過此閾值時，AE 停止搜尋並維持當前曝光設定。

(9) ``same_block_num_delay_th``：AE 準備離開穩定狀態的靜止區塊數閾值。

.. note::
  - 靜止區塊數介於此閾值與 (8) ``same_block_num_stable_th`` 之間時，系統進入緩衝期；需持續達到 (14) ``ae_stable_delay`` 所設定的時間後，AE 才離開穩定狀態重新調節亮度。
  - 靜止區塊數低於此閾值時，AE 立即離開穩定狀態並開始調節亮度。

(10) ``y_mean_same_block_th_l``：景物相似度偵測啟用的 Y Mean 下限。

(11) ``y_mean_same_block_th_h``：景物相似度偵測啟用的 Y Mean 上限。

.. note:: 景物相似度偵測只在 Y Mean 落於 ``y_mean_same_block_th_l`` 與 ``y_mean_same_block_th_h`` 之間時生效。

(12) ``y_mean_enter_high_light_th``：進入強光模式的 Y Mean 閾值。在一般模式下，當環境變亮使曝光時間縮短至 Flicker Step（50Hz：10ms；60Hz：8.3ms）且 Y Mean 高於此閾值時，AE 判定已進入強光環境，並繼續縮短曝光時間切換至強光模式。

(13) ``y_mean_exit_high_light_th``：離開強光模式的 Y Mean 閾值。在強光模式下，當環境轉暗使曝光時間延長至 Flicker Step 且 Y Mean 低於此閾值時，AE 判定環境已回到正常亮度，切換回一般模式並繼續增加曝光時間。

(14) ``ae_stable_delay``：AE 穩定保持時間，單位為毫秒。AE 收斂完成進入穩定狀態後，即便環境亮度出現變化，系統也會持續忽略調整請求；只有當亮度偏差連續超出穩定範圍的累積時間達到此設定值，才重新觸發 AE 調節。

(15) ``y_mean_delay_th_l``：``ae_stable_delay`` 機制作用的 Y Mean 下限。

(16) ``y_mean_delay_th_h``：``ae_stable_delay`` 機制作用的 Y Mean 上限。

.. note:: ``ae_stable_delay`` 保持機制只在 Y Mean 落於 ``y_mean_delay_th_l`` 與 ``y_mean_delay_th_h`` 之間時生效。

(17) ``ae_enter_stable_th``：進入穩定狀態的 AE Step 閾值。在非穩定狀態下，當 AE Step 的比值差距持續落在此閾值範圍內達到一定時間後，AE 進入穩定狀態。

(18) ``ae_exit_stable_th``：AE 重新搜尋的觸發閾值。在穩定狀態下，若當前 Y Mean 與目標值的差距超過此設定，``ae_stable_delay`` 計時開始；計時結束後仍超出時，AE 離開穩定狀態重新調節亮度。

.. note:: ``ae_enter_stable_th`` < ``ae_exit_stable_th``

< 圖像調試視窗的 AE \\ AE Attribute 參數頁面 - 2/2 >

|image4|

AE Step 定義為 AE 目標亮度與目前畫面亮度（Y Mean）的比值（AE Step = 目標值 ÷ Y Mean），反映畫面與目標之間的差距大小。AE 收斂速度會根據此比值動態切換 Slow Mode 與 Fast Mode。

畫面亮度接近目標時（AE Step 比值接近 1），AE 進入 Slow Mode，以固定的小步伐緩步調整，避免畫面亮度反覆震盪。當差距超過閾值 ``ae_enter_slow_mode_th`` 時，切換至 Fast Mode，以較大且動態的步伐快速逼近目標。

(19) ``ae_enter_slow_mode_th``：Slow Mode 與 Fast Mode 的切換閾值。

(20) ``ae_slow_mode_step_th_h``：Slow Mode 下，當畫面亮度仍偏離目標較多（比值差距 > 0.1）時使用的收斂步伐，為 Slow Mode 的較大步長。

(21) ``ae_slow_mode_step_th_l``：Slow Mode 下，當畫面亮度已相當接近目標（比值差距 ≤ 0.1）時使用的收斂步伐，為 Slow Mode 的精細步長。

.. note:: ``ae_slow_mode_step_th_h`` / ``ae_slow_mode_step_th_l`` 數值越大，Slow Mode 收斂速度越快。設定需滿足：``ae_slow_mode_step_th_h`` ≥ ``ae_slow_mode_step_th_l`` > 0，且 ``ae_slow_mode_step_th_l`` < 0.1。

(22) ``ae_fast_stepgreater_th_h``：Fast Mode 下，環境光變暗（需增加曝光）且畫面與目標亮度差距較大時的收斂步伐，為最大步長。

(23) ``ae_fast_stepgreater_th_m``：Fast Mode 下，環境光變暗且畫面與目標亮度差距適中時的收斂步伐，為中等步長。

(24) ``ae_fast_stepgreater_th_l``：Fast Mode 下，環境光變暗且畫面與目標亮度差距較小時的收斂步伐，為較小步長。

.. note:: ``ae_fast_stepgreater_th_h`` / ``ae_fast_stepgreater_th_m`` / ``ae_fast_stepgreater_th_l`` 數值越大，Fast Mode（變暗方向）收斂速度越快。設定需滿足：
          ``ae_fast_stepgreater_th_h`` > ``ae_fast_stepgreater_th_m`` > ``ae_fast_stepgreater_th_l`` > 1。

(25) ``ae_fast_stepgreater_th_pow``：Fast Mode 下，環境光變暗且畫面已非常接近目標時，改用冪次方式計算收斂步伐的冪數。

.. note:: ``ae_fast_stepgreater_th_pow`` 數值越大，收斂速度越快。設定需介於 0 到 1 之間（0 < 此值 < 1）。

(26) ``ae_fast_step_less_th_h``：Fast Mode 下，環境光變亮（需減少曝光）且畫面與目標亮度差距較大時的收斂步伐，為最大步長。

(27) ``ae_fast_step_less_th_l``：Fast Mode 下，環境光變亮且畫面與目標亮度差距較小時的收斂步伐，為較小步長。

.. note:: ``ae_fast_step_less_th_h`` / ``ae_fast_step_less_th_l`` 數值越小，Fast Mode（變亮方向）收斂速度越快。光線變亮時收斂方向與變暗相反（步伐值需小於 1 以縮減曝光），設定需滿足：
          0 < ``ae_fast_step_less_th_h`` < ``ae_fast_step_less_th_l`` < 1。

(28) ``ae_fast_step_less_th_pow``：Fast Mode 下，環境光變亮且畫面已非常接近目標時，改用冪次方式計算收斂步伐的冪數。

.. note:: ``ae_fast_step_less_th_pow`` 數值越大，收斂速度越快。

.. _ae-attribute-ext:

3.2.3 AE\\AE Attribute Extension 參數說明
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

< 圖像調試視窗的 AE \\ AE Attribute Extension 參數頁面 - 1/2 >

|image5|

(1) ``dyn_fps_min``：動態降幀的最小幀率。

(2) ``dyn_fps_setting``：動態降幀功能的對應設定。

    - TH 為 Gain 的閾值設定，僅此欄位可做調整，單位為一倍增益（1x Gain）。
    - 當增益提升到達 TH 的條件時，FPS 會由前一個 Index 對應的數值降至當前 Index 對應的數值。

.. note::
   以下圖為例，當 30FPS 時的增益上升達到 128x Gain 時，會降幀為 25FPS；此時因曝光時間增加，增益會降低而小於 128x Gain，不會再觸發繼續降幀的行為。

   |image6|

Sorting AE 主要應用於高對比場景，針對極亮極暗區域的 Y mean 計算權重進行調整。

Sorting AE 將畫面切分為多個區塊並由暗到亮排列，各區塊依位置給予不同的計算權重，再算出加權後的亮度值。

共有兩組 Sorting Table 可用：

- ``sort_win_weight_low``：亮暗區的權重差異小，各區塊的影響力趨於均等。
- ``sort_win_weight_high``：亮暗區的權重差異大，對特定亮度區域有更強的側重。

透過設定 ``sort_mode``\ ，可選擇直接使用其中一組 Table，或將兩組按比例混合後使用。

(3) ``sort_weight_ratio``：Sorting AE 在最終 Y Mean 計算中所佔的比重。

  數值越大，最終 Y Mean 越多來自 Sorting AE 的計算結果；數值越小，越多來自標準 AE Weight Table 的計算結果。

(4) ``sort_mode``：Sorting AE 的運作模式，共五種：

    - Off：關閉 Sorting AE，僅使用標準 AE Weight Table。
    - Auto：根據畫面中的過曝佔比自動決定兩組 Table 的混合比例（由 ``saturated_range_th1`` / ``saturated_range_th2`` / ``saturated_range_th3`` 控制）。
    - Manual：由 ``manual_level`` 手動指定兩組 Table 的混合比例。
    - Dark (Low Light Priority)：側重暗部亮度補償，可依不同 Gain / ETGain 動態調整混合比例。
    - Bright (High Light Priority)：側重高光過曝保護，可依不同 Gain / ETGain 動態調整混合比例。

|image7|

.. warning:: **RealCam 僅可切換模式查看效果，此項設定並不會紀錄到 IQ 設定中；實際運行時，此項設定是透過 SW 端來設置的**

(5) ``saturated_range_th1``：Auto 模式下，過曝佔比低於此閾值時，直接使用 ``sort_win_weight_low`` 作為 Sorting Table。

(6) ``saturated_range_th2``：Auto 模式下，過曝佔比到達此閾值時，兩組 Table 各半混合（50% Low + 50% High）。

(7) ``saturated_range_th3``：Auto 模式下，過曝佔比高於此閾值時，直接使用 ``sort_win_weight_high`` 作為 Sorting Table。

.. note::
  三個閾值將過曝佔比劃分為四個區段，決定最終採用的 Sorting Table：

  - 低於 ``saturated_range_th1``：純 ``sort_win_weight_low``
  - ``saturated_range_th1`` ~ ``saturated_range_th2``：插值混合，比例偏向 ``sort_win_weight_low``
  - ``saturated_range_th2`` ~ ``saturated_range_th3``：插值混合，比例偏向 ``sort_win_weight_high``
  - 高於 ``saturated_range_th3``：純 ``sort_win_weight_high``

(8) **AE Sorting**：編輯 ``sort_win_weight_low`` 及 ``sort_win_weight_high`` 兩組 Sorting Table 的權重內容。

.. note::
  Table 中的位置由左上到右下對應畫面由暗到亮的區塊。依 ``sort_mode`` 不同，High Table 的權重設計方向也不同：

  - **Bright Mode（側重高光保護）：High Table 的權重由左上至右下遞增，讓較亮區塊獲得更高的計算權重。**
  - **Dark Mode（側重暗部補償）：High Table 的權重由左上至右下遞減，讓較暗區塊獲得更高的計算權重。**

  |image8|

.. warning:: **按下 Reverse 按鍵時，會將原本的 Table 權重變為倒序。**

(9) ``manual_level``：Manual 模式下，兩組 Sorting Table 的混合強度，數值範圍為 0～100。

  - 數值越大，越偏向 ``sort_win_weight_high``\ （對特定亮度區域的側重效果越強）。
  - 數值越小，越偏向 ``sort_win_weight_low``\ （各區塊權重趨於均等）。

(10) ``dark_table``：Dark 模式下，依 Gain / ETGain 動態切換 Sorting Table 的對應設定。

  - 閾值基準可選擇 Gain 或 ETGain 其中之一。
  - TH 為閾值，Table 為該 TH 條件下對應使用的 Sorting Table（可調整範圍 0～16）。
  - Count of Dynamic 設定動態切換的組數。

  |image9|

(11) ``bright_table``：Bright 模式下，依 Gain / ETGain 動態切換 Sorting Table 的對應設定。

  - 閾值基準可選擇 Gain 或 ETGain 其中之一。
  - TH 為閾值，Table 為該 TH 條件下對應使用的 Sorting Table。
  - Count of Dynamic 設定動態切換的組數。

  |image10|

AE target 有兩個機制可動態降低：根據 ETGain 以及根據飽和區比重。

- ETGain 越高，代表環境越暗，此時降低 AE target 可避免 Gain 過高而產生大量雜訊。
- 飽和區比重越高，代表畫面過曝範圍越大，此時降低 AE target 可縮小過曝區域。

當兩個機制同時開啟時，取降幅較小者為準。

(12) ``dyn_target_etgain``：根據 ETGain 動態降低 ``y_mean_target`` 的功能。適用於低光環境，透過降低亮度目標值，讓 AE 使用較低的 Gain 以減少雜訊。

.. note::
  - Enable：是否開啟此功能，勾選時代表開啟。
  - ``src_th`` 為 ETGain 閾值設定；``extend_value`` 為在 ``src_th`` 條件下對應要降低的 ``y_mean_target`` 差值設定。
  - ``src_th`` 的條件數值為 ``dyn_target_etgain`` **未開啟**\ 時的 ETGain；開啟後 Tool 所看到的 ETGain 已是重新計算過後的數值。

  |image11|

(13) ``dyn_target_saturate``：根據飽和區域比例動態降低 ``y_mean_target`` 的功能。適用於背光環境，透過降低亮度目標值，縮小過曝區域的範圍。

.. note::
  - Enable：是否開啟此功能，勾選時代表開啟。
  - ``src_th`` 為飽和區域比例閾值設定；``extend_value`` 為在 ``src_th`` 條件下對應要降低的 ``y_mean_target`` 差值設定。

  |image12|

< 圖像調試視窗的 AE \\ AE Attribute Extension 參數頁面 - 2/2 >

|image13|

**根據以下參數設定，AE 在** ``y_mean_target_l`` **與** ``y_mean_target_h`` **間會有不同的收斂行為。**

(14) ``hist_contrast_power``：決定 AE 在目標區間內偏向防過暗或防過曝的收斂趨勢，數值介於 0～1。

.. note:: 數值越接近 1，AE 在目標區間內越傾向往較亮方向收斂（側重防過暗）；數值越接近 0，越傾向往較暗方向收斂（側重防過曝）。

(15) ``hist_pos_th_l``：防過暗機制的暗部亮度閾值。直方圖中亮度低於此值的像素視為暗部。

.. note:: 防過暗機制的收斂目標：將暗部像素（亮度低於 ``hist_pos_th_l``\ ）的佔比控制在 ``hist_percentage_th_l`` 以下。

(16) ``hist_pos_th_h``：防過曝機制的亮部亮度閾值。直方圖中亮度高於此值的像素視為過曝區域。

.. note:: 防過曝機制的收斂目標：將過曝像素（亮度高於 ``hist_pos_th_h``\ ）的佔比控制在 ``hist_percentage_th_h`` 以下。

(17) ``hist_percentage_th_l``：防過暗機制容許的最大暗部像素佔比。

(18) ``hist_percentage_th_h``：防過曝機制容許的最大過曝像素佔比。

.. note:: ``hist_percentage_th_l`` / ``hist_percentage_th_h`` 為千分比，數值範圍為 0～1000。

(19) ``hist_target_alpha_ET_gain_th_l``：直方圖影響 AE target 的 ETGain 下限。當 ETGain 高於此閾值時（通常為較暗環境），AE target 固定不受直方圖資訊影響。

(20) ``hist_target_alpha_ET_gain_th_h``：直方圖影響 AE target 的 ETGain 上限。當 ETGain 低於此閾值時（通常為較亮環境），AE target 完全跟隨直方圖資訊動態調整。

.. note::
  ETGain 落於 ``hist_target_alpha_ET_gain_th_l`` 與 ``hist_target_alpha_ET_gain_th_h`` 之間時，AE target 為直方圖調整值與固定值的混合結果，隨 ETGain 比例漸變。

3.2.4 AE\\HDR 參數說明
^^^^^^^^^^^^^^^^^^^^^^^

< 圖像調試視窗的 AE \\ HDR 參數頁面 >

|image14|

(1) Exposure Mode：唯讀。顯示目前的曝光模式。HDR 表示感測器以高動態範圍模式（長短曝交替）進行曝光；Linear 表示感測器以一般線性模式進行曝光。

(2) Exposure Ratio Current：唯讀。顯示目前長短曝之間的實際曝光比，其值由 Exposure Ratio 相關參數與當前環境對比共同決定。

(3) Exposure Ratio Status：唯讀。顯示目前 HDR 曝光比的收斂狀態。

  - **Stable**：當前畫面亮度與過曝比例均符合設定目標，曝光比維持穩定。
  - **Unstable**：畫面亮度或過曝比例偏離目標，系統持續嘗試調整中。若長期處於 Unstable，可嘗試放寬 Mean Tolerance Thd 或 Over Ratio Thd 的設定值。

(4) Brightness Hist Mean Current：唯讀。顯示目前亮區的亮度均值，作為是否需要調整曝光比的參考依據。當此值與 Brightness Hist Mean Target 的差距超出 Mean Tolerance Thd 所允許的範圍時，系統將觸發曝光比切換。

(5) Brightness Hist Mean Target：亮區的亮度目標值。此值設定越小，系統越傾向採用較大的曝光比，以在亮部保留更多細節。

(6) Mean Tolerance Thd1：亮區亮度偏差的第一級判斷閥值，對應輕微偏差的觸發門檻。

(7) Mean Tolerance Thd2：亮區亮度偏差的第二級判斷閥值，對應中等偏差的觸發門檻。

(8) Mean Tolerance Thd3：亮區亮度偏差的第三級判斷閥值，對應較大偏差的觸發門檻。

.. note::
  三組閥值分別對應不同程度的亮度偏差，共同決定曝光比切換的時機；設定值越大，對應程度的偏差越不容易觸發切換。須滿足 Thd1 ≤ Thd2 ≤ Thd3。

(9) Over Exposure Area Bin Num：定義「過曝區」的判斷範圍。亮度直方圖以 0~255 共 256 個 Bin 表示，Bin Num 決定從最亮端（Bin 255）往前計算多少個 Bin 內的亮度視為過曝。設定值越大，判定為過曝的亮度範圍越廣，過曝保護效果越明顯。

(10) Over Ratio Current：唯讀。顯示目前畫面中過曝區域的佔比，作為是否需要調整曝光比的參考依據。當此值超出 Over Ratio Thd 所允許的範圍時，系統將觸發曝光比切換。

(11) Over Ratio Thd1：過曝比例的第一級判斷閥值，對應輕微過曝的觸發門檻。

(12) Over Ratio Thd2：過曝比例的第二級判斷閥值，對應中等過曝的觸發門檻。

(13) Over Ratio Thd3：過曝比例的第三級判斷閥值，對應較大過曝的觸發門檻。

.. note::
  三組閥值共同決定縮小曝光比（降低過曝程度）的時機；設定值越大，對應程度的過曝越容易觸發降低曝光比的動作。須滿足 Thd1 ≤ Thd2 ≤ Thd3。

(14) Dyn Ratio Range：動態限制 HDR 曝光比的收斂區間。啟用後，曝光比只能在設定的上下限範圍內調整，可避免在極端條件下曝光比過度拉大或縮小。

(15) Dyn Ratio Range By Etgain：根據 ETGain 動態調整 HDR 曝光比的允許區間。不同的 ETGain 條件（反映不同環境亮度）可對應不同的曝光比上下限。

.. note::
  ``src_th`` 為 ETGain 的閾值設定，``min`` 與 ``max`` 為該條件下對應的曝光比下限與上限。

  |image15|

(16) Inc Ctrl：在 HDR AE 收斂過程中，根據當前 Gain 大小決定是否主動拉大曝光比。此機制用於在暗部環境下，優先透過增加曝光比提升長曝的曝光量，而非繼續拉高 Gain，以降低畫面雜訊。

(17) Inc Ctrl Gain Thd：觸發主動拉大曝光比的 Gain 閥值。當 Gain 超過此設定值，且曝光時間已達 Flicker Step 基準（50Hz：10ms；60Hz：8.3ms）時，系統將主動拉大曝光比，並改以 Flicker Step 為單位繼續調整曝光時間。

.. note::
  以預設值 Gain Thd 16、Flicker mode 50Hz 為例：

  - Gain 小於 16 倍時，曝光時間收斂的最大上限為 10ms
  - Gain 大於 16 倍時，系統主動拉大曝光比，並改用 Flicker Step 繼續收斂曝光時間

**HDR Convergence: 高動態範圍曝光收斂步階的相關控制參數。**

(18) Dark To Bright Step：畫面由暗轉亮時的曝光收斂速度係數。數值大於 1 時畫面調亮速度較快；數值小於 1 時速度較慢，可依應用需求調整畫面變亮的反應速度。

(19) Bright To Dark Step：畫面由亮轉暗時的曝光收斂速度係數。數值大於 1 時畫面調暗速度較快；數值小於 1 時速度較慢，可依應用需求調整畫面變暗的反應速度。

(20) Step Table：根據當前亮度與目標亮度的差距，配置不同收斂步階的參數群組，僅 Fast Mode 下適用。

< 圖像調試視窗的 AE \\ HDR \\ StepTable 參數頁面 >

|image16|

**Increase：畫面由暗到亮調整步階的參數類別，當環境光源變暗增加曝光的參數，此時 Step >= 1**

(1) Step High: 當光線變暗，Y Mean 與目標值差距較大時的收斂步伐大小，為最大調整步長

(2) Step Middle-High: 當光線變暗，Y Mean 與目標值差距的收斂步伐大小，為次大調整步長

(3) Step Middle: 當光線變暗，Y Mean 與目標值差距的收斂步伐大小，為中等調整步長

(4) Step Low: 當光線變暗，Y Mean 與目標值差距的收斂步伐大小，為較小調整步長

(5) Step Vary Low (Pow): 當光線變暗，Y Mean 與目標值差距差異為小的調整步長冪數

**Decrease：畫面由亮到暗調整步階的參數類別**

(6) Step High: 當光線變亮，Y Mean 與目標值差距較大時的收斂步伐大小，為最大調整步長

(7) Step Middle: 當光線變亮，Y Mean 與目標值差距的收斂步伐大小，為中等調整步長

(8) Step Low: 當光線變亮，Y Mean 與目標值差距的收斂步伐大小，為較小調整步長

(9) Step Vary Low (Pow): 當光線變亮，Y Mean 與目標值差距差異為小的調整步長冪數

3.2.5 AE\\Detail Adjustment 頁面說明
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

< 圖像調試視窗的 AE \\ Detail Adjustment 頁面 >

|image17|

(1) Mean Value：以曲線圖呈現畫面的 Y mean 亮度統計，用於觀察 AE 收斂是否穩定。

  統計來源選項（三者可同時疊加顯示）：

  - **Exposure Statis**：Linear 模式或 HDR Fusion 後的合成亮度統計
  - **Long Statis**：長曝 Fusion 前的亮度統計
  - **Short Statis**：短曝 Fusion 前的亮度統計

  勾選 **Show Detail** 後，畫面中會同步標示 ``y_mean_target``\ 、``y_mean_target_l``\ 、``y_mean_target_h``\ 、``y_mean_enter_high_light_th``\ 、``y_mean_exit_high_light_th`` 等參考基準線，便於對照目前收斂狀態進行調整。

(2) Y Histogram：以直方圖呈現畫面的亮度分布，用於判斷過暗或過曝的比例是否在預期範圍內。

  統計來源選項與 Mean Value 相同（三者可同時顯示）：

  - **Exposure Statis**：Linear 模式或 HDR Fusion 後的合成統計
  - **Long Statis**：長曝 Fusion 前的統計
  - **Short Statis**：短曝 Fusion 前的統計

  勾選 **Show Detail** 後，直方圖上會疊加標示 ``hist_pos_th_l``\ 、``hist_pos_th_h``\ 、``hist_percentage_th_l``\ 、``hist_percentage_th_h`` 等基準線，便於對照門檻值進行調整。

(3) Metering：以 16x16 數值矩陣呈現畫面各區域的亮度分布，每格對應畫面中一個區塊的亮度數值。此統計資料反映影像整體的曝光率分布，是 AE 進行自動亮度調整的計算基礎；可透過此矩陣快速判斷哪些區域過曝或過暗，評估影像品質是否符合預期。

  統計來源選項（依需求切換顯示）：

  - **Exposure Statis**：Fusion 後合成統計
  - **Long Statis**：長曝 Fusion 前的統計
  - **Short Statis**：短曝 Fusion 前的統計

(4) Exposure Mode：唯讀。顯示目前的曝光模式種類。HDR 表示感測器正以高動態範圍模式（長短曝交替）進行曝光；Linear 表示感測器以一般線性模式進行曝光。

(5) Exposure Ratio：唯讀。顯示目前長短曝之間的曝光比值。

(6) 曝光參數即時數值：唯讀。顯示目前各項曝光時間與增益數值，依統計來源分欄呈現：

  - **Exposure Statis**：Linear 模式或 HDR Fusion 後的合成曝光資訊
  - **Long Statis**：長曝 Fusion 前的個別曝光參數
  - **Short Statis**：短曝 Fusion 前的個別曝光參數

(7) Auto / Manual：切換自動或手動曝光模式。選擇 Manual 後，可直接在下方欄位設定曝光時間（Exposure Time）與增益（Gain）數值，用於手動驗證特定曝光條件下的畫面效果。

3.3 HDR Fusion
--------------

HDR Fusion（High Dynamic Range Image Fusion；高動態範圍影像合成）是 ISP 影像處理流程中的核心畫質模組。

**功能定義**

系統將感測器（Sensor）輸入的數張不同曝光時間之影像（目前支援 2 張）進行智慧合成，產出一張具備高動態範圍的高畫質影像，在保有亮部細節的同時，也能維持暗部的清晰度。

**啟用條件**

此功能僅在 vHDR 模式下開放配置。

**調校說明**

本模組的相關參數可依據不同的增益條件（Gain）進行獨立設定。部分特定參數亦支援依據不同的曝光比（Exposure Ratio）給予彈性配置。

3.3.1 HDR Fusion\\General 參數說明
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

< 圖像調試視窗的 HDR Fusion \\ General 參數頁面 >

|image18|

(1) Enable: 唯讀。HDR Fusion 的啟用狀態，由系統依照當前曝光模式自動控制，無法手動切換。

  - **Enable**：系統處於 HDR mode，Fusion 功能自動啟動，輸出為多重曝光合成的高動態影像
  - **Disable**：系統處於 Linear mode，Fusion 功能自動關閉，輸出為單曝光影像

(2) Fusion Mode: 選擇 Fusion Rate Estimation 所採用的演算法，決定各 pixel Fusion Rate 的估測依據。

  - **Lumin**：依據短曝 pixel 的亮度估測 Fusion Rate，亮度越高，Fusion Rate 越高
  - **Diff**：依據長、短曝 pixel 的差值估測 Fusion Rate，差值越大，Fusion Rate 越高
  - **Mix**：同時參考 Lumin 與 Diff 兩種方式的估測結果，取兩者中較大值作為最終 Fusion Rate

(3) Clip Mode after Fusion: Fusion 後的影像資料裁切範圍設定。

  - **Non-Clipped**：不對資料做裁切
  - **Clip to 16-bit**：將資料裁切至 16 bit
  - **Clip to 14-bit**：將資料裁切至 14 bit

(4) Debug Mode Fusion Rate: 以灰階影像呈現各 pixel 的 Fusion Rate 分布，可用於觀察合成比例是否符合預期。畫面越亮的區域，代表該區域的短曝成分比例越高。

  - **Enable**：開啟 Fusion Rate 灰階視覺化
  - **Disable**：關閉，回到正常影像輸出

(5) Exposure Ratio: 唯讀。顯示目前系統使用的長短曝光比數值。

3.3.2 HDR Fusion\\Fusion Rate Estimation 參數說明
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

< 圖像調試視窗的 HDR Fusion \\ Fusion Rate Estimation 參數頁面 >

|image31|

Fusion Rate 決定每個 pixel 最終混合多少比例的短曝資料。Rate 越高，短曝成分越多；Rate 越低，長曝成分越多。以下參數定義 Fusion Rate 的估測行為與允許範圍。

(1) Fusion Rate - Max: Fusion Rate 的上限值，可根據曝光比給予不同設定

(2) Fusion Rate - Min: Fusion Rate 的下限值，可根據曝光比給予不同設定

**Lumin Mode：** 依據短曝資料的 pixel 亮度來估測 Fusion Rate，短曝亮度越高，推算出的 Fusion Rate 越高。

(3) Lumin Mode - Thd0: 亮度曲線的第一個轉折點閥值。閥值越高，需要更高的短曝亮度才能觸發 Fusion Rate 提升，短曝主導的區域越小。

(4) Lumin Mode - Thd1: 亮度曲線的第二個轉折點閥值。閥值越高，Fusion Rate 達到最大值所需的短曝亮度門檻越高，短曝主導的區域越小。

**Diff Mode：** 依據長、短曝 pixel 的差值來估測 Fusion Rate，差值越大，推算出的 Fusion Rate 越高。

(5) Diff Mode - Thd0: 差值曲線的第一個轉折點閥值。閥值越高，需要更大的長短曝差值才能觸發 Fusion Rate 提升，短曝主導的區域越小。

  Thd0 支援依據短曝資料的 pixel 亮度分段設置不同閥值，點擊 Edit Table 可開啟設定視窗。

  |image32|

(6) Diff Mode - Offset (Thd1=Thd0+Offset): 定義第二轉折點（Thd1）與 Thd0 之間的間距。Offset 越大，兩個轉折點的差距越大，Fusion Rate 曲線的過渡區間越寬，短曝主導的區域越小。

.. _oep-module:

3.4 Over Exposed Protection
----------------------------

Over Exposed Protection（過曝保護）用於抑制影像過曝區域的溢色（color bleeding）現象。當畫面中某區域亮度超過感測器的動態範圍時，像素值會飽和並發生色彩失真；本模組透過亮度閥值控制，減少此類溢色對畫質的影響。

**功能定義**

針對過曝像素進行亮度壓制，避免高亮區域因數值溢出而造成顏色偏移。

**調校說明**

本模組的相關參數可依據不同的增益條件（Gain）進行獨立設定。

.. _oep-general:

3.4.1 Over Exposed Protection\\General 參數說明
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

< 圖像調試視窗的 Over Exposed Protection \\ General 參數頁面 >

|image33|

(1) Enable: Over Exposed Protection 功能開關。

  - **Enable**：開啟過曝保護，抑制過曝區域溢色現象
  - **Disable**：關閉過曝保護，不對過曝區域進行處理

(2) ISP Mode: 唯讀。顯示目前輸出影像的曝光模式。

  - **HDR**：感測器以多重曝光高動態範圍模式輸出
  - **Linear**：感測器以一般線性單曝光模式輸出

(3) **Clip Mode**：設定 OEP 模組輸出的位元深度上限。當像素亮度值超過所選位元深度的最大值時，超出範圍的亮部資訊將被直接截斷（hard clip）並設為上限值，該部分的亮部細節無法還原。位元深度越低，截斷越早，亮部資訊損失越多，越容易在高飽和度區域引發色偏或溢色。

   - **Non-Clipped**：不截斷，保留 OEP 模組完整計算精度（精度最高，亮部資訊損失最少）。
   - **Clip to 18-bit**：超過 18-bit 範圍（262143）的亮度值截斷至上限。
   - **Clip to 16-bit**：超過 16-bit 範圍（65535）的亮度值截斷至上限。
   - **Clip to 14-bit**：超過 14-bit 範圍（16383）的亮度值截斷至上限。
   - **Clip to 12-bit**：超過 12-bit 範圍（4095）的亮度值截斷至上限（截斷最早，亮部細節損失最多）。

.. warning::
   **HDR mode** 預設 **Non-Clipped**；\ **Linear mode** 預設 **Clip to 12-bit**\ 。若 Linear mode 下高飽和度區域出現溢色，可嘗試提升至較高位元深度（如 14-bit 或 16-bit），以保留更多亮部資訊並降低溢色風險。

(4) Thd0: 亮度抑制的起始閾值（須小於 Thd1）。像素亮度超過此值後，系統開始逐漸壓制其亮度，以防止過曝區域產生溢色。閾值設定越低，抑制介入越早；設定過低則可能在非過曝區域出現顏色錯誤與階調不連續。

(5) Thd1: 亮度抑制的完全飽和閾值（須大於 Thd0）。像素亮度超過此值後，亮度值被完全鎖定，達到最大抑制效果。Thd0 ～ Thd1 之間形成漸變過渡區：兩者差值越大，過渡越平滑；差值越小，亮暗邊界的階調越容易出現不連續現象。

**調適範例：Thd0 / Thd1 對溢色的影響**

調整 Thd0 與 Thd1 的主要目的是防止過曝區域產生\ **溢色**\ （色彩飽和失真）現象。下圖為三組參數設定的對照範例：

- **設定 1**：功能停用（Thd0 = 4095、Thd1 = 4095），作為基準比較。
- **設定 2**：啟用，Thd0 = 700、Thd1 = 1000（參數過強）。
- **設定 3**：啟用，Thd0 = 2600、Thd1 = 3600（適當配置）。

|image29|

.. centered:: 圖：三組 Thd0 / Thd1 設定的參數對照（1：停用基準；2：參數過強；3：適當配置）

下圖為三組設定對應的局部畫質差異：

- **設定 (3) 適當參數**：過曝區域明顯縮小，亮部細節保留較完整，色彩資訊豐富。
- **設定 (2) 參數過強**：抑制效果過激，過曝區域反而擴大，亮部細節流失，並產生\ **顏色錯誤與溢色**\ ，以及顏色與亮度階調的不連續現象。

|image27|

.. centered:: 圖：各組設定的局部畫質對照（左上：粉色玩具；右上：色彩豐富物件；左下：花卉與測試圖；右下：標準色卡）

3.5 Tone Mapping
-----------------

Tone Mapping（色調映射）是影像處理流程中的核心畫質模組，負責將高動態範圍影像（High Dynamic Range）映射至有限動態範圍的顯示媒介（如顯示器），在壓縮動態範圍的同時盡量保留真實環境的細節與色彩。

**功能定義**

本功能包含 General、Edge Detection 與 GTM 三個子模組，分別負責整體開關控制、邊緣細節保留，以及全域映射曲線的配置。

**調校說明**

本模組的相關參數可依據不同的增益條件（Gain）進行獨立設定。

3.5.1 ToneMapping\\General 頁面
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

< 圖像調試視窗的 ToneMapping \\ General 頁面 >

|image34|

(1) Tone Mapping Enable: 功能開關

  - Enable 表示開啟 Tone Mapping 功能
  - Disable 表示關閉

(2) Clip Mode: Tone Mapping 模組結束後的資料是否進行 Clip 處理

  - **Non-Clip**：資料不進行處理，直接輸出 14 bit 影像資料
  - **Scale and Clip to 12bit**：資料除以 4 之後再 Clip 至 12 bit
  - **Clip to 12bit**：資料直接 Clip 至 12 bit，不進行縮放

3.5.2 ToneMapping\\Edge Detection 頁面
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Edge Detection 目的是偵測圖像中的邊緣與紋理特徵，將之提取並保留，並避免邊緣產生 halo effect 現象。

< 圖像調試視窗的 ToneMapping \\ Edge Detection 頁面 >

|image35|

(1) Debug Mode: 功能開關，開啟後能顯示當前畫面各邊緣的 Edge rate；越白代表 Edgeness 愈高，邊緣被強化越多。Edgeness 會根據參數配置而改變。

  - Enable 表示開啟
  - Disable 表示關閉

(2) Edge Map: 顯示 Debug Mode 的模式

  - **Long**：僅顯示長曝幀的 Edgeness
  - **Short**：僅顯示短曝幀的 Edgeness
  - **Final Edge Rate**：最後決策出的 Edge rate，由相同位置的 Long / Short Edgeness 取較大值決定

**Edge direction** 為判斷邊緣方向的相關參數群組。Edge Filter 共偵測 4 種方向（0°、45°、90°、135°），演算法從中選出機率最大的兩個方向 ``d0``\ 、``d1``\ ，再依閾值 ``K0``\ 、``K1``\ 、``K2`` 將結果分為四個等級，決定 ``d0`` 與 ``d1`` 的 Blending 比重（``delta d0`` 越小，代表 ``d0`` 方向的可能性越大，``d0`` blending 權重越高）：

.. list-table::
   :header-rows: 1
   :widths: 12 50 38

   * - 等級
     - 進入條件（以 delta d0 為判斷基準）
     - d0 blending weight
   * - 第一級
     - delta d0 ≤ K0
     - 1（完全依據 d0 方向）
   * - 第二級
     - K0 < delta d0 ≤ K1
     - 由 W0 決定
   * - 第三級
     - K1 < delta d0 ≤ K2
     - 由 W1 決定
   * - 第四級
     - delta d0 > K2
     - 0.5（d0 與 d1 各占 50%）

(3) Direction Factor K0：值越高，越容易進入第一級，即越容易僅參考 ``d0`` 方向，此時 ``d0`` blending weight = 1。

(4) Direction Factor K1：值越高，越容易進入第二級，將依據 ``W0`` 參數值配置第二級權重。

(5) Direction Factor K2：值越高，越容易進入第三級，將依據 ``W1`` 參數值配置第三級權重。

.. note:: ``K0``\ 、``K1``\ 、``K2`` 的配置須維持遞增關係：``K0`` < ``K1`` < ``K2``\ 。

(6) Direction Weight W0：進入第二級時 Blending 的權重，參數值以 256 進行 normalize。

(7) Direction Weight W1：進入第三級時 Blending 的權重，參數值以 256 進行 normalize。

.. note:: ``W0``\ 、``W1`` 須維持遞減關係：``W0`` > ``W1``\ 。

Edge direction 參數關係如下圖所示。其中 ``delta d0`` 越小代表 ``d0`` 方向的可能性越大，``d0`` blending 權重也越大。

|image36|

Edge rate 參數類別根據特徵值計算各紋理邊緣的 ``edge rate``\ 。模組能根據 pixel 亮度資訊調整不同的閾值，長短曝幀的參數也能獨立控制。

(8) Tune mode: 調整 edge rate 時選擇長短曝參數的配置方式

  - **Sync mode**：輸入的參數同時配置在長、短曝參數（當長短曝參數配置不同下選擇 Sync mode 時，短曝參數配置將改變至與長曝參數相同）
  - **Long mode**：輸入參數僅配置在長曝參數
  - **Short mode**：輸入參數僅配置在短曝參數

(9) ～(19) ``Yxx Thd0``：Edge rate 計算的第一個轉折點閾值，Y00～Y100 共十一個參數，分別對應不同亮度區間的閾值。

(20) ``Luma offset``：第一個轉折點與第二個轉折點的 offset，第二轉折點的閾值為 ``Yxx Thd0`` + ``Luma offset``\ 。

(21) Rate Min: 特徵值小於第一個轉折點的 Edge rate 值

(22) Rate Max: 特徵值大於第二個轉折點的 Edge rate 值

3.5.3 ToneMapping\\GTM 頁面
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

GTM 是將高動態範圍的影像資料透過全域映射的方式投影至有限動態範圍的資料。

共有七條不同的 GTM curve 可配置，各 curve 的差異在於輸入資料的最高 bit 數不同。GTM 演算法將根據當前畫面的 EPF max 值（偵測紋理的最大資料值）決定以哪兩條 GTM curve 進行內插，產出套用的 Global Tone Mapping 曲線。模組亦提供手動模式，可直接輸入 EPF max 值取代實際計算值，間接選擇使用的 GTM curve。

< 圖像調試視窗的 ToneMapping \\ GTM 頁面 >

|image37|

(1) Decision Max EPF: 唯讀。顯示當前畫面實際計算出的最大 EPF 值；在自動模式下，此值決定下一幀套用的 GTM 曲線。

(2) Decision Mode: 決定 Max EPF 值的決策方式

  - **Auto Mode**：GTM curve 根據 Decision EPF Max 自動決定使用哪一條 GTM curve
  - **Manual Mode**：根據手動輸入的 Manual Max EPF Value 決定下一幀使用的 GTM curve

(3) Manual Max EPF Value: Decision Mode 為 Manual mode 時，決定 GTM curve 所參考的 Max EPF Value

< 圖像調試視窗的 ToneMapping \\ GTM Curve 頁面 >

|image38|

GTM Curve 頁面用於調整全域映射曲線節點。頁面上方顯示共七條 GTM curve，x 軸代表輸入資料值，y 軸代表全域映射的對應值。七條曲線須符合以下約束條件：

- i 方向為遞增：``gtm[i][0]`` ≤ ``gtm[i][1]`` ≤ ... ≤ ``gtm[i][9]``
- j 方向為遞減：``gtm[0][j]`` ≥ ``gtm[1][j]`` ≥ ... ≥ ``gtm[9][j]``

即同一條 GTM 曲線上節點需為遞增，不同曲線上相同次序的節點需為遞減趨勢。

(1) GTM Target: 此設定將自動產生七條 GTM curve 的配置，數值介於 12~14

.. note:: GTM Target 越大，針對暗區的亮度提升越明顯，但過曝區保護效果越有限；GTM Target 越小，過曝保護效果越好，但可能限制暗區的動態範圍。

3.6 LSC
--------

LSC（Lens Shading Correction；鏡頭陰影校正）用於修正因鏡頭光學特性引入之畫面亮度與色彩不均勻的問題。

**功能定義**

鏡頭邊緣因入射光線角度較大，容易造成畫面四角亮度低於中心的暗角（Luma Shading）；同時，由於不同波長光線的折射率差異，也會造成色彩偏差（Color Shading）。LSC 模組針對這兩類問題進行補償，還原均勻的亮度與色彩分布。

LSC 的調整頁面分為上下兩部分：上半部為自動校正流程（詳見 :ref:`2.5.3 LSC 校正 <lsc-calibration>`\ ），下半部為手動微調已校正參數的介面。本章節主要說明下半部的操作方式。

**調校說明**

本模組的 Adjust Rate 參數可依照色溫 / 增益條件給予不同設定。

|image39|

(1) **NLSC R、G、B curve**：可手動直接修改 NLSC curve。頁面右側以圖形化方式顯示三條 curve，橫軸為像素與畫面中心的距離，縱軸為 curve value。

(2) **MLSC 參數手動修改**。操作步驟如下：

  勾選 Block Enable，畫面中會將 MLSC 的區塊格線顯示出來

  |image40|

  滑鼠點擊欲修改的區塊

  |image41|

  按下介面右上方的【Read】，讀取所選擇區塊的 MLSC 參數值

  修改參數值

  |image42|

  按下介面右上方的【Write】，使參數生效，可在畫面中看到效果

  |image43|

(3) **Adjust Rate**：精度為 1/32，32 表示 Rate 為 1 倍，不改變已校正的 NLSC curve，但會加乘 NLSC 的幅度。支援依不同色溫或增益條件套用不同的 Adjust Rate。

.. note::
  - Adjust Rate 越小，圖像四周亮度越低；Adjust Rate 越大，圖像四周亮度越高。

(4) **Dynamic Thd**：單位為千分比，用於判斷色溫點座標周圍是否有足夠的白色統計點。

.. note::
  - 若當前畫面白色統計點比例大於 ``Start THD``\ ，參數 (3) 的 Rate 依色溫動態設定生效。
  - 若當前畫面白色統計點比例小於 ``End THD``\ ，參數 (3) 的 Rate 不依色溫動態設定生效。
  - 若觀察到參數 (3) 的 Rate 與預期的色溫動態設定值不符，建議檢查 **Dynamic Thd** 設定。

3.7 Gamma
----------

Gamma 校正是影像處理中用於補償人眼視覺感知非線性特性的畫質模組。

**功能定義**

Sensor 的 Input 與 Output 之間的關係是線性的，但人眼視覺對暗部的感知遠較亮部敏感。若不進行校正，線性影像在視覺上會顯得暗部細節不足、對比偏低。Gamma 即用以處理 Sensor 端訊號與人眼感知之間的轉換關係，使輸出影像的亮度分布與人眼感知較為一致。

**調校說明**

本模組的 RGB Gamma 及 Y Global Curve 參數可依據不同的增益條件（Gain）進行獨立設定。

< 圖像調試視窗的 Gamma 頁面 >

|image44|

(1) 圖形化顯示: 根據選擇的 Gamma Type 將 Input / Output 的對應關係以圖形化顯示

(2) 參數: 列出 Gamma 模組的所有參數

(3) Gamma Type: 選擇要調整的 ISP Gamma 模組，共有以下選項：

  .. list-table::
     :header-rows: 1
     :widths: 40 60

     * - 模式
       - 說明
     * - **RGB Gamma**
       - 調整 RGB 三通道的 Gamma 曲線（主要使用）
     * - **Y Gamma**
       - 調整亮度通道的 Gamma 曲線
     * - **Y Global Curve**
       - 調整全域亮度映射曲線（主要使用）
     * - **RGB Gamma + YGC**
       - 組合模式：同時顯示 RGB Gamma 與 Y Global Curve
     * - **RGB Gamma + System Gamma**
       - 組合模式：固定 System Gamma，單獨調整 RGB Gamma，Y Global Curve 自動生成
     * - **Y Global Curve + System Gamma**
       - 組合模式：同時顯示 Y Global Curve 與 System Gamma

.. note::
  System Gamma 並非實際 ISP 中的模組，是 RGB Gamma 及 Y Global Curve 乘積的結果，可視為兩條 Gamma 在亮度上的整體效果。組合模式下可固定 System Gamma 不動，單獨調整 RGB Gamma，此時 Y Global Curve 會根據 RGB Gamma / System Gamma 的關係自動生成。

(4) Adjust Mode: 共有四種曲線調整方式

  - **Curve**：左側圖形化介面上有 4 個控制點可拖拉，拖拉時所有參數以近似線方式調整

  |image45|

  - **Point**：左側圖形化介面上顯示所有參數的控制點可拖拉，拖拉時只調整單一參數

  |image46|

  - **Gamma Value**：以標準 Gamma 式（:math:`Output = Input^{Coef}`）產生曲線；``Slope`` 參數用於控制暗處區域的線性過渡，可避免暗部雜訊過度放大；若不需使用，將 ``Slope`` 設為 -1 即可停用。

  |image47|

  - **Y Gain**：以線性方式生成對應曲線，容易有輸出到不了飽和數值或過飽和區域過多的現象，一般不使用

  |image48|

以下為曲線調整的操作按鈕：

.. list-table::
   :header-rows: 1
   :widths: 12 18 70

   * - 編號
     - 按鈕
     - 功能
   * - (5)
     - Undo
     - 回復到上一次調整
   * - (6)
     - Redo
     - 回復到 Undo 前的狀態
   * - (7)
     - Restore
     - 回復到最初始的狀態
   * - (8)
     - History
     - 參數群組記憶，以供切換

下圖為兩組不同 Gamma 曲線設定的效果對比：左圖為高對比設定；右圖為低對比設定。

|image53|\ |image54|

|image55|\ |image56|

3.8 DRC&DRE
------------

DRC&DRE（Dynamic Range Compression & Dark Range Enhancement；動態範圍壓縮與暗部增強）是用於提升影像動態範圍表現力的畫質模組。

**功能定義**

DRC 模組首先估測圖像動態範圍的原始曲線，依據局部對比判斷、亮度一致性維持等原則，計算並套用動態範圍的目標曲線；接著 DRE 局部補強暗部亮度，並同時提取與加入高頻細節，提升整體影像的視覺豐富度。

**調校說明**

本模組的相關參數可依據不同的增益條件（Gain）進行獨立設定。vHDR mode 下，部分特定參數亦支援依據不同的曝光比（Exposure Ratio）給予彈性配置。

3.8.1 DRC&DRE\\General 參數說明
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

< 圖像調試視窗的 DRC&DRE \\ General 頁面 >

|image49|

(1) **DRC Enable**：動態範圍壓縮/增強功能的開關。

   - **Enable**：開啟，DRC 模組開始運算並套用強化效果，可收斂過曝區域並提升整體動態範圍表現。
   - **Disable**：關閉。

(2) **DRC Mode Selection**：保留（無需調整）。

以下 (3)(4) 共同控制強化曲線的更新行為：

(3) **Update Enable**：控制動態範圍強化曲線是否逐幀更新。

   - **Enable**：每幀將計算出的最新強化曲線傳遞至下一幀生效，能及時因應場景對比變化。
   - **Disable**：關閉更新，強化曲線維持固定，補償強度不隨場景改變。

(4) **Rapid Update**：新曲線生效的過渡速度（僅 Update Enable 為 Enable 時有效）。

   越接近 **Fast**，新強化曲線越快完全套用；越接近 **Slow**，過渡越緩慢，畫面亮度變化較平滑。

(5) **Debug Mode**：顯示 DRC 處理前的灰階影像，用於排查畫面變化是否由 DRC 所引起。

   - **Enable**：開啟。
   - **Disable**：關閉。

3.8.2 DRC&DRE\\Decomposition 參數說明
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

本頁面提取圖像中的紋理、細節等特徵資訊，在動態範圍增強完成後再加回，以保留原有的圖像細節。

< 圖像調試視窗的 DRC&DRE \\ Decomposition 頁面 >

|image50|

(1) **Decomposition Enable**：動態範圍增強後的紋理細節保留/強化功能。

   - **Enable**：開啟，將提取的紋理細節加回至增強後的圖像。
   - **Disable**：關閉。

(2) **Decomposition Debug Mode**：輔助顯示加回紋理細節的位置，可搭配參數 (3)～(5) 調整使用。

(3) **HF Addition Control — 控制模式**：選擇加回紋理細節的方式。

   - **Fully**：不區分紋理或細節，以最大程度、最廣特徵範圍全數加回。
   - **Partial**：選擇性加回，可透過以下兩軸進一步調整頻率成分與強度。

(4) **HF Addition Control — Partial（MF～HF 軸）**：調整加回的頻率成分範圍。

   越接近 **HF**，偏向僅加回高頻細節；越接近 **MF**，除高頻外也加回更多 MF 紋理。

(5) **HF Addition Control — Partial（Low～High 軸）**：調整加回的整體強度。

   越接近 **High** 表示加回越多；越接近 **Low** 表示加回越少。

(6) **Blend Ori Control**：選擇是否將 DRC&DRE 最終結果與原始圖像進行比例混合。

   - **Enable**：啟用混合，可透過 (7)(8) 進一步設定。
   - **Disable**：不混合，直接輸出 DRC 效果圖像。

(7) **Blend Ori Level**：混合比例的決定方式。

   - **Auto**：自動計算混合比例。
   - **Manual**：手動設定混合比例。

(8) **Blend Ori Level 數值**：依 (7) 的選擇，數值意義不同。

  .. list-table::
     :header-rows: 1
     :widths: 15 15 70

     * - 模式
       - 值域
       - 數值意義
     * - **Auto**
       - −15 ～ 15
       - 自動計算混和比例的偏移量。正值偏向原圖方向（DRC 效果減弱），負值偏向強化 DRC 效果
     * - **Manual**
       - 0 ～ 15
       - 直接指定與原圖的混和比例。0 為完全套用 DRC 效果；15 為最大比例保留原圖（DRC 效果最弱）

.. note::
  Auto 與 Manual 兩種模式下，此參數均支援在 vHDR mode 中依曝光比給予不同設定。

  **Blend Ori Level 是調整 DRC 效果最直接有效的參數**，建議優先透過此參數控制整體動態範圍的改善幅度，再搭配其他參數細調。

.. note::
   不同增益分段的 DRC 強度建議採用一致的變化方向（隨增益增加而遞增或遞減），避免增益切換時畫面出現瞬間過亮或過暗的情形。

|image57|

|image58|

3.8.3 DRC&DRE\\Intensity 參數說明
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

< 圖像調試視窗的 DRC&DRE \\ Intensity 頁面 >

|image51|

(1) **Scene Feature Threshold**：單調（低對比）場景的判斷閾值。

   閾值越接近 **Low**，越容易將場景判定為單調場景，系統越傾向輸出無 DRC 效果的原始圖像；越接近 **High**，則要求更高的場景單調程度才切換。

以下 (2)～(11) 為 Contrast Pair（CP；對比度）相關參數，用於計算 CDF Curve（DRC 強度曲線）：

(2) **CP Curve to CDF**：選擇是否以 CP 對比資訊作為 CDF Curve 的計算依據。

   - **Enable**：採用 CP 對比資訊計算 DRC 強度曲線，可有效收斂過曝區域。
   - **Disable**：CDF Curve 計算不考慮 CP 對比資訊。

(3) **CP Curve To CDF Smooth Rate**：CP 模式下 CDF Curve 的平滑程度。

   越接近 **High** 能產出越平緩的變化曲線，避免對比不連續的 Contour 現象。

   .. warning::
      Smooth Rate 建議設定 ≥ 2。設定過低時，CDF Curve 變化過於劇烈，畫面將出現嚴重 Contour 並持續閃動，無法收斂。

(4)～(11) **Vote 1～Vote 8**：共八個亮度分段的權重參數。

   在 CP 計算的基礎上，依不同亮度區間對 CDF Curve 進行加乘調整，可針對暗部、中間調或亮部給予不同的對比強化比重。

以下 (12)～(13) 為 Brightness Preserving（BP；亮度維持）相關參數：

(12) **BP Curve to CDF**：選擇是否以 BP 亮度維持方式作為 CDF Curve 的計算依據。

   - **Enable**：採用 BP 方式計算 DRC 強度曲線，可收斂過曝區域，同時提升暗區亮度。
   - **Disable**：CDF Curve 計算不考慮亮度維持。

(13) **BP Curve To CDF Smooth Rate**：BP 模式下 CDF Curve 的平滑程度。

   越接近 **High** 能產出越平緩的變化曲線，避免亮度不連續的 Contour 現象。

   .. warning::
      Smooth Rate 建議設定 ≥ 2。設定過低時，高亮區域將出現亮度反轉（輸出非單調遞增），並伴隨持續閃動現象。

以下 (14)(15) 共同控制 CP 與 BP 在最終 CDF Curve 中的混合比例：

(14) **Blending Rate Enable**：CP 與 BP 混合功能的開關。

   - **Enable**：啟用，可透過 (15) 調整比例。
   - **Disable**：關閉。

(15) **Blending Rate**：產出 CDF Curve 時 CP 與 BP 的混合比重。

   數值越小偏向純 CP 效果；數值越大偏向純 BP 效果。

(16) **Local Operator Bright Tone**：亮處區域細節的增強強度。

   數值越大，亮處細節增強幅度越高，但雜訊也會隨之增加。

(17) **Local Operator Dark Tone**：暗處區域細節的增強強度。

   數值越大，暗處細節增強幅度越高，但雜訊也會隨之增加。

3.8.4 DRC&DRE\\Dark Range Enhancement 參數說明
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

< 圖像調試視窗的 DRC&DRE \\ Dark Range Enhancement 頁面 >

|image52|

(1) **DRE Enable**：暗部增強功能的開關。

   - **Enable**：開啟，針對亮度低於閾值的像素進行亮度補強。
   - **Disable**：關閉。

以下 (2)～(4) 共同控制 DRE 的作用範圍與強度：

(2) **DRE Control — Dark Tone TH**：DRE 作用區域的亮度上限閾值。

   閾值越接近 **High**，越多暗部像素被納入增強範圍，DRE 作用區域越廣。

(3) **DRE Control — Strength**：DRE 作用區域的亮度增強幅度。

   越接近 **Strong**，暗部亮度提升幅度越大。

.. note::
   (2) 與 (3) 需搭配調整：Dark Tone TH 定義哪些像素屬於「暗部」，Strength 決定這些像素被提升多少。

(4) **DRE Control — Bright Tone TH**：亮部原始亮度的保護閾值。

   閾值越接近 **High**，越多亮部像素維持原始亮度，整體亮度增強程度越低，可抑制畫面白化與低對比現象。

.. note::
  DRE 是視覺效果最直觀的調整模組之一，暗部亮度的提升在畫面上相當顯著，調整後通常可立即察覺差異。

  實際調校建議先確認 Dark Tone TH 圈定出正確的暗部範圍，再逐步提升 Strength；最後視亮部是否出現白化，適當調高 Bright Tone TH 加以保護。

3.9 WDR&Dehaze
--------------

WDR（Wide Dynamic Range；寬動態範圍）是 ISP 影像處理流程中的畫質增強模組，用於改善高對比場景中亮暗區域同時呈現的問題。

**功能定義**

- **WDR**：針對高對比場景，提升暗部區域的亮度，使極端亮暗的區域在影像中均能有合適的亮度呈現。
- **Dehaze（除霧）**：霧霾環境下，畫面色彩黯淡、對比度下降，細節清晰度亦會降低。Dehaze 功能的目的在於緩解霧霾對影像品質的影響。

**調校說明**

本模組屬於 Texture 參數，可依照增益條件（Gain）給予不同設定。在 vHDR 模式下，部分參數可依照曝光比給予不同設定。

3.9.1 WDR&Dehaze\\WDR 參數說明
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

< 圖像調試視窗的 WDR&Dehaze \\ WDR 頁面 >

|image170|

|image171|

(1) **WDR Mode**：WDR 的運作模式，分為 OFF、Manual、Auto 三種。

   - **OFF**：關閉 WDR 功能。
   - **Manual**：手動模式，以 (2) WDR Level 的設定值固定 WDR 強度。
   - **Auto**：自動模式，系統依場景自動調整 WDR 強度，同樣以 (2) WDR Level 作為手動強度參考。

(2) **WDR Level**：Manual / Auto 模式下的 WDR 強度，數值範圍為 0～100。數值越大，暗部區域提升的亮度越明顯。在 vHDR 模式下，此參數可依曝光比給予不同設定。

   .. tip:: WDR Level 的強度是效果最直觀的調整項目，建議以此作為動態範圍調整的起點。

< WDR 效果示意圖（左：WDR 關閉；右：WDR 開啟，暗部亮度明顯提升）>

|image300|

3.9.2 WDR&Dehaze\\Dehaze 參數說明
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

< 圖像調試視窗的 WDR&Dehaze \\ Dehaze 頁面 >

|image172|

(1) **Dehaze Enable**：Dehaze 功能開關。

   - **Enable**：開啟除霧功能。
   - **Disable**：關閉除霧功能。

(2) **Global Control \\ Level**：除霧強度，數值越大效果越強。

(3) **Global Control \\ Change Step**：Dehaze 強度的變化速度控制參數。

(4) **Blending Enable**：開啟原始數值與除霧結果的混合功能。開啟時可避免影像過度增強，但除霧效果會相對降低。

   - **Enable**：開啟混合，效果較柔和。
   - **Disable**：關閉混合，除霧效果最強。

(5) ～ (8) **Statistics Area \\ X Start / X End / Y Start / Y End**：Dehaze 數值統計區域的範圍設定。

   - X Start / X End：水平方向的起始與結束位置，數值範圍為 0～39，須滿足 X Start < X End。
   - Y Start / Y End：垂直方向的起始與結束位置，數值範圍為 0～29，須滿足 Y Start < Y End。

   .. note:: 霧霾區域主要影響範圍與天空區域相同，統計區域建議設定為畫面中央偏上的位置。

(9) **Draw Area Info**：將統計區域標示於畫面上，便於判斷統計範圍是否設定正確。

   - **Enable**：開啟標示。
   - **Disable**：關閉標示。

3.10 WDR Detail Adjust
-----------------------

WDR Detail Adjust（WDR 進階參數）是 ISP 影像處理流程中的進階調整模組，用於精細控制 WDR Auto 模式下場景對比度的估算邏輯與 WDR 強度的映射行為。

**功能定義**

透過分析畫面暗部與亮部的亮度分布，估算當前場景的對比度（Contrast Value），並依此決定 WDR Auto 模式下的作用強度，使 WDR 效果能更精確地對應場景特性。

**調校說明**

本模組屬於 Texture 參數，可依照增益條件（Gain）給予不同設定。

3.10.1 WDR Detail Adjust 參數說明
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

< 圖像調試視窗的 WDR Detail Adjust 參數頁面 >

|image173|

以下為各參數的整體計算流程概覽，說明 Contrast Value 如何從畫面亮度統計，經過各階段處理，最終對應到 WDR 作用強度：

< WDR Auto 模式 — Contrast Value 到 WDR 強度的計算流程 >

|image203|

**[Stage 1] 亮度統計 — 計算場景的亮暗對比**

(1) **Contrast Value**：唯讀。系統根據當前場景估算出的對比度數值，數值越大代表場景的動態對比越強。後續各階段的處理均以此數值為基礎，最終決定 Auto 模式下的 WDR 作用強度。

(2) **low_percent**：取亮度最低的前 ``low_percent`` 比例的像素進行暗部統計，數值範圍為 0～100。

(3) **high_percent**：取亮度最高的前 ``high_percent`` 比例的像素進行亮部統計，數值範圍為 0～100。

(4) **high_bins_avg_rgb_th**：白天模式（RGB Mode，IR LED 關閉）下，亮部區域平均亮度的下限閾值，數值範圍為 0～255。

**[Stage 2] 夜間修正 — 壓縮低對比的 Contrast Value（夜晚模式限定）**

(5) **hist_contrast_intp** 與 **(6) hist_contrast_intp_compress**：夜晚模式（IR Mode，IR LED 開啟）下，對原始 Contrast Value 進行非線性映射，避免低對比場景誤觸 WDR。

< 夜晚模式 Contrast Value 修正曲線示意圖 >

|image174|

映射邏輯如下：

- 若 Contrast Value > ``hist_contrast_intp``\ ：在 ``hist_contrast_intp_compress`` 與 255 之間插值，得到修正後的 Contrast Value（曲線右段）。
- 若 Contrast Value < ``hist_contrast_intp``\ ：在 ``hist_contrast_intp_compress`` 與 0 之間插值，得到修正後的 Contrast Value（曲線左段，向下壓縮）。

**[Stage 3] 暗部加權 — 強調較暗區域的對比特徵**

(7) **expand_num**：``hist_contrast_expand`` 陣列的參數個數，控制可調整的亮度分布區域寬度。

(8) **hist_contrast_expand**：針對較暗的亮度分布區域調整計算 Contrast Value 的權重。點擊【Detail Adjust】可開啟設定視窗。

< hist_contrast_expand Detail Adjust 視窗 >

|image175|\ |image176|

.. note:: 隨著 index 由小到大，``hist_contrast_expand`` 的數值應相對應遞減。

**[Stage 4] 高對比場景調整**

(9) **power_gain**：針對高對比場景進一步調整 Contrast Value，數值範圍為 0～4。

   - 暗區亮度夠低且暗部集中：``power_gain`` 越大，Contrast Value 越大。
   - 暗區亮度較高且分布均勻（畫面浮有霧白）：``power_gain`` 越大，Contrast Value 越小。

**[Stage 5] 強度對應 — 將 Contrast Value 映射至 WDR 目標強度**

(10) **map_num**：``hist_contrast_thd`` 與 ``wdr_target_map`` 陣列的參數個數。

(11) **hist_contrast_thd**：Contrast Value 的分段閾值陣列（X 軸）。點擊【Detail Adjust】可開啟設定視窗。

(12) **wdr_target_map**：對應各段 ``hist_contrast_thd`` 的 WDR 目標強度陣列（Y 軸），數值範圍為 0～255。點擊【Detail Adjust】可開啟設定視窗。

< hist_contrast_thd / wdr_target_map Detail Adjust 視窗 >

|image177|\ |image178|

**[Stage 6] 強度上限與速度控制**

(13) **wdr_target_limit**：WDR 可作用的強度上限閾值，數值範圍為 0～255。超過此上限的目標強度將被截斷，防止 WDR 過強造成偏色與明顯雜訊。

(14) **step**：WDR 強度的變化速度控制參數，控制 WDR 強度的收斂速率，避免畫面亮度驟變。

3.11 GbGr
----------

GbGr（GbGr Balance；GbGr 平衡調整）是 ISP 影像處理流程中的畫質修正模組，用於消除感測器與鏡頭特性不匹配所造成的條紋或網格狀畫面瑕疵。

**功能定義**

由於鏡頭（Lens）與感測器（Sensor）的特性差異，Bayer Pattern 上的 Gr 與 Gb 訊號強度可能不一致，導致畫面出現網格狀或直橫條紋（Pattern）。GbGr Balance 模組的目的即在於緩減此現象。

**調校說明**

本模組屬於 Texture 參數，可依照增益條件（Gain）給予不同設定。

3.11.1 GbGr\\Long Exposure (Linear) 參數說明
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

< 圖像調試視窗的 GbGr \\ Long Exposure (Linear) 頁面 1/2 >

|image179|

(1) **GbGr Balance Enable**：GbGr 平衡調整的功能開關。

   - **Enable**：開啟 GbGr 平衡調整。
   - **Disable**：關閉。

(2) **GbGr Balance Strength**：GbGr 平衡的整體強度控制。數值越大，修正幅度越大。

   .. tip::
      建議優先從這個配置強度開始調整。下圖為調整前後的效果對比（左：未校正，平坦區可見網格狀紋路；右：提升 Strength 後，網格雜訊明顯改善）。

   < GbGr Balance Strength 調整效果對比（左：調整前；右：調整後）>

   |image301|

(3) **Edge Detection Enable**：根據邊緣紋理特徵分區進行 GbGr 平衡處理的功能開關，主要目的是減少邊緣區域在 GbGr 處理後可能出現的副作用（Side Effect）。

   - **Enable**：開啟邊緣偵測分區處理。
   - **Disable**：關閉。

(4) **Edge Detection Debug**：將原始影像中的邊緣紋理分類結果標示於畫面上，便於判斷各區域的分類是否正確。

   - 紫色：藉由紋理強度判定的明顯邊緣區域。
   - 綠色：藉由紋理強度判定的細邊區域。
   - 紅色：藉由紋理強度判定的摩爾紋區域。

(5) **Edge Detection TH0**：第一個轉折點閾值。小於此值的區域以效果較強的方式進行 GbGr 平衡調整。

(6) **Edge Detection TH1**：第二個轉折點閾值。大於此值的區域（紫色）以效果最弱的方式進行 GbGr 平衡調整。

   .. note:: 調整 TH0 及 TH1，使明顯的邊緣區域呈現紫色；細微紋理（如草地等）可不標示為紫色。

(7) **Edge Detection M**：唯讀參數。

(8) **Thin Edge TH**：判斷細邊區域的閾值。大於此值的區域（綠色）以效果次弱的方式進行 GbGr 平衡調整。

(9) **Thin Edge Count TH**：細邊區域判斷的靈敏度控制條件。數值越大，條件越嚴格，越不易判斷為細邊區域。

(10) **Moire Protection Enable**：摩爾紋區域的保護機制開關。

   - **Enable**：摩爾紋區域（紅色）不套用 GbGr 平衡調整。
   - **Disable**：關閉保護。

(11) **Moire TH**：判斷摩爾紋區域的閾值。大於此值的區域（紅色）不進行 GbGr 平衡調整。

(12) **Moire Count TH**：摩爾紋區域判斷的靈敏度控制條件。數值越大，條件越嚴格，越不易判斷為摩爾紋區域。

(13) **CamVal TH Enable**：整體 GbGr 平衡修正量上限機制的開關。

   - **Enable**：以 CamVal TH 為修正量上限。
   - **Disable**：關閉上限限制。

(14) **CamVal TH**：整體 GbGr 平衡調整的修正量上限值。

(15) **MedianDiff TH Enable**：效果最強區域（Debug 圖中的黑色區域，即非 Edge / Thin Edge / Moire 區）的修正量上限機制開關。

   - **Enable**：以 MedianDiff TH 為修正量上限。
   - **Disable**：關閉上限限制。

(16) **MedianDiff TH**：非 Edge / Thin Edge / Moire 區域的 GbGr 平衡修正量上限值。

< 圖像調試視窗的 GbGr \\ Long Exposure (Linear) 頁面 2/2 >

|image180|

(1) **Draw Location Info Enable**：將 Distance Effect 的作用區域標示於畫面上，便於強度調整判別。

   - **Enable**：開啟標示。
   - **Disable**：關閉標示。

(2) **Location TH Enable**：距離判斷功能開關。

   - **Enable**：開啟距離判斷，依像素到畫面中心的距離調整 GbGr 邊緣紋理特徵閾值。
   - **Disable**：關閉距離判斷。

(3) **Location D0**：距離圖像中心小於此數值的像素，不加乘 GbGr 邊緣紋理特徵的判斷閾值。

(4) **Location D1**：距離圖像中心大於此數值的像素，套用 Location TH 的強度來加乘 GbGr 邊緣紋理特徵的判斷閾值。

(5) **Location TH**：GbGr 邊緣紋理特徵閾值的額外加乘設定，作用於距離圖像中心大於 Location D1 的區域。

(6) **Location Rate**：唯讀參數。

3.11.2 GbGr\\Short Exposure (HDR) 參數說明
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

< 圖像調試視窗的 GbGr \\ Short Exposure (HDR) 頁面 >

|image181|

本頁面適用於 **HDR 模式短曝幀**\ ，僅在 HDR 模式下有效。

(1) **GbGr Enable**：短曝幀的 GbGr 功能開關，僅在 HDR 模式下有效。

   - **Enable**：開啟 GbGr 平衡調整。
   - **Disable**：關閉。

(2) **GbGr Strength**：短曝幀 GbGr 的整體強度控制。數值越大，修正幅度越大。

(3) **GbGr Compensation Threshold**：短曝幀的補償上限控制，限制 GbGr 的最大補償強度。

.. _awb-module:

3.12 AWB
---------

AWB（Auto White Balance；自動白平衡）是 ISP 影像處理流程中的色彩校正模組，負責在不同光源環境下自動調整白平衡，使輸出影像的色調與被攝物體的真實色彩保持一致。

**功能定義**

透過合適的白平衡演算法參數設置，使得在場景變換或環境光源變化的情況下，影像輸出的色調仍能維持與原始物體色調一致。

**調校說明**

本模組的相關參數可依據不同的增益條件（Gain）進行獨立設定。

3.12.1 AWB\\General & Manual WB Attribute 參數說明
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

< 圖像調試視窗的 AWB \\ General & Manual WB Attribute 頁面 >

|image182|

(1) **Bypass**：ISP AWB 模組的總開關。

   - **Enable**：關閉 AWB 模組，不套用 ISP WB 增益。
   - **Disable**：啟用 AWB 模組，套用 ISP WB 增益。

(2) **Auto / Manual**：自動 / 手動白平衡的切換。

   - **Auto**：自動白平衡，由系統依場景自動計算 WB 增益。
   - **Manual**：手動白平衡，以 R Gain / G Gain / B Gain 欄位中的設定值作為白平衡參數。

(3) **R Gain**：手動白平衡時的 R 通道增益設定，單位為 1/256 倍。

(4) **G Gain**：手動白平衡時的 G 通道增益設定，單位為 1/256 倍。

(5) **B Gain**：手動白平衡時的 B 通道增益設定，單位為 1/256 倍。

   .. note:: G Gain 在絕大多數情況下應維持為 256，調整白平衡時優先調整 R Gain 與 B Gain。

.. _awb-attribute:

3.12.2 AWB Attribute 參數說明
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

本節包含 AWB 演算法的核心配置參數，分為三個群組：

- **(1)～(11)**：色溫作用區設定，定義 AWB 統計哪些像素、以及各色溫 block 的權重。
- **(12)～(14)**：WB 增益收斂行為，控制 Fine gain 的計算方式與收斂穩定條件。
- **(15)～(17)**：AWB HOLD 條件，防止在不穩定條件下錯誤更新白平衡。

< 圖像調試視窗的 AWB \\ AWB Attribute 頁面 >

|image183|

**色溫作用區設定**

(1) **Detail Adjustment**：點擊【Detail Adjustment】按鈕可開啟 AWB_Analyse 視窗，用於設定色溫作用區的範圍與標準色溫點位置。

< AWB_Analyse 視窗 >

|image184|

< CT_AREA 示意圖（``ct_point``\ 、``white_area``\ 、``gray_area``\ 與 ``ct_curve_fit_line`` 的位置關係） >

|image185|

AWB_Analyse 視窗各區塊說明：

- **左半部（座標平面）**：顯示色溫作用區各元素的空間位置（詳見上方 CT_AREA 示意圖）。

  - ``ct_point``\ （綠色方點）、``white_area_point``\ （紫色框，內側）、``gray_area_point``\ （藍色框，外側）均可拖曳調整；``ct_curve_fit_line``\ 則依 ``ct_point``\ 位置自動擬合產生，不可直接拖曳。
  - ISP 統計分析後，統計點須落在白區或灰區內才會納入 AWB 觸發與收斂運算；白區內統計點的權重高於灰區。
- **右上方**：【Start】/ 【Stop】按鈕，點擊【Start】可即時更新 AWB 各區塊的統計數值。
- **右側第一個表格**：AWB 運算結果，顯示最終 WB 增益（result_gain）與估算色溫值（color_temperature）。
- **右下方表格**：白色與灰色作用區的參數調整表格，可切換設定方式：

  - **IQ Data**：以座標平面上目前拖曳後的位置作為設定。
  - **Param**：以表格內填入的數值參數作為設定。

  .. note:: 建議初次調整先使用 **Param 模式**\ ，確認作用區大小適當並點擊【Write】生效；如需進一步微調，再切換至 **IQ Data 模式**\ 以拖曳方式調整各點位置，設定後同樣點擊【Write】生效。

**Param 模式下的作用區參數：**

< intp_param 調整效果比對（左：``intp_param = 0.08``\ ；``右：intp_param = 0.04``\ ）>

|image186|

- **intp_param**：當兩個相鄰 ``ct_point`` 之間距離較遠時，系統會自動在其間插入 ``area_point`` 以進行插值計算。此參數控制插入點的密集程度：數值越小，插入的 ``area_point`` 越密，對應的 block 數量越多。

  .. note:: 調整 ``intp_param`` 後，``ct_block_num`` 將隨之改變，需重新檢視 ``normal_weight``\  / ``h_ct_weight``\  / ``l_ct_weight`` 的設置是否正確。

- **length_inner_up**：以 ``ct_curve_fit_line`` 為起點，向上延伸至 ``white_area_point`` 的最大距離。數值越大，越多飽和度較高的顏色被納入 AWB 統計白區。
- **length_inner_down**：以 ``ct_curve_fit_line`` 為起點，向下延伸至 ``white_area_point`` 的最大距離。數值越大，越多飽和度較低的顏色被納入 AWB 統計白區。
- **length_outer_up**：以 ``ct_curve_fit_line`` 為起點，向上延伸至 ``gray_area_point`` 的最大距離。數值越大，越多飽和度較高的顏色被納入 AWB 統計灰區。須滿足 ``length_outer_up > length_inner_up``\ 。
- **length_outer_down**：以 ``ct_curve_fit_line`` 為起點，向下延伸至 ``gray_area_point`` 的最大距離。數值越大，越多飽和度較低的顏色被納入 AWB 統計灰區。須滿足 ``length_outer_down > length_inner_down``\ 。

.. note:: ``length_inner`` 系列（白區）與 ``length_outer`` 系列（灰區）的大小關係須嚴格維持：``length_outer_up`` > ``length_inner_up``\ 、``length_outer_down`` > ``length_inner_down``\ ，否則灰區邊界會落在白區內側，導致作用區定義異常。

< ``end_ratio`` 調整效果比對（左：``end_ratio`` = 0.5；右：``end_ratio`` = 1）>

|image187|

- **end_ratio**：高色溫端與低色溫端的 ``area_point`` 距離相對於中間色溫的比值，數值範圍為 0～1。數值越大，高 / 低色溫端作用區越寬，被納入統計的顏色範圍越廣。

(2) **ct_setting**：標準色溫點（ct_point）的相關設定。點擊【Detail Adjust】可開啟設定視窗。

< ct_setting Detail Adjust 視窗 >

|image188|

   - **Count of CT Points**：採用的標準色溫點數量，可依環境光源種類調整。
   - **CT (K)**：各標準色溫點對應的真實色溫值。
   - **X / Y**：各標準色溫點在 (R/G, B/G) 平面上的座標，建議透過 (1) Detail Adjustment 以拖曳方式調整。

(3) **ct_block_num**：色溫區域塊（block）的個數。建議由 (1) Detail Adjustment 的 intp_param 參數自動產生，避免手動調整。

(4) **a**、**(5) b**、**(6) c**：唯讀。色溫曲線（ct_curve_fit_line）的三個擬合係數。在 (1) Detail Adjustment 中調整座標位置並按下【Write】後會自動更新，不可手動修改。

(7) **white_area_up**：內部色溫區域（white_area）的上方邊界點座標陣列，對應座標平面上由左至右的邊界點位置。建議先透過 Param 模式自動產生，再以拖曳方式微調。

(8) **white_area_down**：內部色溫區域（white_area）的下方邊界點座標陣列。建議先透過 Param 模式自動產生，再以拖曳方式微調。

< white_area_up / white_area_down Detail Adjust 視窗 >

|image189|\ |image190|

(9) **gray_area_up**：外部色溫區域（gray_area）的上方邊界點座標陣列。建議先透過 Param 模式自動產生，再以拖曳方式微調。

(10) **gray_area_down**：外部色溫區域（gray_area）的下方邊界點座標陣列。建議先透過 Param 模式自動產生，再以拖曳方式微調。

< gray_area_up / gray_area_down Detail Adjust 視窗 >

|image191|\ |image192|

(11) **normal_weight**：各色溫 block 的統計權重陣列，包含 w_table（white_area 的內部 block 權重）與 g_table（gray_area 的外部 block 權重）。點擊【Detail Adjust】可開啟設定視窗。

< normal_weight Detail Adjust 視窗 >

|image193|

   .. note:: 調整 ``ct_block_num``\ （與 ``intp_param`` 相關）後，需重新檢視 ``normal_weight`` 設置是否正確。

**AWB 增益計算流程說明：**

AWB 最終輸出的 WB 增益（Final WB gain）由以下三個階段依序計算：

1. **Rough gain**：依據 ``white_area`` 與 ``gray_area`` 的統計點，以 ``normal_weight``\ （或 MIXCT weight）進行加權平均，得到 Rough WB_Gray。若啟用綠區功能（3.12.3），則同時計算 Rough WB_Green，最終 Rough gain 由兩者依比重混合決定。
2. **Fine gain**：以 Rough gain 的收斂位置為中心，對其周圍的統計點再次計算均值，得到更精準的修正量。Fine gain 的修正幅度受 ``fine_gain_th`` 限制。
3. **Final WB gain**：Rough gain 與 Fine gain 的乘積，作為最終套用至影像的 WB 增益。

**WB 增益收斂行為**

(12) **fine_gain_th**：Fine gain（``fine_tune`` 結果）與基準值 256 的差值容許閾值，控制 Fine gain 的修正幅度：

   - 差異 ≤ ``fine_gain_th``：直接以 Rough gain × ``fine_tune`` 結果作為新的 WB 增益。
   - 差異 > ``fine_gain_th``：將 ``fine_tune`` 結果限制在 256 ± ``fine_gain_th`` 範圍內後，再與 Rough gain 相乘。

(13) **final_gain_diff_th**：判斷 AWB 是否進入穩定狀態的差值閾值。若本幀 ``final_gain`` 與上一幀的差異低於此閾值，系統判定 AWB 已收斂，維持穩定狀態、不更新 WB 增益。進入穩定狀態後，判斷閾值會自動加寬為 ±（``final_gain_diff_th`` + magic num），提供遲滯保護，防止 AWB 頻繁切換；此 magic num 為韌體內部固定常數，不可調整。

(14) **white_pixels_ratio**：更新 Fine gain 所需的最低白點數目比值。若畫面中的白點比例低於此閾值，代表可供統計的白色區域不足，系統不執行 Fine gain 更新。

**AWB HOLD 條件**

以下兩個參數在特定條件下使 AWB 進入 HOLD 狀態，暫停 WB 增益更新，避免在不穩定的統計條件下錯誤修正白平衡：

(15) **awb_stable_delay**：AWB 收斂完成進入穩定狀態後，維持 HOLD 的持續時間，單位為毫秒。期間內即便環境光源改變，也不更新 WB 設定。

(16) **rg_bg_num_th**：色溫區域統計點數量的最低閾值。當統計點個數低於此值時，代表畫面中接近白色的區域不足，進入 ``AWB_HOLD`` 狀態。

(17) **min_bright_th**：畫面亮度的最低閾值。當 AE 統計後的畫面亮度低於此值時，代表環境過暗、AWB 統計資訊不可靠，進入 ``AWB_HOLD`` 狀態。

.. _awb-ext1:

3.12.3 AWB Attribute Extension 1 參數說明 - 綠區功能
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

當拍攝場景中含有大面積綠色物體（如植物、草地、樹木）時，大量綠色像素可能使 AWB 誤判環境光源色溫，導致白平衡偏移。綠區（Green Area）功能透過獨立偵測畫面中的綠色區域，計算出另一組 Rough WB_Green，再與正常灰白區的 Rough WB_Gray 依比重混合，使最終白平衡不受大面積綠色干擾。

綠區功能的調校依以下順序進行：

1. **規範綠區範圍**：設定 ``x_min`` / ``x_max`` / ``y_min`` / ``y_max`` / ``b_delta``\ ，定義 (R/G, B/G) 平面上的綠色像素偵測邊界。
2. **規範 D50 / D65 基準色溫點**：設定 ``d50`` / ``d65``\ ，作為估算灰點線段位置的依據。建議直接從 ``ct_setting`` 的對應色溫點座標取得。
3. **規範灰點估計線段的投影關係**：設定 ``k_green_est`` 與 d→b 映射參數（``d_min`` / ``d_def`` / ``d_max``；``b_max`` / ``b_def`` / ``b_min``\ ），決定 Rough WB_Green 的收斂位置。

**頁面 1/2 — 綠區觸發條件與基準色溫點**

< 圖像調試視窗的 AWB \\ AWB Attribute Extension 1 頁面 1/2 >

|image194|

(1) **enable**：綠區偵測及對應白平衡功能的開關。

   - **Enable**：開啟綠區功能。
   - **Disable**：關閉。

(2) **gray_thd**：色溫區域中灰點個數的判斷閾值，控制進入 ``AWB_GREEN_MODE`` 的條件鬆緊：

   - 灰點數量 > ``gray_thd``\ ：綠區點數須大於 ``enter_green_thd`` 才可進入 ``AWB_GREEN_MODE``\ （條件較嚴）。
   - 灰點數量 ≤ ``gray_thd``\ ：綠區點數只需大於 ``rg_bg_num_th`` 即可進入 ``AWB_GREEN_MODE``\ （條件較寬）。

(3) **enter_green_thd**：當灰點數量大於 ``gray_thd`` 時，進入 ``AWB_GREEN_MODE`` 所需的最低綠區點數閾值。

(4) **exit_green_thd**：處於 ``AWB_GREEN_MODE`` 時，退出此狀態所需的綠區點數下限。綠區點數低於此值時，系統退出綠區模式。

(5) **d50**：綠區計算所需的 D50 色溫基準灰點座標（R/G, B/G 平面）。若標準色溫點設定中有 D50 燈源，可從 AWB Attribute 的 ``ct`` 欄位取得對應座標。點擊【Detail Adjust】可開啟設定視窗。

(6) **d65**：綠區計算所需的 D65 色溫基準灰點座標。設定方式同 ``d50``\ 。點擊【Detail Adjust】可開啟設定視窗。

< d50 / d65 Detail Adjust 視窗 >

|image195|\ |image196|

**頁面 2/2 — 綠區邊界與收斂映射參數**

< 圖像調試視窗的 AWB \\ AWB Attribute Extension 1 頁面 2/2 >

|image197|

以下 (1)～(5) 定義綠區在 (R/G, B/G) 平面上的邊界範圍。

< 綠區邊界示意圖（``x_min`` / ``x_max`` / ``y_min`` / ``y_max`` / ``b_delta``\ ）>

|image198|

(1) **x_min**：綠區邊界 R/G 的最小值。

(2) **y_min**：綠區邊界 B/G 的最小值。

(3) **x_max**：綠區邊界 R/G 的最大值。

(4) **y_max**：綠區邊界 B/G 的最大值。

(5) **b_delta**：綠區梯形斜邊相對於 D50–D65 連線的截距偏移量。透過調整 ``b_delta``\ ，可使綠區邊界與色溫區域保持適當間距，避免兩者重疊。

以下 (6)～(7) 控制綠區均值點投影至估計灰點線段的方式，決定 Rough WB_Green 的計算位置。

**綠區 Rough WB_Green 計算原理**：系統在 (R/G, B/G) 平面上估算一條代表灰點所在的線段，該線段的斜率固定為 ``k_gray_est``\ （由 ``d50`` 與 ``d65`` 自動計算），截距 b 由 d（均值點到色溫曲線的距離）與 b 的映射關係（參數 (8)～(13)）決定。綠區均值點依 ``k_green_est`` 設定的斜率投影至此線段，投影位置即為 Rough WB_Green。

< 綠區參數示意圖（``k_green_est``\ 、d、b 與 Rough WB_Green 的關係）>

|image199|

(6) **k_green_est**：綠區均值點投影至灰點估計線段時所使用的斜率，數值建議設定在 0.5～1.5 之間。

   - 數值越大：投影位置偏向高色溫方向，白平衡含較多黃色成分。
   - 數值越小：投影位置偏向低色溫方向，白平衡含較少黃色成分。

(7) **k_gray_est**：唯讀。``d50`` 與 ``d65`` 標準色溫點之間的斜率，依 ``d50`` 與 ``d65`` 的設定自動計算，不可手動修改。

以下 (8)～(13) 控制綠區均值點到色溫曲線距離 d 與灰點線段截距 b 之間的映射關係，決定 Rough WB_Green 的收斂位置偏高色溫還是低色溫。

(8) **d_min**：綠區統計均值點到色溫曲線（``ct_curve_fit_line``\ ）距離 d 的最小值。

(9) **d_def**：在 D50 和 D65 色溫下，以 24 色標準色卡（24-patch colorchecker）第 14 個色塊（綠色）作為綠區均值點，該點到色溫曲線的距離，作為 d 的參考基準值。

(10) **d_max**：唯讀。綠區統計均值點到色溫曲線距離 d 的最大值，依 ``x_min`` 與 ``y_min`` 設定自動計算。

(11) **b_max**：d 為最小值（``d_min``\ ）時，對應的灰點線段截距。數值越大，收斂偏高色溫，白平衡含較多黃色成分。

(12) **b_def**：d 為基準值（``d_def``\ ）時，對應的灰點線段截距。

(13) **b_min**：d 為最大值（``d_max``\ ）時，對應的灰點線段截距。數值越大，收斂偏低色溫，白平衡含較少黃色成分。

.. note:: Rough gain 的最終結果由 Rough WB_Green 與 Rough WB_Gray 依綠區和灰/白區的統計比重混合決定：綠區統計資料越多，Rough WB_Green 的比重越高；反之，以 Rough WB_Gray 為主。

.. _awb-ext2:

3.12.4 AWB Attribute Extension 2 參數說明 - 混合色溫功能
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

當場景中同時存在高色溫（如日光燈）與低色溫（如白熾燈）光源時，AWB 的收斂方向容易在兩種色溫之間來回震盪。混合色溫（AWB_MIXCT）功能透過持續監控高色溫與低色溫統計點的比例，自動調整 AWB 統計的權重，使 AWB 穩定收斂於場景中佔比較大的光源色溫。

**功能定義**

AWB_MIXCT 以 **兩層結構** 運作：

**外層：AWB_MIXCT_NONE ↔ AWB_MIXCT_MODE**

- **AWB_MIXCT_NONE**：功能未啟用，或高低色溫統計點數均未達門檻（``enter_num_th``\ ）。AWB 使用 ``normal_weight``\ ，MIXCT 狀態機不執行。
- **AWB_MIXCT_MODE**：功能啟用且統計點數超過門檻後進入。系統持續監控高低色溫比值，並根據比值在以下兩個子模式之間切換。

**內層（AWB_MIXCT_MODE 內的子模式）**

- **高色溫模式**：改用 ``h_ct_weight`` 對各 ``ct_block`` 加權，強化高色溫區域的統計影響，使 AWB 收斂至高色溫方向。
- **低色溫模式**：改用 ``l_ct_weight`` 對各 ``ct_block`` 加權，強化低色溫區域的統計影響，使 AWB 收斂至低色溫方向。

狀態切換的依據為 **比值**\ （= 高色溫區域統計點數 ÷ 低色溫區域統計點數），詳見下方示意圖。

< AWB_MIXCT 狀態轉換示意圖 >

|image204|

.. note::
   「比值」= ``ct_block`` index 小於等於 ``h_ct_index`` 的統計點數（高色溫區域），除以 ``ct_block`` index 大於等於 ``l_ct_index`` 的統計點數（低色溫區域）。比值越大，代表場景中高色溫光源的佔比越高。

**調校說明**

< 圖像調試視窗的 AWB \\ AWB Attribute Extension 2 頁面 >

|image200|

**(1) 啟用開關**

(1) **enable**：混合色溫功能的開關。

   - **Enable**：開啟混合色溫偵測功能。高低色溫統計點數超過 ``enter_num_th`` 後，系統進入 ``AWB_MIXCT_MODE``\ 。
   - **Disable**：關閉，``AWB_MIXCT`` 狀態機不執行，AWB 始終以 ``normal_weight`` 統計所有色溫區域。

**(2)(3) 高低色溫區域定義**

(2) **h_ct_index**：``ct_block`` 陣列中， index 小於等於 ``h_ct_index`` 的 block 定義為高色溫區域（``ct_block`` index 越小，對應的色溫 K 值越高）。

(3) **l_ct_index**：``ct_block`` 陣列中， index 大於等於 ``l_ct_index`` 的 block 定義為低色溫區域（``ct_block`` index 越大，對應的色溫 K 值越低）。

   須滿足：``h_ct_index`` < ``l_ct_index``\ 。

**(4) 統計點數門檻**

(4) **enter_num_th**：高色溫或低色溫區域的統計點數需超過此閾值，才進行混合色溫判斷，以避免統計點數過少時誤觸發。

**(5)～(8) AWB_MIXCT_NONE ↔ AWB_MIXCT_MODE 進入／退出條件**

以下四個參數透過遲滯區間設計，控制系統進出 AWB_MIXCT_MODE：

(5) **enter_th_l**：進入 AWB_MIXCT_MODE 的比值下限。在 NONE 狀態下，比值需大於 ``enter_th_l`` 才可能進入。

(6) **enter_th_h**：進入 AWB_MIXCT_MODE 的比值上限。在 NONE 狀態下，比值需小於 ``enter_th_h`` 才可能進入。

   當 ``enter_th_l`` < 比值 < ``enter_th_h`` 時，進入 ``AWB_MIXCT_MODE``\ ；進入後再依 ``enter_h_ct_th`` 決定子模式（高或低色溫模式）。

(7) **exit_th_l**：退出 ``AWB_MIXCT_MODE`` 的比值下限。在 ``AWB_MIXCT_MODE`` 下，若比值 < ``exit_th_l``\ ，則退回 ``AWB_MIXCT_NONE``\ 。

(8) **exit_th_h**：退出 ``AWB_MIXCT_MODE`` 的比值上限。在 ``AWB_MIXCT_MODE`` 下，若比值 > ``exit_th_h``\ ，則退回 ``AWB_MIXCT_NONE``\ 。

   .. note::
      進入與退出條件須形成遲滯區間，請確保：

      - ``exit_th_l`` < ``enter_th_l``\ （退出下限小於進入下限）
      - ``enter_th_h`` < ``exit_th_h``\ （進入上限小於退出上限）

**(9)～(11) 高色溫模式 ↔ 低色溫模式 切換條件**

進入 AWB_MIXCT_MODE 時，以及穩定運作期間，以下三個參數決定子模式的切換：

(9) **enter_h_ct_th**：進入 AWB_MIXCT_MODE 時的子模式判斷閾值。若比值 > enter_h_ct_th，則進入高色溫模式；否則進入低色溫模式。

(10) **exit_h_ct_th**：穩定處於高色溫模式時，若比值下降至 < exit_h_ct_th，表示高色溫光源比例減弱，切換至低色溫模式。數值設定越小，代表越不容易切換至低色溫模式。

(11) **exit_l_ct_th**：穩定處於低色溫模式時，若比值上升至 > exit_l_ct_th，表示高色溫光源比例增強，切換至高色溫模式。數值設定越大，代表越不容易切換至高色溫模式。

   .. note::
      高低色溫子模式切換閾值與 NONE/MODE 退出閾值須滿足以下關係：

      - ``exit_th_l`` < ``exit_h_ct_th`` < ``exit_l_ct_th`` < ``exit_th_h``

**(12)(13) 各子模式統計權重**

(12) **h_ct_weight**：高色溫模式下各 ``ct_block`` 的統計權重（``w_table`` / ``g_table``\ ）。應對高色溫區域的 block 設定較高權重，使 AWB 收斂至高色溫方向。點擊【Detail Adjust】可開啟設定視窗。

< ``h_ct_weight`` Detail Adjust 視窗 >

|image201|

(13) **l_ct_weight**：低色溫模式下各 ``ct_block`` 的統計權重（``w_table`` / ``g_table``\ ）。應對低色溫區域的 block 設定較高權重，使 AWB 收斂至低色溫方向。點擊【Detail Adjust】可開啟設定視窗。

< l_ct_weight Detail Adjust 視窗 >

|image202|

   .. note:: 調整 ``ct_block_num``\ （與 ``intp_param`` 相關）後，需重新檢視 ``h_ct_weight`` 與 ``l_ct_weight`` 的設置是否正確。

3.13 AWB Gain Adjust
---------------------

AWB Gain Adjust（自動白平衡增益調整）以 AWB 演算法計算所得的白平衡增益為基礎，允許針對 R 與 B 成分進行個人化色調微調，而不影響 AWB 的自動收斂行為。

**功能定義**

本模塊在 AWB 輸出的 WB 增益之上，額外乘上 R / B 調整係數，使畫面色調偏向使用者偏好而不改變 AWB 的統計與收斂邏輯。

**調校說明**

本模塊的參數可依色溫或增益條件給予不同設定。

< 圖像調試視窗的 AWB Gain Adjust 頁面 >

|image205|

(1) **R Gain Adjust**：R 成分的增益調整設定，基準值為 256（不調整）。

   - 數值調大：R 成分上升，畫面呈現偏紅。
   - 數值調小：R 成分下降，畫面呈現偏青。

(2) **B Gain Adjust**：B 成分的增益調整設定，基準值為 256（不調整）。

   - 數值調大：B 成分上升，畫面呈現偏藍。
   - 數值調小：B 成分下降，畫面呈現偏黃。

   .. tip::
      建議謹慎使用，會影響整體風格。此模組允許不同色溫區與不同增益區間做不同調整。

< AWB Gain Adjust 調整效果示意圖（左：基準值 R=256, B=256；右上：調大 R Gain；右下：調大 B Gain）>

|image302|

.. _cac-module:

3.14 CAC
---------

CAC（Chroma Aberration Compensation；色差補償）針對鏡頭色差造成的紋理邊緣色邊（常見為紫邊或綠邊）進行補償。

**功能定義**

鏡頭對不同波長的光線折射率略有差異，導致各色光的聚焦位置不完全重合，在高對比邊緣產生色邊。CAC 模塊以三個維度判定補償區域：亮度（Edge Map）、紋理邊緣特徵（Edge Filter）及彩度（Saturation Map），再以可控比例將原始圖像與補償後圖像混合，達到可調強度的色差補償效果。

**調校說明**

本模塊屬於 Texture 類別，可依增益條件給予不同設定。

**補償開關與混合強度**

< 圖像調試視窗的 CAC 頁面 1/3 >

|image206|

(1) **CAC Enable**：功能開關。Enable 表示啟動色差補償；Disable 表示關閉。

(2) **Blending Rate Debug Mode**：以灰階顯示原始圖像與補償圖像的混合強度分布，強到弱對應白至黑，供觀察補償效果的作用範圍。

(3) **Progressive Enable**：補償混合強度的決定方式。

   - **Enable**：依各像素的邊緣特徵與彩度動態決定混合強度。
   - **Disable**：全畫面採用固定的 Manual Blending Rate 強度。

(4) **Manual Blending Rate**：Progressive Enable 為 Disable 時的全域固定混合強度。

**邊緣作用區域設定**

< 圖像調試視窗的 CAC 頁面 2/3 >

|image207|

(1) **Edge Map Debug Mode**：以灰階顯示以亮度與紋理特徵判定的 CAC 邊緣作用區域，強到弱對應白至黑。

(2) **Edge Filter Window Size**：紋理邊緣濾波器大小，可選值為 15 / 13 / 11 / 9。值越大，偵測紋理變化的範圍越寬；值越小，可偵測更細緻的紋理變化。可調整的 Coef 個數隨 Window Size 而異。

(3)～(10) **Edge Filter Coef 0～7**：Edge Filter 的各係數。``Coef 0`` 為中心係數，編號越大代表越邊緣位置的係數，建議維持 ``Coef 0`` ≥ ``Coef 1`` ≥ ... ≥ ``Coef 7``\ 。

(11)～(13) **Y Calculation Coef R / G / B Weighting**：計算亮度時各成分的權重，三者之和須等於 16。

(14)～(22) **Edge Y Thd0**：邊緣判斷第一個轉折點的閾值，共九組，分別對應不同亮度條件。

(23) **Edge Thd Offset**：第一轉折點（``Thd0``\ ）到第二轉折點（``Thd1``\ ）的差值，即 ``Thd1 = Thd0 + Edge Thd Offset``\ 。三個區間的作用如下：

   - 邊緣特徵值 < ``Thd0``\ ：不在 CAC 作用區域內，不補償。
   - ``Thd0`` ≤ 邊緣特徵值 ≤ ``Thd1``\ ：補償強度依插值漸進增強。
   - 邊緣特徵值 > ``Thd1``\ ：以最強 CAC 強度補償。

**彩度作用區域與補償來源**

< 圖像調試視窗的 CAC 頁面 3/3 >

|image208|

(1) **Saturation Map Debug**：以灰階顯示以彩度判定的 CAC 作用區域，強到弱對應白至黑。

(2) **Saturation Thd0**：彩度判斷第一個轉折點的閾值。

(3) **Saturation Thd1**：彩度判斷第二個轉折點的閾值。三個區間的作用如下：

   - 彩度 < ``Thd0``\ ：不在 CAC 作用區域內，不補償。
   - ``Thd0`` ≤ 彩度 ≤ ``Thd1``\ ：補償強度依插值漸進增強。
   - 彩度 > ``Thd1``\ ：以最強 CAC 強度補償。

(4) **Blending Source**：CAC 補償圖像的來源，補償效果由強至弱：

   - **Gray**：使用灰階圖像，效果最顯著。
   - **Low Saturation - Gray Blending**：混合低彩度與灰階圖像，效果居中。
   - **Low Saturation**：使用低彩度圖像，效果最輕微。

(5) **Low Saturation - Gray Ratio**：選擇 Low Saturation - Gray Blending 時，調整低彩度圖像與灰階圖像的混合比例。偏向 Low Sat. 表示採用較多低彩度圖像；偏向 Gray 表示採用較多灰階圖像。

.. _fcr-mcr-uvs:

3.15 FCR & MCR & UVS
----------------------

本章節涵蓋三個色彩雜訊抑制模塊：FCR（偽彩抑制）、MCR（摩爾紋色彩抑制）與 UVS（UV 顏色抑制）。三者均屬於 Texture 類別，可依增益條件給予不同設定。

.. _fcr-module:

3.15.1 FCR 偽彩抑制
^^^^^^^^^^^^^^^^^^^^

FCR（False Color Reduction；偽彩抑制）抑制圖像紋理邊緣及平坦區域因 ISP 處理所產生的偽彩現象。

**功能定義**

ISP 處理過程中，EEH（邊緣強化）與 INTP（色彩內插）可能在紋理邊緣與平坦區域引入偽彩。FCR 針對這兩個來源分別提供強度可調的抑制功能。

**調校說明**

本模塊屬於 Texture 類別，可依增益條件給予不同設定。

< 圖像調試視窗的 FCR 頁面 >

|image209|

(1) **EEH Reduction Enable**：EEH 模塊的偽彩抑制開關。Enable 表示啟動；Disable 表示關閉。

(2) **EEH Reduction Strength**：EEH 偽彩抑制強度，作用於圖像紋理邊緣區域，越接近 Strong 抑制越強。

   .. tip::
      建議優先從 EEH Reduction Strength 的強度開始調整。下圖為 FCR 啟用前後的效果對比，可觀察不同區域的偽彩改善情形。

< FCR 啟用效果對比（各局部放大區塊：左為 Enable，右為 Disable）>

|image303|

(3) **INTP Log Enable**：INTP 模塊的偽彩抑制開關。Enable 表示啟動；Disable 表示關閉。

(4) **INTP FCR Texture**：INTP 模塊針對紋理區域的偽彩抑制強度，越接近 Strong 抑制越強。

(5) **INTP FCR Flat**：INTP 模塊針對平坦區域的偽彩抑制強度，越接近 Strong 抑制越強。

.. _mcr-module:

3.15.2 MCR 摩爾紋色彩抑制
^^^^^^^^^^^^^^^^^^^^^^^^^^

MCR（Moire Color Reduction；摩爾紋色彩抑制）針對圖像中摩爾紋區域的偽色現象進行抑制。

**功能定義**

拍攝週期性細紋圖案（如布料、百葉窗）時，感測器採樣頻率與圖案空間頻率產生干涉，形成帶有偽色的摩爾紋。MCR 以紋理特徵與彩度兩組條件判定摩爾紋區域，再使該區域的 R / G / B 三成分強度趨於一致，消除偽色。作用範圍僅限摩爾紋判定區域，不影響其他區域的色彩表現。

**調校說明**

本模塊屬於 Texture 類別，可依增益條件給予不同設定。

< 圖像調試視窗的 MCR 頁面 >

|image210|

(1) **Moire Condition Enable**：功能開關。Enable 表示啟動摩爾紋抑制；Disable 表示關閉。

**紋理判定條件**

(2) **Moire Condition TH**：紋理閾值。閾值越低，越容易將像素判定為摩爾紋；閾值越高，判定越嚴格。

(3) **Moire Condition Offset**：方向性判斷的基底值。數值越高，越難將像素定義為具有方向性。

(4) **Moire Condition Slope**：方向性判斷的斜率值。數值越高，越難將像素定義為具有方向性。

**彩度判定條件與修正幅度**

(5) **Moire Saturation Limitation**：納入摩爾紋判定的彩度上限。設定過高時，高彩度的正常色彩區域可能被誤判；建議保守設定。

(6) **Moire Revised Amplitude**：R / G / B 三成分每次的修正幅度。數值越大，各成分強度趨近越快；設定過大可能造成偏色。

(7) **Moire Revised Limitation**：R / G / B 三成分的修正量上限。數值越大，可抑制越明顯的偽色。

**計算結果（唯讀）**

(8) **MCR R Rate**：MCR 計算所得的 R 成分修正量。

(9) **MCR B Rate**：MCR 計算所得的 B 成分修正量。

.. _uvs-module:

3.15.3 UVS UV 顏色抑制
^^^^^^^^^^^^^^^^^^^^^^^^

UVS（UV Suppress；UV 顏色抑制）針對低飽和度區域的色彩噪點進行抑制，同時保留高飽和度區域的色彩表現。

**功能定義**

在低照度或高增益情境下，低飽和度區域容易出現 UV 色彩噪點（彩噪）。UVS 以飽和度為條件，在低飽和度區域套用 UV 抑制使噪點趨近灰階，高飽和度區域不受影響。

**調校說明**

本模塊屬於 Texture 類別，可依增益條件給予不同設定。

< 圖像調試視窗的 UVS 頁面 >

|image211|

**抑制強度**

(1) **UVS Rate**：對飽和度低於 ``thd0`` 區域套用的抑制強度。數值越大強度越強；設定至最大時，效果等同灰階。

**作用區域閾值**

``thd0`` / ``thd1`` 在 UV 平面上分別對應內外圈邊界，如下圖所示：

< UVS ``thd0`` / ``thd1`` 對應的內外圈示意 >

|image212|

(2) **UVS thd0**：第一個轉折點（內圈邊界）。飽和度低於此值的區域套用完整的 UVS Rate 強度。

(3) **UVS thd1**：第二個轉折點（外圈邊界）。飽和度高於此值的區域完全不套用 UVS；介於 ``thd0``\ ～ ``thd1`` 之間時，強度以插值漸進過渡。

**計算結果（唯讀）**

(4) **UVS Slope**： ``thd0`` 至 ``thd1`` 之間的插值斜率，由系統自動計算。

下圖為調整 UVS Rate 前後的效果對比，顯示低飽和度區域的彩噪在套用 UVS 後明顯改善。

< UVS Rate 調整效果對比（各局部放大區塊：左為 UVS Rate=0，右為 UVS Rate=16；須滿足 UVS ``thd1`` > UVS ``thd0``）>

|image304|

3.16 UV Color Tune
-------------------

UV Color Tune（UV 顏色調整）針對單一色系進行個別的飽和度或色相調整，可在不影響其他顏色表現的前提下，修改特定色域的色彩呈現。

**功能定義**

UV Color Tune 在 YUV 色彩空間的 UV 平面上，將色彩空間切分為 16 個區域，每個區域以一個可拖曳的白點獨立控制。移動白點可改變該色域的飽和度（距中心距離）與色相（角度）。

**調校說明**

本模塊的參數可依色溫或增益條件給予不同設定。

< 圖像調試視窗的 UV Color Tune 頁面 >

|image213|

(1) **UV Color Tune 控制點**：UV 平面圖上共 16 個白點，每個白點對應一個色域，直接拖曳進行調整：

   - **向外拖動**：該色域飽和度增加。
   - **向內拖動**：該色域飽和度降低。
   - **順時針旋轉**：色相往低色相方向偏移（例如：紫色→藍色）。
   - **逆時針旋轉**：色相往高色相方向偏移（例如：紫色→紅色）。

(2) **Reset**：回復至無調整的初始狀態。

(3) **復原鍵**：回復至上一次的調整狀態。

(4) **Get ROI / Release ROI**：按下 Get ROI 後，Preview 畫面出現可拖動的紫色方框，框選欲觀察的顏色區域；確認後按 Release ROI 回覆待機，可繼續框選其他區域。

(5) **Matting**：進入 Get ROI 狀態後可用。按下後，框選 ROI 的原始顏色以紅點標示於 UV 平面（Source Image），調整後的顏色以藍點標示（Tuned Image）。下圖為將藍色轉換為綠色的操作示意。

< UV Color Tune Matting 示意圖（藍色→綠色轉換） >

|image214|

.. note:: Matting 功能為模擬顯示，需按下右上方的【Write】後才會實際生效。

此一功能能夠對單一色塊做單獨的色彩校正，但色相的強制改變會對圖像產生破壞性的修正，建議盡量以 CCM 或 AWB 等方式進行色彩校正，UV Color Tune 僅作為輔助。

3.17 UV Offset Tune
--------------------

UV Offset Tune（UV 顏色位移調整）對全畫面的 U / V 成分施加固定偏移，使整體色調朝特定方向偏移，適合畫面色調的全局微調。

**功能定義**

UV Offset Tune 在 YUV 色彩空間中對 U 或 V 分量加上固定偏移量。與 UV Color Tune 的單色系調整不同，本模塊的調整作用於所有顏色。

**調校說明**

本模塊的參數可依色溫條件給予不同設定。

< 圖像調試視窗的 UV Offset Tune 頁面 >

|image215|

(1) **U Offset**：U 成分的位移量，基準值為 0（不調整）。數值越大畫面越偏藍；數值越小畫面越偏黃。

(2) **V Offset**：V 成分的位移量，基準值為 0（不調整）。數值越大畫面越偏紅；數值越小畫面越偏青。

.. _dpc-module:

3.18 DPC 壞點補償
------------------

DPC（Defect Pixel Compensation；壞點補償）針對影像感測器的像素缺陷進行即時補強，修正異常過亮或過暗的像素，同時兼顧紋理與細節的保留。

**功能定義**

影像感測器上的部分像素可能因製程缺陷或老化而輸出異常值（亮壞點或暗壞點）。DPC 模塊以周圍像素的統計值為參考，自動替換異常像素值，並可依場景亮度分別控制補償強度，確保影像內容完整性。DPC 可即時動態處理壞點，包含單一 Bayer Channel 中的連續壞點；建議依照場景亮度情況調整強度，例如低照度場景可搭配較強的補償設定。

**調校說明**

本模塊屬於 Texture 類別，可依增益條件給予不同設定。

DPC 模塊分為兩個參數頁面，分別對應不同的曝光路徑：

- :ref:`3.18.1 Long Exposure Path <dpc-long>`：適用於 Linear mode 及 HDR mode 長曝幀。
- :ref:`3.18.2 Short Exposure Path <dpc-short>`：僅適用於 HDR mode 短曝幀。

.. _dpc-long:

3.18.1 DPC\\Long Exposure Path（Main Path）
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

本頁面控制 Linear mode 及 HDR mode 長曝幀的 DPC 壞點補償參數。

< 圖像調試視窗的 DPC \\ Long Exposure Path (Main Path) 頁面 >

|image216|

(1) **DPC Enable**：功能開關。Enable 表示開啟；Disable 表示關閉。

(2) **DPC Type**：壞點補償類型。

   - **Single**：單一壞點補償模式，涵蓋 Single Defect 及 Serial Defect 類型。
   - **Multiple**：連續壞點補償模式，涵蓋 Jumped Defect 及 Cluster Defect 類型。

   .. note:: Multiple（Cluster Defect）補償會大量損失細節，建議優先由 Sensor 端把關。實務上僅在極高增益（約 256x 以上）時才啟用，以緩解 impulse noise。

(3) **Process Type**：壞點偵測的處理類別。

   - **Normal**：一般處理模式，能偵測大部分壞點。
   - **Keep Detail**：具備較強的細節保留能力，但部分分布類型的壞點可能無法成功偵測。

(4) **Write Back Mode**：Enable 時可獲得較強的補點效果。

**Bright Defect Strength（亮壞點補償強度）**

可分別針對暗處區域與亮處區域設定亮壞點的補償強度：

(5) **Dark Tone**：亮壞點位於暗處區域的補償強度。越接近 Strong，越容易判定為壞點、補點強度越強。

(6) **Bright Tone**：亮壞點位於亮處區域的補償強度。越接近 Strong，越容易判定為壞點、補點強度越強。

(7) **Reserve Texture**：補點區域的紋理與細節保留程度。越接近 As Less，保留越少、補點強度越強。

**Dark Defect Strength（暗壞點補償強度）**

可分別針對暗處區域與亮處區域設定暗壞點的補償強度：

(8) **Dark Tone**：暗壞點位於暗處區域的補償強度。越接近 Strong，越容易判定為壞點、補點強度越強。

(9) **Bright Tone**：暗壞點位於亮處區域的補償強度。越接近 Strong，越容易判定為壞點、補點強度越強。

(10) **Reserve Texture**：補點區域的紋理與細節保留程度。越接近 As Less，保留越少、補點強度越強。

(11) **Dark/Bright Tone Boundary**：暗處區域與亮處區域的亮度分界值。

.. _dpc-short:

3.18.2 DPC\\Short Exposure Path
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

本頁面控制 HDR mode 短曝幀的 DPC 壞點補償參數。與 3.18.1 相比，本頁面不含 Write Back Mode 參數。

< 圖像調試視窗的 DPC \\ Short Exposure Path 頁面 >

|image217|

(1) **DPC Enable**：功能開關。Enable 表示開啟；Disable 表示關閉。

(2) **DPC Type**：壞點補償類型。

   - **Single**：單一壞點補償模式，涵蓋 Single Defect 及 Serial Defect 類型。
   - **Multiple**：連續壞點補償模式，涵蓋 Jumped Defect 及 Cluster Defect 類型。

   .. note:: Multiple（Cluster Defect）補償會大量損失細節，建議優先由 Sensor 端把關。實務上僅在極高增益（約 256x 以上）時才啟用，以緩解 impulse noise。

(3) **Process Type**：壞點偵測的處理類別。

   - **Normal**：一般處理模式，能偵測大部分壞點。
   - **Keep Detail**：具備較強的細節保留能力，但部分分布類型的壞點可能無法成功偵測。

**Bright Defect Strength（亮壞點補償強度）**

(4) **Dark Tone**：亮壞點位於暗處區域的補償強度。越接近 Strong，越容易判定為壞點、補點強度越強。

(5) **Bright Tone**：亮壞點位於亮處區域的補償強度。越接近 Strong，越容易判定為壞點、補點強度越強。

(6) **Reserve Texture**：補點區域的紋理與細節保留程度。越接近 As Less，保留越少、補點強度越強。

**Dark Defect Strength（暗壞點補償強度）**

(7) **Dark Tone**：暗壞點位於暗處區域的補償強度。越接近 Strong，越容易判定為壞點、補點強度越強。

(8) **Bright Tone**：暗壞點位於亮處區域的補償強度。越接近 Strong，越容易判定為壞點、補點強度越強。

(9) **Reserve Texture**：補點區域的紋理與細節保留程度。越接近 As Less，保留越少、補點強度越強。

(10) **Dark/Bright Tone Boundary**：暗處區域與亮處區域的亮度分界值。

.. _intp-module:

3.19 INTP 色彩內插（去馬賽克）
--------------------------------

INTP（Color Interpolation；Demosaic；色彩內插；去馬賽克）從 Bayer 原始資料重建每個像素的完整 RGB 值，是全彩圖像輸出的基礎處理步驟。

**功能定義**

影像感測器採用 Bayer 排列，每個像素僅感測單一色彩通道（R、G 或 B）。INTP 模塊依據周圍像素的色彩資訊，推算各像素缺失的兩個色彩通道，重建完整的全彩圖像。

重建流程分為三個階段，各對應一個參數頁面：

1. **Feature Detection**：分析圖像中各區域的特徵類型（Moire、Thin Edge、Edge、Texture、Flat Area 等），作為後續內插演算法的選擇依據。
2. **Interpolation**：依據特徵分類結果，為各區域選用相應的內插演算法，並設定銳化強度。
3. **Denoise**：對內插結果執行去噪處理，抑制內插過程引入的噪聲。

**調校說明**

本模塊屬於 Texture 類別，可依增益條件給予不同設定；vHDR mode 下，部分參數可依曝光比給予不同設定。調校建議依 Feature Detection → Interpolation → Denoise 的順序進行，確認每個階段的結果符合預期後再進行下一步。

3.19.1 INTP\\Feature Detection 頁面
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Feature Detection 頁面設定各類特徵的判定條件，決定圖像中每個區域的特徵類型。

< 圖像調試視窗的 INTP \\ Feature Detection 頁面 >

|image218|

(1) **Feature Detection Debug Mode**：以色彩疊加方式顯示各像素的特徵分類結果，用於確認各區域的特徵類型是否符合預期，各顏色對應的類別如下圖所示。

< Feature Detection Debug Mode 各類別對照 >

|image219|

(2) **Flat Prior to Texture Enable**：平坦區優先判定的開關。

   - **Enable**：啟用平坦區與紋理區的進一步區分，依 Flat Prior to Texture TH 判定。以下 (3)～(12) 僅在此模式下有效。
   - **Disable**：不區分平坦區與紋理區，全部歸類為紋理區。

(3) **Flat Prior to Texture TH**：平坦區的判定閾值。特徵值低於此閾值的像素歸類為平坦區；高於此閾值的像素歸類為紋理區。vHDR mode 下可依曝光比給予不同設定。

**Distance Effect 參數群（(4)～(9)）**

依像素距圖像中心的距離，對平坦區判定閾值進行加乘，使圖像邊緣區域的像素更容易被歸類為平坦區。加乘強度依距離分為三個區間：

- 距中心距離 < ``thd0``\ ：不加乘，維持原始閾值。
- 距中心距離介於 ``thd0～thd1``\ ：漸進加乘，強度由 Distance Effect Rate 決定。
- 距中心距離 > ``thd1``\ ：套用 Distance Effect Max 的最大加乘強度。

(4) **Draw Location Info Enable**：在畫面上標示 Distance Effect 的作用區域邊界，供調整時觀察。

(5) **Distance Effect Enable**：Distance Effect 功能開關。Enable 表示啟動；Disable 表示關閉。

(6) **Distance Effect thd0**：距離加乘的起始閾值（內圈邊界）。距中心小於此距離的像素不套用加乘。

(7) **Distance Effect Rate**：唯讀。 ``thd0`` 至 ``thd1`` 之間的漸進加乘斜率，由系統依 ``thd0``\ 、 ``thd1`` 與 Distance Effect Max 自動計算。

(8) **Distance Effect thd1**：距離加乘的飽和閾值（外圈邊界）。距中心大於此距離的像素套用最大加乘強度。

(9) **Distance Effect Max**：距中心距離超過 ``thd1`` 時，對平坦區判定閾值的最大加乘量。

**Adaptive Brightness 參數群（(10)～(12)）**

依像素亮度對平坦區判定閾值進行加乘，使亮區或暗區的像素更容易被歸類為平坦區：

(10) **Adaptive Brightness Enable**：Adaptive Brightness 功能開關。Enable 表示啟動；Disable 表示關閉。

(11) **Adaptive Brightness Bright Tone TH**：亮區的閾值加乘量。數值越高，亮區像素越容易被歸類為平坦區。

(12) **Adaptive Brightness Dark Tone TH**：暗區的閾值加乘量。數值越高，暗區像素越容易被歸類為平坦區。

**Moire Condition 參數群（(13)～(16)）**

設定摩爾紋特徵的判定條件：

(13) **Moire Condition Enable**：摩爾紋特徵判定的開關。Enable 表示啟動；Disable 表示關閉。

(14) **Moire Condition TH**：摩爾紋特徵強度的判定閾值。閾值越低，越多像素被歸類為摩爾紋；閾值越高，判定標準越嚴格。

(15) **Moire Condition Offset**：摩爾紋方向性判斷的基底偏移值。數值越高，像素越難被判定為具有特定方向性的摩爾紋。

(16) **Moire Condition Slope**：摩爾紋方向性判斷的斜率。數值越高，像素越難被判定為具有特定方向性的摩爾紋。

**Thin Edge Condition 參數群（(17)～(22)）**

設定 Thin Edge 的判定條件，以及各方向性子類別的區分強度。各子類別的對應關係如本節末的示意圖所示。

(17) **Thin Edge Condition TH**：Thin Edge 特徵強度的判定閾值。閾值越低，越多像素被歸類為 Thin Edge；閾值越高，判定標準越嚴格。

(18) **Edge Condition Offset**：Thin Edge 方向性判斷的基底偏移值。數值越高，像素越難被判定為具有特定方向性。

(19) **Edge Condition Slope K2**：H/V Edge 子類別的方向性判定斜率。數值越高，越難將像素歸類為 H/V Edge。

(20) **Edge Condition Slope K3**：H/V Edge Bias 子類別的方向性判定斜率。數值越高，越難將像素歸類為 H/V Edge Bias。

(21) **Edge Condition Slope K4**：H/V Edge Bevel 子類別的方向性判定斜率。數值越高，越難將像素歸類為 H/V Edge Bevel。

(22) **Edge Condition Slope K5**：H/V Edge Texture 子類別的方向性判定斜率。數值越高，越難將像素歸類為 H/V Edge Texture。

< (17)～(22) Thin Edge 各方向性子類別對應示意圖 >

|image220|

3.19.2 INTP\\Interpolation 頁面
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Interpolation 頁面依據 Feature Detection 的分類結果，為各特徵區域選用對應的全彩內插演算法，並設定銳化強度。

< 圖像調試視窗的 INTP \\ Interpolation 頁面 >

|image221|

(1) **Filtering Mode**：整體內插模式選擇。

   - **Default**：標準銳化強度的內插模式。
   - **Sharp**：高銳化強度的內插模式。

(2) **Flat Mode**：平坦區的內插演算法選擇。

   - **LPF RGB + HPF G**：使用低頻 RGB 內插並疊加高頻 G 成分，強化細節表現。選此模式時 Smooth Rate 可調。
   - **LPF RGB**：僅使用低頻 RGB 成分進行內插。選此模式時 Smooth Rate 不可調。

(3) **Flat Mode Smooth Rate**：LPF RGB + HPF G 模式下，高頻 G 成分的強化幅度。數值越高，細節強化越明顯；設定過高時，邊緣可能出現拉鍊狀異常現象。

(4) **Edge Weight K2**：H/V Edge 區域方向性內插的權重。數值越高，越傾向以水平／垂直方向進行內插；數值越低，越傾向以無方向性方式進行內插。

(5) **Edge Weight K3**：H/V Edge Bias 區域方向性內插的權重，作用方式與 (4) 相同。

(6) **Edge Weight K4**：H/V Edge Bevel 區域方向性內插的權重，作用方式與 (4) 相同。

(7) **Sharpen**：內插後的整體銳化強度。越接近 Strong，銳化效果越強。

3.19.3 INTP\\Denoise 頁面
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Denoise 頁面設定內插完成後的去噪強度與作用區域。去噪強度依據 Feature Detection 所計算的 H/V Detection 特徵值決定：特徵值越低的區域（紋理越少）套用越強的去噪；特徵值越高的區域（邊緣、紋理區）去噪強度越低。最大去噪強度為固定值，(3)～(5) 控制不同特徵值區間的去噪強度分布。

< 圖像調試視窗的 INTP \\ Denoise 頁面 >

|image222|

(1) **Denoise Enable**：去噪功能開關。Enable 表示啟動；Disable 表示關閉。

(2) **Denoise Debug Mode**：以灰階顯示各像素的去噪強度。越黑表示去噪強度越強；越白表示去噪強度越弱。

(3) **Blend LPF Condition TH0**：去噪作用區域的第一個轉折點閾值。H/V Detection 特徵值低於 ``TH0`` 的像素套用最強去噪；高於 ``TH0`` 的像素去噪強度開始遞減。 ``TH0`` 越高，套用最強去噪的像素範圍越廣。 vHDR mode 下可依曝光比給予不同設定。

(4) **Blend LPF Condition TH1**：去噪作用區域的第二個轉折點閾值。H/V Detection 特徵值高於 ``TH1`` 的像素不套用去噪，維持內插結果。 ``TH1`` 越高，不套用去噪的像素範圍越窄。 vHDR mode 下可依曝光比給予不同設定。

(5) **Blend LPF Condition Slope**： ``TH0`` 至 ``TH1`` 之間的去噪強度過渡斜率，由系統依 ``TH0`` 與 ``TH1`` 自動計算。

.. tip::

   這個模塊調整效果最明顯的地方。
    ``TH0`` 與 ``TH1`` 決定 Denoise 的作用區域；可透過 Denoise Debug Mode 觀察作用範圍，越接近黑色代表去噪強度越強。下圖示範不同 ``TH0`` / ``TH1`` 設定的效果比較：左側數值較高（ ``TH0=3543`` 、 ``TH1=4877`` ），去噪範圍較廣；右側數值較低（ ``TH0=533`` 、 ``TH1=1563`` ），暗部仍可見較多雜點。

|image305|

.. _nr-module:

3.20 Noise Reduction
---------------------

NR（Noise Reduction；降噪）是 ISP 影像處理流程中用於抑制感測器雜訊的畫質模組，包含時間軸的 TNR（Temporal Noise Reduction；時間軸降噪）與空間軸的 SNR（Spatial Noise Reduction；空間軸降噪）。

**功能定義**

靜態場景的像素可同時套用 TNR 與 SNR；動態場景（移動物體）的像素則僅套用 SNR，避免時間軸降噪在動態區域產生殘影（Ghost）效果。

**調校說明**

本模組可依照增益條件（Gain）給予不同設定。在 vHDR mode 下，部分特定參數亦支援依據不同的曝光比（Exposure Ratio）給予彈性配置。

NR 模組的參數依功能區分為 General、Motion Detection、Detail Extraction 三個參數類型，可於圖像調試視窗的調試功能區點選各類型切換對應介面。

3.20.1 Noise Reduction\\General 參數說明
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

< 圖像調試視窗的 NR \\ General 參數頁面 >

|image223|

General 頁面包含 NR 模組的基本設定、降噪模式選擇及各強度參數，說明如下。

**基本設定**

(1) **Load Noise Curve Txt**：載入 RNR（RAW Noise Reduction）校正完成後產生的 Noise Curve 參數檔案。此校正資料能使降噪模型正確對應當前 Sensor 的雜訊分布特性，為後續降噪強度設定的基礎依據。

(2) **RAW NR Enable**：RAW 域降噪功能的整體開關。

   - **Enable**：啟用 RAW 降噪。
   - **Disable**：停用 RAW 降噪。

(3) **RAW NR Selection**：降噪模式選擇，決定使用的降噪軸向組合，三者擇一：

   - **3DNR（TNR + SNR）**：同時使用時間軸降噪（TNR）與空間軸降噪（SNR）。靜態場景套用 TNR 與 SNR，動態場景（移動物體）僅套用 SNR，以避免殘影（Ghost）問題。此為一般場景下的建議模式。
   - **TNR Only**：僅使用時間軸降噪（TNR）。
   - **SNR Only**：僅使用空間軸降噪（SNR）。

**降噪強度**

(4) **Static Scene TNR Strength**：靜態場景的時間軸降噪（TNR）強度。調整方向越接近 **Strong**，降噪效果越強；越接近 **Weak**，降噪效果越弱。

   .. note::
      當 RAW NR Selection 設為 **SNR Only** 時，此參數無效。在 vHDR mode 下，此參數支援依曝光比（Exposure Ratio）分別設定。

(5) ～ (8) **Static Scene SNR Strength**：靜態場景的空間軸降噪（SNR）強度。調整方向越接近 **Strong**，降噪效果越強。此組包含以下四個子參數：

   - **LP_TH0 / LP_TH1**：低頻（Low Pass）成分的降噪判定閾值下限與上限。閾值越低，進入降噪範圍的低頻訊號越多，整體降噪強度越大。
   - **HP_TH0 / HP_TH1**：高頻（High Pass）成分的降噪判定閾值下限與上限。閾值越低，進入降噪範圍的高頻訊號越多，整體降噪強度越大。

   .. note::
      當 RAW NR Selection 設為 **TNR Only** 時，此組參數無效。

(9) ～ (12) **Motion Scene SNR Strength**：動態場景中移動區域的空間軸降噪（SNR）強度。調整方向越接近 **Strong**，降噪效果越強。子參數定義與 (5)～(8) 相同，包含 ``LP_TH0`` 、 ``LP_TH1`` 、 ``HP_TH0`` 、 ``HP_TH1`` 。

   .. note::
      當 RAW NR Selection 設為 **TNR Only** 或 **SNR Only** 時，此組參數無效。

(13) **3DNR Strength**：3DNR 模式下靜態場景與動態場景降噪輸出的混合比例。調整方向越接近 **Strong**，靜態場景的輸出比例越高，整體降噪效果越強。

   .. note::
      當 RAW NR Selection 設為 **TNR Only** 或 **SNR Only** 時，此參數無效。

**靜態場景 SNR 觸發條件（3DNR 模式專用）**

以下兩個參數僅在 RAW NR Selection 設為 **3DNR** 時有效，用於控制靜態場景中特定空間頻率區域的 SNR 觸發行為。區域類型（平坦區 / 邊緣區）由 3.20.3 節 Detail Extraction 的空間頻率閾值分析所決定。

(14) **Static Scene Flat Enable**：控制靜態場景中平坦區域（低空間頻率區域）的 SNR 觸發行為。

   - **Enable**：平坦區域同時套用 SNR 與 TNR 降噪。
   - **Disable**：平坦區域僅套用 TNR 降噪。

(15) **Static Scene Edge Enable**：控制靜態場景中邊緣區域（高頻差異區域）的 SNR 觸發行為。

   - **Enable**：邊緣區域同時套用 SNR 與 TNR 降噪。
   - **Disable**：邊緣區域僅套用 TNR 降噪。

3.20.2 Noise Reduction\\Motion Detection 參數說明
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

< 圖像調試視窗的 NR \\ Motion Detection 參數頁面 >

|image224|

Motion Detection 頁面用於設定靜態／動態場景的判定閾值。ISP 透過比較當前幀與前一幀的像素差值，決定每個區域是靜態（套用 TNR + SNR 或純 TNR）還是動態（僅套用 SNR），以避免時間軸降噪在移動區域產生殘影（Ghost）。

**除錯工具**

(1) **Debug Mode**：以視覺化方式呈現每個像素的靜態／動態判定結果，可在頁面下方選擇要顯示的統計圖層。

   - 一般調校查看 **Overall**、**LP_all**、**HP_all** 三個圖層即可。
   - 若需進一步分析各色彩通道的差異，可選擇 **LP_R**、**LP_G**、**LP_B**、**LP_Y** 作為參考依據。

**靜態／動態判定閾值**

以下各參數以閾值對（TH0 / TH1）定義判定範圍：TH0 為靜態判定的上限，TH1 為動態判定的下限。當差值低於 TH0 時，判定為靜態；差值高於 TH1 時，判定為動態；介於兩者之間時，則依比例進行過渡。閾值整體越高（越接近 **High**），代表需要更大的幀間差值才能觸發動態判定，即對運動的敏感度越低。

(2) ～ (3) **Low Freq Difference G_TH0、G_TH1**：以 Bayer G 通道的低頻幀間差值判定靜態與動態場景。G 通道承載主要的亮度資訊，為最主要的判定依據。

(4) ～ (5) **Low Freq Difference RB_TH0、RB_TH1**：以 Bayer R、B 通道的低頻幀間差值進行判定。閾值調整方式與效果同 (2)～(3)，可補充色彩通道的動態偵測。

(6) ～ (7) **Low Freq Difference Y_TH0、Y_TH1**：以亮度 Y 通道的低頻幀間差值進行判定。閾值調整方式與效果同 (2)～(3)。

(8) ～ (9) **High Freq Difference G_TH0、G_TH1**：以 Bayer G 通道的高頻幀間差值進行判定。高頻差值對細微紋理移動較為敏感，可補充低頻差值無法偵測的細微運動。閾值調整方式與效果同 (2)～(3)。

**調試建議**

< Motion Detection 調整效果對比圖 >

|image372|

適度降低 MD 的作用區域與 NR 強度，可緩解移動物件的不自然拖影現象，同時在噪點抑制與動靜態場景間盡量維持平衡。此步驟需反覆 fine-tune，在 NR 模組中屬相對耗時的調整項目。

.. _nr-detail-extraction:

3.20.3 Noise Reduction\\Detail Extraction 參數說明
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

< 圖像調試視窗的 NR \\ Detail Extraction 頁面 >

|image225|

|image226|

Detail Extraction 頁面用於根據影像的空間頻率，將每個像素區域分類為平坦區、紋理區或高頻差異區，並據此決定靜態場景中各區域套用的 SNR 強度。此分類結果將直接影響 3.20.1 節 General 頁面中 **Static Scene Flat Enable** 與 **Static Scene Edge Enable** 的 SNR 觸發行為。

**除錯工具**

(1) **Debug Mode**：以視覺化方式呈現影像中各像素的空間頻率分類結果，可在畫面上確認每個區域被判定為平坦區（Flat）、紋理區（Texture）或高頻差異區（Edge）的分布情況。

   - 平坦區（Flat）：套用 **Static Scene Flat Enable** 的 SNR 設定。
   - 高頻差異區（Edge）：套用 **Static Scene Edge Enable** 的 SNR 設定。

**空間頻率分段與 Detail 強度設定**

(2) **Detail Extraction**：點擊【Detail Adjust】按鈕開啟設定視窗，可調整空間頻率閾值（X0～X3）與各區段對應的 Detail 數值（Y0～Y3）。

(3) **Spatial Frequency X0～X3**：定義空間頻率的分段閾值，將影像區域劃分為三類：

   - 空間頻率 **小於 X0**：判定為平坦區（Flat）。
   - 空間頻率 **介於 X1 與 X2 之間**：判定為紋理區（Texture）。
   - 空間頻率 **大於 X3**：判定為高頻差異區（Edge）。

(4) **Detail Extraction Y0～Y3**：設定對應各 X 分段區間的 Detail 數值。Detail 數值決定各區域套用 Static Scene SNR 的強度：

   - 調整方向越接近 **Detail**，數值越大，SNR 強度越弱（保留較多細節）。
   - 調整方向越接近 **Smooth**，數值越小，SNR 強度越強（降噪效果越明顯）。

   **Ymin** 與 **Ymax** 為 Detail 數值的下限與上限，可限定所有 Y0～Y3 的有效範圍，防止過強或過弱的 SNR 效果。調整邏輯與上述相同：數值越小，SNR 強度越強。

3.21 Short Path Noise Reduction（2DNR）
----------------------------------------

Short Path Noise Reduction（短曝路徑降噪；2DNR）功能僅在 HDR（High Dynamic Range）模式下使用。當偵測到靜態平坦區域時，使用空間軸降噪（Spatial Noise Reduction）進行處理；當偵測到移動區域時，則強化銳利度以保留細節並避免下採樣(Downsampling)帶來的模糊效果。

**調校說明**

本模組屬於 Texture 參數，可依照增益條件（Gain）給予不同設定。在 vHDR mode 下，部分特定參數亦支援依據不同的曝光比（Exposure Ratio）給予彈性配置。

3.21.1 Short Path Noise Reduction\\General 參數說明
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

< 圖像調試視窗的 Short Path Noise Reduction \\ General 頁面 >

|image227|

General 頁面包含 2DNR 模組的基本開關設定，以及降噪曲線的載入入口，說明如下。

**基本設定**

(1) **Update Noise Curve**：載入 2DNR 校正完成後產生的結果檔案（2DNR_Result.txt），以更新短曝路徑的降噪曲線參數。此校正資料能使降噪模型正確對應當前 Sensor 的雜訊特性，為後續降噪強度設定的基礎依據。

(2) **Short Path Noise Reduction Enable**：短曝路徑降噪（2DNR）功能的整體開關。

   - **Enable**：啟用短曝路徑降噪。
   - **Disable**：停用短曝路徑降噪。

(3) **Short Path Sharpness Enable**：短曝路徑銳化（Sharpness）功能的開關。啟用後，對偵測到移動的區域進行銳化處理，以補償 HDR 模式下短曝路徑降噪或下採樣可能帶來的細節模糊。

   - **Enable**：啟用短曝路徑銳化。
   - **Disable**：停用短曝路徑銳化。

(4) **Boundary Median Filter**：影像邊界中值濾波功能的開關。啟用後，對影像邊緣區域套用中值濾波，可有效減少邊界處的環狀雜訊（Ringing Artifact）。

   - **Enable**：啟用邊界中值濾波。
   - **Disable**：停用邊界中值濾波。

3.21.2 Short Path Noise Reduction\\Noise Reduction & Sharpness 參數說明
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

< 圖像調試視窗的 Short Path Noise Reduction \\ Noise Reduction & Sharpness 頁面 >

|image228|

Noise Reduction & Sharpness 頁面用於設定 2DNR 的降噪強度曲線與銳化強度曲線。兩條曲線共同定義每個像素依亮度值所套用的處理方式：像素值（pixel value）較低的暗部區域進行降噪；像素值較高的亮部區域進行銳化。

.. note::
   此頁面所有參數均支援依曝光比（Exposure Ratio）分別設定。

**降噪強度參數**

(1) **Noise Reduction Thd 0**：降噪強度曲線的起始閾值（第一個轉折點）。像素值低於此閾值時，以 **NR Rate** 設定的強度進行完整降噪處理。

(2) **Noise Reduction Thd 1**：降噪強度曲線的結束閾值（第二個轉折點）。像素值介於 ``Thd 0`` 與 ``Thd 1`` 之間時，降噪強度隨像素值增加而線性遞減；像素值高於此閾值時，不套用降噪處理。

(3) **NR Rate**：降噪處理的整體強度。數值越小，降噪效果越強。

(4) **G Delta Clip**：降噪處理前，限制 G 通道前後幀差值的最大允許量。數值越小，對差值的抑制越強，降噪效果越強；數值越大，則保留更多幀間差異。

**銳化強度參數**

(5) **Sharpness Thd 0**：銳化強度曲線的起始閾值（第一個轉折點）。像素值低於此閾值時，不套用銳化處理。

(6) **Sharpness Thd 1**：銳化強度曲線的結束閾值（第二個轉折點）。像素值介於 ``Thd 0`` 與 ``Thd 1`` 之間時，銳化強度隨像素值增加而線性遞增；像素值高於此閾值時，以 **Sharpness Rate** 設定的強度進行完整銳化處理。

(7) **Sharpness Rate**：銳化處理的整體強度。數值越大，銳化效果越強。

.. note::
   各閾值之間的大小關係須滿足以下條件：

   **Sharpness Thd 1 > Sharpness Thd 0 > Noise Reduction Thd 1 > Noise Reduction Thd 0**

   此順序確保降噪與銳化的作用區間不重疊，避免兩者相互干擾。

**強度曲線示意**

(8) **Show Curve**：點擊可在頁面中顯示由上述參數構成的強度曲線，如下圖所示：

< Short Path NR & Sharpness 強度曲線示意圖 >

|image229|

X 軸為像素值，由左至右代表亮度由暗（低值）到亮（高值）。Y 軸為處理強度 Rate：

- **Rate = 256**：不進行任何處理。
- **Rate < 256**：數值越低，降噪強度越強（對應暗部區域）。
- **Rate > 256**：數值越高，銳化強度越強（對應亮部區域）。

曲線會隨主頁面各參數的調整即時更新，可作為設定結果的視覺化確認依據。

.. _eeh-module:

3.22 Edge Enhance
------------------

Edge Enhance（EEH；邊緣強化）是 ISP 影像處理流程中的畫質增強模組，用於強化影像中紋理邊緣的清晰度，同時抑制過度銳化（Over/Undershoot）與噪點累積的影響。模組針對靜態場景中的平坦與紋理區域，預先判斷感測器輸出的邊緣特性，並依此進行處理。功能分為 General、Y Smooth、Edge Detection、Y Sharp 及 Overshoot/Undershoot 五個參數類型。

**調校說明**

本模組屬於 Texture 參數，可依照增益條件（Gain）給予不同設定。在 vHDR mode 下，部分特定參數亦支援依據不同的曝光比（Exposure Ratio）給予彈性配置。

進行 Edge Enhance 調校時，建議遵循以下原則：

- **分段漸進調整**：每次以較小的幅度調整銳化強度，逐步確認效果，避免一次性過度強化造成邊緣異常。
- **先降噪再銳化**：若畫面存在明顯雜訊，應先完成 Noise Reduction 調校，再進行 Edge Enhance 調整。直接對含有大量雜訊的影像執行銳化，會同時強化雜訊，導致畫質劣化。

3.22.1 Edge Enhance\\General 參數說明
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

< 圖像調試視窗的 EEH \\ General 頁面 >

|image230|

General 頁面包含 EEH 模組的整體開關設定，說明如下。

(1) **Edge Enhance Enable**：邊緣強化功能的整體開關。

   - **Enable**：啟用邊緣強化。
   - **Disable**：停用邊緣強化。

(2) ～ (5) 保留欄位，無需調整。

3.22.2 Edge Enhance\\Y Smooth 參數說明
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Y Smooth 是邊緣強化前的預處理步驟。EEH 在銳化邊緣之前，先以 Y Smooth 判斷每個像素區域的邊緣特徵強弱：**平坦區**\ （邊緣特徵值低）套用強平滑，以抑制感測器雜訊進入銳化流程；**邊緣區**\ （邊緣特徵值高）逐漸減弱平滑，以保留紋理細節，避免後續銳化因過度平滑而喪失邊緣清晰度。

Y Smooth 所依據的 X 軸輸入為 **Edge Detection 計算後的邊緣特徵值**\ （edge feature value），而非影像像素的亮度值（pixel luminance）。特徵值越低，代表該像素區域越接近平坦；特徵值越高，代表邊緣或紋理越明顯。

< 圖像調試視窗的 EEH \\ Y Smooth 頁面 >

|image231|

Y Smooth 頁面的參數分為兩個功能群組：**Smooth Effect**\ （依邊緣特徵值決定平滑強度）與 **Distance Effect**\ （依距圖像中心的距離動態補償平滑強度）。

**除錯工具**

(1) **Y Smooth Debug Mode**：以灰階視覺化方式顯示平滑處理的強度分布，可在畫面上確認各區域的平滑情況：

   - **越黑（暗）**：Smooth Effect 越強，代表該區域為平坦區（邊緣特徵值低），雜訊被大量抑制。
   - **越白（亮）**：Smooth Effect 越弱，代表該區域為邊緣或紋理區（邊緣特徵值高），保留較多細節供後續銳化使用。

**Smooth Effect**

依據 Edge Detection 輸出的邊緣特徵值（edge feature value）大小，對各像素區域套用對應強度的平滑處理。強度曲線如下圖所示：

|image232|

X 軸為邊緣特徵值，數值由左至右由低到高，代表從平坦區過渡到邊緣區。Y 軸為平滑強度（smooth rate）：曲線從左側高平滑強度（``rate0``\ ）在 ``thd0`` 處開始下降，至 ``thd1`` 處降至低平滑強度（``rate1`` / residual），之後維持最低強度（保留邊緣細節）。

(2) **thd0**：Smooth Effect 開始遞減的邊緣特徵值閾值（第一個轉折點）。特徵值低於 ``thd0`` 的像素區域判定為平坦區，套用 ``rate0`` 的最強平滑。``thd0`` 越大，平坦區的判定範圍越廣，平滑作用影響的區域越多。

(3) **thd1**：Smooth Effect 遞減至最低強度的邊緣特徵值閾值（第二個轉折點）。特徵值介於 ``thd0`` 與 ``thd1`` 之間時，平滑強度依比例線性遞減；特徵值高於 ``thd1`` 時，套用 ``rate1`` 的最弱平滑（保留邊緣細節）。``thd1`` 越大，平滑強度遞減的過渡區間越寬。

(4) **rate0**：特徵值低於 ``thd0`` 時（平坦區）套用的平滑強度，為曲線的最高強度值。數值越大，對平坦區的平滑效果越強，雜訊抑制越明顯。

(5) **rate1**：特徵值高於 ``thd1`` 時（邊緣區）套用的平滑強度，為曲線的最低強度值（residual）。數值越小，邊緣區保留的細節越多；一般建議維持在接近 0 的低值以避免影響後續銳化品質。

(6) **Smooth Strength**：保留欄位，無需調整。

**Distance Effect**

鏡頭光學特性會造成畫面邊緣的影像更容易出現雜訊，Distance Effect 依據像素距圖像中心（image center）的距離，對遠離中心的區域額外補強 Smooth Effect 強度，以均衡整體畫面的平滑效果。

(7) **Y Smooth Draw Location Info**：將 Distance Effect 的作用範圍邊界標示於即時畫面上，可輔助確認 ``Dist_thd0`` 與 ``Dist_thd1`` 的作用位置是否符合預期。

(8) **Dist_thd0**：距圖像中心的距離低於此值時，不套用 Distance Effect 補強，Smooth Effect 維持 Smooth Effect 參數群的原始強度設定。

(9) **Dist_thd1**：距圖像中心的距離高於此值時，以 ``Dist_Max`` 設定的最大幅度補強 Smooth Effect 強度。

(10) **Dist_Max**：Distance Effect 補強 Smooth Effect 的最大幅度上限。作用於距圖像中心大於 ``Dist_thd1`` 的區域；距離介於 ``Dist_thd0`` 與 ``Dist_thd1`` 之間的區域，則依距離比例線性漸變，直至達到 ``Dist_Max``\ 。

(11) **Smooth Effect Button**：點擊可彈出 Smooth Effect 強度曲線的預覽視窗，顯示目前 ``thd0``\ 、``thd1``\ 、``rate0``\ 、``rate1`` 設定值所對應的曲線形狀，供視覺化確認設定結果。

3.22.3 Edge Enhance\\Edge Detection 參數說明
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Edge Detection 是 EEH 模組的核心分析步驟，透過 Mean Filter 與 Laplacian Filter 兩組濾波器，計算每個像素的邊緣特徵值（edge feature value），並以此作為後續兩個處理步驟的共同輸入依據：

- **Y Smooth**\ （3.22.2）：以邊緣特徵值決定平滑強度，特徵值越低（平坦區）→ 套用越強的平滑（抑制雜訊）。
- **Y Sharp**\ （3.22.4）：以邊緣特徵值決定銳化強度，特徵值越高（邊緣區）→ 套用越強的銳化（強化邊緣）。

< 圖像調試視窗的 EEH \\ Edge Detection 頁面 >

|image233|

**除錯工具**

此頁面提供兩個獨立的 Debug 視覺化工具，分別對應 Y Smooth 與 Y Sharp 的作用結果。

(1) **Y Smooth Debug Mode**：以灰階視覺化方式顯示各像素區域的平滑強度分布。越黑（暗）代表 Smooth Effect 越強（平坦區，邊緣特徵值低）；越白（亮）代表 Smooth Effect 越弱（邊緣區，邊緣特徵值高）。

(2) **Sharp Edge Map Enable**：以灰階視覺化方式顯示各像素區域的銳化強度分布。越白（亮）代表 Sharp Effect 越強（邊緣區，邊緣特徵值高）；越黑（暗）代表 Sharp Effect 越弱（平坦區，邊緣特徵值低）。

.. note::
   兩個 Debug 工具的灰階方向**互為相反**，正確反映了 Y Smooth 與 Y Sharp 的設計：平坦區（暗）= 強平滑 + 弱銳化；邊緣區（亮）= 弱平滑 + 強銳化。

**Y Mean Filter 參數群**

在計算邊緣特徵值之前，先對影像進行平均濾波（Mean Filter），將雜訊去除，使 Edge Detection 的結果更穩定。Mean Filter 共有 ``para0``\ ～ ``para4`` 五個係數，採對稱設計： ``para3`` 自動同步 ``para1``\ 、``para4`` 自動同步 ``para0``\ ，無需個別設定。

(3) **para0**：Mean Filter 的最外側係數（兩端，預設值最小）。

(4) **para1**：Mean Filter 的次外側係數。

(5) **para2**：Mean Filter 的中心係數（峰值，預設值最大）。

**Y Laplacian Filter 參數群**

Laplacian Filter 用於計算高頻差異，輸出即為邊緣特徵值的來源。共有三個係數（``para0``\ 、``para1``\ 、``para2``\ ），採對稱設計：``para1`` 為中心峰值，``para0`` 與 ``para2`` 為兩端。

(6) **para0**：Laplacian Filter 的左端係數（兩端對稱，對應 ``para2``\ ）。

(7) **para1**：Laplacian Filter 的中心係數（峰值，預設值最大）。

(8) **para2**：Laplacian Filter 的右端係數（兩端對稱，對應 ``para0``\ ）。

(9) **Edge Detection Button**：點擊可彈出預覽視窗，顯示目前 Y Mean Filter 與 Y Laplacian Filter 係數所對應的濾波器強度曲線，供視覺化確認設定結果。

< Edge Detection Button 預覽視窗（上：Y Mean Filter ；下：Y Laplacian Filter ）>

|image243|

3.22.4 Edge Enhance\\Y Sharp 參數說明
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Y Sharp 依據 Edge Detection 輸出的邊緣特徵值（edge feature value），對各像素區域套用對應強度的銳化處理。與 Y Smooth 的曲線方向相反——邊緣特徵值越高（邊緣區），套用的銳化強度越強；特徵值越低（平坦區），銳化強度越弱或不套用。

Y Sharp 頁面的參數分為兩個功能群組：**Sharp Effect**\ （依邊緣特徵值決定銳化強度）與 **Distance Effect**\ （依距圖像中心的距離動態補償銳化強度）。

< 圖像調試視窗的 EEH \\ Y Sharp 頁面 >

|image234|

**除錯工具**

(1) **Sharp Edge Map Enable**：以灰階視覺化方式顯示各像素區域的銳化強度分布。越白（亮）代表 Sharp Effect 越強（邊緣區，邊緣特徵值高）；越黑（暗）代表 Sharp Effect 越弱（平坦區，邊緣特徵值低）。此 debug 圖的灰階方向與 Y Smooth Debug Mode 互為相反。

**Sharp Effect**

依據 Edge Detection 輸出的邊緣特徵值大小，對各像素區域套用對應的銳化強度。強度曲線如下圖所示：

|image235|

X 軸為邊緣特徵值（edge feature value），數值由左至右由低到高，代表從平坦區過渡到邊緣區。Y 軸為銳化強度（sharp rate）：曲線從左側低銳化強度（``rate0``\ ）在 ``th0`` 處開始上升，至 ``th1`` 處升至高銳化強度（``rate1``\ ），之後維持最高強度。此方向與 Y Smooth 曲線（左高右低）相反。

由於不同影像亮度下，Sensor 雜訊特性不同，Y Sharp 提供九個亮度條件（Y0 ～ Y256）各自獨立設定 ``th0``\ ，以確保暗部與亮部的銳化行為分別最佳化。

(2) ～ (10) **Y0_th0 ～ Y256_th0**：在不同影像亮度條件（Y level）下，銳化強度曲線的起始邊緣特徵值閾值（``th0``\ ），共九個參數。Y0 對應暗部條件，Y256 對應亮部條件。``th0`` 越大，需要更強的邊緣特徵才能開始套用銳化，銳化的作用範圍越窄（更保守）；``th0`` 越小，較弱的邊緣特徵即可觸發銳化，銳化範圍越廣。

(11) **offset（th1 = th0 + offset）**：銳化強度曲線從起始閾值（``th0``\ ）爬升至最大強度閾值（``th1``\ ）的區間寬度。``th1`` 由公式 ``th1 = Yxx_th0 + offset`` 計算得出，全部 Y 條件共用同一 ``offset`` 值。``offset`` 越大，銳化強度的線性過渡區間越寬，強度增加越平緩；``offset`` 越小，強度增加越陡峭。

(12) **rate0**：邊緣特徵值低於 ``th0`` 時（平坦區邊界）套用的銳化強度，為曲線的最低強度值。數值越小，平坦區的銳化程度越低，與邊緣區的差異對比越大。

   .. note::
      在 vHDR mode 下，此參數支援依曝光比（Exposure Ratio）分別設定。

(13) **rate1**：邊緣特徵值高於 ``th1`` 時（邊緣區）套用的銳化強度，為曲線的最高強度值。數值越大，邊緣銳化效果越強；超過 ``th1`` 後維持此強度不再增加。

   .. note::
      在 vHDR mode 下，此參數支援依曝光比（Exposure Ratio）分別設定。

**Distance Effect**

鏡頭光學特性會造成畫面邊緣的紋理偵測更容易受到變形影響，Distance Effect 依據像素距圖像中心的距離，動態**調低**遠離中心區域的 ``th0`` 閾值，使圖像邊緣區域的銳化起始門檻降低，避免鏡頭邊緣因光學畸變使邊緣特徵值偏低而錯失銳化。

.. note::
   Y Sharp 的 Distance Effect 方向與 Y Smooth 的 Distance Effect 相反：Y Smooth 是對邊緣區域**加強**平滑；Y Sharp 是對邊緣區域**降低** ``th0``\ （使銳化更容易觸發）。兩者共同目的都是補償鏡頭邊緣的光學特性差異。

(14) **Y Sharp Draw Location Effect**：將 Distance Effect 的作用範圍邊界標示於即時畫面上，可輔助確認 ``Dist_thd0`` 與 ``Dist_thd1`` 的作用位置是否符合預期。

(15) **Dist_thd0**：距圖像中心的距離低於此值時，不套用 Distance Effect，Y Sharp 的 ``th0`` 維持原始設定值。

(16) **Dist_thd1**：距圖像中心的距離高於此值時，以 ``Dist_Max`` 設定的最大幅度調低 ``th0``\ （擴大銳化作用範圍）。

(17) **Dist_Max**：Distance Effect 調低 ``th0`` 的最大幅度上限。作用於距圖像中心大於 ``Dist_thd1`` 的區域；距離介於 ``Dist_thd0`` 與 ``Dist_thd1`` 之間的區域，則依距離比例線性漸變，直至達到 ``Dist_Max``\ 。

(18) **NR Sharp Strength**：保留欄位，無需調整。

(19) **Sharp Strength**：保留欄位，無需調整。

(20) **Y Sharp Effect Button**：點擊可彈出 Sharp Effect 強度曲線的預覽視窗，顯示目前 ``th0``\ 、``offset``\ 、``rate0``\ 、``rate1`` 設定值所對應的曲線形狀，供視覺化確認設定結果。

   |image365|

.. tip::
   建議搭配 **Sharp Edge Map** （參數 (1)）觀察銳化作用範圍，確認強化區域是否符合預期後，再調整 ``rate0``\ 、``rate1`` 的強度數值。平坦區應呈現為黑色（不被強化），紋理與邊緣區應呈現為白色（被強化）。

   < Sharp Edge Map 與 Sharp Effect 曲線對應示意 >

   |image366|

3.22.5 Edge Enhance\\Over/UnderShoot 參數說明
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

< 圖像調試視窗的 EEH \\ OverShoot/UnderShoot 頁面 >

|image236|

Over/UnderShoot 頁面用於抑制銳化處理在邊緣過渡區域產生的上衝（Overshoot）與下衝（Undershoot）失真。Overshoot 是指邊緣亮側像素值被過度拉高；Undershoot 是指邊緣暗側像素值被過度壓低。兩者均會導致邊緣出現不自然的光暈（Halo）效果。

(1) **Suppress Strength**：上衝與下衝的抑制強度。調整方向越接近 **Strong**，對銳化邊界的限制越強，Overshoot 與 Undershoot 的幅度越小。

   .. note::
      在 vHDR mode 下，此參數支援依曝光比（Exposure Ratio）分別設定。

(2) **Suppress Strength Button**：點擊可輸出抑制強度的效果預覽，示意如下：

|image237|

3.23 Video Property
--------------------

Video Property（視訊屬性）提供亮度（Brightness）、對比（Contrast）、飽和度（Saturation）等基礎影像觀感的整體調整，適合作為最終輸出影像風格的微調工具。

**調校說明**

本模組屬於 Texture 參數，可依照增益條件（Gain）給予不同設定。在 vHDR mode 下，所有參數均支援依曝光比（Exposure Ratio）給予不同設定。

< 圖像調試視窗的 Video Property 頁面 >

|image238|

(1) **Brightness**：影像整體亮度（brightness）的偏移量。以 0 為基準，無正負單位限制：

   - 等於 0：不調整。
   - 大於 0：整體亮度提升。
   - 小於 0：整體亮度降低。

(2) **Contrast\\Level**：影像對比度（contrast）的強度倍率。以 32 為基準值（不調整）：

   - 等於 32：不調整。
   - 大於 32：對比度提升，亮暗差異放大。
   - 小於 32：對比度降低，亮暗差異縮小。

(3) **Contrast\\Mean**：對比度計算時所參考的亮度基準值（reference luminance level）。系統依每個像素的亮度值與此基準值的差距進行對比放大處理，基準值越偏向亮側或暗側，放大效果的分布會隨之偏移。

   .. note::
      當 Contrast\\Level 設為 32 時，調整此基準值不會對影像產生任何影響。

(4) **Saturation**：影像整體飽和度（saturation）的強度倍率。以 64 為基準值（不調整）：

   - 等於 64：不調整。
   - 大於 64：飽和度提升，色彩更鮮豔。
   - 小於 64：飽和度降低，色彩趨向灰階。

< 亮度變化範例 >

|image_vp_brightness|

< 對比變化範例 >

|image_vp_contrast|

< 彩度變化範例 >

|image_vp_saturation|

.. _ldc-module:

3.24 LDC
---------

LDC（Lens Distortion Correction；鏡頭畸變校正）是 ISP 影像處理流程中的幾何校正模組，用於補償廣角鏡頭因光學特性引入的幾何畸變，透過數學模型的逆向映射（inverse mapping）還原無畸變的影像。

**功能定義**

鏡頭畸變分為兩類：

- **桶型畸變（Barrel Distortion）**：影像邊緣向外膨脹，常見於廣角鏡頭。
- **枕型畸變（Pincushion Distortion）**：影像邊緣向內收縮，常見於長焦鏡頭。

.. note::
   AmebaPro2 僅支援桶型畸變的校正，不支援枕型畸變的處理。

< 圖像調試視窗的 LDC 頁面 >

|image239|

**基本設定**

(1) **LDC Enable**：鏡頭畸變校正功能的整體開關。

   - **Enable**：啟用鏡頭畸變校正。
   - **Disable**：停用鏡頭畸變校正。

**影像來源設定（Image Source）**

(2) **Get Sensor Size**：自動讀取目前連線影像的感測器（sensor）輸出尺寸，並填入下方 ``Width``\  / ``Height`` 欄位，作為後續校正計算的基礎解析度依據。

(3) **Width**：影像的水平解析度（pixels）。

(4) **Height**：影像的垂直解析度（pixels）。

(5) **Center**：用來調整畫面中心與邊緣的比例，當校正後的畫面出現「中間過胖、兩邊/上下被壓扁（變窄）」的現象時，可調整此參數來使整體畫面比例均勻。調整範圍（0.4 ～ 0.99）。

**校正演算法設定（Algorithm）**

(6) **Alpha**：設定對應鏡頭的實際視角（Field of View，FOV）的基礎值，可調整範圍為 70 ～ 140。若鏡頭 FOV 超過 140°，工具無法進行有效的畸變補正。

以下以兩組實際測試案例，說明 Alpha 所能處理的 FOV 規格範圍：

**Case #1：1080P，對角線 FOV = 120°（符合規格）**

|image391| |image392|

.. centered:: 左：校正前（桶型畸變）；右：LDC 啟用後（幾何畸變有效修正，直線恢復正常）

**Case #2：1536P，對角線 FOV = 180°（超出規格）**

|image393| |image394|

.. centered:: 左：校正前（嚴重桶型畸變）；右：LDC 啟用後（邊緣殘留黑色弧形，FOV 超出可修正範圍）

(7) **Slide**：控制校正後影像的輸出縮放比例，須與 Alpha 搭配調整，兩者共同決定最終的有效 FOV。實務調整原則如下：

  - 當Alpha 設定較大、導致邊緣模糊時：調小 Slide 值，此時畫面會向外擴展（Zoom Out），將原本過度拉伸的邊緣區域向內收縮，不僅能改善邊緣畫質，還能擴大有效視角（FOV）。

  - 當 Alpha 設定較小、導致邊緣出現無效線條（Dummy Line）時：調大 Slide 值，此時畫面會向內放大（Zoom In），將邊緣的無效線條推至畫面外進行裁切，藉此消除畫面異常（此操作會使有效 FOV 稍微變小）。
  
**離線模擬（Offline Simulation）**

(8) **Offline Simulation**：暫不支援用戶使用。

3.25 DayNight
--------------

DayNight（日夜切換）依據影像曝光統計資訊（ETGain）與彩色像素分布，自動判斷是否需要切換至日間（RGB Mode）或夜間（IR Mode）的功能模組。

**功能定義**

本模組目前搭配 SDK 上層最佳化建議機制運作，由 ISP 提供當前的 ETGain 與色彩偵測結果作為切換依據，最終由 SDK 決定 RGB / IR Mode 的切換時機。

ETGain 的計算公式為：``ETGain = 曝光時間（ms）× 10 × 總增益（total gain）``。ETGain 越大，代表當前場景越暗、曝光越長；越小則代表場景越亮。

< 圖像調試視窗的 DayNight 頁面 >

|image241|

**RGB → IR 切換設定**

(1) **RGB2IR Delay**：切換至 IR Mode 的延遲幀數。系統需持續偵測到觸發條件，並維持超過此延遲設定的幀數後，才執行從 RGB Mode 切換為 IR Mode 的動作；單位為幀（frame）。設定越大，切換反應越遲緩，可避免因短暫光線變化引起的誤切換。

(2) **RGB2IR ETGain TH**：觸發切換至 IR Mode 的 ETGain 閾值（threshold）。當 ETGain 超過此設定值時，判定為夜間場景，觸發 RGB → IR 切換。

   .. note::
      設定值越大，需要更暗（更長曝光）的環境才能觸發切換，越不容易進入 IR Mode。

**IR → RGB 切換設定**

IR Mode 切換回 RGB Mode 需同時滿足 **ETGain 條件**\ （參數 4）與 **彩色像素比例條件**\ （參數 5、6）兩個觸發條件。

(3) **IR2RGB Delay**：切換至 RGB Mode 的延遲幀數。系統需持續偵測到觸發條件並超過此延遲設定後，才執行從 IR Mode 切換為 RGB Mode 的動作；單位為幀（frame）。

(4) **IR2RGB ETGain TH**：觸發切換至 RGB Mode 的 ETGain 閾值。當 ETGain 低於此設定值時，滿足 ETGain 觸發條件之一。

   .. note::
      設定值越小，需要更亮（更短曝光）的環境才能滿足此條件，越不容易進入 RGB Mode。為避免模式在臨界點反覆切換（hunting），須確保 ``IR2RGB ETGain TH`` < ``RGB2IR ETGain TH``，使兩個方向的觸發閾值保持適當的遲滯（hysteresis）間距。

(5) **IR2RGB Color Ratio TH**：在 IR Mode 下，判斷 ``IR2RGB Color Block`` 偵測區域中彩色像素（color pixel）佔所有像素的比例閾值。當彩色像素比例超過此值時，判定場景已具備足夠的可見光源，滿足色彩觸發條件之一。

   .. note::
      設定值越小，需要更高比例的彩色像素才能滿足此條件，越不容易進入 RGB Mode。

(6) **IR2RGB Color Block Count TH**：滿足 ``IR2RGB Color Ratio TH`` 判定條件的區塊數量閾值。當符合條件的區塊數量達到或超過此值時，滿足切換至 RGB Mode 的色彩觸發條件之一。

**IR2RGB 偵測區域設定**

(7) **IR2RGB Color Block**：設定用於判定 IR → RGB 切換時的彩色像素偵測作用區域（region of interest）。

   系統將畫面切分為 5×5 共 25 個區塊，勾選的區塊為參與彩色比例偵測的有效範圍。由於相機的紅外燈照射範圍主要為畫面中心區域，建議僅選取中央區塊以提高偵測穩定性。

   頁面右上方即時顯示目前的 ETGain 數值、滿足 ``Color Ratio TH`` 條件的區塊數量與佔比，可作為微調參數 (2)、(4)、(6) 的參考依據。

   < IR2RGB Color Block 偵測區域示意圖 >

   |image242|

   頁面左方座標區域顯示各區塊的即時統計結果，其中紅框標示出目前符合參數 (5) 設定值的區塊位置，可用來輔助確認閾值設定是否合適。

**狀態顯示**

(8) **Day/Night Status**：唯讀欄位。顯示目前 ISP 根據統計結果給出的 RGB / IR Mode 切換建議狀態，可用於即時監控模組運作是否符合預期。

.. _advanced-features:

4. 進階功能
-----------

本章節介紹 RealCam Pro 的輔助進階功能，主要應用於系統除錯與特殊開發需求：

- **4.1** 節介紹暫存器讀寫功能，可即時存取芯片或感測器的暫存器資料，為問題排查的常用手段。暫存器位址資訊不對外公開，如有需要請洽詢支援窗口取得對應位址。

- **4.2** 節說明 RealCam Pro 擷取之 .cap RAW 檔的資料格式，包含檔頭結構與像素資料的編碼方式，供需要自行解析 RAW 影像資料時參考。

- :ref:`4.3 <add-iq-interval>` 節說明 IQ 參數區間的新增方式。芯片內建動態圖像參數機制，可依場景照度與色溫切換不同的調試參數。當現有區間設定無法滿足特定場景的調校需求時，可依本節說明新增色溫、曝光比或增益區間。

- **4.4** 節說明 IQ Table 的內部儲存邏輯，以及如何在 vHDR 模式下新增可根據曝光比（ET-gain）動態調整的 texture 參數。

.. _register-rw:

4.1 暫存器讀寫
--------------

暫存器讀寫功能可在連線樣機的狀態下，即時存取芯片或感測器的暫存器數值，用於確認硬體運作狀態或進行問題排查。暫存器位址資訊不對外公開，操作時請依支援窗口提供的位址進行讀寫。

**開啟步驟**

1. 點選 RealCam Pro 主畫面工具列上的 **Vendor**。
2. 於下拉式選單中選取 **Reg Set Tool**，即可開啟「暫存器讀寫視窗」。

< RealCam Pro 主畫面 — Vendor 下拉式選單 >

|image244|

< 暫存器讀寫視窗 >

|image245|

暫存器讀寫視窗中，可透過 **Sensor** / **Controller** 單選按鈕切換讀寫目標，分別對應感測器端與 ISP 芯片端的暫存器存取。各暫存器的位址定義請參閱對應的規格書。

|image272|

.. _raw-data-format:

4.2 Realtek RAW Data 格式介紹
------------------------------

透過「2.2.3 Capture RAW」功能所擷取的 RAW Data 檔案，副檔名為 ``.cap``，採用 Little-endian byte order 儲存，檔案結構分為以下兩部分：

**1) 檔頭資訊（Header）**

檔案起始處佔用 128 Bytes，以每 4 Bytes 為一組依序記錄下列資訊：

- **(1) 標籤（Magic Number）**：固定為 ``0x59 55 59 32``\ （YUY2 的 ASCII 編碼），作為格式識別碼，表示每個 Pixel 佔用 2 Bytes。
- **(2) 幀寬（Width）**：影像的水平解析度，單位為 pixels。以 2688 × 1520 為例，欄位值 ``0x80 0A 00 00`` 解析後為 ``0x00 00 0A 80``，即 2688 pixels。
- **(3) 幀高（Height）**：影像的垂直解析度，單位為 lines。以 2688 × 1520 為例，欄位值 ``0xF0 05 00 00`` 解析後為 ``0x00 00 05 F0``，即 1520 lines。
- **(4) 有效資料長度**：不含檔頭的影像資料總位元組數（Width × Height × 2 Bytes）。以 2688 × 1520 為例，欄位值 ``0x00 B0 7C 00`` 解析後為 ``0x00 7C B0 00``，即 8,171,520 Bytes。

**2) 有效資料（Pixel Data）**

緊接在檔頭之後，每 2 Bytes 對應一個 Pixel Value，採 Little-endian byte order 儲存。例如：Pixel Value 為 438 時，檔案中儲存為 ``0xB6 0x01``；Pixel Value 為 1023 時，儲存為 ``0xFF 0x03``。

< 以文字編輯器開啟 2688 × 1520 RAW Data 的檔案內容示意 >

|image246|

4.2.1 使用 ImageJ 調整 RAW Data 的影像尺寸
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

當軟體模擬或特殊開發需求需要特定尺寸的 RAW Data 時，可使用開源影像工具 `ImageJ <http://www.imagej.net/>`_ 對現有 .cap 檔進行裁切或縮放，操作步驟如下：

1. 執行 ``File → Import → RAW...``，開啟目標 .cap 檔。匯入設定填入：位元深度 16 bits、正確的影像寬高、First Image Offset 設為 128，並勾選 **Little-endian byte order**。

   |image264|

   |image265|

2. 依需求選擇以下其中一種方式調整影像尺寸：

   - **裁切**：在影像上框選所需範圍後，執行 ``Image → Crop``。
   - **縮放**：執行 ``Image → Scale``，輸入目標尺寸。

3. 執行 ``File → Save As → RAW Data...`` 將影像存檔。
4. ImageJ 存出的 RAW 檔預設為 Big-endian byte order。需重新以步驟 1 的方式匯入該檔案，再次以 ``File → Save As → RAW Data...`` 另存，以還原為 Little-endian byte order。
5. 以文字編輯器（Hex 模式）開啟新存的 RAW 檔，將原始 .cap 檔的前 128 Bytes 檔頭複製至新檔案開頭，並依新影像的實際尺寸更新幀寬、幀高及有效資料長度三個欄位，即完成轉換。

.. _add-iq-interval:

4.3 新增 IQ 參數區間
---------------------

操作本節前，請先詳閱「3.0 圖像調試步驟」章節，確認已了解 IQ 參數在色溫區間與增益區間的運作方式。

新增參數區間可提升各場景下的參數調校精確度。當特定場景需要獨立的參數配置時，可為其新增專屬的色溫區間、曝光比區間或增益區間。對於未涵蓋在新增區間內的其他條件，系統將依照相鄰區間執行靜態切換，或動態做線性內插。

在 ISP Tuning Pro 主視窗中點選【Index Manager】，開啟 Index Manager 視窗後，依下列各節步驟操作。

< Index Manager 視窗 >

|image247|

4.3.1 新增色溫區間
^^^^^^^^^^^^^^^^^^^

1. 在左側面板選取 **Color Temp./Gain**（勿選取 **High Temp**）。
2. 在上方下拉選單選取目標 ISP 模組，例如 **BLC**。
3. 在區間列表中，點選欲插入新區間位置的相鄰區間。
4. 點擊【Add】。系統將以所選相鄰區間為基礎，複製產生新區間；新區間的 IN/OUT 閾值與參數初始值由複製或線性內插方式產生。
5. 點擊【Save】儲存新區間。

|image248|

6. 依場景需求修改新區間的 IN/OUT 閾值。
7. 再次點擊【Save】完成設定。

|image249|

4.3.2 新增曝光比區間
^^^^^^^^^^^^^^^^^^^^^

1. 在左側面板選取 **Color Temp./Gain**（勿選取 **High Temp**）。
2. 在上方下拉選單選取支援依曝光比切換的 ISP 模組，例如 **GammaRGB** 或 **Texture**。
3. 在區間列表中，點選欲插入新區間位置的相鄰區間。
4. 點擊【Add】。系統將複製產生新區間；新區間的 IN/OUT 閾值預設為 1，須依實際曝光比條件手動修改。參數初始值由複製或線性內插方式產生。
5. 點擊【Save】儲存新區間。

|image250|

6. 依場景需求修改新區間的 OUT 閾值。
7. 再次點擊【Save】完成設定。

|image251|

4.3.3 新增增益區間
^^^^^^^^^^^^^^^^^^^

1. 在左側面板選取 **Color Temp./Gain**（勿選取 **High Temp**）。
2. 在上方下拉選單選取目標 ISP 模組，例如 **BLC**。
3. 確認新增的增益區間所屬的色溫區間，再於區間列表中點選欲插入新區間位置的相鄰區間。
4. 選擇增益閾值的切換基準單位：**Gain** 或 **ETGain**。
5. 點擊【Add】。系統將以所選相鄰區間為基礎，複製產生新區間；新區間的 IN/OUT 閾值與參數初始值由複製或線性內插方式產生。
6. 點擊【Save】儲存新區間。

|image252|

7. 依場景需求修改新區間的 OUT 閾值。
8. 再次點擊【Save】完成設定。

|image253|

5. 圖像調試範例
----------------

本章節彙整圖像調試範例，可作為初始調試的參考指引。

- **5.1** 節說明整體圖像調試流程，依照其順序能在建議的步驟下完成模組校正，並進行基礎主客觀的圖像調試。
- :ref:`5.2 <flat-noise>` 節後彙整了數項圖像品質問題的分析與參考解決方案，可依內容快速定位建議調試的模組並改善問題。

.. _overall-tuning-flow:

5.1 整體的圖像調試流程
-----------------------

圖像調試模組間具有相依性，不見得依照 ISP 模組的前後順序進行。本節依據調試的先後順序整理 1～12 個調試模組，以避免調試不夠精準或需要反覆多次調試的情況。

5.1.1 圖像調試流程區塊圖
^^^^^^^^^^^^^^^^^^^^^^^^^

**(1) 流程概覽**

|image266|

- 根據鏡頭及感測器特性進行的校正項目，依照文件 2.5 校正模組步驟進行校正操作。
- 根據產品定位需求或對比機特性之主觀設定或調整項目。
- 依據環境光源特性動態切換 ISP 參數之生效區間配置。

**(2) 流程順序說明**

.. list-table::
   :header-rows: 1
   :widths: 15 85

   * - 模組
     - 步驟說明
   * - BLC
     - 目的為修正影像感測器的基底值。根據感測器、PCB 不同需重新校正，校正出的數值也可能依增益不同而有所差異。
   * - RNR
     - 基於 BLC 校正結果，分析感測器在各增益值下的雜訊特性分布。根據感測器不同、PCB 改版等情況皆需重新校正。
   * - LSC
     - 修正因鏡頭光學特性使感測器受光不均勻的現象，其補償訊號將進入 AE 統計資訊，需在 AE 調試前完成。根據鏡頭不同需重新校正。
   * - AE
     - BLC 與 LSC 會影響 AEC 統計資料，需完成上述校正模組後再進行 AEC 調試。
   * - 增益區間
     - AE 調試後決定各照度環境下的收斂增益，基於此設定增益區間，使後端 ISP 模組根據區間位置動態配置參數。
   * - Gamma
     - 基於 AE 亮度在各增益區間調試，使畫面亮度呈現接近人眼感官。此步驟完成後，即完成影像亮度基礎調試。
   * - AWB
     - 根據模組特性校正白平衡統計區域及環境色溫判斷曲線，於亮度模組調試後進行。
   * - 色溫區間
     - AWB 校正後可估測各光源下的色溫值，基於此設定色溫區間，使後端 ISP 模組根據區間範圍動態配置參數。
   * - CCM
     - 透過該模組實現色再現目的，基於色溫區間配置不同參數，使各色溫下的顯色能獨立精確對應。
   * - UV tune
     - 基礎調試不建議改動。該模組基於 CCM 再針對特定顏色進行色相、色度調整。至此完成影像亮度、顏色基礎調試。
   * - 編碼設定
     - 編碼設定將影響影像細節紋理的呈現，因此開始 Texture 調試前，需先進行設定。
   * - Texture
     - 針對影像紋理細節調試，建議基於影像亮度、顏色基礎調試的結果後再開始進行。

**(3) 硬體置換確認模組**

影像基礎調試需花費一定時間，當系統某些硬體規格更換後，呈現結果可能與原先調試有所差異。下表整理了更換元件與確認模組的對照關係：

.. list-table::
   :header-rows: 2
   :widths: 20 20 20 20 20
   :align: center
   :class: left-text-table

   * - 動作
     - 重新校正（◎）
     - 不需校正（✕）
     - 基礎調試（○）
     - 實拍確認（△）
   * - 項目
     - 感測器
     - 鏡頭模組更換（光圈不變）
     - 鏡頭模組更換（光圈改變）
     - 供電系統
   * - BLC
     - ◎
     - ✕
     - ✕
     - ◎
   * - RNR
     - ◎
     - ✕
     - ✕
     - ◎
   * - LSC
     - ◎
     - ◎
     - ◎
     - ✕
   * - AWB
     - ◎
     - ◎
     - ◎
     - ✕
   * - CCM
     - ◎
     - ◎
     - ◎
     - ✕
   * - AEC
     - ○
     - △
     - △
     - △
   * - Gamma
     - ○
     - △
     - △
     - △
   * - 增益區間
     - ○
     - △
     - ○
     - △
   * - 色溫區間
     - ○
     - ○
     - ○
     - △
   * - Texture
     - ○
     - △
     - ○
     - △
  
.. _tuning-preparation:

5.1.2 調試前準備
^^^^^^^^^^^^^^^^^

**(1) 實驗器材準備**

- **24-Patch Color Checker**：IQ 校正使用。

  |image267|

- **色度／照度計**：量測環境光源之實際色溫值（K）與照度值（Lux）。

  |image268|

- **輝度箱或均勻透光壓克力板材** （作為 diffuser）：LSC 校正使用。

  |image269| |image270|

- **對色燈箱**：需可調控光源色溫

  |image271|

- **光源**：建議可置換光源如下，供基礎校色使用，可依實際應用情況增減不同種類光源。

  .. list-table::
     :header-rows: 1
     :widths: 20 12 12 12 12 12 12 12

     * - 光源種類
       - D75
       - D65
       - D50
       - CWF
       - U30
       - A
       - H
     * - 標的色溫
       - 7500K
       - 6500K
       - 5000K
       - 4000K
       - 3000K
       - 2850K
       - 2300K

- **標準景**：建議為 D65 或 D50 之可調亮度光源，且光照均勻分布。標準景內容建議包含：

  - 亮暗對比：亮區、暗區
  - 均勻色塊佈景：色卡、絹布毯
  - 各種紋理：毛線、地板、草皮、文字⋯等
  - 解像力測試圖卡：star chart、TV line chart
  - 移動物件：固定軌跡之移動物、人物移動

  |image272|

  .. list-table::
     :header-rows: 1
     :widths: 20 27 27 26

     * - 光源亮度
       - >300 lux
       - 20 lux
       - 3 lux
     * - 模擬場景
       - 戶外或室內辦公等照度充足之環境。
       - 較陰暗的室內環境、夜晚市區等低照度環境。
       - 光源極其微弱的室內、夜晚郊外等極低照度環境。

**(2) 軟體確認**

- 根據手冊 2.2.1 RTSP Streaming 確認可正確輸出影像串流。
- 根據手冊 2.2.2 RAW Streaming 確認可輸出 RAW Data 串流。

5.1.3 基礎影像調試範例
^^^^^^^^^^^^^^^^^^^^^^^

1. **BLC** — 針對感測器上的暗電流進行校正。現今主流感測器通常已自行將暗電流補償至一固定偏移值（pedestal），此偏移值即為 ISP BLC 模組需校正的對象。校正步驟請參閱 :ref:`2.5.1 BLC 校正 <blc-calibration>`\ ；須注意各增益值下的 BLC 數值可能不同。若畫面偏紫或偏綠，且能排除白平衡的影響，通常是 BLC 數值不準確所致：BLC 填入值偏低會使畫面偏紫，偏高則偏綠，暗部區域尤為明顯。

   |image273|

2. **RNR** — 針對感測器與平台供電系統在不同增益值下的雜訊特性進行校正，校正步驟請參閱 :ref:`2.5.2 RNR & 2DNR 校正 <rnr-calibration>`\ 。須注意即使使用相同感測器，若感測器設定或供電條件改變，其雜訊特性均可能隨之改變，需重新校正。

3. **LSC** — 鏡頭光學特性導致感測器中央與邊緣的受光程度不均，產生 Lens Shading 現象。校正步驟請參閱 :ref:`2.5.3 LSC 校正 <lsc-calibration>`\ 。

   |image274|

4. **AE** — AE 模組透過控制曝光時間與增益的收斂來決定影像的基礎亮度。完整參數說明請參閱 :ref:`3.2 AE <ae-module>`。初步調試建議優先確認以下三個方向：

   - **AE target**：畫面亮度的收斂目標值，可根據需求或對比機特性進行設定。須注意 AE target 作用於 RAW domain 的線性亮度，最終影像亮度亦受 Gamma 相關模組影響。

     |image275| |image276| |image277|

   - **Max Gain（total_gain_max）**：AE 允許使用的最大增益值，為 Analog gain、Digital gain、ISPD gain 三者之乘積。Analog gain 上限通常為 16x，ISPD gain 通常為 1x。Max Gain 設定越大，低照度下的顯像能力越強，但雜訊亦越大。

   - **FPS**：每秒幀率，決定相機的最大曝光時間（最大曝光時間 = 1/fps 秒）。AE 模組提供動態降幀機制（``dyn_fps_setting``），當增益值達到設定閾值時自動降低 FPS，藉由延長最大曝光時間來降低增益並維持畫面亮度，可依需求啟用。最大曝光時間越長，低照度顯像能力越強，但動態模糊（Motion blur）也會越明顯。

5. **增益區間** — 完成 AE 基礎配置後，相機在不同環境亮度下會透過曝光時間與增益的動態調整收斂至 AE target。由於增益值與雜訊程度呈正相關，ISP 通常會依據當前收斂的增益值動態切換參數，以在各照度條件下達到最佳畫質。

   AmebaPro2 支援在數個標定增益值下分別配置畫質參數。當 AE 收斂至兩個標定增益值之間時，系統自動以線性內插計算對應的參數值，詳見第 3 章序論的增益生效示意圖：

   |image278|

   增益區間的初步配置，建議以 :ref:`5.1.2 調試前準備 <tuning-preparation>` 所述標準景分別在 3 種照度下量測 AE 收斂增益後進行設定，標定增益值建議略大於實際收斂值。範例如下：

   .. list-table::
      :header-rows: 1
      :widths: 25 25 25 25

      * - 光源亮度
        - 300 lux
        - 20 lux
        - 3 lux
      * - 模擬場景
        - 戶外或室內辦公等照度充足之環境。
        - 較陰暗的室內環境、夜晚市區等低照度環境。
        - 光源微弱的室內、夜晚郊外等極低照度環境。
      * - AE 收斂增益（範例）
        - 1.2x
        - 6.8x
        - 42x
      * - 標定增益（範例）
        - 2x
        - 8x
        - 45x
      * - 調試順序
        - 1st
        - 2nd
        - 3rd

   增益區間的新增與修改方式請參閱 :ref:`4.3 <add-iq-interval>`。上述範例可如下配置，並依照增益由低到高的順序依序調試。基礎調試建議 LSC、GammaRGB、GammaYGC、CCM、UV tune、Texture 等模組採用相同的增益區間配置。

   < Realcam Pro 增益區間設定示意 >

   |image279|

6. **Gamma** — AE 模組決定畫面的整體亮度後，Gamma 各模組會進一步調整整體對比度與亮暗區的分布。AmebaPro2 具備 Gamma 功能的模組共有三種，建議配置相同的增益區間。

   - **GammaRGB** — 位於 CCM 之後的 RGB domain LUT，由 R、G、B 三個通道各自獨立的曲線組成。對色彩影響較大，也是 CCM 校正調試的重要參考依據；若無對比機風格對齊需求，建議以 Gamma 2.2 曲線作為基礎配置。詳細說明請參閱 **3.7 Gamma**。

     .. list-table::
        :header-rows: 1
        :widths: 25 25 25 25

        * - GammaRGB 建議配置
          -
          -
          -
        * - 光源亮度
          - 300 lux
          - 20 lux
          - 3 lux
        * - 標定增益（範例）
          - 2x
          - 8x
          - 45x
        * - 調試順序
          - 1st
          - 2nd
          - 3rd
        * - 配置建議
          - Gamma 2.2
          - Gamma 2.2
          - Gamma 2.2

   - **GammaYGC** — Y Global Curve，位於 GammaRGB 之後的 YUV domain 單通道 LUT，僅作用於亮度分量 Y。對色彩影響較小，基礎調試流程建議設為線性曲線（Linear）。詳細說明請參閱 **3.7 Gamma**。

     .. list-table::
        :header-rows: 1
        :widths: 25 25 25 25

        * - GammaYGC 建議配置
          -
          -
          -
        * - 光源亮度
          - 300 lux
          - 20 lux
          - 3 lux
        * - 標定增益（範例）
          - 2x
          - 8x
          - 45x
        * - 調試順序
          - 1st
          - 2nd
          - 3rd
        * - 配置建議
          - Linear curve
          - Linear curve
          - Linear curve

     以下為 GammaYGC 的調適範例，以 Y Global Curve 的 **Coef** 參數為例，示範兩種不同配置的曲線形狀差異，及其對應的標準景實拍效果。Coef 值越小，曲線對暗部的提升幅度越大，整體畫面對比度越高；Coef 值越大，曲線趨近線性，畫面對比度較低但整體亮度更均勻。

     < GammaYGC 調適範例：Y Global Curve 曲線設定（左：Coef=0.5 高對比；右：Coef=0.6 低對比）>

     |image296|\ |image297|

     < 對應標準景實拍效果對比（左：高對比；右：低對比）>

     |image298|\ |image299|

   - **DRC & DRE** — 主要功能為動態範圍的壓縮與擴展，可達成寬動態影像的效果。進行基礎調試流程時建議關閉此模組。

   .. note::
      - **調試建議**：高照度場景建議以 GammaRGB 為調整主軸；低照度場景建議以 GammaYGC 為調整主軸，以兼顧飽和度與彩噪的綜合表現。
      - **Y Gamma**：位於 ISP 後端，調整節點較少，基礎調試建議維持線性曲線（Linear）設定。
      - **System Gamma**：為 GammaRGB 與 GammaYGC 的乘積，作為整體亮度增強效果的參考值，可於 RealCam 的 Gamma 頁面切換至組合模式觀察。

   完成前述 AE、Gamma 模組以及增益區間配置後，各亮度條件下的影像基礎亮度即已設定完畢，接下來進行顏色相關模組的校正與標定。

7. **AWB** — 感測器的 G channel 感光靈敏度通常優於 R 與 B，因此灰色物體在 RAW data 中呈現偏綠的現象。AWB 模組透過計算並套用一組 R、B 增益（WB gain），使灰色物體在輸出影像中呈現中性灰。AmebaPro2 AWB 模組的校正包含以下兩項核心工作：

   - 透過各色溫光源下的灰色統計點，建構 AWB 候選灰色的統計範圍（即下圖藍綠色與紫色框線所圍繞的區域）。
   - 透過各色溫標定點的連線建立色溫估算曲線。AWB 計算結果（紅點）在該曲線上的最近距離位置即為當前判定的光源色溫。

   |image280|

   詳細校正步驟請參閱 :ref:`2.5.4 AWB 校正 <awb-calibration>`\ 。與 AE 的主觀配置不同，白平衡可透過量測影像中灰色區塊的色彩數值客觀確認是否準確，演算法估算的色溫值亦可與色溫計量測結果進行比對驗證。

8. **色溫區間** — 完成 AWB 校正後即可估測當前環境光源的色溫值。由於不同光譜特性的光源在 AWB 統計上有所差異，顏色相關模組在不同色溫下需透過色溫區間套用對應的參數。色溫區間與增益區間的線性內插不同，採固定切換搭配遲滯機制，以避免色溫估算值在邊界附近時發生參數反覆切換的現象，詳見第 3 章序論的色溫生效示意圖。

   切換機制說明如下：

   - **固定切換**：相同 CT index 內的參數配置固定不變，不進行插值計算。
   - **遲滯機制**：若當前使用 CT index 1 的參數，當環境色溫上升時，估測色溫需超過 CT_2IN 才切換至 CT index 2；若色溫再度下降，估測色溫需低於 CT_2OUT 才切回 CT index 1。色溫區間的新增與修改方式請參閱 :ref:`4.3 <add-iq-interval>`。

   |image281|

   建議先以色溫計實際量測各燈源的色溫值（參見 :ref:`5.1.2 調試前準備 <tuning-preparation>` 的對色燈箱光源表格），並以量測結果為基礎設定色溫區間的切換範圍。若某燈源為常用場景，建議將其切換邊界設定較寬（例如 D50 可設為估測色溫大於 6000K 或小於 4150K 才切換），以降低套用錯誤參數的風險。

   範例如下：

   |image295|

   < Realcam Pro 色溫區間設定示意 >

   |image282|

9. **CCM** — 此模組是實踐「色再現」的核心工具，將感測器的 RGB 響應轉換為接近人眼感知的 RGB 色彩空間，其參數為一組 3×3 矩陣。校正流程請參閱 :ref:`2.5.5 CCM 校正 <ccm-calibration>`\ 。

   |image283| |image284|

   CCM 的動態參數配置通常為「二維」架構，同時具有增益區間與色溫區間兩個動態變因：

   - **色溫區間**：為使各色溫環境下均能精準實踐色再現，建議依前述各色溫區間分別進行校正配置。
   - **增益區間**：除色彩準確度外，可依喜好調整各增益條件下的 CCM 飽和度。高飽和度 CCM 在增益值升高時會放大彩噪，因此建議在高增益條件下適度降低飽和度。飽和度調整方法請參閱 :ref:`2.5.5 CCM 校正 <ccm-calibration>`\ ：

   .. list-table::
      :header-rows: 1
      :widths: 25 25 25 25

      * - 光源亮度
        - >300 lux
        - 20 lux
        - 3 lux
      * - 模擬場景
        - 戶外或室內辦公等照度充足之環境。
        - 較陰暗的室內環境、夜晚市區等低照度環境。
        - 光源微弱的室內、夜晚郊外等極低照度環境。
      * - 飽和度建議
        - 120%～125%
        - 105%～110%
        - 80% 或單位矩陣（CCM bypass）

10. **UV tune** — UV tune 模組將 UV 色彩空間平均切分為 16 個向量，可對各向量獨立進行色相與飽和度調整。由於調整幅度過大時有顏色反轉的風險，基礎調試流程建議不主動調整此模組，配置成與 CCM 相同的增益及色溫區間即可。詳細調試步驟請參閱 **3.16 UV Color Tune**。

    |image285|

    依照前述步驟即完成影像色彩與亮度的基礎配置。後續進行細節紋理調試前，需先完成串流編碼參數的設定。

11. **編碼設定** — 編碼是將通過 ISP 處理後的影像串流進行資料壓縮的過程。雖然編碼設定並非 ISP 參數調整，對色彩與亮度的影響也相對有限，但對細節紋理的完整呈現影響顯著，因此在 Texture 調試開始前需先從 Firmware 中確認以下編碼參數：

    - **Codec**：選擇視訊壓縮標準，依需求設定 H.265 或 H.264。
    - **Bitrate mode**：串流位元率模式。若無特殊需求，建議設定為 CVBR，可在頻寬與影像品質之間取得較佳的平衡。
    - **Bitrate**：串流位元率。若有對比機，建議與其對齊；若自行設定，基於 CVBR 建議設定 Bitrate Min = 1 Mbps、Bitrate Max = 2 Mbps。
    - **GOP**：串流序列的基本存取單位，建議設定為 frame rate 的 2 倍。例如 FPS 為 30 時，GOP 應設為 60。

    .. tip::

       可於以下範例檔案中修改相關定義：

       .. code-block:: c

          #define V1_BPS    2*1024*1024  // Bitrate（2 Mbps）
          #define V1_RCMODE 2            // Rate control mode：1 = CBR，2 = VBR

          #define USE_H265  0            // 0 = H.264，1 = H.265
          #if USE_H265
          #define VIDEO_TYPE  VIDEO_HEVC
          #define VIDEO_CODEC AV_CODEC_ID_H265
          #else
          #define VIDEO_TYPE  VIDEO_H264
          #define VIDEO_CODEC AV_CODEC_ID_H264
          #endif

       .. code-block:: none

          位置：project/realtek_amebapro2_v0_example/src/mmfv2_video_example/mmf2_video_example_v1_init.c
          GitHub：https://github.com/Ameba-AIoT/ameba-rtos-pro2/blob/main/project/realtek_amebapro2_v0_example/src/mmfv2_video_example/mmf2_video_example_v1_init.c

12. **Texture** — Texture 並非單一 ISP 模組，而是數個與細節紋理相關模組的統稱。其增益區間及調試準則如下，若有明確的對比機風格目標，則以對比機表現為優先參考：

    .. list-table::
       :header-rows: 1
       :widths: 20 26 27 27

       * - 光源亮度
         - 300 lux
         - 20 lux
         - 3 lux
       * - 模擬場景
         - 戶外或室內辦公等照度充足之環境。
         - 較陰暗的室內環境、夜晚市區等低照度環境。
         - 光源微弱的室內、夜晚郊外等極低照度環境。
       * - 標定增益（範例）
         - 2x
         - 8x
         - 45x
       * - 調試順序
         - 1st
         - 2nd
         - 3rd
       * - 調試準則
         - 盡量提升場景細節紋理的銳利度，但避免過度強化而產生明顯的高頻跳動雜訊。
         - 在避免明顯雜訊的前提下維持細節清晰度，允許極輕微的拖影。
         - 盡量抑制平坦區雜訊，避免銳利化模組同時強化雜訊；在微量拖影與跳動雜訊之間取得平衡。

    以下介紹基礎調試流程中需確認的重要模組：

    - **INTP** — 將 RAW data 去馬賽克並重建為 RGB 色彩影像的模組，調試說明請參閱 :ref:`3.19 INTP <intp-module>`。INTP 依據畫面各區域的特徵類型（平坦區、紋理、邊緣等）選用對應的插補演算法，因此調試重點在於確認各區域的特徵分類是否準確。工具提供 Feature Detection Debug Mode，以不同顏色標示各影像特徵類別。

      由於平坦區與細節紋理之間存在判斷模糊地帶，建議高照度場景以判定為細節紋理為主；低照度高增益場景以判定為平坦區為主，以有效抑制雜訊：

      |image287| |image289|

      |image288| |image290|

    - **Noise Reduction** — AmebaPro2 的主要降噪模組，需基於 RNR 校正結果才能發揮最佳效果，調試說明請參閱 :ref:`3.20 Noise Reduction <nr-module>`。模組由 SNR（空間軸降噪）與 3DNR（時間軸降噪）組成，3DNR 透過多幀疊合可完整還原影像細節；但若畫面中存在移動物體，幀間位置差異可能導致疊合錯誤，造成移動物拖影。

      模組提供 Motion Detection 功能，對判定為移動物件的區域改用 SNR 處理。可透過 Debug Mode 以色彩標示動靜區域，確認閾值設定是否適當（靜止區域顯示為黑色，移動區域顯示為白色）：

      |image291| |image292|

      此模組的基礎必要程序為：

      - 完成 RNR 校正並導入 Noise Curve 參數。
      - 透過 Debug Mode 確認移動物閾值設定是否適當。

    - **Edge Enhance** — ISP 後端用於提升細節紋理清晰度的重要模組，調試說明請參閱 :ref:`3.22 Edge Enhance <eeh-module>`。調試的主要原則為適度強化紋理與邊緣線條，同時避免對平坦區的高頻雜訊造成放大。必要調試項目為 Y Sharp 功能的兩個部分：

      - 利用 Sharp Edge Map 確認強化區域是否準確（平坦區應顯示為黑色，紋理與邊緣應顯示為白色）：

        |image293|

      - 調整 Y Sharp rate0、rate1 的強度數值，達到適當的強化程度：

        |image294|

    Texture 各模組的調試結果相互影響，因此 RealCam 提供多種 Debug Map 以輔助確認各模組的參數配置是否適當。從 :ref:`5.2 <flat-noise>` 開始彙整了各類畫質問題的調試範例，可作為遭遇問題時的排查參考。

.. _flat-noise:

5.2 平坦區雜點顯現
-------------------

**(1) 問題描述**

Sensor Model: JXF37H

20 Lux 場景，在黑布上，會有白色噪點在跳動。

|image306|

**(2) 調試過程、結果**

開啟 **Feature Detection Debug Mode**，觀察黑布區域，可見大部分像素落在平坦區，但仍有少數細小的 impulse 雜訊被歸類至 texture 或 edge 區域。

|image307|

提高 INTP Denoise 的強度，經測試，適度調高 ``TH1`` 即可有效改善此問題。

|image308|

可觀察到紅圈處黑布的白色噪點有所改善，但藍框處草皮的清晰度隨之降低。

|image309|

切換至 Edge Enhance 頁面，開啟 **Sharp Edge Map Enable**\ （Debug 模式）。

|image310|

可觀察到紅圈處黑布幾乎未被銳化增強，藍框處草皮區域也僅有少量被增強。

|image311|

調整亮度閾值，使得紅圈處黑布區域保持不被加強，但藍框處草皮區域加強變多。

|image312|

最終效果為：黑布白色雜點消除，而草皮清晰度略降。

|image313|

|image314|

5.3 低照度下的減輕移動物體拖影
---------------------------------

**(1) 問題描述**

Sensor Model: JXF37H

在低照度（10 Lux）場景下，移動物體會有拖影的現象。

|image315|

**(2) 調試過程、結果**

開啟 Motion Detection 頁面的 **Debug Mode**。

|image316|

觀察 Debug 結果，可見移動區域（motion）與靜止區域（static）的判定界線相當清晰。

|image317|

適度調降 Motion Detection 的閾值，讓移動物體經過的背景區域在物體移走後，短暫延遲進入 static 判定、持續套用 SNR 單幀處理；待該區域正式切回 static 並開始 TNR 多幀疊合時，畫面中已累積更多無移動物體的幀，可有效減輕拖影現象。

|image318|

但此時其他靜態區域的雜訊也會隨之增加。

|image319|

最終效果為：拖影減輕，但相對的靜態區域的雜訊增加。

|image320|

5.4 低照度下的色斑雜訊的處理
--------------------------------

**(1) 問題描述**

Sensor Model: JXF37H

在低照度場景下的色斑雜訊。

|image321|

**(2) 建議流程**

|image322|

**(3) 實際範例**

**UV Color Tune** — 看到問題影片，色斑雜訊多為黃綠色色斑雜訊，檢視 UV Tune，推測綠斑雜訊可能是 UV Tune 強化出來的。調整 UV Tune 回原始值，犧牲一些綠色物體的表現，來換取改善綠斑雜訊。

< 調整前（左）/ 調整後（右）>

|image323|\ |image324|

結果：水泥、柏油（紅框）的綠斑雜訊有改善，而綠樹（黃圈）變得較不翠綠。

|image325|

**暗部區域利用FCR & MCR & UVS \\ UV Suppression改善彩噪**

調整前：灰色物體上有許多黃綠色色斑雜訊；調整後：保持高彩物件的色彩，將灰色物體上的色斑雜訊消除。

< 調整前（上）/ 調整後（下）>

|image326|
|image327|

|image328|
|image329|

**亮部區域利用DRC & DRE \\ Over Exposed Protection（過曝區域色彩保護機制）改善彩噪**

調整前：中、低亮度的灰色區域色斑雜訊不嚴重，而高亮區域有綠色色斑；調整後：高亮區域的綠色色斑良好的消除掉，遠處色板有稍微掉色。

< 調整前（上）/ 調整後（下）>

|image330|
|image331|

|image332|
|image333|

5.5 低照度下斑狀色彩雜訊減緩，平坦區去噪與明顯邊緣增強
-----------------------------------------------------------

**(1) 問題描述**

Sensor Model: SC3235

在低照度場景（Gain > 100x）下，維持一定飽和度要求時畫面有斑狀色彩雜訊，且平坦區有顆粒感噪聲。

|image334|

**(2) 調整概念與步驟**


**斑狀色彩雜訊** — 要緩解色斑雜訊，可將 CCM 的飽和度降低，透過提高 Video Property 的 Saturation 以維持接近的畫面飽和度。

降低 CCM 飽和度：

|image335|

提高 Video Property 中的 Saturation 設定（設定值為 64 相當於不調整的效果）：

|image336|

**平坦區顆粒感噪聲** — 透過觀察 INTP 中的 Denoise Debug Mode，發現平坦區域中有部分點狀像素被誤判為綠色（非平坦區處理）；適度調高 ``TH0`` / ``TH1``\ ，讓平坦區域像素盡量落入黑色分類（平坦區處理）。

|image337|

開啟 INTP 中的 Denoise Debug Mode，平坦區域中有部分點狀會被判定成綠色：

|image338|

適度調高 ``TH0`` / ``TH1``\ （需注意避免影響明顯邊緣）：

|image339|

盡量讓平坦區域落入黑色：

|image340|

**明顯邊緣增強** — 因 INTP 避免放出平坦區噪點，會有較大信心不會增強到平坦區噪點，可適度加強明顯邊緣。將 Edge Enhance 的 ``Sharp Rate1`` 增加，增強明顯邊緣：

|image341|

**(3) 調整效果**

緩解斑狀色彩雜訊，平坦區去噪與明顯邊緣增強。

|image342|

5.6 邊緣異色處理
------------------

**(1) 問題描述**

IC Model: Amebapro2

畫面中高對比邊緣處，出現異色現象。

|image343|

|image344|

**(2) 調整概念與步驟**

要緩解邊緣異色，有下列 4 個相關功能可以嘗試，建議依照介紹順序依序進行確認。

**I. INTP Log Mode**

檢查 FCR & MCR & UVS 頁面中，INTP Log Enable 設定是否開啟，一般預設建議為開啟，無需做更改。

INTP Log Enable 開啟前後的對比（開啟後，邊緣偏色的顏色飽和度會些微下降，視覺感受上較不明顯）：

|image345|

< INTP Log Disable（左）/ INTP Log Enable（右）>

|image346|

**II. INTP FCR**

檢查 FCR & MCR & UVS 頁面中，INTP FCR 設定是否合適。不同的 INTP FCR 設定，效果對比如下（當 INTP FCR 的設定不合適時，會使邊緣偏色的現象越趨明顯）：

|image347|

< 疊加合適的 INTP FCR 配置不會有邊緣偏色（左）/ 不當的 INTP FCR 配置會有邊緣偏色（右）>

|image348|

**III. EEH Reduction**

檢查 FCR & MCR & UVS 頁面中，EEH Reduction Enable 設定是否開啟，以及調高 EEH Reduction Strength。EEH Reduction Strength 設定為最高時，開啟前後的對比如下（開啟後，邊緣偏色的顏色飽和度會再些微下降，視覺感受上較不明顯）：

|image349|


< EEH Reduction Disable（左）/ 疊加 EEH Reduction Enable （右）開啟後邊緣偏色更不明顯>

|image350|

**IV. CAC Function**

調整 CAC Function 設定，使偏色區域落到 CAC 作用範圍內。CAC Function 中有設計 Debug 功能，可用以輔助觀察作用的區域，相關說明請參閱 :ref:`3.14 CAC <cac-module>`。

.. note::
   CAC Function 調整上要注意，對於偏垂直方向的邊緣異色效果較佳，水平方向的抑制效果較差。強度增加可以提高緩解邊緣異色的效果，但如要透過此功能完全消除水平方向的邊緣異色，可能的 Side Effect 是會導致其他區域的顏色損失，因此 CAC Function 應作為緩解邊緣異色的最後手段使用。

使偏色區域落到 CAC 作用範圍設定下，CAC Function 開啟前後的對比（開啟後，邊緣偏色的顏色飽和度會再下降，視覺感受上較不明顯）：

< CAC Disable（左）/ 疊加 CAC Enable （右）開啟後邊緣偏色更不明顯>

|image351|

**(3) 調整效果**

調整 4 個相關功能前後對比差異如下：

|image352|

5.7 CCM 色彩手動微調
----------------------

**(1) 問題背景**

Sensor 的 RGB 分量對光譜的響應與人眼不同，CCM（Color Correction Matrix）負責將 Sensor RGB 空間轉換為人眼感知的 RGB 空間。

< CCM 校正前後效果比對 >

|image356|

使用 Sensor 抓拍到的 24 色卡場景下前 18 個色塊的實際顏色資訊與期望值，計算 3×3 CCM 矩陣。輸入顏色經 CCM 矩陣處理後的結果與期望值差距越小，CCM 矩陣越理想。

|image357|

.. note::
   此校正流程所產出的 CCM 適用於正常亮度環境。在不同增益條件下，可基於校正結果進一步調整飽和度與色相（操作步驟請參閱 :ref:`2.5.5 CCM 校正 <ccm-calibration>`\ ）。

在不同色溫下，CCM 係數差異較大，ISP 中一般存有多組 CCM 係數，由 AWB 估算出當前色溫後選擇合適的一組或進行線性混合。低照度高增益情況下，有時也會縮小 CCM 係數的絕對值以降低彩噪。

CCM 調適為\ **高度客製化**\ 的流程，即使相同的 Sensor + Lens 組合，不同客戶之間也會因喜好或主客觀測試標準而有不同的參數配置。

**(2) 調試過程：手動微調特定色塊**

以下示範針對 24 色卡第 13 個色塊（藍色）進行手動微調，使其更接近標準色值的操作流程。

透過圖像分析工具量測，可得知 Patch 13 的實際 RGB 值為 **(52, 65, 254)**，而標準色卡的期望值為 **(39, 63, 147)**，紅色分量偏高，需降低。

< 24 色卡（左）與 Patch 13 位置標示（右）>

|image358|\ |image359|

< Patch 13 標準 RGB 值 >

|image360|

進入 **ISP Tuning Pro → CCM** 頁面，取消勾選 **Base/Tuned**，使 CCM 矩陣與 Final Result 一致，再關閉 Dynamic Control。降低紅色分量，得到新的 CCM 係數如下：

|image361|

RealCam 提供 CCM 防呆機制，視窗右側會顯示三組係數總合是否為 256，供調試者判斷當前中性色是否色偏：

|image362|\ |image363|

**(3) 調整效果**

Patch 13 的 R 分量從 52 降至 39，紅色整體飽和度下降，色彩更接近標準值：

|image364|

5.8  ISP 畫質異常來源診斷流程
-----------------------------

當畫面出現異常紋理或雜訊時，建議依以下步驟快速定位問題來源：

**步驟一：釐清 ISP 模組影響**

- **EEH**：直接關閉 Edge Enhance 模組（Enable → Disable），觀察問題是否消失。若消失則問題源自 EEH 後端處理，可針對 Y Sharp 與 Smooth 參數進行調整。
- **INTP**：INTP 模組無法關閉，但可開啟 **Feature Detection Debug Mode** 觀察平坦區雜訊是否被錯誤歸類為紋理或邊緣區，再依分類結果調整對應參數（請參閱 :ref:`3.19 INTP <intp-module>`）。

**步驟二：擷取 RAW Data 進行分析**

若問題現象（如水平條紋）懷疑源自供電雜訊等 Sensor 端問題，建議：

1. 調整場景至便於觀察的狀態（例如對黑布拍攝）。
2. 擷取 RAW Data 後，以工具提升 Mean 與 Contrast，增加問題的可視性。
3. 將 R、Gr、Gb、B 各通道單獨顯示，避免不同 Channel 像素值差異干擾判斷。
4. 如有必要，進一步擷取 ISP 不同處理節點的輸出資料進行逐節比對。

5.9 常見畫質問題調整建議
------------------------

**拖影與雜訊平衡**

調整 Motion Detection 的 ``thd0`` / ``thd1`` 閾值，在靜態區域降噪效果與移動物件拖影之間取得平衡。靜態區域由 3DNR 多幀疊合處理，移動物件區域則切換至 SNR 單幀處理，以避免幀間錯誤疊合。詳見 **5.3**。

**區域對比偏弱**

調整 Y Global Curve（GammaYGC）及 Video Property 的對比度參數，同時注意不要過度放大雜訊。

**偽彩（色邊）**

依偽彩分布位置選擇對應模組：

- 摩爾紋區域 → 調整 :ref:`3.15.2 MCR <mcr-module>`
- 邊緣區域 → 調整 :ref:`3.15.1 FCR <fcr-module>`；改善有限時再考慮 :ref:`3.14 CAC <cac-module>`\ （對垂直邊緣效果較佳，水平邊緣抑制有限，且需注意是否造成正確色彩損失）
- 灰色區域大面積低頻彩噪 → 透過 :ref:`3.15.3 UVS <uvs-module>` 改善（需確認是否影響飽和色彩表現）

**環境溫度與熱雜訊**

高溫環境下硬體發熱會產生熱雜訊。BLC 模組可依增益區間動態補償，但作用範圍僅限已配置的曝光區間；Sensor 本身因發熱產生的熱雜訊無法由 ISP 直接偵測，除非各溫度下的曝光參數為固定值。詳見 :ref:`3.1 BLC <blc-module>`。

**低光色度噪聲**

極低照度下建議搭配 RNR 校正結果配置 SNR 與 3DNR 強度。3DNR 多幀疊合有助細節還原，但移動物件易產生拖影；可調整 Motion Detection ``thd0`` / ``thd1`` 閾值區隔動靜區域，靜態區以 3DNR、動態區以 SNR 處理。詳見 :ref:`3.20 Noise Reduction <nr-module>`。

**高頻紋理區域過度平滑**

在 NR 處理中，隨機噪點與真實紋理（如毛髮、織物）的特徵相近，處理不當易造成細節損失。排查方式如下：

開啟 INTP 的 Denoise Debug Mode，觀察平坦區中是否有像素被誤判為綠色（非平坦區處理）。

< INTP Denoise Debug Mode 示意圖 >

|image368|

適度調高 ``TH0`` / ``TH1``\ ，使平坦區像素落入黑色分類（平坦區處理）。

|image367|

再參照 :ref:`3.20.3 NR Detail Extraction <nr-detail-extraction>`，設定 ``X0`` ～ ``X3`` 以區分平坦、紋理、邊緣區，並對各區配置適當的 SNR 強度：

< NR detail extraction 參數配置示意 >

|image369|

根據不同亮度區間，分別定義 SNR 強度及作用區域配置。

6. IQ / ISP 機制深入說明
--------------------------

本章彙整 AE、AWB 等核心 ISP 模組的內部運作機制與模組補充調試說明，適合需要進一步理解參數背後邏輯的調試人員參考。

6.1 AE 控制機制與調試參數
--------------------------

6.1.1 Sorting AEC 作用機制
^^^^^^^^^^^^^^^^^^^^^^^^^^

Sorting AEC 主要針對高對比場景，調整亮暗區域的計算權重，改變最終使用的 Y Mean 數值。系統將 Metering Base Y Mean 與 Sorting Base Y Mean 依比例混合後作為 AE 收斂的依據。

Sorting Table 由 16×16 的權重矩陣構成，各格依亮度由暗至亮排列，共有兩組：

- **Low Table**：各區塊權重差異小，影響力趨於均等。
- **High Table**：各區塊權重差異大，對特定亮度區域有較強的側重。

Sorting Table 支援四種運作模式，透過 ``sort_mode`` 參數設定：

- **Auto**：依過曝區域比例及閾值參數（``saturated_range_th1`` / ``th2`` / ``th3``）自動決定 High / Low Table 的混合比例。
- **Manual**：手動指定 High / Low Table 的混合比例。
- **Dark**：以暗部亮度補償為主；可依不同 Gain / ETGain 設定對應的混合比例。
- **Bright**：以過曝保護為主；可依不同 Gain / ETGain 設定對應的混合比例。

詳細參數說明請參閱 :ref:`3.2.3 AE Attribute Extension <ae-attribute-ext>`。

6.1.2 AEC Fast / Slow Mode 切換機制
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

AE 收斂過程分為 Fast Mode 與 Slow Mode，切換依據為 AE Step（= ``ymean_target`` ÷ ``y_mean``\ ）：

- **Slow Mode**：AE 處於穩定狀態，每次調整量小且固定，避免畫面亮度震盪。
- **Fast Mode**：AE Step 超過 ``ae_enter_slow_mode_th`` 閾值時切換至此模式，調整量大且動態，快速逼近目標亮度；待亮度再次收斂後自動回到 Slow Mode。

詳細參數說明請參閱 :ref:`3.2.2 AE Attribute <ae-attribute>`。

6.1.3 AE 目標值的動態調整機制
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

AE 目標值（``y_mean_target``）支援兩種動態降低機制，兩者同時啟用時取降幅較小者：

- **依 ETGain 降低目標值**：ETGain 越高代表環境越暗，此時降低目標值可避免 Gain 過高而累積大量雜訊。僅能降低目標值，不可提升。
- **依過曝區比重降低目標值**：過曝像素佔比越高時降低目標值，縮小過曝範圍。僅能降低目標值，不可提升。

Histogram 機制：當 Y Mean 落於 ``y_mean_target_l`` 與 ``y_mean_target_h`` 之間時，系統進一步分析亮暗區域的直方圖比例，依參數閾值動態調整收斂位置。

詳細參數說明請參閱 :ref:`3.2.3 AE Attribute Extension <ae-attribute-ext>`。

6.1.4 AE 穩定與防振盪機制
^^^^^^^^^^^^^^^^^^^^^^^^^

AE 穩定狀態的判斷採遲滯設計，需滿足 ``ae_enter_stable_th < ae_exit_stable_th``：

- **進入穩定狀態**：AE Step 持續落在 ``ae_enter_stable_th`` 範圍內達到一定時間後，AE 進入穩定狀態。
- **維持穩定狀態**：AE Step 介於 ``ae_enter_stable_th`` 與 ``ae_exit_stable_th`` 之間時，系統先確認景物相似度（same block 機制），若判定為靜態場景則維持穩定。
- **離開穩定狀態**：進入穩定後，需持續滿足 ``ae_stable_delay`` 所設定的時間後，才允許 AE 重新調節。

景物相似度穩定機制（same block）：將畫面切分為 16×16 個區塊，統計亮度變化低於 ``y_mean_same_block_diff_th`` 的靜止區塊數量，依數量對應判斷進入 ``AE_SAME_BLOCK_STABLE``、``AE_SAME_BLOCK_DELAY`` 或 ``AE_SAME_BLOCK_UNSTABLE`` 三種狀態。

詳細參數說明請參閱 :ref:`3.2.2 AE Attribute <ae-attribute>`。

6.1.5 室內 / 室外光源的 AEC 曝光處理機制
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

當曝光時間短於最小 Anti-Flicker 時間（50Hz：10 ms；60Hz：8.3 ms）且 AE 尚未達到穩態時，系統依以下條件判斷是否進入高光模式：

- **進入高光模式**：Y Mean 超過 ``y_mean_enter_high_light_th`` 時進入。高光模式下，AE 不受 Anti-Flicker 時間限制，可繼續縮短曝光時間。
- **離開高光模式**：曝光時間回到 Anti-Flicker 倍數，且 Y Mean 低於 ``y_mean_exit_high_light_th`` 時離開。

詳細參數說明請參閱 :ref:`3.2.2 AE Attribute <ae-attribute>`。

6.1.6 AEC 影響的 ISP 動態參數切換機制
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

AE 收斂的 Gain / ETGain 決定當前所在的增益區間，進而觸發各 ISP 模組的動態參數切換或線性內插。各模組可依增益或色溫條件分別配置獨立參數，詳細說明請參閱各模組的調校說明章節，以及 :ref:`4.3 新增 IQ 參數區間 <add-iq-interval>`。

6.2 AWB 控制機制與調試參數
--------------------------

6.2.1 AWB 校正流程摘要
^^^^^^^^^^^^^^^^^^^^^^

基本校正流程請參閱 :ref:`2.5.4 AWB 校正 <awb-calibration>`\ ，核心步驟如下：

- 手動設定各色溫燈源對應的 **CT Point** （標準色溫點座標），系統依此自動擬合 ``CT_Curve_Fit_Line``。
- ``white_area``、``gray_area``、``intp_param``、``end_ratio`` 等作用區參數建議先使用預設值，待色溫點設定完成後，再依實際場景統計分布調整 ``normal_weight`` 及作用區範圍。

詳細參數說明請參閱 :ref:`3.12.2 AWB Attribute <awb-attribute>`。

6.2.2 Final WB Gain 計算方式
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

AWB 輸出的最終白平衡增益（Final WB Gain）由以下三個階段依序計算：

1. **Rough Gain**：

   - 依 ``white_area`` 與 ``gray_area`` 的統計點，以 ``normal_weight``\ （或 MIXCT weight）進行加權平均，得到 ``Rough WB_Gray``。
   - 若啟用綠區功能，同時計算 Rough WB_Green，最終依色溫區域與綠區的統計點數量比例混合兩者。

2. **Fine Gain**：以 Rough Gain 的收斂位置為中心，對周圍統計點再次計算均值，得到更精準的修正量；修正幅度受 ``fine_gain_th`` 限制。

3. **Final Gain = Fine Gain × Rough Gain**。

6.2.3 色溫估算方式
^^^^^^^^^^^^^^^^^^

AWB 色溫估算流程如下：

1. 各手動校正的色溫點（標有實際量測色溫值）構成色溫擬合曲線（``CT_Curve_Fit_Line``）。
2. 將 Final WB Gain 映射至擬合曲線上，找出最近距離的對應位置。
3. 依相鄰標準色溫點的色溫值進行線性內插，估算出當前場景的實際色溫值。

6.2.4 進階功能：綠區（Green Area）運作機制
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

當場景中存在大面積綠色物體（如植物、草地）時，AWB 容易因綠色像素過多而誤判環境色溫。綠區功能提供一組獨立的 WB 增益計算路徑（Rough WB_Green），與正常灰白區的 Rough WB_Gray 依比重混合，使白平衡結果不受大面積綠色干擾。

校正步驟依序為：

1. 規範綠區邊界範圍（``x_min`` / ``x_max`` / ``y_min`` / ``y_max`` / ``b_delta``\ ）。
2. 設定 D65 / D50 基準色溫點座標（``d65`` / ``d50``\ ）。
3. 設定綠區均值點到色溫曲線的投影關係（``k_green_est`` 及 d→b 映射參數）。

詳細參數說明請參閱 :ref:`3.12.3 AWB Attribute Extension 1 <awb-ext1>`。

6.2.5 進階功能：混合色溫（MIXCT）運作機制
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

當場景中同時存在高色溫與低色溫光源時，AWB 可能在兩種色溫之間反覆切換。MIXCT 功能透過監控高低色溫統計點的比值，自動切換 AWB 所使用的統計權重表（``h_ct_weight / l_ct_weight``），使白平衡穩定收斂於佔比較高的光源色溫。

運作流程如下：

1. 高色溫或低色溫區域統計點數量超過 ``enter_num_th`` 後，進入 ``AWB_MIXCT_MODE``\ 。
2. 依高低色溫比值判斷套用\ **高色溫模式**\ （``h_ct_weight``）或\ **低色溫模式**\ （``l_ct_weight``）。
3. 持續監控比值，依遲滯閾值判斷是否切換子模式或退出 ``AWB_MIXCT_MODE``\ 。

詳細參數說明請參閱 :ref:`3.12.4 AWB Attribute Extension 2 <awb-ext2>`。

6.2.6 AWB 穩定與防振盪機制
^^^^^^^^^^^^^^^^^^^^^^^^^^

**AWB Stable 判斷**：比較本幀與前幀 Final WB Gain 的差異是否超過 ``final_gain_diff_th`` ：

- 差異未超過閾值：AWB 進入穩定狀態，保持當前增益不更新。
- 穩定狀態下的閾值帶寬會自動加寬（± ( ``final_gain_diff_th + magic num`` )），提供遲滯保護，防止頻繁切換。此 ``magic num`` 為韌體內部固定常數，不可調整。

**AWB Hold 條件**：以下任一條件成立時，AWB 進入 Hold 狀態，暫停增益更新：

- ``awb_stable_delay`` ：AWB 進入穩定後，在設定時間內維持 Hold 狀態。
- ``rg_bg_num_th`` ：色溫區域內的統計點數量低於此閾值，表示畫面中可供統計的白色區域不足。
- ``min_bright_th`` ：AE 統計的畫面亮度低於此閾值，表示環境過暗，AWB 統計資訊不可靠。

詳細參數說明請參閱 :ref:`3.12.2 AWB Attribute <awb-attribute>`。

6.2.7 Pro1 / Pro2 灰區統計資訊映射差異
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. list-table::
   :header-rows: 1
   :widths: 20 40 40

   * - 項目
     - AmebaPro 1
     - AmebaPro 2
   * - WB 統計點數
     - 100
     - 256
   * - 灰區外統計點處理
     - 映射至灰區邊緣，確保有足夠的統計點參與計算
     - 不納入計算，僅統計灰區內的像素
   * - 區域劃分方式
     - 單一灰區
     - 依色溫擬合曲線分為白區與灰區，各區域再依色溫切分為多個 Block，各 Block 統計權重獨立配置

6.3 模組補充調試說明
--------------------

6.3.1 DPC 調試經驗
^^^^^^^^^^^^^^^^^^

**壞點類型分類**

下圖為業界常用的壞點類型定義，AmebaPro2 DPC 將內部壞點歸納為以下兩種：

< DPC 壞點類型示意圖 >

|image373|

- **Single Defect**：涵蓋上圖的 Single Defect 與 Serial Defect（單一或連續鄰近壞點）。
- **Cluster Defect**：涵蓋上圖的 Jumped Defect 與 Cluster Defect（跳躍式或群聚壞點）。

.. note::
   各 Sensor 廠對 Gr / Gb 是否視為同一 Channel 的定義不同，可能影響壞點類型的歸類結果，使用前請確認所用 Sensor 規格書的定義。

**Pro1 / Pro2 差異與調試建議**

AmebaPro2 與 AmebaPro1 的 DPC 做法相同，均支援 Single Defect 與 Cluster Defect 兩種壞點類型。調試建議如下：

- Cluster Defect 補償會大量損失影像細節，建議優先由 Sensor 端把關，在 ISP 端盡量不啟用。
- 實務上僅在極高增益（約 256x 以上）時，才考慮啟用 Cluster Defect 補償以緩解 impulse noise。

詳細參數說明請參閱 :ref:`3.18 DPC <dpc-module>`。

6.3.2 LDC 調試經驗
^^^^^^^^^^^^^^^^^^

AmebaPro2 的 LDC 算法與 AmebaPro1 相同，支援水平方向（桶型畸變）的校正。調試重點在於 FOV（視角）與校正量之間的平衡：

- 先依應用需求確定目標 FOV，再以此為基準調整畫面中心與邊緣的視野占比。
- 過度校正會使邊緣畫質劣化或裁切有效視角；校正不足則畫面邊緣仍有可見畸變。

詳細參數說明請參閱 :ref:`3.24 LDC <ldc-module>`。

6.3.3 Dynamic Range 客觀畫質量測
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

目前採用 **TE241 chart** 進行動態範圍量測（可依客戶需求改用 ITDR 36，支援 100 dB 或 150 dB 量測範圍）。量測指標以 **Imatest** 工具輸出的 SNR10 與 SNR1 的動態範圍值（DR）作為客觀畫質標準。

.. note::
   目前量測流程已建立，測試 criteria（合格標準）尚在規範制定中。

6.3.4 LSC 調試經驗
^^^^^^^^^^^^^^^^^^

AmebaPro2 的 LSC 校正建議使用 DNP 光源燈箱或壓克力板，確保校正時的光源均勻度。不同色溫（如 D50 與 D65）均會影響校正結果；RealCam 支援針對各色溫分別進行 LSC 校正，不同色溫區間的校正結果會進行線性內插，以平滑過渡。

均勻性驗證標準依各客戶規格書而定，建議在校正前確認目標規格。詳細校正流程請參閱 :ref:`2.5 校正模組 <section-2-5>` 及《AmebaPro2 IQ Tuning Manual_Calibration》。

6.3.5 Max FPS 與 Min FPS 配置說明
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

AmebaPro2 的影像幀率由兩個獨立參數控制：**Max FPS**\ （開流最高幀率）與 **Min FPS**\ （ISP 可降至的最低幀率）。兩者共同決定 ISP 在不同照度下的幀率動態範圍。

**幀率動態調整機制**

幀率並非固定值，ISP 會依 IQ Bin 中設計的 P_Chart，在不同曝光階段階段性地降低幀率以延長曝光時間（降幀長曝）。各路串流的實際幀率，以開流時設定的最高幀率為基準按比例計算：

- **Max FPS**：開流時設定的幀率，即該路串流的最高幀率上限；可參考 ``sensor.h`` 中的 fps 配置。
- **Min FPS**：由 IQ Bin 決定，為 ISP 降幀的下限幀率。

**多路串流降幀範例**

以雙路串流為例，說明各路幀率的連動關係：

- CH0 開流 20 fps（ISP 最大幀率）；CH4 開流 10 fps（為最大幀率的一半）。
- 當場景變暗觸發降幀，ISP 幀率由 20 fps 降至 10 fps 時，CH4 幀率依開流比例同步調整：10 fps → 5 fps。

**API 配置方式**

可透過以下 API 在開流時動態設定或查詢 Max / Min FPS：

.. code-block:: c

   int isp_set_max_fps(int val);
   int isp_get_max_fps(int *pval);
   int isp_set_min_fps(int val);
   int isp_get_min_fps(int *pval);


.. note::
   建議於開流完成後，主動呼叫 ``isp_set_min_fps()`` 再覆寫一次最低幀率，以確保 IQ Bin 中的設定不被系統預設值蓋掉。完整 API 定義請參閱 ``isp_ctrl_api.h`` ：

   .. code-block:: none

      位置：component/video/driver/RTL8735B/isp_ctrl_api.h
      GitHub：https://github.com/Ameba-AIoT/ameba-rtos-pro2/blob/main/component/video/driver/RTL8735B/isp_ctrl_api.h

6.4 RealCam 工具說明
--------------------

6.4.1 RealCam 離線授權申請
^^^^^^^^^^^^^^^^^^^^^^^^^^

**現象**：RealCam 需要離線授權，無法透過網路自動完成註冊。

**處置方式**：

1. 開啟 RealCam，於授權頁面匯出註冊申請檔 ``Realcam_activation.bin``。
2. 將該檔案寄送給系統管理員申請授權。
3. 系統管理員核發後，將 activation key 發送至指定信箱。
4. 依信件指示輸入 activation key，即可完成離線註冊。

.. _iq-bin-layout:

6.4.2 IQ Bin 空間配置說明
^^^^^^^^^^^^^^^^^^^^^^^^^^

``firmware_isp_iq.bin`` 為燒錄至 FW 的最終封裝檔，其內部空間配置說明如下：

- 總空間：**1 MB**，可容納 4～5 組 IQ bin 組合。
- 每組 IQ bin 大小上限約 **128 KB**。

< firmware_isp_iq.bin 空間配置示意圖 >

|image377|

.. note::
   詳細的空間配置規劃與多組 IQ bin 的組合方式，請參閱 **AN0700 Application Note 第 2.4 節**。

6.4.3 Anti-Flicker 頻率設定（50 Hz / 60 Hz）
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Anti-flicker（防閃爍）功能透過設定 AE 的電源頻率偵測值，使曝光時間與市電頻率同步，避免畫面出現閃爍條紋。

**設定方式一：AT Command**

.. code-block:: none

   ATIC=1,0x0018,<value>

**value 對照表**：

.. list-table::
   :header-rows: 1
   :widths: 20 80

   * - value
     - 說明
   * - 0
     - Disable（關閉防閃爍）
   * - 1
     - 50 Hz（適用台灣、歐洲、中國等地區）
   * - 2
     - 60 Hz（適用美國、日本等地區）
   * - 3
     - Auto（自動偵測頻率）

**查詢目前設定值：**

.. code-block:: none

   ATIC=0,0x0018

**設定方式二：API 呼叫**

可修改 ``isp_ctrl_api.h`` 。

.. code-block:: c

   /* 設定 */
   isp_set_power_line_freq(int val);

   /* 查詢 */
   isp_get_power_line_freq(int *pval);

參數定義與 AT Command 相同（0 = Disable、1 = 50 Hz、2 = 60 Hz、3 = Auto）。完整 API 定義請參閱 ``isp_ctrl_api.h``：

.. code-block:: none

   位置：component/video/driver/RTL8735B/isp_ctrl_api.h
   GitHub：https://github.com/Ameba-AIoT/ameba-rtos-pro2/blob/main/component/video/driver/RTL8735B/isp_ctrl_api.h

6.5 HDR Mode IQ 配置說明
------------------------

本節說明 HDR mode 的啟用方式，以及與 Linear mode 在 IQ table 配置上的主要差異。

6.5.1 HDR Mode 啟用方式
^^^^^^^^^^^^^^^^^^^^^^^^

在進行 HDR IQ 調試前，請先確認以下兩項前置條件。

**前置條件一：確認 Sensor 支援 HDR**

並非所有 Sensor 均支援 HDR mode。請參閱 ``sensor.h`` 中的 Sensor 列表，確認所使用的 Sensor 在 ``ISP_HDR`` 欄位標示 ``v``。

.. code-block:: none

   位置：``project/realtek_amebapro2_v0_example/inc/sensor.h``
   GitHub：https://github.com/Ameba-AIoT/ameba-rtos-pro2/blob/main/project/realtek_amebapro2_v0_example/inc/sensor.h

< sensor.h Sensor 支援 HDR 列表（ISP_HDR 欄有 v 表示支援） >

|image395|

**前置條件二：在程式碼中啟用 HDR Mode**

於初始化參數結構中，將 ``isp_init_enable`` 設為 ``1``，並將 ``init_isp_items.init_hdr_mode`` 設為 ``0x01`` 以啟用 HDR mode（``0x00`` 為 Linear mode）。

.. note::
   僅有 ``sensor.h`` 中 ``ISP_HDR`` 欄位標示 ``v`` 的 Sensor 才支援 HDR mode（請先確認前置條件一）。

參考範例：``mmf2_video_example_v1_param_change_init.c``

.. code-block:: c

   /* ISP 初始化設定 */
   init_params.isp_init_enable = 1;

   /* HDR mode 設定（0x00 = Linear mode；0x01 = 啟用 HDR mode） */
   init_params.init_isp_items.init_hdr_mode = 0x01;

   /* WDR 處理模式（0x00 = 關閉；0x01 = 啟用 WDR Manual；0x02 = 啟用 WDR Auto） */
   init_params.init_isp_items.init_wdr_mode = 0x02;

< mmf2_video_example_v1_param_change_init.c 中的 init_hdr_mode 設定位置（紅框）>

|image396|

.. note::
   啟用 HDR mode 後，程式碼會自動將 Sensor fps 切換至 HDR 模式對應的幀率（如範例中的 ``hdr_mode_fps = 15``）。完整範例程式碼位置：

   .. code-block:: none

      位置：project/realtek_amebapro2_v0_example/src/mmfv2_video_example/mmf2_video_example_v1_param_change_init.c
      GitHub：https://github.com/Ameba-AIoT/ameba-rtos-pro2/blob/main/project/realtek_amebapro2_v0_example/src/mmfv2_video_example/mmf2_video_example_v1_param_change_init.c

6.5.2 HDR mode 與 Linear mode IQ table 差異
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

HDR mode 的 IQ table 在 Linear mode 的基礎上，額外包含以下專屬模組的參數配置：

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - 模組
     - 說明
   * - **HDR Fusion**
     - 負責將長曝（主路）與短曝（短曝路）的 RAW data 合成為一張寬動態影像。需針對長短曝的融合比率、過渡區設定進行獨立調試。
   * - **Over Exposed Protection（OEP）**
     - 抑制過曝區域的亮度，防止高亮場景產生溢色並保留亮部細節。HDR mode 須將 Clip Mode 設為 **Non-Clipped**；Linear mode 通常預設 12-bit。詳見 :ref:`3.4.1 <oep-general>`。
   * - **Tone Mapping**
     - 將高動態範圍的 HDR 影像壓縮至可顯示的亮度範圍，同時保留亮暗區細節。
   * - **AE HDR 參數**
     - 包含長曝 / 短曝的比率（ET ratio）控制、以及短曝路的 AE 收斂行為，與 Linear mode 的 AE 參數配置方式不同。
   * - **BLC Short Exposure Path**
     - 短曝路有獨立的 BLC 校正配置，需與長曝路分開調試。
   * - **GbGr Short Exposure**
     - 短曝路有獨立的 GbGr 平衡配置。
   * - **2DNR（短曝路）**
     - HDR mode 下，短曝路需獨立配置 2DNR 降噪強度，以對應短曝高增益帶來的雜訊特性，不可直接套用長曝路（Linear mode）的 2DNR 配置。

.. note::
   HDR 與 Linear mode 的 IQ table 大部分參數可共用，主要差異在於 min fps 設定與 OEP 的 clip mode，編寫 HDR IQ bin 時須特別注意這兩項的設計邏輯（詳見 :ref:`7.2.1 <hdr-min-fps-issue>` 與 :ref:`7.2.3 <oep-error>`）。

7. 常見問題排查
----------------

本章彙整使用 RealCam 工具與 HDR mode 時常見的問題現象與排查方式。

7.1 RealCam 工具常見問題
------------------------

7.1.1 RealCam 無法偵測 UVCD 串流
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**現象**：調適工具突然無法偵測 UVCD 串流，即使重啟設備後問題依然存在。

**處置方式**：使用 Realtek 提供的 RealCam 快取清除工具：

1. 開啟快取清除小程式。

   |image370|

2. 按下 **Delete** 按鈕清除快取。
3. 畫面顯示 **Finish** 即完成。

   |image371|

.. note::
   若清除快取後問題仍未解決，請確認 USB 驅動程式版本，並聯繫 FAE 窗口。

.. _iq-bin-upload-fail:

7.1.2 IQ Bin 燒錄後無法連上 UVC Camera
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**現象**：以 IQPackTool 產生 IQ Bin 後，將其整合至 SDK 並重新編譯 Firmware 燒錄；燒錄完成後，系統無法連線至 UVC Camera。

**原因**：IQ Bin 存在兩種格式，用途不同，不可互換：

- **含檔頭版本（With Header）**：由 Realtek 提供，供整合至 Firmware 中使用。
- **無檔頭版本（Without Header）**：由使用者完成 IQ 調試後透過 IQPackTool 輸出，僅供 RealCam 工具載入使用，不可直接整合至 Firmware。

若將無檔頭的 IQ Bin 整合至 Firmware 並燒錄，系統無法正確解析檔案格式，導致 UVC Camera 連線失敗。

**處置方式：手動加入檔頭**

檔頭需在檔案最前端填入 **Data Size** 欄位，規格如下：

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - 欄位
     - 規格
   * - 欄位名稱
     - Data Size（資料大小）
   * - 長度
     - 4 Bytes（32-bit unsigned integer）
   * - 儲存格式
     - Little-Endian（LSB 存於最低位址）
   * - 最大支援大小
     - 128 KB

**操作範例**：以 IQ.bin 檔案大小 20,572 Bytes 為例，說明加入檔頭的完整流程。

**步驟一：確認 IQ.bin 的位元組大小**

在 IQ.bin 上按右鍵選擇【內容】，記錄「大小」欄位顯示的位元組數值（**勿使用「磁碟大小」**）。

< 從檔案內容視窗確認 IQ.bin 的精確大小 >

|image374|

**步驟二：將檔案大小轉換為 4 Bytes Little-Endian 格式**

1. 十六進位轉換：20,572 (Dec) = 0x0000505C (Hex)
2. 依 Little-Endian 排列（低位位元組置前）： ``5C 50 00 00`` 

**步驟三：以 Hex 編輯器開啟原始 IQ.bin，確認開頭內容**

< 原始 IQ.bin 開頭內容（藍框），尚未加入檔頭 >

|image375|

**步驟四：在 Offset 0x00 插入 4 Bytes，完成檔頭寫入**

將 ``5C 50 00 00`` 插入至檔案最前端；原始內容整體後移 4 Bytes，正式資料從 Offset 0x04 開始。

< 加入 4-byte 檔頭後的結果：紅框為新插入的 Data Size（ ``5C 50 00 00``），藍框為後移至 Offset 0x04 的原始內容 >

|image376|

.. note::
   ``firmware_isp_iq.bin`` 的空間配置與多組 IQ bin 的組合方式，請參閱 :ref:`6.4.2 IQ Bin 空間配置說明 <iq-bin-layout>`。

7.1.3 NV16toYUY2.dll 註冊失效導致無法出圖
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**現象**：已執行 ``NV16toYUY2.dll`` 註冊，但 NV16 格式仍無法出圖；或裝置管理員的 USB UVC CLASS 項目出現驚嘆號。

**原因**：dll 未實際寫入系統登錄檔，註冊並未成功。可執行 ``regedit`` 確認，若找不到對應的登錄項目，即為此問題。

**處置方式**：

1. 將 ``NV16toYUY2.dll`` 複製至 ``C:\Windows`` 目錄下。
2. 對該 dll 按右鍵，選擇\ **以系統管理員身分執行**\ 完成註冊。

   < 以系統管理員身分執行 RegFilter.bat >

   |image386|

3. 執行 ``regedit``，確認登錄檔中已出現對應項目（FriendlyName 欄位應顯示 ``NV16ToYUV2 Transform``）。

   < regedit 登錄項目確認示意 >

   |image387|

**若依上述步驟仍無法成功**：

可能是系統中存在衝突的第三方軟體登錄碼，導致 dll 註冊失敗。建議使用 **RegClean Pro** 等登錄表修復工具掃描並清除異常項目後，重新執行上述步驟。

< RegClean Pro 介面 >

|image388|

.. note::
   登錄表修復工具可能會刪除部分軟體的登錄碼，請確認備份後再使用。

7.1.4 RealCam 顯示 No Device
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**現象**：開啟 RealCam 後，裝置欄位顯示 **No Device**，無法偵測到相機。

**處置方式**：

1. 確認 RealCam 以\ **系統管理員身分**\ 開啟（對 ``RealCam.exe`` 按右鍵 → 以系統管理員身分執行）。
2. 若以管理員權限開啟後仍顯示 No Device，可使用 **AMCap** 等 UVC 測試工具確認相機是否正常被系統偵測，以區分是軟體環境問題或系統層問題。

7.1.5 序列埠出現 [ISP Err] 錯誤訊息
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**現象**：板端序列埠 log 出現 ``[ISP Err]`` 訊息，例如：

< [ISP Err] log 訊息示意 >

|image389|

**原因**：``[ISP Err]`` 表示 ISP 發生掉 frame，USB 訊號傳輸不完整所致。

**處置方式**：

更換具備完整資料傳輸能力的 USB 線材。部分 USB 線僅支援充電功能，不具備資料傳輸能力，請確認使用的線材為\ **資料傳輸線**\ 而非充電專用線。

7.1.6 RealCam 未顯示 ISP Tuning 頁面
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**現象**：開啟 RealCam 後，ISP Tuning Pro 頁面未出現。

**原因**：韌體中 IQ tuning 功能尚未啟用。``platform_opts.h`` 是韌體中集中管理各功能開關的設定檔，其中 ``CONFIG_TUNING`` 控制 ISP Tuning 頁面的啟用與否，預設值為 ``0``\ （關閉）。

**處置方式**：

於 ``platform_opts.h``\ 中，將 ``CONFIG_TUNING`` 的值由 ``0`` 改為 ``1``，重新編譯並燒錄韌體後即可正常顯示 ISP Tuning 頁面。

參考檔案：``platform_opts.h``

.. code-block:: c

   /* 修改前（預設關閉） */
   #define CONFIG_TUNING   0   //support IQ Tuning

   /* 修改後（啟用） */
   #define CONFIG_TUNING   1   //support IQ Tuning

.. code-block:: none

   位置：project/realtek_amebapro2_v0_example/inc/platform_opts.h
   GitHub：https://github.com/Ameba-AIoT/ameba-rtos-pro2/blob/main/project/realtek_amebapro2_v0_example/inc/platform_opts.h



|image390|

.. centered:: 圖：將 ``CONFIG_TUNING`` 設為 ``1`` 以啟用 IQ Tuning 頁面

7.2 HDR Mode 問題排查
---------------------

.. _hdr-min-fps-issue:

7.2.1 HDR Sensor IQ bin 的 min fps 設定不洽當導致無法出圖
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**現象**：使用 HDR mode 起流後，相機無法正常出圖。

**背景**：完整的 IQ bin（Complete IQ）由 6 組 IQ table 組成，結構如下（參見 :ref:`2.3 <upload-download>`）：

.. list-table::
   :header-rows: 1
   :widths: 15 15 20 50

   * - Group
     - Index
     - 模式
     - 說明
   * - 0
     - 0
     - Linear Day
     - Linear mode 白天參數
   * - 0
     - 1
     - Linear Night
     - Linear mode 夜視參數
   * - 0
     - 2
     - Linear Other
     - Linear mode 其他模式參數
   * - 1
     - 3
     - HDR Day
     - HDR mode 白天參數
   * - 1
     - 4
     - HDR Night
     - HDR mode 夜視參數
   * - 1
     - 5
     - HDR Other
     - HDR mode 其他模式參數

**原因**：

起流至 HDR mode 時，ISP 的初始化順序如下：

1. **初始化**：ISP 先以 Linear Day（Index 0）的 IQ table 進行起流初始化。
2. **模式切換**：初始化完成後，系統切換至 HDR Day（Index 3）模式。
3. **FPS 衝突**：若 HDR Day 的 ``dyn_fps_min`` **低於** Linear Day 的 ``dyn_fps_min``，ISP 在切換過程中偵測到 min fps 異常衝突，導致系統無法正常出圖。

.. important::
   HDR mode 每幀需完成長曝與短曝兩次曝光；若 min fps 過低，長短曝之間的時間間隔增大，移動物體會在兩次曝光間產生位移，融合後形成 **motion ghost**\ （運動鬼影）。因此，**HDR Day（Index 3）**\ 的 ``dyn_fps_min`` 應大於或等於 **Linear Day（Index 0）**\ 的值，以同時避免 ISP 模式切換衝突與運動鬼影問題。

**排查與修正方式**：

建議以 RealCam 分別載入 Partial IQ 檔案進行比對，以快速定位問題所在的 IQ table。

1. 以 RealCam 載入 ``partialIQ_0.bin``\ （對應 Linear Day，Index 0），進入 **AE → AE Attribute**\ 頁面，記錄 ``dyn_fps_min`` 的值（記為 **A**\ ）。
2. 改載入 ``partialIQ_3.bin``\ （對應 HDR Day，Index 3），進入相同頁面，記錄 ``dyn_fps_min`` 的值（記為 **B**\ ）。
3. 若 **B < A**，即確認為本問題根源，依實際需求選擇以下其中一種修正方式：

   - **方式一（調高 HDR Day min fps）**\ ：若應用場景對 motion ghost 較敏感（如移動攝影），或 HDR Day 的 min fps 原本即應設定較高，將 ``partialIQ_3.bin`` 的 ``dyn_fps_min`` 修改為大於或等於 **A** 的值。
   - **方式二（調低 Linear Day min fps）**\ ：若 Linear Day 的 min fps 設定偏高（例如 A=15，而 HDR Day 使用 B=10 仍在合理範圍內），可將 ``partialIQ_0.bin`` 的 ``dyn_fps_min`` 修改為小於或等於 **B** 的值。

4. 依選擇的方式修改並儲存後，重新以 IQPackTool 將所有 Partial IQ 打包為 Complete IQ bin，燒錄後確認問題解除。

7.2.2 使用舊版 IQ 資料結構導致功能無法生效
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**現象**：燒錄 ``iq_sensor.bin`` 後，特定模組的設定值未如預期生效，或功能表現異常。

**原因**：SDK 中存有多個 ``iq_sensor.bin``，部分使用者可能拿到舊版 SDK 裡的 IQ bin 去做影像調試。由於過舊版本參數結構缺少新增欄位，ISP 會忽略對應參數並套用預設值，導致調試結果無法反映在畫面上。

**建議做法**：以下列指定 bin 檔作為各模式的初始 IQ，確保使用正確的 IQ 資料結構：

- HDR mode：``iq_gc4663.bin``
- Linear mode：``iq_gc2053.bin``

.. _oep-error:

7.2.3 在HDR mode 使用 Over Exposure Protection 模組時 RealCam 出現報錯
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**現象**：在 RealCam 中調整 Over Exposure Protection 模組的參數時，工具出現報錯訊息。

**原因**：使用的IQ bin 的 IQ 資料結構版本不符所致。使用 Realtek 提供的建議初始 IQ bin（``iq_gc4663.bin``）則不會發生此問題。

**處置方式**：

以 ``iq_gc4663.bin`` 作為 HDR mode 的初始 IQ 進行調試。若已以其他 bin 為基礎調試，建議改以 ``iq_gc4663.bin`` 重新建立 IQ，確認報錯是否消失。

< Over Exposure Protection 模組報錯訊息示意 >

|image385|

7.2.4 HDR 調試後以 Load IQ 方式載入效果不一致
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**現象**：完成 HDR 參數調試後，以 RealCam 的 Load IQ 方式載入 IQ bin，影像出現 contour（輪廓線）或畫質異常，與調試結果不符。

**原因**：Load IQ 載入的當下，ISP 內部某些參數可能仍處於 blending 過渡狀態（新舊數值尚未收斂），導致影像出現 contour。

**處置方式**：

將 Partial IQ（.bin）\ **重新燒錄至板端**\ ，讓 ISP 在起流時以完整的初始化流程套用參數，可確實排除上述 contour 現象。Load IQ 適合快速預覽，但不應作為 HDR 參數的最終驗證方式。

7.2.5 Sensor Binning Mode 在 HDR Mode 無法出流
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

如有發生此現象，可能是 Sensor 的 **Binning mode**\ （像素合併模式）不支援 HDR mode。請聯繫Sensor廠處理。

.. |image1| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image1.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image1.jpg
.. |image2| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image2.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image2.jpg
.. |image3| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image3.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image3.jpg
.. |image4| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image4.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image4.jpg
.. |image5| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image5.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image5.jpg
.. |image6| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image6.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image6.jpg
.. |image7| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image7.jpg
   :width: 50%
.. |image8| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image8.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image8.jpg
.. |image9| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image9.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image9.jpg
.. |image10| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image10.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image10.jpg
.. |image11| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image11.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image11.jpg
.. |image12| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image12.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image12.jpg
.. |image13| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image13.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image13.jpg
.. |image14| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image14.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image14.jpg
.. |image15| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image15.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image15.jpg
.. |image16| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image16.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image16.jpg
.. |image17| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image17.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image17.png
.. |image18| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image18.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image18.jpg
.. |image19| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image19.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image19.png
.. |image20| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image20.png
   :width: 30%
.. |image21| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image21.png
   :width: 30%
.. |image22| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image22.png
   :width: 30%
.. |image23| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image23.png
   :width: 40%
.. |image24| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image24.png
   :width: 100%
.. |image25| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image25.png
   :width: 100%
.. |image26| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image26.png
   :width: 100%
.. |image30| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image30.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image30.png
.. |image31| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image31.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image31.jpg
.. |image32| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image32.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image32.jpg
.. |image33| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image33.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image33.jpg
.. |image34| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image34.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image34.jpg
.. |image35| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image35.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image35.jpg
.. |image36| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image36.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image36.jpg
.. |image37| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image37.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image37.jpg
.. |image38| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image38.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image38.jpg
.. |image39| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image39.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image39.jpg
.. |image40| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image40.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image40.jpg
.. |image41| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image41.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image41.jpg
.. |image42| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image42.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image42.jpg
.. |image43| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image43.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image43.jpg
.. |image44| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image44.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image44.jpg
.. |image45| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image45.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image45.jpg
.. |image46| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image46.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image46.jpg
.. |image47| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image47.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image47.jpg
.. |image48| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image48.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image48.jpg
.. |image49| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image49.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image49.jpg
.. |image50| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image50.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image50.jpg
.. |image51| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image51.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image51.jpg
.. |image52| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image52.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image52.jpg
.. |image27| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image27.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image27.png
.. |image29| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image29.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image29.png
.. |image53| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image53.png
   :width: 48%
.. |image54| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image54.png
   :width: 48%
.. |image55| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image55.png
   :width: 48%
.. |image56| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image56.png
   :width: 48%
.. |image57| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image57.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image57.png
.. |image58| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image58.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image58.png
.. |image76| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image76.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image76.jpg
.. |image77| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image77.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image77.png
.. |image78| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image78.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image78.png
.. |image79| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image79.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image79.png
.. |image80| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image80.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image80.png
.. |image81| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image81.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image81.png
.. |image82| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image82.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image82.png
.. |image83| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image83.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image83.jpg
.. |image84| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image84.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image84.jpg
.. |image85| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image85.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image85.jpg
.. |image86| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image86.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image86.jpg
.. |image87| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image87.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image87.jpg
.. |image88| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image88.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image88.jpg
.. |image89| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image89.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image89.png
.. |image90| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image90.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image90.png
.. |image91| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image91.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image91.png
.. |image92| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image92.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image92.png
.. |image93| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image93.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image93.png
.. |image94| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image94.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image94.jpg
.. |image95| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image95.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image95.png
.. |image96| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image96.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image96.png
.. |image97| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image97.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image97.jpg
.. |image98| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image98.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image98.jpg
.. |image99| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image99.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image99.jpeg
.. |image100| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image100.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image100.png
.. |image101| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image101.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image101.png
.. |image102| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image102.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image102.png
.. |image103| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image103.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image103.jpeg
.. |image104| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image104.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image104.png
.. |image105| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image105.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image105.png
.. |image106| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image106.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image106.png
.. |image107| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image107.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image107.jpeg
.. |image108| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image108.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image108.jpeg
.. |image109| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image109.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image109.png
.. |image110| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image110.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image110.png
.. |image111| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image111.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image111.png
.. |image112| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image112.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image112.png
.. |image113| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image113.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image113.png
.. |image114| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image114.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image114.png
.. |image115| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image115.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image115.png
.. |image116| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image116.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image116.png
.. |image117| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image117.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image117.jpeg
.. |image118| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image118.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image118.png
.. |image119| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image119.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image119.png
.. |image120| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image120.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image120.png
.. |image121| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image121.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image121.png
.. |image122| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image122.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image122.jpeg
.. |image123| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image123.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image123.png
.. |image124| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image124.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image124.png
.. |image125| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image125.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image125.png
.. |image126| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image126.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image126.png
.. |image127| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image127.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image127.jpeg
.. |image128| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image128.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image128.png
.. |image129| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image129.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image129.jpeg
.. |image130| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image130.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image130.jpeg
.. |image131| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image131.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image131.png
.. |image132| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image132.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image132.png
.. |image133| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image133.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image133.png
.. |image134| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image134.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image134.jpeg
.. |image135| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image135.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image135.png
.. |image136| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image136.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image136.jpg
.. |image137| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image137.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image137.jpg
.. |image138| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image138.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image138.jpg
.. |image139| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image139.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image139.jpg
.. |image140| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image140.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image140.jpg
.. |image141| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image141.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image141.jpg
.. |image142| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image142.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image142.jpg
.. |image143| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image143.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image143.jpg
.. |image144| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image144.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image144.jpg
.. |image145| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image145.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image145.jpg
.. |image146| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image146.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image146.jpg
.. |image147| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image147.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image147.jpg
.. |image148| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image148.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image148.jpg
.. |image149| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image149.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image149.jpg
.. |image150| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image150.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image150.jpg
.. |image151| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image151.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image151.jpg
.. |image152| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image152.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image152.jpg
.. |image153| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image153.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image153.jpg
.. |image154| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image154.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image154.jpg
.. |image155| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image155.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image155.jpg
.. |image156| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image156.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image156.jpg
.. |image157| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image157.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image157.jpg
.. |image158| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image158.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image158.jpg
.. |image159| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image159.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image159.png
.. |image160| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image160.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image160.png
.. |image161| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image161.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image161.png
.. |image162| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image162.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image162.png
.. |image163| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image163.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image163.png
.. |image164| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image164.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image164.png
.. |image165| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image165.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image165.png
.. |image166| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image166.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image166.png
.. |image167| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image167.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image167.png
.. |image168| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image168.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image168.png
.. |image169| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image169.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image169.jpg
.. |image170| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image170.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image170.png
.. |image171| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image171.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image171.png
.. |image172| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image172.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image172.png
.. |image173| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image173.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image173.png
.. |image174| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image174.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image174.jpeg
.. |image175| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image175.png
   :width: 45%
.. |image176| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image176.png
   :width: 45%
.. |image177| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image177.png
   :width: 45%
.. |image178| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image178.png
   :width: 45%
.. |image179| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image179.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image179.png
.. |image180| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image180.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image180.png
.. |image181| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image181.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image181.png
.. |image182| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image182.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image182.png
.. |image183| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image183.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image183.png
.. |image184| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image184.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image184.jpeg
.. |image185| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image185.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image185.jpeg
.. |image186| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image186.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image186.jpeg
.. |image187| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image187.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image187.png
.. |image188| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image188.png
   :width: 48%
.. |image189| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image189.png
   :width: 48%
.. |image190| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image190.png
   :width: 48%
.. |image191| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image191.png
   :width: 48%
.. |image192| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image192.png
   :width: 48%
.. |image193| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image193.png
   :width: 48%
.. |image194| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image194.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image194.png
.. |image195| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image195.png
   :width: 48%
.. |image196| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image196.png
   :width: 48%
.. |image197| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image197.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image197.png
.. |image198| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image198.jpeg
   :width: 70%
.. |image199| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image199.jpeg
   :width: 60%
.. |image200| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image200.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image200.png
.. |image201| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image201.png
   :width: 48%
.. |image202| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image202.png
   :width: 48%
.. |image203| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image203.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image203.png
.. |image204| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image204.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image204.png
.. |image205| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image205.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image205.png
.. |image206| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image206.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image206.png
.. |image207| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image207.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image207.png
.. |image208| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image208.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image208.png
.. |image209| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image209.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image209.png
.. |image210| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image210.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image210.png
.. |image211| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image211.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image211.png
.. |image212| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image212.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image212.jpeg
.. |image213| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image213.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image213.jpeg
.. |image214| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image214.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image214.jpeg
.. |image215| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image215.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image215.png
.. |image216| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image216.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image216.png
.. |image217| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image217.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image217.png
.. |image218| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image218.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image218.png
.. |image219| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image219.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image219.png
.. |image220| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image220.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image220.png
.. |image221| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image221.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image221.png
.. |image222| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image222.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image222.png
.. |image223| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image223.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image223.png
.. |image224| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image224.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image224.png
.. |image225| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image225.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image225.png
.. |image226| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image226.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image226.png
.. |image227| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image227.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image227.png
.. |image228| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image228.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image228.png
.. |image229| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image229.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image229.png
.. |image230| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image230.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image230.png
.. |image231| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image231.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image231.png
.. |image232| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image232.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image232.png
.. |image233| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image233.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image233.png
.. |image234| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image234.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image234.png
.. |image235| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image235.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image235.png
.. |image236| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image236.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image236.png
.. |image237| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image237.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image237.png
.. |image238| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image238.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image238.png
.. |image239| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image239.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image239.png
.. |image240| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image240.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image240.png
.. |image241| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image241.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image241.png
.. |image242| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image242.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image242.png
.. |image243| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image243.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image243.jpg
.. |image244| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image244.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image244.png
.. |image245| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image245.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image245.jpeg
.. |image246| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image246.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image246.png
.. |image247| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image247.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image247.png
.. |image248| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image248.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image248.png
.. |image249| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image249.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image249.png
.. |image250| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image250.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image250.png
.. |image251| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image251.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image251.png
.. |image252| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image252.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image252.png
.. |image253| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image253.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image253.png
.. |image254| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image254.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image254.png
.. |image255| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image255.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image255.jpeg
.. |image256| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image256.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image256.jpeg
.. |image261| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image261.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image261.png
.. |image262| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image262.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image262.png
.. |image263| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image263.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image263.png
.. |image264| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image264.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image264.png
.. |image265| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image265.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image265.png
.. |image266| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image266.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image266.jpg
.. |image267| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image267.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image267.jpg
.. |image268| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image268.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image268.jpg
.. |image269| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image269.jpg
   :width: 40%
.. |image270| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image270.jpg
   :width: 40%
.. |image271| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image271.jpg
   :width: 40%
.. |image272| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image272.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image272.png
.. |image273| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image273.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image273.jpg
.. |image274| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image274.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image274.jpg
.. |image275| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image275.jpg
   :width: 32%
.. |image276| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image276.jpg
   :width: 32%
.. |image277| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image277.jpg
   :width: 32%
.. |image278| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image278.jpg
   :width: 45%
.. |image279| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image279.jpg
   :width: 45%
.. |image280| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image280.jpg
   :width: 45%
.. |image281| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image281.jpg
   :width: 60%
.. |image282| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image282.jpg
   :width: 50%
.. |image283| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image283.jpg
   :width: 40%
.. |image284| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image284.jpg
   :width: 40%
.. |image285| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image285.jpg
   :width: 80%
.. |image287| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image287.jpg
   :width: 48%
.. |image288| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image288.jpg
   :width: 48%
.. |image289| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image289.jpg
   :width: 48%
.. |image290| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image290.jpg
   :width: 48%
.. |image291| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image291.jpg
   :width: 48%
.. |image292| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image292.jpg
   :width: 48%
.. |image293| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image293.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image293.jpg
.. |image294| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image294.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image294.jpg
.. |image295| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image295.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image295.jpg
.. |image296| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image296.png
   :width: 48%
.. |image297| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image297.png
   :width: 48%
.. |image298| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image298.png
   :width: 48%
.. |image299| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image299.png
   :width: 48%
.. |image300| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image300.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image300.png
.. |image301| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image301.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image301.png
.. |image302| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image302.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image302.png
.. |image303| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image303.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image303.png
.. |image304| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image304.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image304.png
.. |image305| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image305.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image305.png
.. |image306| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image306.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image306.jpeg
.. |image307| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image307.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image307.jpeg
.. |image308| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image308.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image308.png
.. |image309| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image309.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image309.jpeg
.. |image310| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image310.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image310.png
.. |image311| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image311.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image311.jpeg
.. |image312| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image312.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image312.png
.. |image313| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image313.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image313.jpeg
.. |image314| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image314.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image314.jpeg
.. |image315| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image315.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image315.jpeg
.. |image316| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image316.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image316.jpeg
.. |image317| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image317.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image317.jpeg
.. |image318| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image318.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image318.png
.. |image319| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image319.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image319.jpeg
.. |image320| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image320.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image320.jpeg
.. |image321| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image321.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image321.jpeg
.. |image322| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image322.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image322.png
.. |image323| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image323.jpeg
   :width: 48%
.. |image324| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image324.jpeg
   :width: 48%
.. |image325| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image325.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image325.jpeg
.. |image326| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image326.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image326.png
.. |image327| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image327.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image327.jpeg
.. |image328| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image328.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image328.png
.. |image329| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image329.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image329.jpeg
.. |image330| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image330.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image330.png
.. |image331| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image331.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image331.jpeg
.. |image332| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image332.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image332.png
.. |image333| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image333.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image333.jpeg
.. |image334| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image334.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image334.jpeg
.. |image335| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image335.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image335.png
.. |image336| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image336.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image336.png
.. |image337| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image337.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image337.png
.. |image338| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image338.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image338.jpeg
.. |image339| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image339.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image339.png
.. |image340| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image340.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image340.jpeg
.. |image341| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image341.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image341.png
.. |image342| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image342.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image342.jpeg
.. |image343| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image343.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image343.jpeg
.. |image344| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image344.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image344.jpeg
.. |image345| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image345.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image345.png
.. |image346| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image346.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image346.jpeg
.. |image347| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image347.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image347.png
.. |image348| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image348.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image348.jpeg
.. |image349| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image349.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image349.png
.. |image350| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image350.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image350.jpeg
.. |image351| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image351.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image351.jpeg
.. |image352| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image352.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image352.jpeg
.. |image_vp_brightness| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image353.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image353.png
.. |image_vp_contrast| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image354.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image354.png
.. |image_vp_saturation| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image355.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image355.png
.. |image356| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image356.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image356.png
.. |image357| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image357.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image357.png
.. |image358| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image358.png
   :width: 15%
.. |image359| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image359.jpg
   :width: 80%
.. |image360| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image360.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image360.png
.. |image361| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image361.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image361.png
.. |image362| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image362.png
   :width: 48%
.. |image363| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image363.png
   :width: 48%
.. |image364| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image364.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image364.jpg
.. |image365| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image365.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image365.png
.. |image366| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image366.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image366.png
.. |image367| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image367.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image367.png
.. |image368| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image368.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image368.png
.. |image369| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image369.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image369.png
.. |image370| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image370.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image370.png
.. |image371| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image371.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image371.png
.. |image372| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image372.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image372.png
.. |image373| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image373.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image373.jpeg
.. |image374| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image374.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image374.png
.. |image375| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image375.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image375.jpeg
.. |image376| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image376.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image376.jpeg
.. |image377| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image377.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image377.png
.. |image378| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image378.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image378.png
.. |image379| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image379.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image379.png
.. |image380| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image380.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image380.png
.. |image381| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image381.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image381.png
.. |image382| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image382.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image382.png
.. |image383| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image383.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image383.png
.. |image384| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image384.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image384.png
.. |image385| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image385.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image385.png
.. |image386| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image386.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image386.png
.. |image387| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image387.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image387.png
.. |image388| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image388.jpeg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image388.jpeg
.. |image389| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image389.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image389.png
.. |image390| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image390.jpg
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image390.jpg
.. |image391| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image391.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image391.png
   :width: 49%
.. |image392| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image392.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image392.png
   :width: 49%
.. |image393| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image393.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image393.png
   :width: 49%
.. |image394| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image394.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image394.png
   :width: 49%
.. |image395| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image395.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image395.png
.. |image396| image:: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image396.png
   :target: ../../_static/user_manual/41_IQ_Tuning_Manual_V2_CHT/image396.png
