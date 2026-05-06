# Paper 13: Lattice QFT Test — Code & Data

**A Lattice Quantum Field Theory Test of the Static Escrow Postulate: 1+1D and 3+1D Falsification with Modular-Hamiltonian Partial Survival**

[![DOI](https://img.shields.io/badge/DOI-10.5281%2Fzenodo.20043421-blue)](https://doi.org/10.5281/zenodo.20043421)
[![License: MIT](https://img.shields.io/badge/Code-MIT-green)](https://opensource.org/licenses/MIT)
[![License: CC BY 4.0](https://img.shields.io/badge/Data-CC_BY_4.0-lightgrey)](https://creativecommons.org/licenses/by/4.0/)
[![Track: Entropic Bounds](https://img.shields.io/badge/Track-2_·_Entropic_Bounds-8b5cf6)](https://windstorminstitute.org/#track2)

> **Track 2 of the Windstorm Institute — Entropic Bounds in Analog Systems.** Companion to the paper's *Code and Data Availability* section. Supplement to [Paper 11 (Gravitational Entropy Escrow)](https://github.com/Windstorm-Institute/gravitational-entropy-escrow).

---

## Published paper

- **[Windstorm-Institute/lattice-qft-test](https://github.com/Windstorm-Institute/lattice-qft-test)** — paper PDF, abstract, headline results
- **Website article:** [windstorminstitute.org/articles/lattice-qft-test.html](https://windstorminstitute.org/articles/lattice-qft-test.html)
- **Zenodo (concept, always latest):** [10.5281/zenodo.20043421](https://doi.org/10.5281/zenodo.20043421)
- **v0.7 specifically:** [10.5281/zenodo.20057538](https://doi.org/10.5281/zenodo.20057538)

## Contents

| File | What it does |
|------|-------------|
| **[`lattice_1d_modular.py`](lattice_1d_modular.py)** | 1+1D production code. Reproduces every numerical claim in v0.7 §VI.C. Implements the single-mass-in-A protocol used to derive the corrected regime-dependent characterization of ΔK(d1). Williamson decomposition with vacuum-mode projection (modes with ν − ½ < 1e-9 projected to zero contribution); validation V1 (relative-entropy positivity) and V2 (modal-vs-original basis cross-check, agreement at machine precision ~1e-15). |
| **[`CONSOLIDATED_FINDINGS.md`](CONSOLIDATED_FINDINGS.md)** | Pre-publication analysis document showing how the v0.6/v0.7 corrections were derived from the published v0.5 data. Includes raw numerical tables, sliding-window power-law fits over the full L scan, the d1=1 Dirichlet anomaly, the heavy-mass saturation result, the 1+1D vs 3+1D dimensional comparison at matched m = 1, and a multi-LLM provider verification audit (Perplexity ×2 trustworthy at ≤0.05%, Gemini disagreed by 6×–23× and was determined to be fabricated; lessons echo the methodology of Paper 12). |

## Reproduction

```bash
pip install numpy scipy
python3 lattice_1d_modular.py
```

- **Python:** 3.11+
- **Dependencies:** `numpy`, `scipy.linalg.eigh` (no GPU required)
- **Total compute:** approximately 10 minutes on a single CPU core for the full 1+1D scan
- **Hardware tested:** Ubuntu 24.04 / Linux laptop and RTX 5090 workstation; CPU-only path

The 1+1D scan reproduces:

- All ratio_v05 values in Section 1.1 (two-mass protocol reproduction)
- The α = 0.648 single-power-law fit (Section 1.2) and the sliding-window fits (Section 1.3) that decompose the smooth crossover
- The small-d1 BW recovery at α = +1.02, prefactor ~1/30 (Section 1.4)
- The d1=1 Dirichlet wall anomaly (Section 2.1) and the heavy-mass saturation regime (Section 2.2)
- The 1+1D vs 3+1D matched-mass comparison numbers (Section 3.4)

The 3+1D production code (`lattice_3d_modular_external.py`) ships with the companion paper deposit on Zenodo. The dispatch script that performed external-provider verification is the same code, run in three independent sandboxes for ground-truth cross-validation.

## Methodology context — why the single-mass-in-A protocol matters

The v0.5 paper used a two-mass protocol (one mass on each side of the cut, separated by L) and reported "ΔK ∝ L^{0.7±0.05} sublinear scaling" as the headline 1+1D modular result. The v0.6/v0.7 update introduced a single-mass-in-A protocol (one mass placed at chosen perpendicular distance d1 from the cut, no second mass) which cleanly disentangles d1 from L. At equal L the integer-division placement of the v0.5 protocol can yield equal d1 values, masking the underlying d1-structure.

When the same data are re-analyzed under a single-mass-in-A protocol with sliding-window power-law fits, the local exponent is regime-dependent — α ≈ +1.0 in the small-d1 BW window, smoothly transitioning to α ≈ +0.5 in the decay tail at d1 ≥ 16. The previously-published "0.7" was a single-power fit averaging across this smooth crossover. The corrected story is BW linear at small d1 with a 1/30 prefactor; the 1/30 itself is conjectured in §VI.D to reflect a UV/IR mismatch between the BW formula (an IR/long-wavelength prediction) and a localized lattice mass at correlation length ξ ∼ 1 lattice spacing (a UV-scale perturbation).

## External-provider verification audit

The 3+1D dispatch script `lattice_3d_modular_external.py` was sent to three external LLM sandboxes for independent execution. Results, per Appendix A of the consolidated findings:

| Provider | Agreement | Verdict |
|----------|-----------|---------|
| Perplexity Run #1 | matches local ground-truth ≥ 6 decimal places | trustworthy (skipped N=14 due to sandbox timeout — acceptable) |
| Perplexity Run #2 | matches ≤ 0.05% | trustworthy (substituted N=13 for N=14 — acceptable; flagged as protocol substitution) |
| Gemini | disagreement of 6×–23× with no consistent error pattern; signature of code non-execution | **fabricated; discarded** |

Had Gemini's results been incorporated without ground-truth verification, the v0.7 update would have published claims (alpha ≈ 1.1, "actually closer to BW") that are false. This audit is the kind of methodological infrastructure that makes multi-LLM physics review actually load-bearing rather than confidence-laundering. It echoes the same lesson from Paper 12's C8 audit: multi-LLM cross-validation requires external ground-truth anchoring; without it, the most confident-sounding LLM may simply be the most hallucinatory.

---

## Discuss this code

- **Bug, reproduction failure, or unexpected output?** → [Open an Issue](../../issues)
- **Q&A — version compatibility, hardware, generalization to other inputs?** → [Start a Discussion](../../discussions)
- **Discuss the paper itself** → [Comments on the website article](https://windstorminstitute.org/articles/lattice-qft-test.html#comments) or [Issues on the Institute repo](https://github.com/Windstorm-Institute/lattice-qft-test/issues)

---

## The Windstorm Institute

### Track 1 — The Throughput Basin · 9 papers (Papers 1–9 globally; 1st through 9th in this track; arc complete)

| # | Paper | DOI |
|---|-------|-----|
| 1 | [The Fons Constraint](https://github.com/Windstorm-Institute/fons-constraint) | [10.5281/zenodo.19274048](https://doi.org/10.5281/zenodo.19274048) |
| 2 | [The Receiver-Limited Floor](https://github.com/Windstorm-Institute/receiver-limited-floor) | [10.5281/zenodo.19322973](https://doi.org/10.5281/zenodo.19322973) |
| 3 | [The Throughput Basin](https://github.com/Windstorm-Institute/throughput-basin) | [10.5281/zenodo.19323194](https://doi.org/10.5281/zenodo.19323194) |
| 4 | [The Serial Decoding Basin τ](https://github.com/Windstorm-Institute/serial-decoding-basin) | [10.5281/zenodo.19323423](https://doi.org/10.5281/zenodo.19323423) |
| 5 | [The Dissipative Decoder](https://github.com/Windstorm-Institute/dissipative-decoder) | [10.5281/zenodo.19433048](https://doi.org/10.5281/zenodo.19433048) |
| 6 | [The Inherited Constraint](https://github.com/Windstorm-Institute/inherited-constraint) | [10.5281/zenodo.19432911](https://doi.org/10.5281/zenodo.19432911) |
| 7 | [The Throughput Basin Origin](https://github.com/Windstorm-Institute/throughput-basin-origin) | [10.5281/zenodo.19498582](https://doi.org/10.5281/zenodo.19498582) |
| 8 | [The Vision Basin](https://github.com/Windstorm-Institute/vision-basin) | [10.5281/zenodo.19672827](https://doi.org/10.5281/zenodo.19672827) |
| 9 | [The Hardware Basin](https://github.com/Windstorm-Institute/hardware-basin) | [10.5281/zenodo.19672921](https://doi.org/10.5281/zenodo.19672921) |

### Track 2 — Entropic Bounds in Analog Systems · 4 papers (Papers 10–13 globally; 1st through 4th in this track; line of inquiry active)

| # | Paper | DOI |
|---|-------|-----|
| 10 | [Phonon Extraction Bound (BEC Analog Gravity)](https://github.com/Windstorm-Institute/phonon-extraction-bound) *(1st in track)* | [10.5281/zenodo.20014391](https://doi.org/10.5281/zenodo.20014391) |
| 11 | [Gravitational Entropy Escrow](https://github.com/Windstorm-Institute/gravitational-entropy-escrow) *(2nd in track; framework paper)* | [10.5281/zenodo.20032023](https://doi.org/10.5281/zenodo.20032023) |
| 12 | [C8 Clarification Note](https://github.com/Windstorm-Institute/c8-clarification-note) *(3rd in track; companion to Paper 11)* | [10.5281/zenodo.20041992](https://doi.org/10.5281/zenodo.20041992) |
| 13 | [Lattice QFT Test of the Static Escrow Postulate](https://github.com/Windstorm-Institute/lattice-qft-test) *(this paper — 4th in track; supplement to Paper 11)* | [10.5281/zenodo.20043421](https://doi.org/10.5281/zenodo.20043421) |

**Website:** [windstorminstitute.org](https://windstorminstitute.org)

---

*Code: MIT License · Data: CC BY 4.0*
