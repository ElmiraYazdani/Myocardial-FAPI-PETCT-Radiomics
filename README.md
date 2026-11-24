
# 🫀 Myocardial FAPI PET/CT Radiomics

### Myocardial [⁶⁸Ga]Ga-FAPI-2286 PET/CT Radiomics for Predicting Adverse Cardiac Events in Heart Failure and Coronary Artery Disease

![Python](https://img.shields.io/badge/Python-3.13-3776AB?logo=python&logoColor=white)
![License](https://img.shields.io/badge/Code-MIT-2EA44F)
![Data](https://img.shields.io/badge/Data-CC_BY_4.0-blue)
![Research](https://img.shields.io/badge/Research-Pilot_Study-7952B3)
![Validation](https://img.shields.io/badge/Validation-Nested_CV-008080)
![Status](https://img.shields.io/badge/Manuscript-Submitted-orange)

---

## 📖 Overview

Myocardial fibroblast activation plays a central role in adverse cardiac remodeling and the progression of cardiovascular disease. Fibroblast activation protein inhibitor (FAPI) PET provides a noninvasive approach to characterizing fibroblast activation, while radiomics enables quantitative assessment of myocardial intensity, morphology, and spatial heterogeneity beyond conventional uptake metrics.

This repository accompanies a pilot feasibility study investigating the prognostic potential of myocardial **[⁶⁸Ga]Ga-FAPI-2286 PET/CT radiomics** in patients with heart failure and coronary artery disease (CAD).

The proposed framework integrates standardized myocardial segmentation, multimodal radiomic feature extraction, machine learning, rigorous internal validation, and explainable artificial intelligence (XAI).

A central objective is to evaluate not only predictive discrimination but also the extent to which insufficient validation may lead to optimistic performance estimates in small, high-dimensional clinical datasets.

---

## 🎯 Research Objectives

- Develop a standardized framework for myocardial PET/CT radiomic characterization.
- Investigate patient-level and segment-level radiomic modeling strategies.
- Evaluate the prognostic potential of myocardial FAPI uptake and tissue heterogeneity.
- Compare radiomic models with conventional clinical and imaging biomarkers.
- Quantify model optimism through apparent and internally validated performance.
- Identify candidate imaging biomarkers using explainable machine learning.
- Support transparent and reproducible cardiovascular imaging research.

---

## 📊 Study Cohort

| Characteristic | Description |
|:---|:---|
| 👥 Patients | 20 |
| ❤️ Clinical population | Heart failure and coronary artery disease |
| ⚠️ Adverse cardiac events | 7 patients (35%) |
| ✅ No qualifying event | 13 patients (65%) |
| 🧬 Radiotracer | [⁶⁸Ga]Ga-FAPI-2286 |
| 🩻 Imaging modalities | FAPI PET and CT |
| 🫀 Myocardial segmentation | AHA 17-segment model |
| 📐 Additional segmentation | 7 manually delineated cardiac structures |
| 📈 Radiomic features | 107 per region and imaging modality |
| 🧪 Modeling experiments | 3 |
| 🤖 Machine learning classifiers | 10 |

---

## 🧪 Experimental Design

Three complementary experiments were conducted using the same patient cohort and clinical outcome.

| Experiment | Modeling strategy | Unit of analysis | Experimental design |
|:---:|:---|:---|:---|
| **E1** | Patient-level radiomics | 20 patients | 11 feature sets × 10 classifiers; repeated nested cross-validation |
| **E2** | Segment-level radiomics | 340 myocardial segments / 140 cardiac structures | Regional modeling with patient-grouped cross-validation |
| **E3** | Prespecified FAPI uptake-based modeling | 20 patients | 12 logistic regression models, nested model selection, and 500 label permutations |

### 👤 E1 — Patient-Level Radiomic Modeling

Radiomic features extracted from the 17 myocardial segments were aggregated using their mean and standard deviation to generate patient-level predictors.

Features extracted from manually delineated cardiac structures were retained individually.

| Segmentation approach | PET | CT | Combined PET/CT |
|:---|---:|---:|---:|
| AHA 17-segment model | 214 | 214 | 428 |
| Manual cardiac structures | 749 | 749 | 1,498 |

Clinical, radiomic, and combined feature sets were evaluated using ten machine learning classifiers.

### 🫀 E2 — Segment-Level Radiomic Modeling

Regional radiomic features were analyzed individually to investigate the potential prognostic value of spatial myocardial information.

- 340 myocardial segments.
- 140 manually delineated cardiac structures.
- Patient-grouped training and testing partitions.
- Patient-level prediction through aggregation of regional probabilities.

All regions belonging to an individual patient were assigned exclusively to either the training or testing partition within each split.

### 📈 E3 — Prespecified FAPI Uptake-Based Modeling

Twelve logistic regression models were evaluated using one or two physiologically motivated predictors, including:

- Global myocardial FAPI uptake.
- Maximum segmental uptake.
- Mean uptake of the three highest-uptake segments.
- Intersegment uptake heterogeneity.
- Extent of elevated myocardial uptake.
- Maximum coronary territory uptake.
- Myocardium-to-aorta uptake ratio.
- Top-three-segment-to-aorta uptake ratio.
- Left ventricular ejection fraction (LVEF).

Nested model selection and 500 full-procedure label permutations were used to assess predictive performance and statistical significance.

---

## 🤖 Machine Learning Classifiers

The following classification algorithms were investigated:

| Category | Classifier |
|:---|:---|
| Linear models | Elastic-net logistic regression |
| Support vector machines | Linear SVM |
| Support vector machines | RBF SVM |
| Ensemble learning | Random forest |
| Ensemble learning | Extremely randomized trees |
| Boosting | Gradient boosting |
| Boosting | XGBoost |
| Boosting | LightGBM |
| Instance-based learning | k-nearest neighbors |
| Probabilistic learning | Gaussian naive Bayes |

Hyperparameter optimization was performed using **Optuna** within the inner cross-validation partitions.

---

## 🛡️ Leakage-Controlled Validation

All data-dependent procedures were confined to the corresponding training partitions.

### Preprocessing and Feature Selection

- Constant-feature removal.
- Median imputation.
- Yeo–Johnson transformation.
- Volume-dependence filtering.
- Redundancy filtering.
- Minimum redundancy–maximum relevance (mRMR).
- Elastic-net stability selection.
- Feature standardization.
- Synthetic minority oversampling (SMOTE).
- Optuna-based hyperparameter optimization.

### Internal Validation

| Component | Configuration |
|:---|:---|
| Outer cross-validation | Stratified 5-fold |
| Repetitions | 20 |
| Inner cross-validation | Stratified 3-fold |
| Segment-level partitioning | Patient-grouped |
| Random seed | 100 |
| Confidence intervals | 2,000 patient-level bootstrap resamples |
| E3 permutation testing | 500 permutations |
| E3 bootstrap optimism correction | 1,000 resamples |

This design was implemented to minimize information leakage, account for within-patient correlations, and reduce model-selection bias.

---

## 📉 Performance Evaluation and Model Optimism

Predictive performance was assessed using:

- Area under the receiver operating characteristic curve (ROC-AUC).
- Balanced accuracy.
- Sensitivity and specificity.
- Bootstrap 95% confidence intervals.
- Paired comparisons with conventional reference models.
- Holm correction for multiple comparisons.
- Label-permutation testing.
- Sensitivity analysis excluding patient P12.
- Comparison of apparent and internally validated performance.

Bootstrap optimism correction was additionally performed for the prespecified uptake-based and conventional reference models.

---

## 🔍 Explainable Artificial Intelligence (XAI)

Shapley additive explanations (**SHAP**) were employed to investigate individual feature contributions and the consistency of model explanations.

The explainability framework includes:

- Out-of-fold SHAP analysis.
- TreeExplainer for tree-based ensemble models.
- LinearExplainer for logistic regression.
- Feature selection frequency across outer training partitions.
- Comparison of final-model and out-of-fold feature contributions.
- Global and individual feature visualization.

SHAP analysis is exploratory and is not used for model selection, performance estimation, or causal inference.

---

## 💡 Principal Findings

- Standardized myocardial radiomic feature extraction was technically feasible across all evaluated cardiac regions.
- CT-derived LV segmental radiomics provided the most consistent exploratory radiomic signal.
- Global myocardial FAPI uptake was numerically higher in patients experiencing adverse cardiac events.
- Apparent predictive performance was substantially higher than internally validated performance, highlighting the importance of rigorous validation.
- Radiomic models did not establish statistically significant incremental predictive value over conventional reference biomarkers.

The findings identify candidate imaging biomarkers and provide a methodological foundation for future adequately powered, externally validated studies.

> [!IMPORTANT]
> This repository represents an exploratory pilot research study.
> The models are not intended for clinical decision-making or individual patient risk assessment.

---

## 📁 Repository Structure

```text
Myocardial-FAPI-PETCT-Radiomics/
│
├── data/
│   ├── Clinical and radiomic feature datasets
│   └── DATA_DICTIONARY.md
│
├── 01_segmentation/
│   └── Myocardial segmentation and AHA LV17 generation
│
├── 02_radiomics/
│   └── Label verification and radiomic feature extraction
│
├── 03_analysis/
│   └── E1, E2, E3, SHAP and sensitivity analyses
│
├── 04_apparent_auc/
│   └── Apparent performance and optimism correction
│
├── 05_manuscript_figures/
│   └── Study design and performance visualization
│
├── requirements.txt
├── run_all.sh
├── LICENSE
└── README.md
```

### Repository Modules

| Module | Description | Runs on shared data |
|:---|:---|:---:|
| `data/` | Pseudonymized clinical and radiomic datasets | — |
| `01_segmentation/` | PET/CT-guided myocardial segmentation | ❌ |
| `02_radiomics/` | PyRadiomics extraction and label verification | ❌ |
| `03_analysis/` | Machine learning, validation, SHAP, and sensitivity analyses | ✅ |
| `04_apparent_auc/` | Apparent AUC and bootstrap optimism correction | ✅ |
| `05_manuscript_figures/` | Manuscript figure generation | ✅ |
| `run_all.sh` | End-to-end execution of analysis modules | ✅ |

> [!NOTE]
> The original PET/CT images and segmentation masks are not publicly distributed. Consequently, the segmentation and radiomic extraction modules require access to the original imaging data.

---

## ⚙️ Installation

### Requirements

- Python 3.13
- Bash or Git Bash
- Multicore CPU recommended

### Create a Virtual Environment

```bash
python -m venv .venv
```

**Linux / macOS**

```bash
source .venv/bin/activate
```

**Windows**

```bash
.venv\Scripts\activate
```

### Install Dependencies

```bash
pip install -r requirements.txt
```

### Run the Complete Analysis

```bash
PY=python JOBS=6 bash run_all.sh
```

On Windows, execute the shell script using Git Bash.

| Configuration | Details |
|:---|:---|
| Estimated runtime | Approximately 6 hours |
| Reference hardware | 6-core desktop CPU |
| Output directory | `runs/` |

Runtime may vary depending on the hardware configuration and software environment.

---

## 📤 Generated Outputs

| Output path | Description |
|:---|:---|
| `runs/Primary_withP12/E1_patient-level/summary/performance_summary.csv` | Patient-level classification performance and confidence intervals |
| `runs/Primary_withP12/E2_lesion-level/summary/` | Segment-level and aggregated patient-level performance |
| `runs/Primary_withP12/E3_patient-aggregated/results/` | FAPI uptake models, LVEF comparisons, and permutation results |
| `runs/Sensitivity_withoutP12/` | E1 and E2 sensitivity analyses |
| `runs/apparent_auc/` | Apparent and internally validated performance; Supplementary Table S3 |
| `*/figures_manuscript/` | Manuscript figures |
| `E3_patient-aggregated/figures/` | FAPI uptake analysis figures |
| `XAI_SHAP/figures/` | SHAP-based model interpretation |

---

## 🧰 Software and Dependencies

| Library | Application |
|:---|:---|
| NumPy | Numerical computation |
| pandas | Data management |
| PyRadiomics | Radiomic feature extraction |
| scikit-learn | Machine learning and cross-validation |
| Optuna | Hyperparameter optimization |
| imbalanced-learn | Class imbalance management |
| XGBoost | Gradient boosting |
| LightGBM | Gradient boosting |
| SHAP | Explainable artificial intelligence |
| Matplotlib | Scientific visualization |

Exact dependency specifications are provided in `requirements.txt`.

---

## 🔄 Reproducibility

All random seeds are fixed at **100**.

The repository is designed to support reproducible execution of the machine learning, statistical evaluation, and visualization workflows using the shared derived datasets.

Minor numerical differences may occur across:

- Operating systems.
- Python and library versions.
- BLAS implementations.
- Hardware configurations and parallel execution environments.

For optimal reproducibility, use the software versions specified in `requirements.txt`.

---

## 🔐 Data Availability and Privacy

The `data/` directory contains derived, pseudonymized research data, including:

- Study-specific patient identifiers.
- Radiomic feature datasets.
- 22 clinical variables.
- Binary clinical outcome labels.

No directly identifying information, patient names, medical record numbers, dates, or original medical images are included.

The original PET/CT images and segmentation masks are not publicly available. Requests for access should be directed to the corresponding authors and are subject to institutional and ethical requirements.

See [`DATA_DICTIONARY.md`](data/DATA_DICTIONARY.md) for detailed variable definitions.

---

## 📚 Associated Publication

This repository accompanies the following manuscript:

> Karimi M, Yazdani E, Salimi Y, Karamzade-Ziarati N, Vosoughi H, Rasaneh S, Mahdavi SR, Zaidi H.
>
> **Myocardial [⁶⁸Ga]Ga-FAPI-2286 PET/CT Radiomics for Predicting Adverse Cardiac Events in Heart Failure and Coronary Artery Disease: A Leakage-Controlled Pilot Feasibility Study.**
>
> Manuscript submitted.

### Citation

If you use the code, data, or methodological framework in your research, please cite the associated manuscript.

```bibtex
@unpublished{karimi_fapi_radiomics,
  author = {Karimi, M. and
            Yazdani, E. and
            Salimi, Y. and
            Karamzade-Ziarati, N. and
            Vosoughi, H. and
            Rasaneh, S. and
            Mahdavi, S. R. and
            Zaidi, H.},
  title = {Myocardial [68Ga]Ga-FAPI-2286 PET/CT Radiomics
           for Predicting Adverse Cardiac Events in Heart
           Failure and Coronary Artery Disease:
           A Leakage-Controlled Pilot Feasibility Study},
  note = {Manuscript submitted}
}
```

The final bibliographic information and DOI will be added following publication.

---

## 📜 License

| Resource | License |
|:---|:---|
| Source code | [MIT License](LICENSE) |
| Shared derived datasets | [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) |

Appropriate attribution to the original study and authors is requested when using the code or datasets.

---

## 🤝 Contact and Contributions

Scientific discussions, methodological feedback, reproducibility reports, and research collaborations are welcome.

For questions regarding the implementation, please use the GitHub Issues section.

For scientific inquiries or requests concerning the original imaging data, please contact the corresponding authors.

---

<div align="center">

### 🫀 Molecular Imaging · Radiomics · Machine Learning

**Advancing transparent and reproducible cardiovascular imaging research**

</div>
