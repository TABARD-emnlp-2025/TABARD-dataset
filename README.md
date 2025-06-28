# TABARD: A Benchmark for Tabular Anomaly Detection and Reasoning

**TABARD** is a large-scale benchmark for evaluating the ability of large language models (LLMs) to detect, reason over, and explain fine-grained anomalies in tabular data. This repository contains the full release of the TABARD dataset as used in our paper:

> **TABARD: A Novel Benchmark for Tabular Anomaly Analysis, Reasoning and Detection**  
> *(Submitted to EMNLP 2025)*

---

## 🧹 Overview

TABARD is constructed by injecting synthetic anomalies into real-world tables sourced from:

- **WikiTQ** (Pasupat & Liang, 2015)  
- **FeTaQA** (Nan et al., 2021)  
- **Spider + BEAVER** (Yu, 2018; Chen et al., 2024)  

It includes **8 anomaly types**:
- Value
- Factual
- Logical
- Temporal
- Calculation
- Security
- Normalization
- Consistency

---

## 📁 Directory Structure

```bash
TABARD/
├── FetaQA/
│   ├── Pertubuted/
│   ├── Stripped/
│   └── YesNo/
├── Spider_BEAVER/
│   ├── Pertubuted/
│   ├── Stripped/
│   └── YesNo/
└── WikiTQ/
    ├── Pertubuted/
    ├── Stripped/
    └── YesNo/
```
### 🔹 Root-Level Categories
- `FetaQA/`, `Spider_BEAVER/`, and `WikiTQ/` represent the three base datasets from which TABARD anomalies are derived.

### 🔹 Inside Each Dataset Folder
Each dataset folder contains three subdirectories corresponding to different **anomaly representations**:

- **`Pertubuted/`**  
  Contains full tables where synthetic anomalies have been injected into one or more cells.  
  Example: `Pertubuted/Calculation_Based_Anomaly_FeTaQA/`

- **`Stripped/`**  
  Contains stripped-down versions of the same tables with the `@@@` anomaly markers removed. This format highlights the core structure of the table while maintaining the underlying anomalies without visual tags.

- **`YesNo/`**  
  Contains binary-labeled tables where each cell is marked "Yes" (if anomalous) or "No" (if clean). These files are schema-aligned with the perturbed version and serve as ground truth.

### 🔹 File Format
Each subfolder (e.g., `Calculation_Based_Anomaly_FeTaQA`) includes:
- `.json` files representing individual tables

---

## 📊 Dataset Statistics

| Dataset          | #Tables | Avg. Rows | Avg. Columns |
|------------------|---------|-----------|--------------|
| WikiTQ + FeTaQA  | 4,840   | 17        | 5            |
| Spider + BEAVER  | 455     | 349       | 7            |
| **Total**        | 5,295   | 33        | 5            |

---

## 🔬 Anomaly Examples

- `@@@_2000-01-01` → **Temporal anomaly** (e.g., invalid shipping date)  
- `@@@_Atlantis` → **Factual anomaly** (non-existent entity)  
- `@@@_-$500` → **Value anomaly** (negative salary)  

---

## 🧠 Prompting Methods Evaluated

We benchmark multiple LLM prompting strategies:

- Zero-shot & Few-shot prompts (Levels **L1–L4**)
- **Chain-of-Thought (CoT)** reasoning
- Multi-reasoning self-verification: **MUSEVE**, **SEVCOT**
- Constraint-based method: **NSCM**

---

## 📥 Download

The dataset release includes:
- `.json` files for all perturbed, stripped, and binary-labeled tables  
- Metadata for anomaly type, explanation, and injection log  

---

## Citation

If you use TABARD in your research, please cite:

```bibtex
@inproceedings{tabard2025,
  title={TABARD: A Novel Benchmark for Tabular Anomaly Analysis, Reasoning and Detection},
  author={Anonymous},
  booktitle={Proceedings of the Conference on Empirical Methods in Natural Language Processing (EMNLP)},
  year={2025}
}
```

---

## License & Usage

TABARD is released under the **MIT License** for academic and research use only.  
All data is anonymized and derived from public datasets.  
See [`LICENSE.md`](./LICENSE.md) for full terms.

---

## 🙏 Acknowledgments

TABARD builds on the foundation of:
- **WikiTQ** (Pasupat & Liang, 2015)  
- **FeTaQA** (Nan et al., 2021)  
- **Spider** (Yu, 2018)  
- **BEAVER** (Chen et al., 2024)  

We thank the annotators and the open-source community for their contributions to data integrity and reproducibility.
