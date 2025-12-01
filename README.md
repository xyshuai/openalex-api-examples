# 📚 OpenAlex API Demo Repository

**Python Examples for Basic Paging and Cursor Paging**

*(Created for demo and use by someone who is learning API data retrieval and scientometric workflows.)*

---

## 🌟 Overview

This repository provides Python examples showing how to retrieve scholarly metadata from the OpenAlex API using two approaches:

- **Basic Paging (≤10k records)** – simple API pagination with `page=1,2,3...`
- **Cursor Paging (Full Harvest)** – scalable harvesting using `cursor=*` to retrieve hundreds of thousands of records


---

## ⚠️ Disclaimer

This repository is **not** an official OpenAlex resource. For authoritative documentation, visit the official repo:

👉 **https://github.com/ourresearch/openalex-api-tutorials**

The examples here serve demonstration purposes.

---

## 🗂 Repository Structure

```
openalex-api-tutorial/
│
├── basic_paging_demo.py      # Simple pagination example (≤10k)
├── cursor_paging_demo.py     # Full harvesting with cursor pagination. Please Install local IDEs (Cursor/VS Code/Pycharm,etc) to run the script.
└── README.md                 # Documentation (this file)
```

---

## 🚀 1. Quick Start

### 🔧 Requirements

- Python ≥ 3.8
- Packages: `requests`, `csv` (built-in), `time`, `os`

Install (if needed):
```bash
pip install requests
```

### ▶️ Running the Scripts

**Basic Paging (small demo_≤10k)**
```bash
python basic_paging_demo.py
```

**Cursor Paging (larger demo_≥10k)**
```bash
python cursor_paging_demo.py
```

CSV files will be automatically saved to: `D:\OpenAlex\Data\`

---

## 🔍 2. How the OpenAlex API Works

OpenAlex provides a very flexible `filter=` system. In this demo, we intentionally choose the following filter as an example:

```
open_access.is_oa:true
authorships.countries:countries/my
publication_year:2023-2024
type:types/article|types/review
```

**Meaning:**

| Condition | Explanation |
|-----------|-------------|
| `open_access.is_oa:true` | Only Open Access papers |
| `authorships.countries:countries/my` | At least one author affiliated with a Malaysian institution |
| `publication_year:2023-2024` | Limit to 2 years |
| `type:article OR review` | Research Article and Review Paper only |

📘 **Note:** These filters are only for demonstration, not a recommendation for scientometric analysis.

**Official filter reference:** https://docs.openalex.org/api-entities/works/filter-works

---

## 📄 3. Basic Paging Demo (≤10k Records)

This script is ideal for:
- teaching API basics
- small experiments
- Test by retrieving the first 10–100 records

### 🔎 Key characteristics

✔ Uses `page=1,2,3…`  
✔ Supports up to 10,000 records (OpenAlex hard limit)  
✔ Good for beginners

### 📌 Snippet

```python
PARAMS = {
    "filter": ",".join([
        "open_access.is_oa:true",
        "authorships.countries:countries/my",
        "publication_year:2023-2024",
        "type:types/article|types/review",
    ]),
    "sort": "cited_by_count:desc",
    "per_page": 10,
    "page": 1,
    "mailto": "your.name@domain.com",
}
```

To retrieve all available pages, simply loop `page=1..N`.

---

## 🌐 4. Cursor Paging (≥10k Records)

OpenAlex allows you to access as many records as you like using Cursor Paging:

```
cursor=*
```

**Cursor pagination returns:**
- extremely large datasets
- no upper limit (hundreds of thousands of rows)
- stable ordering
- safe for large-scale scientometric research

### 🧩 Why cursor pagination is necessary?

Because `page=` is limited to 10k, but `cursor` has **no limit**.

However, note that **OpenAlex's free API tier imposes a daily limit of 100,000 requests per user per day, with a maximum of 10 requests per second**. While cursor paging allows you to harvest an unlimited number of records sequentially, this **rate limit still applies to the total number of API calls**, even when using cursors.

### 🛠 Features in the full-harvest script:

- Fetches one year at a time
- Saves each year to its own CSV
- Automatically handles:
  - OA fields
  - Topic taxonomy
  - Primary topic
  - Top 5 topics
  - APC lists
  - FWCI
  - Citation percentile
  - Author affiliations
  - Corresponding authors
  - ISSN / journal venue inference
  - Institution country codes

---

## 📊 5. Output Fields Included

The CSV includes:

| Category | Fields |
|----------|--------|
| **Core metadata** | id, doi, title, publication_year, type, language |
| **Citation metrics** | cited_by_count, fwci, citation_percentile, top1pct, top10pct |
| **OA info** | is_oa, oa_status, oa_url, license, version |
| **Venue** | journal, issn_l |
| **Authors** | first_author, first_author_country_codes, authors, authors_affiliations |
| **Author-level features** | corresponding_authors, corresponding_author_country_codes |
| **Topic modeling** | primary_topic_*, topics_top5 |
| **APC** | apc_list_values |
| **SDG** | sdg_labels |

---


## 🌲 6. Example Output (Preview)

Below is a simplified preview of selected fields from the harvested dataset:

| openalex_id | doi | title | year | type | lang | cited_by_count | journal | issn_l | oa_status | oa_url | license | version | first_author | primary_topic_name | primary_topic_domain | primary_topic_field | primary_topic_subfield | corresponding_authors | apc_list_values | fwci | citation_percentile |
|-------------|-----|-------|------|------|------|----------------|----------|---------|-----------|--------|---------|----------|--------------|---------------------|------------------------|-----------------------|--------------------------|------------------------|------------------|-------|----------------------|
| https://openalex.org/W4383273076 | https://doi.org/10.3390/healthcare11131946 | Antimicrobial Resistance: A Growing Serious Threat for Global Public Health | 2023 | review | en | 1218 | Healthcare | 2227-9032 | gold | https://mdpi.com/... | cc-by | publishedVersion | Md. Abdus Salam | Antibiotic Use and Resistance | Life Sciences | Immunology and Microbiology | Applied Microbiology and Biotechnology | Md. Abdus Salam | 1800 CHF | 412.99 | 1 |
| https://openalex.org/W4388673199 | https://doi.org/10.1016/s0140-6736(23)01859-7 | The 2023 report of the Lancet Countdown on health and climate change | 2023 | review | en | 829 | The Lancet | 0140-6736 | green | https://ncbi.nlm.nih.gov/... | cc-by | submittedVersion | Marina Romanello | Climate Change and Health Impacts | Physical Sciences | Environmental Science | Health, Toxicology and Mutagenesis | Marina Romanello | 6830 USD | 193.46 | 1 |
| https://openalex.org/W4322617812 | https://doi.org/10.1016/j.ijme.2023.100790 | Generative AI and the future of education | 2023 | article | en | 814 | IJME | 1472-8117 | hybrid | https://doi.org/... | cc-by | publishedVersion | Weng Marc Lim | Ethics and Social Impacts of AI | Social Sciences | Social Sciences | Safety Research | Weng Marc Lim | 3560 USD | 330.23 | 0.99995 |

---

## 🧠 7. Common Issues & Notes

### ❗ 1. 403 Forbidden

**Causes:**
- malformed filter
- illegal characters (`%2F`)
- too many requests

**Solution:**
- never embed encoded slashes in filter
- use email with `mailto=`
- add delay (1.0–1.5s)

### ❗ 2. JSONDecodeError

**Causes:**
- OpenAlex sends incomplete response under heavy load

**Solution:**
- catch exception, retry
- add sleep
- use cursor instead of page

### ❗ 3. Wrong publication year

Even with filters, OpenAlex may return adjacent years due to indexing delays.

**Solution:**
```python
if w["publication_year"] != YEAR:
    continue
```

---

## 📝 8. How to Cite OpenAlex (Recommended)

```
Priem, J., Piwowar, H., & Orr, R. (2022). OpenAlex: A fully-open index of scholarly works, authors, venues, institutions, and concepts. ArXiv. https://arxiv.org/abs/2205.01833
```

---

## 🏆 9. Best Practices

- Always test with `per_page=5` before running full harvest
- Save each year separately
- Keep raw CSV and create a cleaned CSV
- Never modify the cursor manually
- Avoid hammering the API → use proper delays
- Always include `mailto=`

---

## 💡 10. Useful Tools After Harvesting

| Task | Recommended Tools |
|------|-------------------|
| **Topic Modeling** | BERTopic |
| **Network Mapping** | VOSviewer, Gephi |
| **Bibliometric Analysis** | Bibliometrix (R) |
| **Data cleaning** | pandas |
| **Visualization** | Plotly, SCImago Graphica |
| **Citation Analysis** | CiteInsight |

---

## 🙌 11. Acknowledgments

This repo is inspired by the official OpenAlex tutorials:

👉 **https://github.com/ourresearch/openalex-api-tutorials**

Thanks to the OpenAlex team for providing a fully open scholarly index.

---

## 🎓 12. About This Repository

 - This repository was created for demo purposes.
 - Researchers may freely reuse and adapt the scripts.
 - However, please note that these are not official OpenAlex scripts.

---

**Happy Harvesting! 🚀**
