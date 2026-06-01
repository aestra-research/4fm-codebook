# Version Lineage and Build Methodology

This document explains how each version of the 4FM codebook was produced, and **why traditional file-level git versioning is not sufficient** as the canonical record.

## Why not just `git log`?

Traditional git history records *what changed in the repository* — useful for tracking deposits, edits, and PR merges.

But the codebook's version lineage is **not a linear single-author edit chain.** Each substantive version is the output of a structured multi-LLM process:

1. **Three LLMs** (Claude Sonnet 4.5, ChatGPT 5 Thinking, Gemini 2.5 Turbo) **independently** iterate through v1.0 → v3.x against the same four Längle sources, using the same prompts.
2. **Each LLM produces a single-LLM merge** (v4.0) of its own iterations — a consolidation of the LLM's own progression.
3. **Each LLM produces a multi-LLM merge** (v5.0) consolidating all three v4.0 candidates — three parallel cross-LLM merges, not a single sequential one.
4. **Manual researcher review** selects the strongest v5.0 candidate, which becomes the MASTER.

This is *parallel iteration + cross-LLM consolidation*, not *single-author edits*. `git log` would show "added v1.0 / v1.1 / v1.2 / v1.3 / v2.0 / v3.0 / … / v4.0 / v5.0" — a flat chain that loses the parallelism and the LLM-by-LLM strengths.

`docs/lineage.md` (this file) records the methodology. `git log` records the *deposit* of each version into this repo. The two are complementary:

| What | Tracked by |
|---|---|
| When each file was uploaded to the repo | `git log` |
| Which LLM produced each version | `lineage.md` |
| Which prompt and source set produced each version | `lineage.md` |
| What changed conceptually between adjacent versions | `lineage.md` |
| Which v5.0 was promoted to MASTER and why | `lineage.md` (researcher review note) |
| What changed file-byte-wise between adjacent versions | `git diff` |

## Per-LLM strengths

The multi-LLM approach is intentional, not redundant. Each LLM brings different characteristics to codebook work — observed consistently here and in the companion [12FEP build](https://github.com/aestra-research/12fep-codebook). Documenting them here so future researchers know what to expect from each track:

| LLM | Typical strengths in codebook work | Typical signatures |
|---|---|---|
| **Claude Sonnet 4.5** | Theoretical depth, phenomenological grounding, extensive version-history tracking, careful merge rationale | Largest output. Catches source duplications (e.g., Längle 2011 = 2002). Preserves nuance. The v5.0 promoted to MASTER. |
| **ChatGPT 5 Thinking** | Practical coding structure, hierarchical code IDs (e.g., `FM1.ST1`), explicit inclusion/exclusion criteria, supporting-quote citation density | Most systematic sub-theme breakdowns. Strong for applied research coding. |
| **Gemini 2.5 Turbo** | Organizational clarity, conceptual framework tables, epistemological chains, accessibility | Cleanest structural overview. Compact outputs (often smaller — sometimes a signal of context-window loss; check for size drops). |

These signatures are not absolute — they shifted across runs. But they justify why "three LLMs in parallel" produces something stronger than any single LLM: their strengths are complementary, and their weaknesses are non-overlapping, so consensus tends to converge on the best contribution of each.

## Build methodology — current MASTER (v5.0)

**Source set (4 Längle papers):**

- **Längle (1992)** — *Was bewegt den Menschen? Die existentielle Motivation der Person.* Existenzanalyse, 9(2). Establishes the 4FM structure.
- **Längle (2002)** — *Die Grundmotivationen menschlicher Existenz als Wirkstruktur existenzanalytischer Psychotherapie.* Fundamenta Psychiatrica, 16(1), 27–42. Expands prerequisites, coping reactions, pathological outcomes.
- **Längle (2003)** — *The art of involving the person.* European Psychotherapy, 4(1), 47–58. Motivational process structure, dialogical paradigm, assessment questions.
- **Längle (2011)** — *The Existential Fundamental Motivations Structuring the Motivational Process.* In Leontiev (Ed.), Motivation, Consciousness and Self-Regulation, 27–42. Nova Science Publishers.

**Note on Längle 2002 / 2011:** Claude flagged during the build that the 2002 and 2011 publications are textually identical (same content, same pages 27–42, same publisher pipeline; the 2002 paper is the original conference version, the 2011 paper is the published version in Leontiev's edited volume). Both citations are preserved in the MASTER's `codebook_metadata.source_notes.duplicate_sources` field for transparency. This is a documented case of the **LLM-flagged source duplication audit** pattern catalogued in the WCET4 paper.

**Prompts used:** the WCET4 paper *Catalog of LLM Prompts (from Thesis)* — Google Doc ID `1smLo49YXOyCat8UTZU54EfDW0MzIHL4Wq4QNImMzOqQ` — documents the prompt set for codebook construction, single-LLM merge, multi-LLM merge, and verification.

### Stage 1 — Independent per-LLM iteration through v1.0 → v1.3 (incremental source addition)

Each LLM was given the four Längle sources one at a time, producing a new version per source added:

| Version | Sources included | Per-LLM files |
|---|---|---|
| v1.0 | Längle (1992) only | `claude-4fm-v1.0.json`, `chatgpt-4fm-v1.0.json`, `gemini-4fm-v1.0.json` |
| v1.1 | + Längle (2002) | `…-4fm-v1.1.json` |
| v1.2 | + Längle (2003) | `…-4fm-v1.2.json` |
| v1.3 | + Längle (2011) | `…-4fm-v1.3.json` |

Gemini's v1.3 split into three sub-versions (`v1.3.0`, `v1.3.1`, `v1.3.2`) reflecting iterative refinement during the 2011 integration step.

### Stage 2 — Systematic restructuring (v2.0)

Each LLM independently restructured its v1.3 into a consistent, applied-research-friendly structure: per-FM sub-themes, psychological questions, existential needs, coping reactions, pathological outcomes, and temporal analysis framework.

Claude's v2.0 was produced in two passes (`v2.0-pass1`, `v2.0-pass1-pass2`) reflecting an explicit refinement step.

### Stage 3 — Progressive integration (v3.0 → v3.x)

Each LLM ran a sequence of integration passes folding the v2.0 structure back together with the theoretical depth of v1.x. Claude and ChatGPT produced v3.0 → v3.3; Gemini started at v3.1 (no v3.0) and went through v3.4. The version counts differ per LLM because each LLM proceeded at its own pace and convergence point.

### Stage 4 — Single-LLM self-merge (v4.0)

Each LLM was given **its own** v1.3, v2.0, and v3.x files and asked to merge them into a single consolidated v4.0. This step produced three parallel single-LLM consolidations:

| LLM | v4.0 file |
|---|---|
| Claude Sonnet 4.5 | `codebook/versions/claude/claude-4fm-v4.0-self-merge.json` |
| ChatGPT 5 Thinking | `codebook/versions/chatgpt/chatgpt-4fm-v4.0-self-merge.json` |
| Gemini 2.5 Turbo | `codebook/versions/gemini/gemini-4fm-v4.0-self-merge.json` |

Note: ChatGPT's v4.0 file was originally named with the typo "ChatPGT5" — corrected in this repo.

### Stage 5 — Multi-LLM cross-merge (v5.0)

Three days after the v4.0 self-merges, each LLM was given **all three v4.0 files** and asked to produce a cross-LLM consolidation (v5.0). This step produced three parallel candidates for the final MASTER:

| LLM | v5.0 file |
|---|---|
| Claude Sonnet 4.5 | `codebook/versions/claude/claude-4fm-v5.0-multi-merge.json` *(byte-identical to `codebook/4fm-master.json` — see selection note below)* |
| ChatGPT 5 Thinking | `codebook/versions/chatgpt/chatgpt-4fm-v5.0-multi-merge.json` |
| Gemini 2.5 Turbo | `codebook/versions/gemini/gemini-4fm-v5.0-multi-merge.json` |

### Stage 6 — Manual researcher selection of MASTER

The researcher (Graham Nelson-Zutter) manually reviewed the three v5.0 candidates and selected Claude's v5.0 as the MASTER. Claude's v5.0 was selected for:

- Integrating ChatGPT's detailed sub-theme coding structure (code IDs, inclusion/exclusion criteria, supporting quotes).
- Integrating Gemini's clean organizational structure (epistemological chains, conceptual frameworks).
- Preserving Claude's own theoretical depth and phenomenological grounding.

Claude's v5.0 was marked `COMPREHENSIVE` in the original development workspace filename (`***MERGED-multiple-Claude_EA_LT_4FMs_Codebook_v5_0_COMPREHENSIVE.json` — the leading `*` characters were used in iCloud to force top-of-Finder sort).

The MASTER also documents (in its `codebook_metadata.merged_from_versions` field) what each v4.0 candidate contributed to the integration — providing a transparent rationale that any researcher reading the JSON can verify.

The MASTER is byte-identical to `codebook/versions/claude/claude-4fm-v5.0-multi-merge.json`. The MASTER lives at `codebook/4fm-master.json` as the canonical "use this one" pointer; the symmetric `versions/claude/claude-4fm-v5.0-multi-merge.json` copy makes the 3×N parallel structure visible in the `versions/` directory for provenance auditing.

### Stage 7 — Researcher quote verification

After MASTER selection, the researcher manually verified that every Längle quote embedded in the MASTER traces back to a verifiable line in the Längle source PDFs. This is the **Quote-Anchored Source Verification** technique documented in the WCET4 paper.

## Future versions — the workflow contract

When the codebook is updated (new Längle source incorporated, FM sub-theme refined, etc.), the same multi-LLM consensus process should be followed:

1. Open a GitHub Issue describing the proposed change and rationale.
2. Open a draft PR running Stages 1–5 with the chosen LLM panel. Attach per-LLM iteration outputs, single-LLM v4 merges, and multi-LLM v5 candidates from each LLM in the PR.
3. Document the MASTER selection rationale in the PR.
4. Researcher quote-verification (Stage 7) happens in the PR conversation.
5. Merge the PR and tag a new release (e.g., `v1.1`). GitHub → Zenodo auto-mints a version DOI.

**The PR discussion becomes the lineage record for that version.** It supplements (not replaces) the entry that should be added to this file when the release is tagged.

## Related artifact — the EA 12FEP Codebook

This 4FM codebook is the foundational artifact. The 12 Fundamental Existential Prerequisites (12FEP) framework is *derived from* the 4FM framework — each of the four Fundamental Motivations decomposes into three Prerequisites for a total of twelve.

The 12FEP codebook follows a parallel methodology (multi-LLM cross-validation, quote-anchored Längle sources) with a smaller source set (Längle 2002 + 2011b — note that the 2011b shorthand here distinguishes from the 2011 source above; both refer to the same Leontiev volume publication):

- Repository: https://github.com/aestra-research/12fep-codebook
- Concept DOI: https://doi.org/10.5281/zenodo.20481464

The two codebooks are intended to be used together: the 4FM for the underlying motivational structure, the 12FEP for finer-grained scoring of participant testimony along the three Prerequisites under each FM.

## On contributor attribution

Per the README: LLMs (Claude, ChatGPT, Gemini, etc.) are **tools used in the methodology**, not contributors or co-authors. They do not appear in `CITATION.cff`, `.zenodo.json`, git commit `--author`, or this lineage file's contributor lists.

Future human contributors who substantively shape a version (in the sense of ICMJE / CRediT author criteria) should be added to `CITATION.cff` with their explicit consent.

## Creation Timeline — original authoring dates

The codebook was authored October 25 – October 29, 2025, ~7 months before this repo was created (June 2026). The git commit dates here reflect when files were deposited into the public repo; the authentic creation timestamps from the development workspace are below.

| File | Original creation (PT) | Stage |
|---|---|---|
| `versions/chatgpt/chatgpt-4fm-v1.0.json` | 2025-10-25 23:37 | Stage 1 — Längle 1992 |
| `versions/chatgpt/chatgpt-4fm-v1.1.json` | 2025-10-25 23:41 | Stage 1 — + Längle 2002 |
| `versions/chatgpt/chatgpt-4fm-v1.2.json` | 2025-10-25 23:45 | Stage 1 — + Längle 2003 |
| `versions/chatgpt/chatgpt-4fm-v1.3.json` | 2025-10-25 23:49 | Stage 1 — + Längle 2011 |
| `versions/gemini/gemini-4fm-v1.0.json` | 2025-10-25 23:59 | Stage 1 — Längle 1992 |
| `versions/gemini/gemini-4fm-v1.1.json` | 2025-10-25 23:59 | Stage 1 — + Längle 2002 |
| `versions/gemini/gemini-4fm-v1.2.json` | 2025-10-25 23:59 | Stage 1 — + Längle 2003 |
| `versions/gemini/gemini-4fm-v1.3.0.json` | 2025-10-26 00:00 | Stage 1 — + Längle 2011 (sub-pass 0) |
| `versions/gemini/gemini-4fm-v1.3.1.json` | 2025-10-26 00:05 | Stage 1 — + Längle 2011 (sub-pass 1) |
| `versions/gemini/gemini-4fm-v1.3.2.json` | 2025-10-26 00:10 | Stage 1 — + Längle 2011 (sub-pass 2) |
| `versions/claude/claude-4fm-v1.0.json` | 2025-10-26 00:59 | Stage 1 — Längle 1992 |
| `versions/claude/claude-4fm-v1.1.json` | 2025-10-26 01:05 | Stage 1 — + Längle 2002 |
| `versions/claude/claude-4fm-v1.2.json` | 2025-10-26 01:22 | Stage 1 — + Längle 2003 |
| `versions/claude/claude-4fm-v1.3.json` | 2025-10-26 08:54 | Stage 1 — + Längle 2011 |
| `versions/claude/claude-4fm-v2.0-pass1.json` | 2025-10-26 09:32 | Stage 2 — systematic restructuring (pass 1) |
| `versions/gemini/gemini-4fm-v2.0.json` | 2025-10-26 09:39 | Stage 2 — systematic restructuring |
| `versions/chatgpt/chatgpt-4fm-v2.0.json` | 2025-10-26 22:34 | Stage 2 — systematic restructuring |
| `versions/claude/claude-4fm-v2.0-pass1-pass2.json` | 2025-10-26 22:35 | Stage 2 — systematic restructuring (pass 2) |
| `versions/gemini/gemini-4fm-v3.1.json` | 2025-10-26 22:37 | Stage 3 — progressive integration |
| `versions/gemini/gemini-4fm-v3.2.json` | 2025-10-26 22:37 | Stage 3 — progressive integration |
| `versions/gemini/gemini-4fm-v3.3.json` | 2025-10-26 22:40 | Stage 3 — progressive integration |
| `versions/gemini/gemini-4fm-v3.4.json` | 2025-10-26 22:41 | Stage 3 — progressive integration |
| `versions/claude/claude-4fm-v3.0.json` | 2025-10-26 22:44 | Stage 3 — progressive integration |
| `versions/chatgpt/chatgpt-4fm-v3.0.json` | 2025-10-26 22:46 | Stage 3 — progressive integration |
| `versions/chatgpt/chatgpt-4fm-v3.1.json` | 2025-10-26 22:47 | Stage 3 — progressive integration |
| `versions/chatgpt/chatgpt-4fm-v3.2.json` | 2025-10-26 22:48 | Stage 3 — progressive integration |
| `versions/claude/claude-4fm-v3.1.json` | 2025-10-26 22:49 | Stage 3 — progressive integration |
| `versions/chatgpt/chatgpt-4fm-v3.3.json` | 2025-10-26 22:51 | Stage 3 — progressive integration |
| `versions/claude/claude-4fm-v3.2.json` | 2025-10-26 22:56 | Stage 3 — progressive integration |
| `versions/claude/claude-4fm-v3.3.json` | 2025-10-26 23:16 | Stage 3 — progressive integration |
| `versions/chatgpt/chatgpt-4fm-v4.0-self-merge.json` | 2025-10-26 23:23 | Stage 4 — single-LLM self-merge |
| `versions/gemini/gemini-4fm-v4.0-self-merge.json` | 2025-10-26 23:24 | Stage 4 — single-LLM self-merge |
| `versions/claude/claude-4fm-v4.0-self-merge.json` | 2025-10-26 23:30 | Stage 4 — single-LLM self-merge |
| `versions/chatgpt/chatgpt-4fm-v5.0-multi-merge.json` | 2025-10-29 14:42 | Stage 5 — multi-LLM cross-merge |
| `versions/gemini/gemini-4fm-v5.0-multi-merge.json` | 2025-10-29 14:43 | Stage 5 — multi-LLM cross-merge |
| `versions/claude/claude-4fm-v5.0-multi-merge.json` (= `4fm-master.json`) | 2025-10-29 15:12 | Stage 5 — multi-LLM cross-merge (promoted to MASTER per researcher review) |

Source: filesystem mtimes from the original codebook development workspace (iCloud, `~/Library/Mobile Documents/com~apple~CloudDocs/LibreOffice/*EA & LT Codebooks/LLMS/`). Mtimes are preserved at the source files and are verifiable by anyone with access to that workspace.

## Note on filename normalization

In the original development workspace, filenames varied across LLMs:

- Claude: `Claude-EA_LT_4FMs_Codebook_vX.Y.json`
- ChatGPT: `ChatGPT5-EA_LT_4FMs_Codebook_vX.Y.json` (with one typo: `MERGED-single-LLM-ChatPGT5-…_v4.0.json` — corrected here)
- Gemini: `Gemini-EA_LT_4FMs_Codebook_vX.Y.json`
- v4.0 merges: `MERGED-single-LLM-{LLM}-…_v4.0.json`
- v5.0 merges: `MERGED-multi-LLM-{LLM}-…_v5.json` (Gemini, ChatGPT) or `***MERGED-multiple-{LLM}_…_v5_0_COMPREHENSIVE.json` (Claude)

For symmetric in-repo naming, all files have been normalized to `{llm}-4fm-{version}[-suffix].json`. The byte content of the codebooks themselves is unchanged.
