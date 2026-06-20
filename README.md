# RF-DETR TUM RGB Instance Segmentation

Struktur project yang sudah dipakai:
- dataset lokal: `TUM_RGB_Segmentation`
- format anotasi: COCO segmentation
- model: `RFDETRSegSmall` secara default
- device: `cuda:0`
- checkpoint utama untuk testing: `checkpoint_best_total.pth`
- mask refinement hanya dilakukan sebagai post-processing

---

## 1. Struktur folder

```text
rfdetr_tum_rgb/
├── TUM_RGB_Segmentation/
│   ├── train/
│   │   ├── _annotations.coco.json
│   │   └── *.jpg / *.png
│   ├── valid/
│   │   ├── _annotations.coco.json
│   │   └── *.jpg / *.png
│   └── test/
│       ├── _annotations.coco.json
│       └── *.jpg / *.png
├── src/
├── scripts/
└── outputs/
```

Default path yang dipakai:

```bash
/home/rogki/kuliah/semester_6/computer_vision/afterUTS/final_project/rfdetr_tum_rgb
```

Jika folder beda, set environment variable `PROJECT_ROOT`.

---

## 2. Install dependency

Aktifkan virtual environment, lalu install dependency:

```bash
pip install -r requirements.txt
```

Kalau `rfdetr` belum ada:

```bash
pip install -U "rfdetr[train,loggers,metrics]" supervision opencv-python pandas matplotlib pycocotools
```

---

## 3. Cek dataset dan anotasi

```bash
python scripts/01_check_dataset.py
```

Script ini mengecek:
- folder `train`, `valid`, `test`
- file `_annotations.coco.json`
- jumlah gambar
- jumlah anotasi
- jumlah segmentation annotation
- kategori/class

---

## 4. Training model

Default training:
- model: RFDETRSegSmall
- epoch: 10
- batch size: 2
- grad accumulation: 8
- device: cuda:0

```bash
python scripts/02_train.py
```

Atau dengan setting manual:

```bash
MODEL_SIZE=small EPOCHS=10 BATCH_SIZE=2 GRAD_ACCUM_STEPS=8 python scripts/02_train.py
```

Output training akan disimpan ke:

```text
outputs/local_rfdetr_seg_small_<timestamp>/
```

Di dalamnya akan ada:
- checkpoint `.pth`
- `training_summary.json`

Checkpoint terbaik yang dipakai untuk inference:

```text
checkpoint_best_total.pth
```

---

## 5. Testing, evaluasi, dan visualisasi segmentasi

Setelah training selesai:

```bash
OUTPUT_DIR=/path/ke/folder/output/training python scripts/03_test_evaluate_visualize.py
```

Contoh sesuai project:

```bash
OUTPUT_DIR=/home/rogki/kuliah/semester_6/computer_vision/afterUTS/final_project/rfdetr_tum_rgb/outputs/local_rfdetr_seg_small_20260615_214947 python scripts/03_test_evaluate_visualize.py
```

Script ini menghasilkan:
- inference raw mask
- basic refinement
- aggressive refinement
- evaluasi IoU, Dice, Pixel Accuracy, Precision, Recall
- false positive / false negative analysis
- visualisasi segmentasi untuk PPT

Output default:

```text
final_evaluation_outputs/
├── csv/
│   ├── per_image_metrics.csv
│   └── summary_metrics.csv
├── masks/
│   ├── raw/
│   ├── basic_refined/
│   ├── aggressive_refined/
│   ├── gt/
│   ├── fp_raw/
│   └── fp_aggressive/
└── visualizations/
    ├── comparison_full/
    ├── comparison_simple/
    └── error_focus/
```

---

## 6. Visualisasi training metrics

```bash
python scripts/04_visualize_training_metrics.py
```

Output:

```text
training_metrics_visualization/
├── 01_detection_metrics_trend.png
├── 02_segmentation_metrics_trend.png
├── 03_precision_recall_f1_trend.png
├── 04_final_epoch_metrics_bar.png
├── metrics_per_epoch.csv
└── metrics_summary.txt
```

---

## 7. Jalankan semua tahap setelah training

Kalau sudah punya checkpoint dan hanya ingin evaluasi + visualisasi:

```bash
OUTPUT_DIR=/path/ke/output/training python scripts/05_run_after_training.py
```

---

## 8. Environment variable penting

| Variable | Default | Fungsi |
|---|---|---|
| `PROJECT_ROOT` | path project Rogki | lokasi root project |
| `DATASET_DIR` | `PROJECT_ROOT/TUM_RGB_Segmentation` | lokasi dataset |
| `OUTPUT_DIR` | otomatis timestamp saat training | lokasi output training / checkpoint |
| `MODEL_SIZE` | `small` | nano/small/medium/large |
| `EPOCHS` | `10` | jumlah epoch training |
| `BATCH_SIZE` | `2` | batch size training |
| `GRAD_ACCUM_STEPS` | `8` | gradient accumulation |
| `NUM_WORKERS` | `4` | dataloader workers |
| `THRESHOLD` | `0.5` | confidence threshold inference |

---

## 9. Keterangan hasil visualisasi

Pada folder `visualizations`,

### `comparison_simple`
Format:

```text
Input | Raw Prediction | Refined Prediction | Ground Truth
```

### `comparison_full`
Format:

```text
Input | Ground Truth | Raw | Basic | Aggressive | Removed | FP Raw | FP Refined
```

### `error_focus`
Format:

```text
Input | Raw | Aggressive | Removed | FP Before | FP After | FN After | Ground Truth
```

Keterangan:
- Raw = hasil asli model RF-DETR
- Basic refinement = opening + closing + small-component filtering
- Aggressive refinement = opening + closing + simpan top-K komponen terbesar
- FP = false positive, area non-manusia yang salah diprediksi sebagai manusia
- FN = false negative, area manusia pada ground truth yang tidak berhasil diprediksi
- Removed = area yang dihapus oleh refinement
