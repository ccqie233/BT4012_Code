# BT4012 Final Report: Colab Notebooks

Seven English notebooks cover the report workflow and the two final submissions. Each notebook contains only its own required implementation and dependencies. No extra Python file needs to be uploaded to Colab.

To generate only the final submissions, run **06 and 07 directly**; they need only the four raw CSV files.
The rest 5 files are included to demonstrate exploration history.
Run 02-04 before 05 to create evaluation results. 


| Notebook | Sections after setup and data loading |
|---|---|
| 01 Data Preprocessing | Labels and graph checks; raw inputs; temporal splits; label distribution and feature shift |
| 02 Baseline Models | Linear/tree/neural baselines; GraphSAGE; optional neural candidates; three-boosting ridge baseline |
| 03 Feature Engineering | Tree graph features; TabM representations; graph shape; missing neighbours; deletion/interactions; original graph pairs; extended tree search; scaling/time diagnostics |
| 04 TabM Optimization | Forward-window comparison; parameter grids; internal selection windows and PLE bins; dropout 0.15 |
| 05 Model Evaluation | Load saved experiments; check predictions; window summaries; per-period metrics; historical reference |
| 06 Final Optimized TabM | Choose recipe; select epochs; full refit; seed averaging; submission checks and download |
| 07 Final TabM and Tree Stacking | Fixed recipe; optional outer validation; chronological OOF; logistic meta-model; full refit; submission checks and download |

01 contains no model implementation or training package. 05 reads saved results and never trains models. TabR and its license appear only in 02 and 04, where TabR is compared. 06 contains only TabM; 07 contains TabM, LightGBM, XGBoost, ExtraTrees and the logistic stacker.

## Run in Colab

1. Put `train.csv`, `test.csv`, `txs_edgelist.csv` and `sample_submission.csv` in one Google Drive folder.
    - by default, create a folder named 'bt-4012-competition-2026' to place the 4 csv files
    - create a folder named 'BT4012_Final_Report_English' to store notebook output results
2. Upload a notebook to Colab. Select a GPU runtime for neural-model training.
3. Set `DATA_DIR` in Section 1 (follow what you set up in step 1). Keep `RUN_NAME` and `OUTPUT_ROOT` consistent across notebooks.
4. Run the cells from top to bottom. The first cells install the required packages and mount Drive. `DATA_SOURCE='upload'` also supports direct CSV upload.
5. Use `PROFILE='report'` for the report settings. Notebooks 06 and 07 default to `report`.
    - Use `PROFILE='quick'` to check execution with a small training budget. 


Optional appendix studies are disabled by default. 
Enable their `RUN_...` = True switches in the relevant sections. Currently all switches set to True.
Parameter grids and extended feature searches can require multiple Colab sessions.
Completed folds and models are saved to Drive and reused when their data, code and parameters match.

Set `DOWNLOAD_CSV=True` in the final cell to download a submission. Quick-check files are named `QUICK_CHECK_ONLY...` and are not final report submissions.

## Final outputs

- **06:** `submission_tabm_optimized_long_inner.csv`. The default is dropout 0.1, a seven-period inner selection window and seeds 17/42/73. Epoch selection is followed by a fresh full-data refit. `EPOCH_SOURCE='reported_fixed'` uses the report's 12/11/7 epochs instead of selecting them again.
- **07:** `submission_tabm_tree_stacking.csv`. The traceable historical pipeline uses seed 42, TabM dropout 0.1 and a three-period inner window. Four non-overlapping OOF blocks train a scaler and logistic meta-model. All base models are then refitted on the known labels.

The reported Public AUCs are **0.96075** for optimized TabM and **0.95173** for stacking. 

