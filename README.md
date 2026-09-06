# 車牌辨識系統（Automatic Number Plate Recognition）

2023 下學期《機器學習》期末專題。本專題以深度學習實作車牌辨識流程：先從影像或網路攝影機畫面中找出車牌區域，再透過 OCR 辨識裁切後的車牌文字，最後儲存辨識結果與車牌影像，作為實驗紀錄與後續檢視用途。

## 專題目標

本專題希望完成一個基本的自動車牌辨識（ANPR）流程，將「車牌位置偵測」與「車牌文字辨識」整合在同一套 Python 實作中。系統可處理靜態影像，也可從網路攝影機持續擷取畫面，進行即時辨識。

## 核心概念與實作方式

### 1. 車牌偵測

使用 TensorFlow Object Detection API 與 **SSD MobileNet V2 FPNLite 320×320** 預訓練模型進行遷移學習。資料集中的車牌先以 Pascal VOC XML 格式標註，再轉換為 TFRecord 格式，並訓練單一自訂類別：`licence`。

訓練設定包含：

- 輸入尺寸：320×320
- 類別數：1（`licence`）
- Batch size：4
- 訓練步數：10,000 steps
- 訓練結果：checkpoint、pipeline config 與 TensorBoard event logs

### 2. OCR 文字辨識

模型輸出車牌的偵測框與信心分數後，程式會：

1. 篩選高於信心門檻的偵測結果。
2. 將標準化座標換算為原始影像的實際像素座標。
3. 裁切車牌區域（ROI, Region of Interest）。
4. 以 EasyOCR 讀取裁切區域中的文字。
5. 將辨識文字、裁切影像與時間戳記寫入結果檔案。

### 3. 即時辨識與結果保存

筆記本提供 OpenCV 網路攝影機串流流程。每個畫面會依序經過車牌偵測、ROI 裁切與 OCR，並將結果寫入 `realtimeresults.csv`。裁切出的車牌影像會存放在 `Detection_Images/`，檔名包含 OCR 結果與儲存時間。

## 專案流程

```text
車牌影像資料集
→ XML 車牌標註
→ 訓練／測試資料切分
→ 轉換為 TFRecord
→ SSD MobileNet 遷移學習
→ 車牌位置偵測
→ 裁切車牌 ROI
→ EasyOCR 文字辨識
→ 輸出 CSV 與裁切結果影像
```

## 專案結構

```text
.
├── 1. Image Collection.ipynb       # Legacy 影像蒐集與標註流程
├── 2. Training and Detection.ipynb # 主要訓練、偵測、OCR 與即時辨識流程
├── Tensorflow/
│   └── workspace/
│       ├── annotations/            # label map、train/test TFRecord
│       ├── images/                 # train/test 影像與 XML 標註
│       └── models/                 # 設定檔、checkpoint、TensorBoard 紀錄
├── scripts/
│   └── generate_tfrecord.py        # TFRecord 產生腳本
├── Detection_Images/               # 已裁切的車牌辨識結果
├── detection_results.csv           # 單張影像辨識結果
├── realtimeresults.csv             # 即時辨識實驗結果
├── Legacy/                         # 原始教學 README 與 2024 設定備份
├── Legacy.md                       # Legacy 材料與來源說明
└── Error Guide.md                  # 安裝與除錯筆記
```

## 專題展示影片

點擊下方縮圖即可觀看專題 demo：

[![車牌辨識系統專題 Demo](https://img.youtube.com/vi/1wjNwM4XAaQ/hqdefault.jpg)](https://www.youtube.com/watch?v=1wjNwM4XAaQ)

> GitHub README 支援圖片與連結，但不支援直接嵌入 YouTube `iframe` 播放器；因此以可點擊的影片縮圖呈現。

## 專題紀錄

- [HackMD 實作筆記](https://hackmd.io/@noz915/HklpVmvUT)
- [期末專題報告投影片（Google Slides）](https://docs.google.com/presentation/d/18U1nnlozaEmMWlh92ARqNW96jJ_mMU0Rcvv_kgZ4-Z4/edit?usp=sharing)
- [YouTube 專題展示影片](https://www.youtube.com/watch?v=1wjNwM4XAaQ)

## Legacy 材料與來源

本專題最初以 Nicholas Renotte 的 TFODCourse 教學架構為學習起點，因此保留了部分原始材料作為 Legacy 紀錄，包括筆記本架構、通用 README、錯誤排除筆記、TFRecord 輔助腳本與 TensorFlow Models 安裝流程。

專題自行完成與調整的部分包括：

- 車牌資料集與 Pascal VOC XML 標註。
- `licence` 類別的 label map 與 TFRecord 資料。
- SSD MobileNet 訓練設定與 checkpoint。
- 車牌 ROI 裁切、信心分數篩選與 EasyOCR 串接。
- 靜態影像與網路攝影機即時辨識。
- CSV 結果紀錄與裁切車牌影像保存。

原始教學 README 已保留於 [`Legacy/README-TFODCourse.md`](Legacy/README-TFODCourse.md)，以記錄當時使用的學習來源與環境設定。

## 參考資料

- [Nicholas Renotte — TFODCourse 原始儲存庫](https://github.com/nicknochnack/TFODCourse)
- [Nicholas Renotte — TensorFlow Object Detection 教學頻道](https://www.youtube.com/c/nicholasrenotte)
- [TensorFlow Object Detection API 文件](https://tensorflow-object-detection-api-tutorial.readthedocs.io/)

## 備註

- 此 repository 是保留 2024 年課程實驗的專案快照。
- 影像與辨識紀錄皆為原專題的測試材料。
- Python 虛擬環境不納入版本控制，可依套件需求重新建立。
- TFRecord 檔案使用 Git LFS 保存，以避免一般 Git 的單檔大小限制。
