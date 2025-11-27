# OpenAlex API Examples for Bibliometric and Scientometric Research

This repository provides two Python examples for collecting scholarly metadata from the [OpenAlex](https://openalex.org/) API:

1. **Basic Paging (≤ 10,000 records)** – simple example using `page` + `per_page`, suitable for small datasets and teaching.
2. **Cursor Paging (Full, Multi-year Harvest)** – robust example using cursor-based pagination to download larger datasets by year.

These scripts are designed for students and researchers in Library and Information Science, bibliometrics, and scientometrics who want to learn how to programmatically collect and preprocess publication data.

---

## 1. Overview of the Two Scripts

### 1.1 Basic Paging – Small Sample (≤ 10k records)

**File (example name):**

- `openalex_basic_paging_colab.py` (or `.ipynb` / your chosen name)

**Characteristics:**

- Uses simple **page-based pagination**: `page` + `per_page`.
- At most **10,000 records per query** (OpenAlex page-based limit).
- Originally configured to retrieve **Top 10 most cited** OA articles by Chinese authors (2005–2014) as a small test.
- Includes rich flattening logic for:
  - Author names and affiliations
  - Concepts and topics (primary topic + top five others)
  - Open Access status and OA URLs
  - APC information
  - Field-weighted citation impact (FWCI)
  - Citation percentile and top 1% / top 10% flags

**Intended usage:**

- Classroom demonstration
- Quick proof-of-concept / pipeline testing
- Exporting a small, interpretable CSV

---

### 1.2 Cursor Paging – Multi-year Full Harvest

**File (example name):**

- `openalex_cursor_paging_full.py`

**Characteristics:**

- Uses **cursor-based pagination** (`cursor=*` + `per_page=200`) which scales beyond 10,000 records.
- Loops over a list of **years** (e.g., 2005–2014) and saves **one CSV per year**:
  - `openalex_2005.csv`, `openalex_2006.csv`, …, `openalex_2014.csv`
- Uses the same enriched set of fields as the basic script, plus:
  - Author affiliations in formatted form
  - Corresponding authors
  - Institution country codes
  - APC list (normalized as a human-readable string)

**Intended usage:**

- Full-scale data collection for a research project
- Multi-year trend analysis and longitudinal scientometric studies
- Building larger topic models or citation analyses

---

## 2. Environment and Requirements

### 2.1 Python Version

- Python **3.8+** is recommended.

### 2.2 Required Libraries

Both scripts use only a small number of standard packages. You can install them with:

```bash
pip install requests

🚀 1. Quick Start
🔧 Requirements
Python ≥ 3.8

Packages: requests, csv (built-in), time, os
