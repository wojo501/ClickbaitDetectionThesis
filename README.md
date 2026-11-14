<div align="center">

# 🎣 Clickbait News Detection and Analysis

### Bachelor Thesis · Warsaw University of Technology

*Faculty of Mathematics and Information Science*

[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebooks-F37626?logo=jupyter&logoColor=white)](notebooks)
[![XGBoost](https://img.shields.io/badge/XGBoost-Best%20Model-189FDD)](final_models)
[![OpenAI](https://img.shields.io/badge/OpenAI-Embeddings%20%26%20Summaries-412991?logo=openai&logoColor=white)](notebooks)
[![HuggingFace](https://img.shields.io/badge/🤗%20Transformers-BERT%20%7C%20RoBERTa%20%7C%20T5-FFD21E)](notebooks)
[![Chrome Extension](https://img.shields.io/badge/Chromium-Extension-4285F4?logo=googlechrome&logoColor=white)](extension)
[![Conda](https://img.shields.io/badge/Conda-Environment-44A833?logo=anaconda&logoColor=white)](environments)

**🏆 Best model: XGBoost + OpenAI embeddings + informativeness measures → F1-score = 0.909**

[Abstract](#-abstract) •
[Data](#-data) •
[Baitness measure](#-baitness-measure) •
[Results](#-results) •
[Browser Extension](#-browser-extension-clickguard) •
[Project Structure](#-project-structure) •
[Setup](#%EF%B8%8F-setup) •
[Authors](#-authors)

<br/>

<img src="assets/visual_abstract.png" alt="Visual abstract of the thesis" width="100%"/>

</div>

---

## 📖 Abstract

This engineering thesis analyses **clickbait articles** using machine learning and artificial intelligence methods. A review of existing research led us to create a custom metric, the **_baitness measure_**, which measures how clickbait-like an article is.

<table>
<tr>
<td width="50%" valign="top">

### 🔍 Detection
- NLP techniques to find patterns in **article titles**
- ML models trained on vectorized text representations
- Transformer-based LLMs used to turn text into **embeddings**
- 15 **informativeness measures**, including the custom **baitness measure**

</td>
<td width="50%" valign="top">

### ✨ Spoiling & Explanation
- Generative AI **summarizes suspicious articles**
- One- to two-sentence spoiler: is it worth your time?
- Predictions **explained** using the analysed metrics
- Shown to users in a **browser extension**

</td>
</tr>
</table>

---

## 🔄 Pipeline

```mermaid
flowchart LR
    A[📰 Raw news data] --> B[🧹 Cleaning &<br/>preprocessing]
    B --> C[📊 EDA]
    C --> D[📐 Informativeness<br/>measures]
    B --> E[🔤 Text vectorization]
    E --> E1[TF-IDF]
    E --> E2[Word2Vec / GloVe]
    E --> E3[OpenAI embeddings]
    E --> E4[RoBERTa]
    D --> F[🤖 Classifiers]
    E1 --> F
    E2 --> F
    E3 --> F
    E4 --> F
    F --> G[🏆 XGBoost<br/>F1 = 0.909]
    G --> H[🧩 Browser extension]
    I[✍️ Spoiling models<br/>RoBERTa · T5 · OpenAI] --> H
```

---

## 📚 Data

We merged several public clickbait datasets (two Kaggle datasets and the Clickbait Challenge) into one balanced dataset of news titles.

<div align="center">
<img src="assets/dataset_sankey.png" alt="Clickbait distribution by dataset" width="90%"/>
<br/><sub><i>Clickbait distribution by source dataset.</i></sub>
<br/><br/>
<img src="assets/top_bigrams.png" alt="Most frequent bigrams in titles" width="85%"/>
<br/><sub><i>Most frequent 2-grams in non-clickbait (green) and clickbait (red) titles.</i></sub>
</div>

---

## 📐 Baitness measure

A custom score built from informativeness measures such as capital letters ratio, second-person pronouns, bait phrases, superlatives, punctuation and readability. **On its own it reaches ROC-AUC = 0.83.**

<p align="center">
  <img src="assets/baitness_distribution.png" alt="Baitness score distribution" width="45%"/>
  &nbsp;
  <img src="assets/baitness_roc.png" alt="ROC curve for baitness measure" width="45%"/>
</p>
<p align="center"><sub><i>Left: baitness score for non-clickbait vs. clickbait titles. Right: ROC curve of the baitness measure used as a classifier (test set).</i></sub></p>

---

## 📊 Results

<div align="center">
<img src="assets/model_comparison.png" alt="Best F1-score per approach" width="85%"/>
</div>

<br/>

<div align="center">

| Approach | Model | F1-score | ROC-AUC |
|:--|:--|:--:|:--:|
| Informativeness measures | Logistic Regression | 0.806 | 0.873 |
| Informativeness measures | Random Forest | 0.822 | 0.897 |
| TF-IDF (raw titles) | Random Forest | 0.829 | 0.906 |
| Word2Vec (Google News) | XGBoost | 0.829 | 0.908 |
| GloVe | XGBoost | 0.829 | 0.908 |
| Direct prompting | GPT-4o-mini | 0.813 | – |
| Fine-tuned transformer | RoBERTa | 0.842 | 0.910 |
| OpenAI embeddings (3072-d) | XGBoost | 0.864 | 0.938 |
| **OpenAI embeddings (1000-d) + informativeness measures** ⭐ | **XGBoost** | **0.909** | **0.969** |

</div>

<p align="center">
  <img src="assets/f1_vs_dimensions.png" alt="F1-score vs number of embedding dimensions" width="48%"/>
  &nbsp;
  <img src="assets/confusion_matrix_best.png" alt="Confusion matrix of the best model" width="40%"/>
</p>
<p align="center"><sub><i>Left: effect of the number of OpenAI embedding dimensions on F1-score. Right: confusion matrix of the best model (XGBoost, 1000-d embeddings + informativeness measures).</i></sub></p>

---

## 🧩 Browser Extension: Clickguard

A fully working extension for **Chromium-based browsers** (Chrome, Edge, Brave, Opera) that protects users **before and after** they open a clickbait article.

<table>
<tr>
<td align="center" width="40%">
<img src="assets/extension_popup.png" alt="Clickguard popup" width="90%"/>
<br/><b>📈 Score, spoiler & explanation</b><br/><sub>Clickbait probability, a short spoiler and the reasons behind the prediction</sub>
</td>
<td align="center" width="60%">
<img src="assets/extension_post_click.png" alt="Post-click detection on a news article" width="100%"/>
<br/><b>📰 Post-click detection</b><br/><sub>Probable (71%) clickbait with spoiler and explanation</sub>
</td>
</tr>
<tr>
<td align="center" width="40%">
<img src="assets/extension_settings.png" alt="Pre-click settings" width="90%"/>
<br/><b>⚙️ Pre-click settings</b><br/><sub>Search engine and news portal detection</sub>
</td>
<td align="center" width="60%">
<img src="assets/extension_google_warnings.png" alt="Pre-click warnings in Google results" width="100%"/>
<br/><b>⚠️ Pre-click warnings</b><br/><sub>Clickbait links flagged in search results before you click</sub>
</td>
</tr>
</table>

### Architecture

<div align="center">
<img src="assets/system_architecture.png" alt="System architecture" width="85%"/>
</div>

```mermaid
flowchart LR
    U[👤 User browsing] --> X[🧩 Extension<br/>JavaScript]
    X -- article title & content --> API[⚙️ Python API]
    API --> M[🏆 XGBoost model<br/>+ baitness measure]
    API --> S[✍️ Spoiler generation<br/>OpenAI]
    M -- score & explanation --> X
    S -- spoiler --> X
```

---

## 📁 Project Structure

```
ClickbaitDetectionThesis/
├── api/             # Python backend service for the browser extension
├── assets/          # Images used in this README
├── data/            # Original and preprocessed data
├── environments/    # Conda environment files
├── extension/       # JavaScript code of the browser extension
├── final_models/    # Final models used by the extension
├── notebooks/       # Jupyter notebooks (research & experiments)
└── other/           # Minor utilities
```

<details>
<summary><b>📓 Notebooks overview (click to expand)</b></summary>

<br/>

| # | Notebook | Description |
|:--:|:--|:--|
| 0 | `0-Function-Helper` | Frequently used helper functions |
| 1 | `1-Getting-Data` | Downloading, saving and merging data; pre-cleaning |
| 2 | `2-EDA` | Exploratory data analysis |
| 2.1 | `2.1-Creating_dataset_[dataset_name]` | Building the final training datasets |
| 2.5 | `2.5-Dummy-Classifier` | Dummy classifier baseline |
| 3 | `3-Informativeness-Measures` | Clickbait metrics; basis of the custom measure |
| 4 | `4-TFIDF-Trivial` | TF-IDF base model |
| 5 | `5-TFIDF_Traditional_ML_Models` | TF-IDF on cleaned vs. raw datasets |
| 6 | `6-Word-Embedding-Models` | Word2Vec and GloVe models |
| 7.5 | `7.5-LLM-OPENAI` | OpenAI embeddings for ML models |
| 8 | `8-BERT-Detection` | BERT for clickbait detection |
| 8.5 | `8.5-BERT-Spoiling` | Fine-tuned RoBERTa and T5-Large for spoiling |
| 9 | `9-OPENAI-Spoiling` | OpenAI models for spoiling |

</details>

---

## ⚙️ Setup

**1. Clone the repository and go to the environments directory**
```bash
git clone https://github.com/matkubita/ClickbaitDetectionThesis.git
cd ClickbaitDetectionThesis/environments
```

**2. Create the Conda environment** (use `eda_env.yaml` for the EDA environment)
```bash
conda env create -f main_env.yaml
```

**3. Activate it**
```bash
conda activate main_env
```

You're ready to run the notebooks. 🚀

---

## 👥 Authors

<div align="center">

<table>
<tr>
<td align="center">
<a href="https://github.com/tymsoncyferki"><img src="https://github.com/tymsoncyferki.png" width="100px" alt="Tymoteusz Urban"/><br/><sub><b>Tymoteusz Urban</b></sub></a>
</td>
<td align="center">
<a href="https://github.com/matkubita"><img src="https://github.com/matkubita.png" width="100px" alt="Mateusz Kubita"/><br/><sub><b>Mateusz Kubita</b></sub></a>
</td>
<td align="center">
<a href="https://github.com/wojo501"><img src="https://github.com/wojo501.png" width="100px" alt="Wojciech Michaluk"/><br/><sub><b>Wojciech Michaluk</b></sub></a>
</td>
</tr>
</table>

**Supervisor**

<a href="https://github.com/awroble"><img src="https://github.com/awroble.png" width="80px" alt="Anna Wróblewska"/><br/><sub><b>Anna Wróblewska</b></sub></a>

**Cooperators**

<a href="https://github.com/DanielDanLab">Daniel Dan</a> ·
Robert Paluch ·
<a href="https://github.com/amajczyk">Adam Majczyk</a>

</div>

---

<div align="center">
<sub>Made at the <b>Warsaw University of Technology</b> · Faculty of Mathematics and Information Science</sub>
</div>
