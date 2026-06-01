# EA 4FM Codebook — for LLM-Assisted Qualitative Analysis

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
<!-- [![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.XXXXXXX.svg)](https://doi.org/10.5281/zenodo.XXXXXXX) — paste real Zenodo DOI badge after first release -->

A structured codebook for scoring participant testimony against Längle's **Four Fundamental Motivations (4FM)** framework, designed for use with Large Language Models (LLMs) as a research instrument.

Built by multi-LLM cross-validation against four primary Längle sources (1992, 2002, 2003, 2011). Every code traces to a verbatim quote with citation. Designed to be downloaded, applied, extended, and forked.

## What this codebook is

A JSON artifact that constrains an LLM to score participant testimony across Längle's Four Fundamental Motivations, with required justification (testimony quote + Längle quote) for every code applied.

It is **not** a fine-tuned model. It is **not** an LLM prompt. It is a structured *constraint* — a reference that you provide to an LLM alongside testimony, so the LLM speaks Längle rather than generic prior knowledge.

For the methodology that produced this codebook, see the companion paper:
*Nelson-Zutter, G. (2026, June). "Do You Understand?!" Best Practices using Artificial Intelligence in Research (and Life). WCET4, Denver.* DOI: forthcoming.

For the companion **EA 12FEP Codebook** (12 Fundamental Existential Prerequisites, derived from this 4FM framework): https://github.com/aestra-research/12fep-codebook · DOI [10.5281/zenodo.20481464](https://doi.org/10.5281/zenodo.20481464)

## Repo layout

```
LICENSE                       CC BY 4.0
NOTICE.md                     Längle source quotation clarification
README.md                     this file
CITATION.cff                  machine-readable citation metadata
.zenodo.json                  Zenodo deposit metadata
codebook/
  ├── 4fm-master.json         the canonical MASTER (current)
  └── versions/               full development lineage
      ├── claude/             Claude Sonnet 4.5's v1.0 → v5.0 chain
      ├── chatgpt/            ChatGPT 5 Thinking's v1.0 → v5.0 chain
      └── gemini/             Gemini 2.5 Turbo's v1.0 → v5.0 chain
docs/
  ├── lineage.md              how each version was produced (multi-LLM build story)
  ├── comparison-matrix.md    cross-LLM v5.0 strength comparison
  ├── integration-summary.md  v5.0 integration rationale
  ├── quick-reference.md      distilled codebook reference
  ├── quick-start-guide.md    practitioner-facing how-to
  ├── merger-summary.md       v4.0 single-LLM merge methodology
  ├── codebook-comparison.md  early version-to-version differences
  └── build-dialogs/          original LLM dialog exports (.odt, Oct 2025)
```

## How to use it

1. Download `codebook/4fm-master.json`.
2. Provide it to your LLM of choice alongside your participant testimony (CSV or transcript).
3. The codebook's `instructions_for_llm` block and `coding_guidelines` section tell the LLM how to apply Längle's 4FM framework, with required quote-anchored justification.
4. The LLM returns codes with justifications; you (the researcher) verify the quote provenance manually before publishing.

See [`docs/quick-start-guide.md`](docs/quick-start-guide.md) for a worked example.

The methodology paper above describes the full workflow including multi-LLM cross-validation (Technique A) and quote-anchored source verification (Technique B).

## How it was built

Three LLMs (Claude Sonnet 4.5, ChatGPT 5 Thinking, Gemini 2.5 Turbo) each independently iterated through v1.0 → v3.x against the same four Längle sources, then produced a single-LLM merge of their own iterations (v4.0), then a multi-LLM merge consolidating all three v4.0 candidates (v5.0). The MASTER is Claude's v5.0 (`v5.0 COMPREHENSIVE`), selected for integrating the theoretical depth, practical coding structure, and clean organization that emerged from the three parallel tracks.

The four Längle sources span the framework's full publication history:

- **Längle, A. (1992).** Was bewegt den Menschen? Die existentielle Motivation der Person. *Existenzanalyse, 9*(2).
- **Längle, A. (2002).** Die Grundmotivationen menschlicher Existenz als Wirkstruktur existenzanalytischer Psychotherapie. *Fundamenta Psychiatrica, 16*(1), 27–42.
- **Längle, A. (2003).** The art of involving the person — the existential fundamental motivations as the structure of the motivational process. *European Psychotherapy, 4*(1), 47–58.
- **Längle, A. (2011).** The Existential Fundamental Motivations Structuring the Motivational Process. In D. Leontiev (Ed.), *Motivation, Consciousness and Self-Regulation* (pp. 27–42). Nova Science Publishers.

**Note on Längle 2002 / 2011:** these two sources contain identical content (the 2011 publication is the English version of the 2002 conference paper, same pages 27–42, same publisher pipeline). Both citations are documented in the codebook for transparency. See [`docs/lineage.md`](docs/lineage.md).

See [`docs/lineage.md`](docs/lineage.md) for the full build methodology.

## How to cite

Use `CITATION.cff` (GitHub renders this automatically as a citation widget on the repo homepage) or:

> Nelson-Zutter, G. (YYYY). *EA 4FM Codebook for LLM-Assisted Qualitative Analysis* (Version X.Y) [Data set]. Zenodo. https://doi.org/10.5281/zenodo.XXXXXXX

For a specific version, use the version DOI; for the artifact in general, use the concept DOI.

## Versioning

Each substantive release is a tagged GitHub release. GitHub-→-Zenodo integration archives each release and mints a version DOI. The concept DOI represents the artifact across all versions.

**Important:** the codebook's version lineage is *not* a linear edit chain — it's a multi-LLM consensus process. See [`docs/lineage.md`](docs/lineage.md) for the rationale and methodology.

## Contributing

Issues, discussions, and PRs welcome. The intended development workflow:

- **Issues** track gaps, errors, edge cases, or proposals for new Längle sources to incorporate.
- **PR discussions** work through methodology proposals (e.g., adding a new FM sub-theme, extending to a different population's testimony).
- **Tagged releases** package consensus-validated updates and ship them with a fresh Zenodo DOI.

When proposing a new version: follow the multi-LLM consensus pattern documented in `docs/lineage.md`. Single-LLM unilateral edits should be flagged for review.

## Authorship and contribution

**Sole author:** Graham Nelson-Zutter (graham@aestra.ca), MSc student, Existential Analysis & Logotherapy, University of Salzburg.

The LLMs used during the methodology (Claude Sonnet 4.5, ChatGPT 5 Thinking, Gemini 2.5 Turbo, and others described in `docs/lineage.md`) are **research instruments**, not contributors or co-authors. They do not receive attribution as contributors in `CITATION.cff`, `.zenodo.json`, or git commit metadata. This follows the position taken by major academic journals (Nature, Science, JAMA, the World Association of Medical Editors) that AI systems cannot meet the criteria for authorship and must not be listed as such.

If a future contributor proposes a PR, they should be added to `CITATION.cff` as a co-author of that version (with their consent). The "LLMs as tools, not authors" rule applies to AI systems specifically and is not negotiable.

## License

This work is released under **Creative Commons Attribution 4.0 International (CC BY 4.0)** — see `LICENSE`.

The Längle source quotations embedded in this codebook are used under academic fair use / quotation right and remain subject to their original publishers' copyright — see `NOTICE.md`.
