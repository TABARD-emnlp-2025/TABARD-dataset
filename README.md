# TABARD: A Benchmark for Tabular Anomaly Detection and Reasoning

TABARD is a large-scale benchmark for evaluating the ability of large language models (LLMs) to detect, reason over, and explain fine-grained anomalies in tabular data. This repository contains the full release of the TABARD dataset as used in our paper:

**“TABARD: A Novel Benchmark for Tabular Anomaly Analysis, Reasoning and Detection”**  
(ACL 2025 Submission)

---

## 🧩 Overview

**TABARD** is constructed by injecting synthetic anomalies into real-world tables sourced from:
- **WikiTQ** (Pasupat & Liang, 2015)
- **FeTaQA** (Nan et al., 2021)
- **Spider + BEAVER** (Yu, 2018; Chen et al., 2024)

It covers **8 distinct anomaly types**:
- **Value**
- **Factual**
- **Logical**
- **Temporal**
- **Calculation**
- **Security**
- **Normalization**
- **Consistency**

Each table is accompanied by:
- A perturbed version with cell-level anomalies
- A `Yes/No` table (binary labels per cell)
- A stripped-down version for focused evaluation

---

## 📁 Directory Structure

**TABARD**/
├── FetaQA/
│ ├── Pertubuted/
│ ├── Stripped/
│ └── YesNo/
├── Spider_BEAVER/
│ ├── Pertubuted/
│ ├── Stripped/
│ └── YesNo/
└── WikiTQ/
├── Pertubuted/
├── Stripped/
└── YesNo/

Each anomaly type (e.g., `Calculation_Based_Anomaly_FeTaQA`) is stored as a subdirectory with `.json` files representing individual tables and their annotations.

---

## 📊 Dataset Statistics

| Dataset         | #Tables | Avg. Rows | Avg. Cols |
|----------------|---------|-----------|-----------|
| WikiTQ + FeTaQA| 4,840   | 17        | 5         |
| Spider + BEAVER| 455     | 349       | 7         |
| **Total**       | 5,295   | 33        | 5         |

Anomalies are injected in ~50% of rows per table, and a small portion of tables are left unperturbed to assess false positives.

---

## 🔬 Anomaly Examples

- `@@@_2000-01-01` → a temporal anomaly (e.g., unrealistic shipping date)
- `@@@_Atlantis` → a factual anomaly (non-existent country)
- `@@@_-$500` → a value anomaly (negative salary)

Each anomaly is labeled and traceable with metadata.

---

## 🧠 Prompting Methods Evaluated

We benchmark a range of LLMs under multiple prompting strategies:
- Zero-shot and few-shot prompts (L1–L4)
- Chain-of-Thought (CoT) reasoning
- Multi-reasoning and self-verification: **MUSEVE**, **SEVCOT**
- Constraint-based reasoning: **NSCM**

---

## 📥 Download

This repository contains:
- `.json` files for all benchmark tables and variants
- Metadata and logs for anomaly source, type, and explanation

---

## 📜 Citation

If you use TABARD in your research, please cite:

```bibtex
@inproceedings{tabard2025,
  title={TABARD: A Novel Benchmark for Tabular Anomaly Analysis, Reasoning and Detection},
  author={Anonymous},
  booktitle={Proceedings of the Annual Meeting of the Association for Computational Linguistics (ACL)},
  year={2025}
}```

⚖️ License & Usage
TABARD is released for academic research and benchmarking only. It is built entirely from public, anonymized datasets and contains no real-world sensitive information. See the full license in LICENSE.md.

🙏 Acknowledgments
This dataset builds on the efforts of prior open-source datasets including WikiTQ, FeTaQA, Spider, and BEAVER. We thank the annotators and the open-source community for enabling this release.
