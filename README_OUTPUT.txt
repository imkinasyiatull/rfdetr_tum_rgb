
PPT-READY VISUALIZATION OUTPUT
==============================

Output root:
/home/rogki/kuliah/semester_6/computer_vision/afterUTS/final_project/rfdetr_tum_rgb/ppt_ready_visualizations

Folder penting:
1. 00_dataset_overview/
   - dataset_split_overview.png
   - cocok untuk slide dataset.

2. 01_training_metrics/
   - 01_detection_metrics_trend.png
   - 02_segmentation_metrics_trend.png
   - 03_precision_recall_f1_trend.png
   - 04_final_epoch_metrics_bar.png
   - cocok untuk slide training result.

3. 02_eval_summary/
   - 00_experiment_summary_slide.png
   - 01_raw_vs_refined_metrics_bar.png
   - 02_total_fp_fn_pixels_bar.png
   - 03_top_delta_iou_aggressive.png
   - 04_top_fp_reduction_aggressive.png
   - cocok untuk slide result/evaluation.

4. 03_case_overview_slides/
   - Slide lengkap 8 panel:
     Input | Ground Truth | Raw Prediction | Basic Refinement |
     Aggressive Refinement | Removed | FP Raw | FP Refined

5. 04_simple_comparison_slides/
   - Slide simpel:
     Input | Raw Mask | Refined Mask | Ground Truth
   - Ini paling aman dipakai di slide utama PPT.

6. 05_difference_focus_slides/
   - Slide untuk bahas false positive:
     Input | Raw | Refined | Removed | FP Before | FP After | FN | GT

7. 07_selected_for_ppt/
   - Gambar yang otomatis dipilih:
     A_best_iou_improvement
     B_biggest_fp_reduction
     C_possible_over_refinement
     D_most_changed_masks

CSV:
- /home/rogki/kuliah/semester_6/computer_vision/afterUTS/final_project/rfdetr_tum_rgb/ppt_ready_visualizations/08_csv/per_image_ppt_metrics.csv
- /home/rogki/kuliah/semester_6/computer_vision/afterUTS/final_project/rfdetr_tum_rgb/ppt_ready_visualizations/08_csv/summary_metrics.csv
- /home/rogki/kuliah/semester_6/computer_vision/afterUTS/final_project/rfdetr_tum_rgb/ppt_ready_visualizations/08_csv/selected_cases_for_ppt.csv

Ringkasan:
- Raw mean IoU        : 0.9492
- Aggressive mean IoU : 0.8355
- Delta IoU           : -0.1137
- Delta Dice          : -0.0718
- FP reduction        : 7,245 pixels

Catatan:
- Raw = hasil prediksi model RF-DETR tanpa post-processing.
- Basic refinement = opening + closing + small-component filtering.
- Aggressive refinement = opening + closing + keep top-K largest components.
- FP = area yang diprediksi sebagai person, tetapi ground truth bukan person.
- Removed = area yang dihapus oleh refinement.
