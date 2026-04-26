<div align="center">

# 🌪️ From Embeddings to Clusters
### A Bilingual NLP Approach to Disaster Event Detection

*Detecting natural disasters from Bengali & English text using multilingual BERT and FAISS clustering.*

![Python](https://img.shields.io/badge/Python-3.9%2B-blue?logo=python&logoColor=white)
![HuggingFace](https://img.shields.io/badge/🤗-Transformers-yellow)
![FAISS](https://img.shields.io/badge/FAISS-Meta_AI-0467DF)
![License](https://img.shields.io/badge/License-MIT-green)
![Status](https://img.shields.io/badge/Status-Research-orange)

</div>

---

## 🧭 Overview

Bangladesh sits at the frontline of climate-driven disasters — cyclones, floods, landslides, earthquakes — yet most automated event-detection tools are built for English. When **Cyclone Amphan** struck in 2020 and the **Sylhet floods** submerged the northeast in 2022, the information was *already there* in news articles and social posts. What was missing was a system that could read it.

This project closes that gap with a **bilingual (Bengali + English) disaster detection framework** that turns unstructured news text into actionable signals using contextual embeddings and similarity-based clustering.

> **TL;DR** — Multilingual BERT generates context-aware sentence embeddings → FAISS clusters them by semantic similarity → the system identifies disaster events without relying on rigid keyword rules.

---

## ✨ Highlights

- 🌐 **Truly bilingual** — handles Bengali and English in a unified embedding space
- 🧠 **Context over keywords** — catches metaphorical vs. real disaster mentions
- 🏷️ **20 fine-grained categories** — 18 specific disaster types + general *Disaster* / *Non-Disaster* tags
- ⚡ **FAISS-powered retrieval** — fast approximate nearest neighbor search at scale
- 📚 **Manually curated dataset** — 4,000 sentences, balanced across categories and languages
- 🔬 **Built for low-resource NLP** — first known application of mBERT + FAISS to Bengali disaster text

---

## 🏗️ Architecture

```
┌─────────────────┐     ┌──────────────────┐     ┌────────────────┐     ┌──────────────────┐     ┌──────────────────┐
│   News / Text   │ ──▶│  Preprocessing   │ ──▶ │  mBERT-base   │ ──▶ │  FAISS Index     │ ──▶ |  Disaster Tag    │
│  (BN + EN)      │     │  Crawl • Tokenize│     │  Cased         │     │  ANN Clustering  │     │  + Confidence    │
└─────────────────┘     │  Lemmatize       │     │  (104 langs)   │     │  θ ≥ 0.8         │     └──────────────────┘
                        └──────────────────┘     └────────────────┘     └──────────────────┘
                                                                                                                                                     
                                                                       
```

---

## 🧪 Methodology

### 1. Dataset
A manually curated, bilingual corpus built from real news sources.

| Tag Type | Bengali | English | Total |
|---|---:|---:|---:|
| Specific Disaster (18 categories) | 1,800 | 1,800 | **3,600** |
| Disaster (general) | 100 | 100 | 200 |
| Non-Disaster | 100 | 100 | 200 |
| **Total** | **2,000** | **2,000** | **4,000** |

Split: **80% train / 20% test**

### 2. Preprocessing
A custom web crawler fetches articles by disaster-related keywords, then:
- strips HTML, ads, and navigation noise
- tokenizes into sentences (punctuation preserved for syntactic integrity)
- deduplicates, lowercases, and lemmatizes

### 3. Contextual Embeddings — `bert-base-multilingual-cased`
Each sentence is wrapped with `[CLS]…[SEP]`, tokenized via WordPiece, and passed through 12 self-attention layers. The final sentence vector is the **mean of all token embeddings**, capturing holistic context across both languages.

### 4. Clustering & Retrieval — FAISS
Each `(embedding, tag)` pair is stored as a document in a FAISS index built via `FAISS.from_documents()`. Queries are embedded, then matched via Approximate Nearest Neighbor search:

$$\text{Similarity}\big(E(Q),\, E(S_i)\big) \geq \theta, \quad \theta = 0.8$$

Only high-confidence matches surface — keeping false positives in check.

---

## 🎯 Disaster Categories

<table>
<tr>
<td>

**Geological**
- Earthquake
- Volcanic Activity
- Landslide
- Avalanche

</td>
<td>

**Hydrological**
- Riverine Flood
- Coastal Flood
- Tsunami
- Storm Surge

</td>
<td>

**Meteorological**
- Hurricane
- Tornado
- Hail
- Lightning
- Cold Wave
- Heat Wave

</td>
<td>

**Climatological**
- Drought
- Wildfire
- Ice Storm
- Winter Weather

</td>
</tr>
</table>

> Plus generalized tags: **Disaster** (ambiguous / multi-hazard) and **Non-Disaster** (metaphor, opinion, or unrelated).

---

## 📊 Results

Evaluated using **Precision, Recall, F1-Score, and Accuracy** across all 20 categories on a held-out 20% test set.

| Category | Precision | Recall | F1-Score | Accuracy |
|---|:---:|:---:|:---:|:---:|
| Specific Disaster | — | — | — | — |
| Disaster (general) | — | — | — | — |
| Non-Disaster | — | — | — | — |
| **Overall** | — | — | — | — |

> *Final metrics will be updated upon publication.*


---

## 📁 Project Structure

```
bilingual-disaster-detection/
├── data/
│   ├── dataset.csv              # Annotated bilingual corpus
│   └── raw/                     # Crawled news articles
├── docs/
│   ├── research_proposal        # Proposed theory
├── models/
│   ├── index.faiss
│   └── index.pkl
├── results/
│   └── metrics.json
├── requirements.txt
└── README.md
```

---

## 🔭 Future Work

- 🏷️ **NER + POS tagging** for richer entity-aware classification
- 🔗 **Multi-sentence segment embeddings** to capture cross-sentence disaster cues
- 💬 **Sentiment analysis** for urgency-based prioritization
- 🌍 **Code-mixed text support** (Banglish) for real-world social media
- 🛰️ **Multimodal fusion** with satellite imagery and IoT sensor streams
- 🧪 **Semi-supervised / active learning** to scale annotation efficiently

---

## 👥 Authors

| Name | Contact |
|---|---|
| **Amina K. Mim** | mim.aminakhatun@gmail.com |
| **Jubiar A. Rabbi** | mdjubiarahmedrabbi@gmail.com |
| **Ankur D. Ananto** | ankurdasananto95@gmail.com |

---

## 📜 License

Released under the **MIT License** — see [`LICENSE`](LICENSE) for details.

---

<div align="center">

**⭐ If this work helps your research, please consider starring the repo!**

*Built with 💙 for resilient communities.*

</div>
