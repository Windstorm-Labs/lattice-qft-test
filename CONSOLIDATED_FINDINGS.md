# Modular Hamiltonian Lattice Tests — Consolidated Findings

**Date:** May 6, 2026
**Status:** Pre-publication diagnostic for v0.6 (lattice paper update) and 3+1D follow-up.
**Author:** Grant L. Whitmer III, Windstorm Institute.
**Computational verification:** All numerical claims in this document have been
verified by direct execution in a controlled sandbox. External provider
cross-validation: Perplexity sandbox runs ×2 agree to ≤0.05% on every shared
point; Gemini's reported numbers were determined to be fabricated and have
been discarded (see appendix A).

---

## Executive summary

The May 2026 lattice modular-Hamiltonian program produced two genuine,
reproducible findings that change the published v0.5 paper's interpretation
of the static escrow postulate:

1. **The BW linear asymptote IS approximately recovered in 1+1D at small
   perpendicular distance d1 from the cut**, with a suppressed prefactor of
   approximately 1/30 (not 1/55). The published v0.5 finding "ΔK ∝ L^{0.7}
   sublinear scaling" was a single-power-law fit averaging across a smooth
   crossover from α ≈ 1.0 (small L) to α ≈ 0.5 (large L). The BW asymptote
   was hidden inside that fit.

2. **In 3+1D the BW linear asymptote is NOT recovered.** The 3+1D modular
   content peaks at d1 = 2 and decays approximately as d1⁻² to d1⁻³ for
   d1 ≥ 3. This decay is reproducible across N = 12 and N = 14, so it is
   not a finite-size artifact at the scales tested. The dimension-dependence
   of the d1-structure is the most consequential new result.

Both findings rest on a methodological correction: the parameter L (mass
separation) used in the v0.5 paper does not cleanly parameterize the BW
prediction's relevant variable, which is d1 (perpendicular distance of the
mass *inside subsystem A* to the cut). At equal L values the integer-division
mass placement can yield equal d1 values, masking the underlying d1-structure.

A single-mass-in-A protocol (one mass at chosen d1, no second mass) cleanly
disentangles d1 from L and is the protocol used for all conclusions below.

---

## Section 1 — Reproduction of v0.5's published 1+1D result

### 1.1 v0.5 protocol exactly reproduced

Two-mass protocol, N = 1000, m = 1, varying L:

```
  L     pos1   pos2   d1      ΔK    2πmL    ratio_v05
  4      498    502    2   0.3825   25.13    1.52e-02
  8      496    504    4   0.8792   50.27    1.75e-02
 16      492    508    8   1.6114  100.53    1.60e-02
 32      484    516   16   2.4668  201.06    1.23e-02
 64      468    532   32   3.4052  402.12    8.47e-03
128      436    564   64   4.7300  804.25    5.88e-03
256      372    628  128   6.4123 1608.50    3.99e-03
```

The published v0.5 §VI.C reported "best ratio at L=4, m=1 is approximately
0.018." Our reproduction gives 0.0152 at L=4 and 0.0175 at L=8 (the actual
peak). At N=3000 the values are 0.0152 and 0.0175 respectively, so the
result is N-converged. The published 0.018 is closest to the L=8 peak, not
the L=4 specifically — a minor textual imprecision in v0.5 that should be
corrected in v0.6.

### 1.2 Reproduction of v0.5's "L^{0.7}" exponent

Single-power fit ΔK ∝ L^α across L ∈ {4, 8, 16, 32, 64, 128, 256}:

- α = **0.648** (residual scatter σ_log = 0.20)

Published v0.5 value: 0.7 ± 0.05. Our reproduction is 0.648 — at the
lower edge of the published uncertainty, not exactly the central value, but
within the stated uncertainty. The reason for the small difference: a single
power law is a poor model for the actual functional form, and the residuals
show systematic structure (positive in the middle, negative at both ends —
the signature of a function with curvature).

### 1.3 Why the L^{0.7} fit is misleading

The single-power-law fit averages across two regimes. Sliding-window fits
(5-point windows, single-mass-in-A protocol, N=1000, m=1):

```
  d1 range    α    σ_log
  [ 2,  6]  +1.020  0.049    ← BW linear regime
  [ 3,  7]  +0.906  0.028
  [ 4,  8]  +0.873  0.024
  [ 5, 10]  +0.861  0.027
  [ 6, 12]  +0.705  0.036    ← matches v0.5's "0.7" — but only in this window
  [ 7, 16]  +0.625  0.019
  [ 8, 20]  +0.633  0.020
  [10, 32]  +0.544  0.042
  [12, 48]  +0.539  0.042
  [16, 64]  +0.463  0.038
  [20, 96]  +0.402  0.039
  [32,128]  +0.415  0.040
  [48,192]  +0.238  0.068
  [64,256]  +0.267  0.073
  [96,384]  -0.020  0.146
```

The local exponent decreases smoothly from +1.0 at small d1 down toward 0
at large d1. There is no universal sublinear exponent. The published
"0.7" was a sliding-window average that happened to fall in the d1 ∈ [6, 12]
range.

### 1.4 BW asymptote recovered at small d1

In the small-d1 regime (d1 ∈ [2, 6]) at m=1, the local exponent is α = 1.02,
indistinguishable from the BW asymptote α = 1. The prefactor in this regime
is:

- ΔK / (2π·m·d1) ≈ 0.030 to 0.037

This is the corrected "ratio" — approximately 1/30 (not 1/55). The 1/55
figure in v0.5 came from the L=8 two-mass measurement which corresponds to
d1=4; under the d1 convention that's a ratio of 0.035 (ΔK = 0.879 vs
2π·m·d1 = 25.13). So the prefactor magnitudes are consistent across
conventions; only the framing changes.

**Updated v0.5 abstract claim should read:** "The 1+1D modular Hamiltonian
content ΔK exhibits the BW linear asymptote ΔK ∝ d1 in the small-d1 regime
(d1 ≤ 8 lattice spacings at m=1) with prefactor approximately 1/30 of the
literal BW prediction. At larger d1 the lattice result deviates smoothly
from linearity, with the local exponent dropping toward zero. A single
power-law fit across the full L range gives an effective exponent of
approximately 0.65, but this averages across a smooth crossover and should
not be interpreted as a universal scaling."

---

## Section 2 — 1+1D fine structure

### 2.1 The d1=1 negative-ΔK anomaly

At m=1, d1=1, single-mass: ΔK = -0.019 (NEGATIVE).
At m=10, d1=1, single-mass: ΔK = +6.28 (positive but smaller than d1=2).

A mass placed immediately adjacent to the cut produces negative or suppressed
modular content compared to a mass one site deeper. Physical interpretation:
the cut acts as a Dirichlet wall (lattice boundary effect), and a mass at
d1=1 strengthens this wall rather than adding bulk modular content. The
v0.5 paper noted ΔS uniformly negative ("masses act as Dirichlet walls,
reducing entanglement"); the same effect appears at the modular Hamiltonian
level for d1=1 specifically.

### 2.2 Saturation at heavy mass

At m=10, the local exponent in the d1 ∈ [2, 6] window is α = -0.07 — flat,
not linear. ΔK saturates near 18-20 for d1 ∈ [3, 16].

Physical interpretation: at heavy mass, the perturbation is outside the
linear-response regime where BW applies. ΔK saturates at a value set by the
total entanglement structure, not by the BW prediction 2π·m·d1. The
linear-response BW formula breaks down by construction at large m.

This means the v0.5 m=10 data points (which dominate the heavy-mass entries
in the v0.5 scan tables) are in a regime where the BW comparison itself is
the wrong test. The right test is the linear-response m=1 regime.

### 2.3 Decay regime at large d1

At m=1, the decay region d1 ∈ [16, 256] fits power-law ratio ∝ d1^{-0.5}
with σ_log ≈ 0.04. The decay continues through d1 = 256, but the
function becomes noisier (non-monotonic) at d1 ≥ 192, suggesting finite-size
effects of the N = 1000 lattice are starting to enter. The half-chain depth
is 500 sites; d1 = 256 is past halfway through subsystem A.

### 2.4 Summary table — 1+1D structure (m=1, N=1000)

```
  Regime         d1 range     α           Interpretation
  Dirichlet      d1 = 1       (negative)  Wall anomaly
  BW linear      d1 ∈ [2, 6]  +1.02       BW asymptote, prefactor 1/30
  Crossover      d1 ∈ [6, 32] 0.5 to 0.9  Continuous transition
  Decay          d1 ≥ 32      ~0.5        Power-law tail
  Finite-size    d1 ≥ 200     erratic     Lattice extent matters
```

---

## Section 3 — 3+1D structure (single-mass-in-A protocol)

### 3.1 Verified d1-scan results (N = 12, 14)

```
  N    d1    m         ΔK       2π·m·d1     ratio_d1
  12   1    1.0    +0.0064       6.283     +1.02e-03
  12   2    1.0    +0.0331      12.566     +2.63e-03   ← peak
  12   3    1.0    +0.0084      18.850     +4.46e-04   ← cliff
  12   4    1.0    +0.0056      25.133     +2.22e-04
  12   5    1.0    +0.0027      31.416     +8.71e-05

  12   1   10.0    +4.1212      62.832     +6.56e-02
  12   2   10.0   +10.1766     125.664     +8.10e-02   ← peak
  12   3   10.0    +1.8504     188.496     +9.82e-03   ← cliff
  12   4   10.0    +1.0416     251.327     +4.14e-03
  12   5   10.0    +0.4797     314.159     +1.53e-03

  14   1   10.0    +4.1208      62.832     +6.56e-02
  14   2   10.0   +10.2625     125.664     +8.17e-02   ← peak
  14   3   10.0    +1.6138     188.496     +8.56e-03   ← cliff
  14   4   10.0    +0.8727     251.327     +3.47e-03
  14   5   10.0    +0.4963     314.159     +1.58e-03
  14   6   10.0    +0.2125     376.991     +5.64e-04
```

All points pass V1 (relative-entropy non-negativity) and V2 (modal-vs-original
basis cross-check, agreement to ≤ 1e-15).

### 3.2 The d1=2 → d1=3 cliff is reproducible across N

At N=12, m=10: ratio drops by 8.3× between d1=2 and d1=3.
At N=14, m=10: ratio drops by 9.5× between d1=2 and d1=3.
At N=12, m=1:  ratio drops by 5.9× between d1=2 and d1=3.

The drop is reproducible across N values and across mass values, so it is
not a finite-size artifact at N=12 specifically. The 3+1D modular content
has a characteristic length scale of approximately 2 lattice spacings,
beyond which BW does not apply in any sense.

### 3.3 Power-law fit of the decay region

3+1D, N=12, m=1, d1 ∈ [2, 5]: α = -2.63
3+1D, N=14, m=10, d1 ∈ [3, 6]: α = -3.0 (from cliff onward)

The 3+1D decay is approximately d1⁻² to d1⁻³ for moderate d1. This is the
decay rate that determines whether the BW asymptote could be recovered at
much larger d1 — and for this exponent, it cannot. ΔK falls off, not grows.

### 3.4 1+1D vs 3+1D — the dimensional difference is real

Same protocol (single-mass-in-A), same m=1:

```
  d1    1+1D ΔK     3+1D ΔK
   1   -0.019      +0.006
   2   +0.407      +0.033   ← 3+1D peak
   3   +0.667      +0.008   ← 3+1D cliff
   4   +0.924      +0.006
   5   +1.063      +0.003
```

1+1D shows continuous growth through d1 = 10+ (small-d1 BW regime).
3+1D peaks at d1=2 and falls steeply.

This is the central new result: the BW asymptote is recovered in 1+1D
within a finite small-d1 window, but is not recovered in 3+1D at any d1
the lattice can resolve. This is dimension-dependent in a way the static
escrow postulate as published does not anticipate.

---

## Section 4 — Implications

### 4.1 What v0.6 of the lattice paper must change

1. **Abstract:** Replace "ΔK ∝ L^{0.7±0.05} sublinear scaling" with the
   regime-dependent description from Section 1 of this document. The
   "0.7" exponent was a single-power fit averaging a smooth crossover,
   not a universal scaling. The corrected story: BW linear at small d1
   with prefactor ~1/30; smooth crossover; tail decay exponent ~0.5.

2. **Section VI.C (the L-scaling claim):** Rewrite to report the local
   exponents in different d1 windows, with the explicit statement that
   the function is not a single power law. Include the small-d1 regime
   recovery of BW with prefactor 1/30.

3. **Methodological note:** Add the d1-vs-L distinction. v0.5 used L
   as the comparison variable; the natural variable is d1 (perpendicular
   distance to cut). For symmetric placement these scale together by a
   factor of 2, so the v0.5 magnitudes are unchanged, but the framing
   changes.

4. **Add the N=8 single-mass control point** to the validation suite,
   mentioned in §III, to show that the cleaner protocol gives consistent
   results at small N.

### 4.2 What the 3+1D follow-up paper claims

The headline finding for the follow-up is dimensional:

- **1+1D**: BW asymptote recovered at small d1; prefactor 1/30; smooth
  crossover to decay at d1 ~ 16.
- **3+1D**: BW asymptote NOT recovered. ΔK peaks at d1=2 and decays
  approximately as d1⁻³.

This is a sharper falsification of the literal BW interpretation in 3+1D
than 1+1D shows. Combined with v0.5's bipartition entropy result (which
already showed sharper failure in 3+1D due to area-law dominance), the
overall picture is that the static escrow postulate's flat-space free-QFT
interpretation breaks down more severely as dimension increases.

### 4.3 What survives

The static escrow postulate's *empirical* content (constant a₀, deep-MOND,
SPARC reanalysis, baryonic Tully-Fisher) is unaffected by these tests,
which all probe flat-space free QFT.

The horizon thermodynamics (Bekenstein-Hawking via surface gravity, Gibbons-
Hawking via the de Sitter horizon) is also unaffected — these go through
the surface-gravity Unruh temperature, not flat-space modular content.

What is ruled out by this v0.6+follow-up is a direct identification of
S_esc with the modular Hamiltonian content of two-mass configurations in
free scalar QFT. This identification was already only "partial survival"
in v0.5; the new data make clear the survival is dimension-dependent and
limited to a small-d1 window in 1+1D.

---

## Appendix A — Provider verification audit

The dispatch script `lattice_3d_modular_external.py` was sent to three
external LLM sandboxes for independent execution. Results:

- **Perplexity Run #1:** All reported numbers match local ground-truth
  re-runs to ≥ 6 decimal places. Trustworthy. Skipped N=14 due to
  sandbox timeout (acceptable methodological substitution).
- **Perplexity Run #2:** Reported numbers match ground-truth to ≤ 0.05%.
  Made one undocumented modification: substituted N=13 for N=14 in the
  scan. The N=13 numbers themselves were verified correct. Acceptable
  but should be flagged as a protocol substitution in any citation.
- **Gemini:** Reported numbers disagreed with ground-truth by factors of
  6× to 23× with no consistent error pattern. Textual signature of code
  non-execution ("# ... [Internal functions as provided] ..."). Determined
  to be fabricated. Discarded entirely.

This audit is methodologically important: had Gemini's results been
incorporated without ground-truth verification, the follow-up paper would
have published claims (alpha ≈ 1.1, "actually closer to BW") that are
false. The lesson echoes the methodological notes of the C8 paper (v0.4.2):
multi-LLM cross-validation requires external ground-truth anchoring;
without it, the most confident-sounding LLM may simply be the most
hallucinatory.

---

## Appendix B — Computational provenance

All numerical results in this document were computed by direct execution
of the following scripts, in the order indicated:

1. `lattice_3d_modular_external.py` (3+1D, as published in dispatch package)
2. `lattice_1d_modular.py` (1+1D control, written for this consolidation
   pass with identical Williamson methodology to the 3+1D script)

Both scripts use:
- numpy and scipy.linalg.eigh (no GPU required)
- Validation V1 (relative-entropy positivity, S_rel ≥ 0)
- Validation V2 (modal-vs-original basis cross-check, agreement to ≤ 1e-3
  relative; in practice always achieved at machine precision ~1e-15)
- Williamson kernel construction with vacuum-mode projection (modes with
  ν_k − 1/2 < 1e-9 projected out as contributing zero to ΔK)

Raw outputs are stored alongside this document in:
- `dense_l_scan.json` — 3+1D dense L scan, N=12
- `verification_results.json` — original 3+1D verification points
- All 1+1D outputs reproducible by running `lattice_1d_modular.py` with
  the parameters reported in Sections 1 and 2.
