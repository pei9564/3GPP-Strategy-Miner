# 3GPP Strategy Miner

A data mining pipeline that scrapes and analyzes 3GPP meeting records to study corporate standardization strategies — built for academic research at NCKU.

## Pipeline Overview

```
3GPP Portal / Excel Work Plan
        │
        ▼
┌─────────────────────────────┐
│  Work Item Analysis (01–05) │
│  • Batch download 2,813 ZIPs│
│  • Parse 812 DOCX files     │
│  • Build WIR–member dyads   │
└────────────┬────────────────┘
             │
             ▼
┌─────────────────────────────┐
│  Specification Analysis     │
│  • Selenium scraping        │
│  • Spec relationship graph  │
│  • SPR change history       │
└────────────┬────────────────┘
             │
             ▼
     Excel / CSV outputs
     + Visualization charts
```

## Tech Stack

| Category | Tools |
|---|---|
| Web Scraping | `selenium`, `requests`, `openpyxl` (hyperlink extraction) |
| File Processing | `zipfile` (in-memory), `docx2txt`, `re` |
| Data Analysis | `pandas`, `matplotlib`, `seaborn`, `plotly` |

## Scale

- **2,813** ZIP archives downloaded from Excel hyperlinks
- **812** DOCX meeting documents parsed
- **45,753** supporting member records generated
- **4,137** unique specifications scraped
- **828** company-to-company collaboration relationships extracted

## Key Technical Highlights

**In-memory ZIP extraction** — downloads and decompresses without touching disk, using `zipfile.ZipFile(io.BytesIO(response.content))` to avoid I/O bottleneck when processing thousands of files.

**Selenium scraping on dynamic content** — handles button clicks, wait states, and XPath-based table extraction from 3GPP's JavaScript-rendered portal pages.

**Regex-based company name normalization** — fuzzy-matches 30+ major telecom companies across inconsistent naming conventions (abbreviations, capitalization, typos) before any data joins.

**Relationship graph construction** — maps parent/child spec hierarchies into company-level collaboration edges, producing a knowledge graph of 828 relationships from 4,251 raw spec links.

---

## Notebooks

### Work Item Analysis

| Notebook | Description |
|---|---|
| [`01_download_extract_and_scrape`](work-item-analysis/01_download_extract_and_scrape.ipynb) | Batch-downloads ZIP files from Excel hyperlinks, extracts in-memory, parses DOCX meeting records via regex |
| [`02_clean_work_item_table`](work-item-analysis/02_clean_work_item_table.ipynb) | Enriches WI table: infers company from WIR name/email, adds download status and TSG classification |
| [`03_merge_wi_sup_and_company_overview`](work-item-analysis/03_merge_wi_sup_and_company_overview.ipynb) | Normalizes company names, merges WI and supporting member tables, builds company participation overview |
| [`04_build_wir_member_dyads`](work-item-analysis/04_build_wir_member_dyads.ipynb) | Generates dyad data (WIR ↔ supporting member pairs) per release, formatted for Ucinet |
| [`05_compute_reputation_and_status`](work-item-analysis/05_compute_reputation_and_status.ipynb) | Constructs dependent/independent/control variable tables for network analysis |

![WI Table](https://github.com/pei9564/NCKU-lab-3GPP-analysis/assets/103319735/33c09f6a-254a-4a69-8f60-552d46319f64)
![Scraped Content 1](https://github.com/pei9564/NCKU-lab-3GPP-analysis/assets/103319735/858268c5-a85b-4e15-aa3a-7db55440bf2c)
![Scraped Content 2](https://github.com/pei9564/NCKU-lab-3GPP-analysis/assets/103319735/f88e45ea-46e2-4dfa-891c-e058b8839845)

### Specification Analysis

| Notebook | Description |
|---|---|
| [`spec_analysis`](specification-analysis/spec_analysis.ipynb) | Scrapes SPR company data via Selenium, normalizes names, maps specs to countries, generates trend visualizations across 2G–5G technology eras |
| [`spec_relationship`](specification-analysis/spec_relationship.ipynb) | Crawls parent/child spec links using XPath, transforms into company-level relationship graph |
| [`spec_history`](specification-analysis/spec_history.ipynb) | Scrapes SPR change history to track ownership shifts over time |

![Spec Relationship Example](https://github.com/pei9564/NCKU-lab-3GPP-analysis/assets/103319735/d9e7f4bc-b1ab-47fd-9301-c2da85cd6e74)
![Spec History Example](https://github.com/pei9564/NCKU-lab-3GPP-analysis/assets/103319735/bb1b3040-9b3b-4a39-8fce-c21edd47fae8)
