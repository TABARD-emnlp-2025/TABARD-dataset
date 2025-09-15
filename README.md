
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

## 📁 Base Directory Structure

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
````

### 🔹 Root-Level Categories

* `FetaQA/`, `Spider_BEAVER/`, and `WikiTQ/` represent the three base datasets from which TABARD anomalies are derived.

### 🔹 Inside Each Dataset Folder

Each dataset folder contains three subdirectories corresponding to different **anomaly representations**:

* **`Pertubuted/`**
  Full tables where synthetic anomalies have been injected into one or more cells.
  Example: `Pertubuted/Calculation_Based_Anomaly_FeTaQA/`

* **`Stripped/`**
  Stripped-down versions of the same tables with the `@@@_` anomaly markers removed. This highlights table structure while preserving underlying anomalies (without visual tags).

* **`YesNo/`**
  Binary-labeled tables where each cell is marked `"Yes"` (if anomalous) or `"No"` (if clean). These files are schema-aligned with the perturbed version and serve as ground truth.

### 🔹 File Format

Each subfolder (e.g., `Calculation_Based_Anomaly_FeTaQA`) includes:

* `.json` files representing individual tables.

---

## 🆕 New variation Release: `Dataset_with_all_exp_variations.zip`

This release adds an **experimental bundle** that unifies labels, merged tables, binary conversions, and multiple controlled **variations** per dataset, plus schema-level datatype metadata.

### High-Level Layout

```bash
Dataset_with_all_exp_variations.zip/
├── FeTaQA/
├── Spider_Beaver/
├── WikiTQ/
├── FeTaQA-merged/
│   ├── LABELS/
│   ├── MERGED/
│   ├── MERGED-STR/
│   ├── MERGED-YES-NO/
│   └── VARIATION_<int>/
├── Spider_Beaver-merged/
│   ├── LABELS/
│   ├── MERGED/
│   ├── MERGED-STR/
│   ├── MERGED-YES-NO/
│   └── VARIATION_<int>/
├── WikiTQ-merged/
│   ├── LABELS/
│   ├── MERGED/
│   ├── MERGED-STR/
│   ├── MERGED-YES-NO/
│   └── VARIATION_<int>/
└── data_types_and_schema_exp/
    ├── FeTaQA/
    ├── Spider_Beaver/
    └── WikiTQ/
```

> `<Dataset_name>` ∈ `["FeTaQA", "Spider_Beaver", "WikiTQ"]` with a corresponding `<Dataset_name>-merged` bundle.

### 📦 Folder Details (within each `<Dataset_name>-merged/`)

1. **`LABELS/`**

   * JSON files with labels for each anomaly category in each table.
   * **Naming pattern:** `<table_file_name>_labels.json`

     * Example: `137_updated.json` → `137_updated_labels.json`

2. **`MERGED/`**

   * JSON files with **all anomaly categories merged** into **one file per table**.
   * Each file contains:

     * Distinct rows
     * Cleaned values
     * Anomalies marked with a **prefix**: `@@@_`

3. **`MERGED-STR/`**

   * Same structure/content as `MERGED/` but with the anomaly prefix `@@@_` **stripped** from values.
   * Enables consumption by systems that should not see explicit anomaly markers.

4. **`MERGED-YES-NO/`**

   * Converts the anomaly marker to a **binary** format:

     * `"Yes"` where the `@@@_` prefix was present (anomaly detected)
     * Original clean value otherwise

5. **`VARIATION_<int>/`**
   Multiple sampling strategies that generate controlled perturbation sets per table. Each variation folder contains:

   * `LABELS/` (variation-specific labels; e.g., `0_updated_var1_labels.json`)
   * `MERGED/`, `MERGED-STR/`, `MERGED-YES-NO/` (variation-specific tables)

   **Variation schemes:**

   * **VARIATION\_1**
     *LCM-style uniform sampling* of perturbed cells across anomaly categories, ensuring **diverse but bounded** perturbation selections per file.
   * **VARIATION\_2**
     *Structure-aware sampling* that **enforces row uniqueness** and **column-frequency limits (K-cap)** to balance structural diversity and constraint.
   * **VARIATION\_3**
     *Performance-stratified sampling* using **weighted** selection based on anomaly-category accuracy (**UNDER/MID/OVER**), emphasizing weaker-performing categories to drive improvement.

### 🧾 Schema & Datatypes (`data_types_and_schema_exp/`)

* Located at: `data_types_and_schema_exp/<Dataset_name>/datatypes.json`
* Aids **datatype inconsistency** detection and **schema-level anomaly** reasoning.

**File format:**

```json
{
  "table_title": "<string>",
  "columns": [
    {
      "column_name": "<string>",
      "data_type": "<string>",
      "description": "<string>"
    }
  ],
  "first 5 rows": [
    { "<column_name_1>": <value>, "<column_name_2>": <value> },
    { "<column_name_1>": <value>, "<column_name_2>": <value> }
  ]
}
```

---

## 📊 Dataset Statistics

| Dataset         | #Tables | Avg. Rows | Avg. Columns |
| --------------- | ------- | --------- | ------------ |
| WikiTQ + FeTaQA | 4,840   | 17        | 5            |
| Spider + BEAVER | 455     | 349       | 7            |
| **Total**       | 5,295   | 33        | 5            |

---

## 🔬 Anomaly Examples

* `@@@_2000-01-01` → **Temporal anomaly** (e.g., invalid shipping date)
* `@@@_Atlantis` → **Factual anomaly** (non-existent entity)
* `@@@_-$500` → **Value anomaly** (negative salary)

---

## 🧠 Prompting Methods Evaluated

* Zero-shot & Few-shot prompts (Levels **L1–L4**)
* **Chain-of-Thought (CoT)** reasoning
* Multi-reasoning self-verification: **MUSEVE**, **SEVCOT**
* Constraint-based method: **NSCM**

---

## 📥 Download

The dataset release includes:

* `.json` files for all **perturbed**, **stripped**, and **binary-labeled** tables
* **Extended experimental bundle:** `Dataset_with_all_exp_variations.zip`
* Metadata for anomaly **type**, **explanation**, **schema (datatypes.json)**, and **injection log**

---

## 📚 Citation

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

## 📜 License & Usage

TABARD is released under the **MIT License** for academic and research use only.
All data is anonymized and derived from public datasets.
See [`LICENSE.md`](./LICENSE.md) for full terms.

---

## 🙏 Acknowledgments

TABARD builds on the foundation of:

* **WikiTQ** (Pasupat & Liang, 2015)
* **FeTaQA** (Nan et al., 2021)
* **Spider** (Yu, 2018)
* **BEAVER** (Chen et al., 2024)

We thank the annotators and the open-source community for their contributions to data integrity and reproducibility.

