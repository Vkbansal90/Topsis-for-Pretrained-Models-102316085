# PyPI Text Summarization — TOPSIS Model Selection

This project benchmarks **six pre-trained text summarization models** available on PyPI using the **TOPSIS (Technique for Order of Preference by Similarity to Ideal Solution)** multi-criteria decision-making framework. 

The goal is to determine the optimal model by balancing **summary quality** (ROUGE, BERTScore) against **computational efficiency** (Inference Time, GPU Memory).

---

## 📋 Table of Contents
- [Overview](#-overview)
- [Methodology (TOPSIS)](#-methodology-topsis)
- [Models Evaluated](#-models-evaluated)
- [Evaluation Metrics](#-evaluation-metrics)
- [Weight Scenarios](#-weight-scenarios)
- [Results & Key Findings](#-results--key-findings)
- [Environment Setup](#-environment-setup)
- [License](#-license)

---

## 📌 Overview
Selecting a text summarization model often involves a trade-off. While large transformer models (like BART or Pegasus) provide high-quality abstractive summaries, they require significant GPU memory and time. Simple extractive models (like LexRank) are extremely fast but may lack semantic depth. 

This project uses **TOPSIS** to mathematically rank these models across three specific use-case scenarios: **Balanced**, **Quality-Priority**, and **Speed-Priority**.

---

## 🔢 Methodology (TOPSIS)
TOPSIS ranks alternatives based on their geometric distance from the **Ideal Best** and **Ideal Worst** solutions.

### The TOPSIS Formula
For each model, the performance score is calculated as:

$$Score = \frac{S^-}{S^+ + S^-}$$

Where:
* $S^+$ is the Euclidean distance to the Ideal Best solution.
* $S^-$ is the Euclidean distance to the Ideal Worst solution.
* A score closer to **1.0** indicates the best-performing model.

---

## 📦 Models Evaluated
All models were implemented using pre-trained weights from PyPI-hosted libraries (`transformers` and `sumy`).

| Model | Library | Type |
| :--- | :--- | :--- |
| **BART** | Transformers | Abstractive |
| **T5** | Transformers | Abstractive |
| **Pegasus** | Transformers | Abstractive |
| **GPT-2** | Transformers | Weak Abstractive |
| **Sumy LexRank** | Sumy | Extractive |
| **Sumy TextRank** | Sumy | Extractive |

---

## ⚙️ Evaluation Metrics
The models were evaluated on the following six criteria:

1.  **ROUGE-1 / ROUGE-2 / ROUGE-L**: Measures n-gram overlap (lexical quality). ↑
2.  **BERTScore**: Measures semantic similarity using contextual embeddings. ↑
3.  **Inference Time (s)**: The time taken to generate the summary. ↓
4.  **Peak Memory (MB)**: Maximum GPU memory consumed during inference. ↓

*(↑ = Maximize, ↓ = Minimize)*

---

## ⚖️ Weight Scenarios
The ranking was tested under three priority configurations:

| Scenario | ROUGE (1, 2, L) | BERTScore | Inference Time | Peak Memory |
| :--- | :--- | :--- | :--- | :--- |
| **Balanced** | 16.6% each | 16.6% | 16.6% | 16.6% |
| **Quality** | 20% each | 20% | 10% | 10% |
| **Speed** | 10% each | 10% | 30% | 30% |

---

## 📊 Results & Key Findings

Based on the TOPSIS analysis, the models were ranked as follows (Balanced Scenario):
<img width="1864" height="589" alt="image" src="https://github.com/user-attachments/assets/48cb9dd2-0db6-416a-a889-60bb05dd1bc5" />


| Rank | Model | TOPSIS Score | Best Category |
| :--- | :--- | :--- | :--- |
| **1** | **Sumy-LexRank** | **1.0000** | Balanced / Speed |
| **2** | **Sumy-TextRank** | **1.0000** | Balanced / Speed |
| **3** | **T5** | **0.9620** | Quality / Speed |
| **4** | **GPT-2** | **0.1874** | Speed-Priority |
| **5** | **BART** | **0.1633** | Quality-Priority |
| **6** | **Pegasus** | **0.0000** | Balanced |

### 🔍 Analysis of Findings
* **Winner (Efficiency):** **Sumy-LexRank** and **Sumy-TextRank** tied for the top spot. Their extremely low inference time (~1s) and low memory footprint made them mathematically superior in the Balanced and Speed-Priority scenarios.
* **Winner (Transformer-based):** **T5** significantly outperformed other transformers like BART and Pegasus. It offered a high quality-to-speed ratio, ranking 3rd overall.
* **Performance Bottlenecks:** **BART** and **Pegasus** were penalized heavily by the TOPSIS algorithm due to their high inference latency (up to 6.2s) and high peak memory usage, despite their known strengths in abstractive quality.
* **Baseline:** **GPT-2** produced lower quality scores, as it is not natively fine-tuned for summarization, leading to a low overall rank.
* graph
* <img width="1493" height="765" alt="image" src="https://github.com/user-attachments/assets/f989bf7b-98aa-450d-b35a-13c079f439d3" />

---

