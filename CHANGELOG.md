# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.1.0] - 2026-10-26

### Added
- `data/processed/index.xlsx` with the computed index in spreadsheet form.

### Changed
- Extended the index from 2026-06 to 2026-07: updated `data/raw/num_hits.csv`,
  `data/processed/index.csv`, `data/processed/index.xlsx`, and the coverage
  end dates in `config/sources.csv`. The search query is unchanged.

## [1.0.0] - 2026-07-31

### Added
- Initial repository for the Rare-Earth Supply Uncertainty (REU) index.
- `config/config.json` documenting the exact boolean search query, language, and
  reporting interval used to build the index.
- `config/sources.csv` listing the media sources with coverage windows and
  importance weights.
- `data/raw/num_hits.csv` with the raw monthly article counts (matching articles
  and total articles) per source.
- `data/processed/index.csv` with the computed index (four series), covering
  2008-01 through 2026-06.
- `README.md` with the methodology, a reproducible `config/` → `data/raw/` →
  `data/processed/` pipeline, and a per-file data dictionary.
- `CITATION.cff` with machine-readable citation metadata (to be updated with the
  journal and DOI upon publication).
- `LICENSE` — Creative Commons Attribution 4.0 International (CC BY 4.0).
- `.gitattributes` for cross-platform LF normalization.
