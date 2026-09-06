# Automatic Number Plate Recognition

2023 下學期《機器學習》期末專題。此專題以 TensorFlow Object Detection API 建立車牌辨識流程：先從影像或網路攝影機畫面中偵測車牌區域，再以 OCR 辨識裁切後的車牌文字，最後保存辨識結果與車牌影像。

## 專題介紹

本專題主要分為兩個步驟：

1. **車牌偵測**  
   使用 TensorFlow Object Detection API 與 SSD MobileNet V2 FPNLite 320×320 預訓練模型進行遷移學習，訓練自訂的 `licence` 類別，找出影像中的車牌位置。

2. **車牌文字辨識**  
   依據偵測框裁切車牌區域（ROI），再使用 EasyOCR 讀取車牌文字；低於信心分數門檻的偵測結果會被過濾。

專題支援靜態影像與網路攝影機即時辨識。辨識結果會輸出至 CSV，裁切出的車牌影像則以辨識結果與時間戳記命名保存。

## 使用技術

- Python、Jupyter Notebook
- TensorFlow 2
- TensorFlow Object Detection API
- SSD MobileNet V2 FPNLite 320×320
- OpenCV
- EasyOCR
- Protocol Buffers
- TensorBoard

## 實作流程

```text
影像資料集
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
├── 2. Training and Detection.ipynb # 主要訓練、車牌偵測與 OCR 流程
├── Tensorflow/
│   └── workspace/
│       ├── annotations/            # label map 與 TFRecord
│       ├── images/                 # train/test 影像與 XML 標註
│       └── models/                 # 設定檔、checkpoint、TensorBoard 紀錄
├── scripts/
│   └── generate_tfrecord.py        # TFRecord 產生腳本
├── Detection_Images/               # 已裁切的車牌辨識結果
├── detection_results.csv           # 單張影像辨識結果
├── realtimeresults.csv             # 即時辨識實驗結果
├── Legacy/                         # 原始教學 README 與 2024 設定備份
├── Legacy.md                       # Legacy 材料說明
└── Error Guide.md                  # 安裝與除錯筆記
```

---

## Legacy materials and provenance

This repository preserves its original learning context. The initial notebook layout, former generic README, error guide, `generate_tfrecord.py`, and TensorFlow Models setup were derived from the TFODCourse walkthrough by Nicholas Renotte.

They are retained as **Legacy** materials because they document the environment and workflow used at the time.

The project-specific work is the adaptation into an ANPR pipeline: the `licence` label map, vehicle-image dataset and XML annotations, TFRecord generation, model configuration, trained checkpoints, EasyOCR integration, real-time detection, result-saving logic, and experiment records.

## Project records

- [Personal implementation notes (HackMD)](https://hackmd.io/@noz915/HklpVmvUT)
- [Final project presentation (Google Slides)](https://docs.google.com/presentation/d/18U1nnlozaEmMWlh92ARqNW96jJ_mMU0Rcvv_kgZ4-Z4/edit?usp=sharing)
- [Project demo video (YouTube)](https://www.youtube.com/watch?v=1wjNwM4XAaQ)

## References

- [Nicholas Renotte — TFODCourse repository](https://github.com/nicknochnack/TFODCourse)
- [Nicholas Renotte — TensorFlow Object Detection Course](https://www.youtube.com/c/nicholasrenotte)
- [TensorFlow Object Detection API](https://tensorflow-object-detection-api-tutorial.readthedocs.io/)

## Notes

- This repository is a preserved 2024 coursework experiment archive.
- The included images and recognition records are test materials from the original project.
- Rebuild Python environments from dependencies instead of committing local virtual environments such as `anprsys/` or `tfod/`.
