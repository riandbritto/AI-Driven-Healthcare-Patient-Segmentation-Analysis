# 🏥 AI-Driven Healthcare Patient Segmentation Analysis

![Python](https://img.shields.io/badge/Python-3.8+-blue?logo=python)
![scikit-learn](https://img.shields.io/badge/scikit--learn-ML-orange?logo=scikit-learn)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange?logo=jupyter)
![Dataset](https://img.shields.io/badge/Dataset-MIMIC--III-green)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)

> Unsupervised machine learning project that segments ICU patients from the MIMIC-III clinical database using K-Means and Hierarchical Clustering, with PCA for dimensionality reduction. Designed to surface meaningful patient subgroups that can inform clinical resource allocation and care planning.

---

## 📌 Table of Contents
- [Project Overview](#project-overview)
- [Dataset](#dataset)
- [Tech Stack](#tech-stack)
- [Project Workflow](#project-workflow)
- [Key Results](#key-results)
- [Visualizations](#visualizations)
- [How to Run](#how-to-run)
- [Project Structure](#project-structure)
- [Learnings & Takeaways](#learnings--takeaways)

---

## Project Overview

Healthcare systems generate enormous amounts of patient data — demographics, lab results, and vital signs — yet much of it goes underutilized for proactive care decisions. This project applies **unsupervised machine learning** to real ICU data from the **MIMIC-III clinical database** to:

- Identify distinct patient segments based on physiological and demographic signals
- Compare the performance of **K-Means** vs **Hierarchical Clustering**
- Use **PCA** to reduce dimensionality while preserving meaningful variance
- Provide a reproducible analysis pipeline that can scale to larger clinical datasets

---

## Dataset

**Source:** [MIMIC-III Clinical Database](https://physionet.org/content/mimiciii/1.4/) via the [mimic-iii-project](https://github.com/DanielSola/mimic-iii-project) repository

The dataset contains de-identified ICU patient records across three data categories:

| Data Type | File | Description |
|---|---|---|
| **Demographic** | `DEMO_DATA.csv` | Age, gender, admission type |
| **Lab Tests** | `BLOOD_GLUCOSE.csv` | Blood glucose measurements over time |
| **Vital Signs** | `HR.csv` | Heart rate readings per patient |

- **Sample size used:** 500 patients (random sample, `random_state=42`)
- **Final feature set:** 50+ clinical variables after merging all three sources

> ⚠️ MIMIC-III requires credentialed access via PhysioNet. This project uses a public subset from the mimic-iii-project repo.

---

## Tech Stack

| Tool | Purpose |
|---|---|
| **Python 3.8+** | Core programming language |
| **pandas & NumPy** | Data loading, cleaning, and transformation |
| **scikit-learn** | StandardScaler, PCA, KMeans, silhouette_score, davies_bouldin_score |
| **SciPy** | Hierarchical clustering (linkage, dendrogram, fcluster) |
| **Matplotlib & Seaborn** | Cluster visualizations, heatmaps, scatter plots |
| **Google Colab** | Notebook execution environment |
| **Excel / CSV** | Final output export for stakeholder reporting |

---

## Project Workflow

```
Raw Data (3 sources)
        ↓
1. Data Loading & Merging
        ↓
2. Missing Value Treatment (median imputation)
        ↓
3. Z-Score Normalization (StandardScaler)
        ↓
4. Exploratory Data Analysis (EDA)
        ↓
5. Dimensionality Reduction (PCA — 95% variance retained)
        ↓
6. K-Means Clustering (Elbow Method → optimal k=4)
        ↓
7. Hierarchical Clustering (Ward linkage + Dendrogram)
        ↓
8. Model Evaluation (Silhouette Score, Davies-Bouldin Index)
        ↓
9. Cluster Comparison & Alignment Heatmap
        ↓
10. Export Final Clustered Dataset (final_clustered_data.csv)
```

---

## Key Results

| Metric | K-Means | Hierarchical |
|---|---|---|
| **Optimal Clusters** | 4 (Elbow Method) | 2–4 (Dendrogram) |
| **Silhouette Score** | 0.303 | Higher (better separation) |
| **Cluster Granularity** | Broader groups | More subgroups identified |
| **Best Use Case** | Speed, scalability | Interpretability, subgroup discovery |

### Key Findings:
- **PCA** retained **95% of variance** while significantly reducing feature dimensions, improving clustering efficiency
- **Hierarchical clustering** outperformed K-Means in separation quality — it identified more meaningful subgroups within the patient population
- **K-Means** showed overlapping clusters (Silhouette: 0.303), suggesting the patient data has complex, non-spherical boundaries
- The **cluster alignment heatmap** revealed that hierarchical clusters captured finer subgroups within each K-Means cluster
- **4 dominant patient segments** emerged, distinguishable by combinations of glucose levels, heart rate patterns, and demographic features

---

## Visualizations

The notebook includes the following plots:

| Visualization | Purpose |
|---|---|
| **Elbow Curve** | Determine optimal K for K-Means |
| **K-Means Scatter Plot** | Visualize cluster separation in 2D |
| **Dendrogram** | Hierarchical merge distances and natural cut points |
| **Hierarchical Scatter Plot** | Cluster assignments in feature space |
| **Cluster Alignment Heatmap** | Cross-compare K-Means vs Hierarchical labels |
| **K-Means vs Hierarchical Scatter** | Side-by-side cluster assignment comparison |
| **Silhouette Score vs K** | Evaluate best number of clusters across k=2 to 9 |

---

## How to Run

### Option 1 — Google Colab (Recommended)
1. Open the notebook in [Google Colab](https://colab.research.google.com/)
2. Run the first cell — it automatically clones the MIMIC-III data repository
3. Run all cells top to bottom

### Option 2 — Local Setup
```bash
# 1. Clone this repo
git clone https://github.com/your-username/healthcare-segmentation
cd healthcare-segmentation

# 2. Install dependencies
pip install -r requirements.txt

# 3. Clone the data repo
git clone http://github.com/DanielSola/mimic-iii-project

# 4. Launch Jupyter
jupyter notebook Healthcare_Analysis.ipynb
```

### Requirements
```
pandas
numpy
matplotlib
seaborn
scikit-learn
scipy
```

---

## Project Structure

```
healthcare-segmentation/
│
├── Healthcare_Analysis.ipynb    # Main analysis notebook
├── final_clustered_data.csv     # Output: clustered patient dataset
├── requirements.txt             # Python dependencies
└── README.md                    # Project documentation
```

---

## Learnings & Takeaways

- **Data quality matters most** — merging 3 clinical data sources with different formats required careful alignment and median imputation before any modeling could begin
- **PCA before clustering** dramatically improves both runtime and cluster quality when dealing with 50+ correlated clinical features
- **No single algorithm wins** — K-Means is fast and interpretable but struggles with overlapping real-world clinical data; Hierarchical clustering revealed more nuanced patient subgroups at the cost of scalability
- **Silhouette Score alone isn't enough** — combining it with Davies-Bouldin Index and visual inspection (dendrogram, scatter plots) gives a more complete picture of cluster quality
- Clinical data segmentation has **real downstream value** — the resulting patient clusters can inform staffing, medication protocols, and early warning systems

---

## Author

**Rian Renold D'Britto**
MS Data Analytics Engineering — Northeastern University
📧 dbritto.r@northeastern.edu
🔗 [LinkedIn](https://linkedin.com/in/rian-dbritto) | [GitHub](https://github.com/rian-dbritto)

---

⭐ If you found this project useful, give it a star!
