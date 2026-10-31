# A Rare-Earth Uncertainty (REU) Index

The Rare-Earth Uncertainty (REU) index measures the intensity of reported disruptions to the global supply of rare-earth elements through comprehensive media analysis. Building on the search-based methodology of Baker, Bloom, and Davis (2016), the index tracks how frequently the news jointly discusses rare-earth elements and supply shortages, providing a high-frequency signal of stress in rare-earth markets.

**Reference**: Morão (2025)  
Hugo Morão,  
Do Rare-Earth Supply Constraints Slow the Green Transition?,  
working paper, 2025.  

## Economic Intuition Behind the Index
Rare-earth elements—neodymium, dysprosium, praseodymium, terbium, and the other lanthanides—are critical inputs to magnets, electric motors, wind turbines, and a wide range of clean-energy and defense technologies. Their supply is highly concentrated and exposed to export controls and processing bottlenecks. Shortages, scarcity, and logistical disruptions in this segment raise input costs, threaten technology supply chains, and propagate into broader economic uncertainty. By quantifying media coverage of rare-earth supply disruptions, the REU index provides an early, quantitative indicator of stress in these strategically important markets.

## Construction Methodology
The index follows the Baker et al. (2016) methodology for search-based indices, measuring the frequency of news articles that jointly reference two semantic categories:

1. **Rare earth (R)** — the rare-earth industry and its elements, identified through an industry code and terms such as *rare earth*, *lanthanide*, *neodymium*, *dysprosium*, *praseodymium*, *terbium*, *europium*, *yttrium*, *samarium*, *cerium*, *lanthanum*, and *scandium*.

2. **Shortage (S)** — supply-disruption language such as *shortage*, *scarcity*, *backlog*, *disruption*, and *bottleneck* (including the relevant supply/shortage subject codes).

An article is counted only when its full text contains at least one term from each category and it is between 100 and 1,999 words long. Articles that are summaries of news, corporate digests, calendars of events, rankings-based content, routine general and market/financial news, news agency materials, letters, headline-only content, personal announcements, contract tenders, obituaries, and TV/Radio listings are excluded to ensure that only relevant articles are selected. The exact boolean query used to retrieve articles is stored in `config/config.json`.

## Coverage and Data Quality
The index is monthly and begins in January 2008. It draws on 25 English-language media sources across major economies, including the United States, United Kingdom, Canada, Australia, and Asia-Pacific markets. Few articles discussed rare-earth shortages before 2010, so some early months read zero. The full list of sources, with each source's coverage window and importance weight, is provided in `config/sources.csv`.

## Repository Structure
```
reu_world/
├── config/
│   ├── config.json      # search definition: boolean query, languages, interval
│   └── sources.csv      # media sources with coverage windows and importance weights
├── data/
│   ├── raw/
│   │   └── num_hits.csv # raw monthly article counts per source (index numerator and denominator)
│   └── processed/
│       ├── index.csv    # computed REU index (two series)
│       └── index.xlsx   # computed REU index (spreadsheet)
├── CITATION.cff         # machine-readable citation metadata
├── CHANGELOG.md         # record of notable changes
├── LICENSE              # license terms (CC BY 4.0)
└── README.md
```

## Reproducing the Index
The index is built in three stages, moving left to right through the folders above:

1. **Define the search** — `config/config.json` holds the exact boolean query (rare-earth and shortage terms, the word-count filter, and excluded subject codes) together with the target language and reporting interval. `config/sources.csv` lists the media sources to query and the coverage window over which each is available.
2. **Collect raw counts** — running the query in `config.json` against each source in `sources.csv` (via the news database, e.g. Factiva) yields `data/raw/num_hits.csv`: for every source and month, the number of matching articles (`num_hits`) and the total number of articles published (`total_articles`).
3. **Compute the index** — the raw counts are scaled by each source's total article volume, aggregated across sources (optionally weighted by `importance`), and standardized to produce `data/processed/index.csv`. The result is provided in both CSV and XLSX form and covers 2008-01 through the latest available month.

### Data Dictionary
**`config/config.json`**
| Field | Description |
| --- | --- |
| `type` | Index type (`index`). |
| `languages` | Languages of the retrieved articles. |
| `interval` | Reporting frequency of the index (`Monthly`). |
| `search_text` | The full boolean query: a rare-earth industry code / rare-earth terms combined with supply-shortage terms, the word-count filter (`wc>99 AND wc<2000`), and excluded subject codes (`NOT ns=...`). |

**`config/sources.csv`**
| Column | Description |
| --- | --- |
| `name` | Source (publication) name. |
| `fry`, `frm`, `frd` | Coverage start — from year, month, day. |
| `toy`, `tom`, `tod` | Coverage end — to year, month, day. |
| `importance` | Relative weight of the source used in the weighted index series. |

**`data/raw/num_hits.csv`**
| Column | Description |
| --- | --- |
| `source` | Source name (matches `sources.csv`). |
| `year`, `month`, `day` | Period of the count (`day` empty for monthly aggregates). |
| `num_hits` | Number of articles matching the REU query in that source-period. |
| `total_articles` | Total number of articles published by the source in that period (scaling denominator). |
| `error` | Retrieval error message, if any (empty on success). |
| `num_tries` | Number of retrieval attempts made. |

**`data/processed/index.csv`** (and `index.xlsx`)
| Column | Description |
| --- | --- |
| `year`, `month` | Period of the observation. |
| `standardized` | REU index: each source's scaled count is divided by its standard deviation, averaged across sources, and rescaled to a mean of 100. |
| `standardized weighted` | The same, with each source weighted by its share of total articles. |

## How to Cite
If you use this index or dataset, please cite the companion paper:

> Hugo Morão, "Do Rare-Earth Supply Constraints Slow the Green Transition?," working paper, 2025.

A machine-readable citation is available in `CITATION.cff`. This entry will be
updated with the journal, volume, and DOI once the paper is published.

## License
The data and documentation in this repository are released under the
[Creative Commons Attribution 4.0 International License (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).
You are free to share and adapt the material, including for commercial purposes,
provided you give appropriate credit by citing the paper above.
