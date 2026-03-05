# CS4168 Project Spec Checklist vs analysis.ipynb

## ✅ Fully covered

| Requirement | Status |
|-------------|--------|
| **Dataset** | Using `tracks2026.csv` ✓ |
| **Clustering: k-means** | Implemented with elbow method ✓ |
| **Clustering: DBSCAN** | Implemented and visualised ✓ |
| **Clustering: drop track_genre** | Excluded from `clustering_cols` ✓ |
| **Clustering: suitable k** | Elbow method used ✓ |
| **Clustering: ≥2 values of k compared** | k=3, 5, 8 inertia printed ✓ |
| **Clustering: visualise** | K-Means and DBSCAN on PCA ✓ |
| **Clustering: meaningful structure / genre** | Crosstab + interpretation ✓ |
| **Classification: popularity_binary** | 0 if ≤ median, 1 if > median ✓ |
| **Classification: remove popularity from features** | `X_class_cols` excludes popularity ✓ |
| **Classification: >1 approach** | Random Forest + Logistic Regression ✓ |
| **Classification: train/test + CV** | train_test_split + cross_val_score ✓ |
| **Classification: feature importance** | RF feature importances plotted ✓ |
| **Regression: popularity as target** | Separate `X_reg` / `y_reg`, no binarisation ✓ |
| **Regression: >1 approach** | Linear Regression + Random Forest Regressor ✓ |
| **Regression: harder than classification** | Discussed in Discussion/Summary ✓ |
| **EDA** | Shape, columns, missing, describe, 5 visualisations ✓ |
| **Visualisations** | Present and relevant ✓ |

---

## ⚠️ Gaps (spec asks for these)

### 1. **“Select and justify the final clustering solution”** (Spec §3.2)
- You use `k_optimal = 5` but do not explicitly **state** that K-Means with k=5 is the chosen solution and **why** (e.g. elbow bend, interpretability, comparison with k=3 and k=8).
- **Add:** A short markdown (or comment) after comparing k=3,5,8 that says which solution you select and why (e.g. “We select k=5 because the elbow plot shows a bend around 5 and it gives a reasonable trade-off between inertia and number of clusters.”).

### 2. **“Justify your final choice” for classification** (Spec §3.3)
- You train RF and Logistic Regression but do not explicitly **choose one** and **justify** (e.g. “We choose Random Forest because it achieved higher accuracy and better F1, and is more robust to scaling.”).
- **Add:** A sentence or short paragraph stating which classifier you select as final and why (metrics + brief justification).

### 3. **“Justify your final choice” for regression** (Spec §3.4)
- You train Linear Regression and RF Regressor but do not explicitly **select one** and **justify** (e.g. “We choose Random Forest Regressor because it has lower RMSE and higher R².”).
- **Add:** A sentence or short paragraph stating which regression model you select as final and why.

### 4. **“Proper machine learning pipelines”** (Spec §4)
- Spec: “All preprocessing must be performed using proper machine learning pipelines.”
- You use `StandardScaler` and `train_test_split` manually but not `sklearn.pipeline.Pipeline` (or `make_pipeline`) to chain preprocessing + model.
- **Add:** For at least one classification and one regression workflow, use e.g. `make_pipeline(StandardScaler(), LogisticRegression(...))` and fit on training data so preprocessing is part of the pipeline (and can be reused for CV/prediction). You can keep existing code and add a “Pipeline version” cell, or refactor one full example into a pipeline.

### 5. **Cross-validation for regression** (Spec §4)
- Spec: “use an appropriate train/test split and cross-validation strategy (for classification and regression only).”
- You use CV for classification but only a single train/test split for regression.
- **Add:** At least one cross-validation for regression (e.g. `cross_val_score` with `scoring='neg_mean_squared_error'` or `'r2'`) and report mean (and optionally std) RMSE or R².

### 6. **“Justify the metric used for model selection”** (Spec §4)
- You use accuracy, RMSE, R² but do not explicitly say **why** these metrics (e.g. accuracy for balanced binary target; RMSE/R² for continuous popularity).
- **Add:** One or two sentences per task (classification and regression) stating why you use the chosen metric(s) for model selection.

### 7. **“Analyse feature importance … for regression”** (Spec §4)
- Spec: “Where applicable, analyse feature importance (e.g., model coefficients, feature importance scores, or alternative methods).”
- You have feature importance for the classifier but not for regression.
- **Add:** For regression, either (a) coefficients for Linear Regression, or (b) `rfr.feature_importances_` for Random Forest Regressor (e.g. bar plot), with a short interpretation.

### 8. **EDA → clustering/prediction** (Spec §3.1)
- Spec: “Comment clearly on your observations and findings and **how they can inform** the following tasks on clustering and predictive analysis.”
- EDA is present but the link to “how this informs clustering and predictive analysis” could be clearer.
- **Add:** A short EDA subsection or paragraph (e.g. after the visualisations) stating how EDA findings inform preprocessing, clustering (e.g. scaling, choice of features), and classification/regression (e.g. which features might matter, missing values).

### 9. **“Explain why clustering succeeded or failed”** (Spec §3.2)
- You discuss that clusters do not align with genre and that they reflect audio features. You could make the **success/failure** wording explicit.
- **Add:** One or two sentences that explicitly say whether you consider clustering to have “succeeded” or “failed” and why (e.g. “Clustering succeeded in revealing acoustically coherent groups but failed to recover genre labels, which is expected because genre was not used as a feature.”).

---

## Optional / nice to have

- **Preprocessing justification:** Short justification for dropping missing rows, scaling, and not using `track_id`/`track_genre` in models (you partly do this; making it a single “Preprocessing choices” subsection would align with the spec).
- **Regression “separate copy”:** Spec says “Use a separate copy of the dataset in which the original popularity column is retained.” You use `df_clean` for both classification and regression; regression does not use the binarised target. For maximum clarity, you could create e.g. `df_reg = df_clean.copy()` and use it only for regression, with a one-line comment that this is the “separate copy” for regression.

---

## Summary

- **Fully met:** Dataset, EDA structure, clustering (k-means + DBSCAN, track_genre dropped, k comparison, visualisation, genre discussion), classification (target, two models, CV, feature importance), regression (two models, difficulty vs classification), and general structure.
- **To align fully with the spec:** Add explicit **selection and justification** for (1) final clustering solution, (2) final classification model, (3) final regression model; add **pipelines** (at least one example with `Pipeline`), **regression CV**, **metric justification**, **regression feature importance**, and a clearer **EDA → clustering/prediction** link and **“clustering succeeded/failed”** statement.
