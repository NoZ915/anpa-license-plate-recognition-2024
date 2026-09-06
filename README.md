# Automatic Number Plate Recognition

2023 下學期《機器學習》期末專題。這是一份以 TensorFlow Object Detection API 建立的車牌辨識實驗紀錄：先從影像或網路攝影機畫面中定位車牌區域，再交由 OCR 讀取車牌文字，並保存辨識結果與裁切影像。

## Project overview

本專題將車牌辨識拆分為兩個階段：

1. **車牌偵測**：以 TensorFlow Object Detection API 和 SSD MobileNet V2 FPNLite 320×320 預訓練模型進行遷移學習，訓練自訂的單一類別 `licence`，預測車牌的邊界框。
2. **車牌文字辨識**：依偵測框裁切 ROI（Region of Interest），再以 EasyOCR 辨識車牌字元；低於設定信心門檻的偵測結果會被過濾。

訓練與推論流程支援靜態影像和網路攝影機即時畫面。辨識結果會記錄在 CSV，裁切出的車牌影像則以辨識文字與時間戳記命名保存。

## Implementation

- **Language and notebooks:** Python, Jupyter Notebook
- **Detection model:** TensorFlow 2, TensorFlow Object Detection API, SSD MobileNet V2 FPNLite 320×320
- **Vision and OCR:** OpenCV, EasyOCR
- **Training workflow:** Pascal VOC XML annotations → TFRecord → transfer learning → checkpoint inference
- **Evaluation records:** TensorBoard events, model checkpoints, CSV results, and cropped detections

The training configuration uses one `licence` class, a batch size of 4, and a 10,000-step training run. The main notebook also includes ROI conversion, detection-score filtering, OCR integration, result persistence, and real-time webcam inference.

## Repository guide

```text
.
├── 1. Image Collection.ipynb       # Legacy collection/labelling workflow
├── 2. Training and Detection.ipynb # Main ANPR training, OCR, and inference workflow
├── Tensorflow/
│   └── workspace/
│       ├── annotations/            # label map and TFRecord files
│       ├── images/                 # train/test images and XML annotations
│       └── models/                 # pipeline config, checkpoints, TensorBoard logs
├── Detection_Images/               # cropped licence-plate results
├── detection_results.csv           # still-image OCR results
├── realtimeresults.csv             # webcam OCR experiment results
├── Error Guide.md                  # legacy installation/troubleshooting notes
└── Legacy.md                       # provenance of legacy course-derived materials
```

## Workflow

```text
Images → XML annotations → train/test split → TFRecord
→ SSD MobileNet transfer learning → licence-plate detection
→ ROI crop → EasyOCR → CSV and cropped-image outputs
```

## Legacy materials and provenance

This repository preserves its original learning context. The initial notebook layout, the former README, the error guide, `generate_tfrecord.py`, and the TensorFlow Models setup were derived from the TFODCourse walkthrough by Nicholas Renotte. They are retained as **Legacy** materials because they document the environment and process used at the time.

The project-specific work is the conversion of that workflow into an ANPR pipeline: the `licence` label map, vehicle-image dataset and annotations, TFRecord generation, model configuration, trained checkpoints, EasyOCR integration, real-time detection, result-saving logic, and experiment records.

## Project records

- [Personal implementation notes (HackMD)](https://hackmd.io/@noz915/HklpVmvUT)
- [Final project presentation (Google Slides)](https://docs.google.com/presentation/d/18U1nnlozaEmMWlh92ARqNW96jJ_mMU0Rcvv_kgZ4-Z4/edit?usp=sharing)
- [Project demo video (YouTube)](https://www.youtube.com/watch?v=1wjNwM4XAaQ)

## References

- [Nicholas Renotte — TFODCourse repository](https://github.com/nicknochnack/TFODCourse)
- [Nicholas Renotte — TensorFlow Object Detection Course](https://www.youtube.com/c/nicholasrenotte)
- [TensorFlow Object Detection API](https://tensorflow-object-detection-api-tutorial.readthedocs.io/)

## Notes

- This is a preserved 2024 experiment archive for coursework and reproducibility.
- The included images and recognition records are test materials from the original project.
- Rebuild the Python environment from documented dependencies rather than committing virtual environments such as `anprsys/` or `tfod/`.

