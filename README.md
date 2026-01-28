# 🍦 Predicting Ice Cream Sales with Azure Machine Learning (AutoML Regression)

This project trains a **regression model** to predict **ice cream sales** from **temperature**, using **Azure Machine Learning Studio** (Automated ML).  
The goal is to help a (fictional) shop *Gelato Mágico* plan production, reduce waste, and avoid missing sales on hot days.

## What’s inside (minimal repo)
- `sorvetes_clean.csv` – the dataset used in Azure ML (temperature + sales)
- `inputs/sentences.txt` – small text file required by the challenge instructions
- `screenshots/` – proof of the Azure ML steps (workspace, data asset, AutoML run, metrics, model registration)

## Dataset
The dataset has two columns:
- `temperature` (feature / X)
- `sales` (target / y)

## Azure ML steps I followed
1. **Create Resource Group + Azure ML Workspace** (public network; auto-created storage/key vault/app insights).  
   Screenshot: `screenshots/01-azure-ml-workspace.png`

2. **Create Compute**
   - Compute instance (for notebooks/tests)
   - Compute cluster (for training jobs)  
   Screenshot: `screenshots/02-compute-instance.png`

3. **Create Data Asset**
   - Type: **Table (mltable)** (recommended for AutoML tabular workloads)
   - Source: local upload (`sorvetes_clean.csv`)
   - Verify preview & schema  
   Screenshots: `screenshots/03-create-data-asset.png`, `screenshots/04-data-asset-review.png`

4. **Run Automated ML (Regression)**
   - Task type: **Regression**
   - Target column: **sales**
   - Primary metric: **Normalized RMSE**
   - Compute target: cluster  
   Screenshots: `screenshots/05-automl-basic-settings.png`, `screenshots/06-automl-target-column.png`, `screenshots/07-job-running.png`

## Results (from the completed run)
- Best model: **VotingEnsemble**  
- **Normalized RMSE ≈ 0.0902** (primary metric)  
  Screenshot: `screenshots/08-job-completed-summary.png`

Other metrics shown in Azure ML:
- **R² ≈ 0.826**
- **MAE ≈ 9.39**
- **RMSE ≈ 12.0**  
Screenshot: `screenshots/09-metrics.png`

## Registering the model (MLflow)
After the job finished, I registered the best model output:
- Model type: **MLflow**
- Job output: **best_model**  
Screenshots: `screenshots/10-register-model.png`, `screenshots/11-registered-model-details.png`

## (Optional) Deploy for real-time predictions
In Azure ML Studio:
1. Go to **Models** → select the registered model
2. Click **Deploy** → choose **Managed online endpoint** (recommended)
3. Create endpoint + deployment
4. Test with a JSON payload like:
```json
{"input_data":[{"temperature":30}]}
```

## Notes
- For a simple linear regression portfolio demo, **AutoML** is great because it logs everything, compares models, and registers to MLflow automatically.
- If you want to force a specific model (e.g., LinearRegression), you can do it in **Designer** or with a custom training script — but this repo focuses on the **challenge-required workflow** and evidence.

---

### Screenshots index
1. Workspace created: `screenshots/01-azure-ml-workspace.png`  
2. Compute created: `screenshots/02-compute-instance.png`  
3. Data asset creation: `screenshots/03-create-data-asset.png`  
4. Data review: `screenshots/04-data-asset-review.png`  
5. AutoML job setup: `screenshots/05-automl-basic-settings.png`  
6. Target column selected: `screenshots/06-automl-target-column.png`  
7. Job running: `screenshots/07-job-running.png`  
8. Completed run summary: `screenshots/08-job-completed-summary.png`  
9. Metrics dashboard: `screenshots/09-metrics.png`  
10. Register model: `screenshots/10-register-model.png`  
11. Registered model details: `screenshots/11-registered-model-details.png`  
