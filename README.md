# NIST FRTE Facial Recognition Demographic Bias

Five interactive visualizations documenting demographic disparities in facial recognition error rates, built from empirically measured data from NIST's Face Recognition Technology Evaluation (FRTE) 1:1 Verification track (2017–2026).

Developed in conjunction with Chapter 3 ("Facial Recognition as Racial Recognition") of an ongoing scholarly manuscript examining surveillance technology and structural bias.

---

## Background

Facial recognition algorithms produce two types of errors. False matches occur when the system incorrectly links two photos of different people — this is what happened to Robert Williams, whose wrongful arrest in 2020 was triggered by an algorithmic false match. False non-matches occur when the system fails to recognize the same person across two photos.

NIST's ongoing FRTE evaluations — covering 1,368 algorithms from 420 developers as of January 2026 — document that both error types are not distributed equally across demographic groups. Eastern European males aged 35–50 consistently appear in the best-performance categories. West African females aged 65–99 consistently appear in the worst. This pattern holds across independent developers, different algorithmic architectures, and different training pipelines (NIST, 2026; Grother, 2022).

The five visualizations below make this pattern operationally legible.

---

## Live Visualizations

| # | Tool | What it shows |
|---|---|---|
| 1 | [Operational false positive calculator →](https://DrMarioDeSean411-arch.github.io/nist-frte-facial-recognition-bias/frte_false_positive_calculator.html) | Expected false matches by demographic group, database size, and algorithm |
| 2 | [FMR demographic heatmap →](https://DrMarioDeSean411-arch.github.io/nist-frte-facial-recognition-bias/frte_fmr_heatmap.html) | Per-cell false match rates across 5 regions × 5 age groups × 3 algorithms × 2 sexes |
| 3 | [Algorithm submission timeline →](https://DrMarioDeSean411-arch.github.io/nist-frte-facial-recognition-bias/frte_bias_persistence_timeline.html) | 1,368 submissions 2017–2026 with bias persistence overlay and key publication markers |
| 4 | [Accuracy vs. equity scatter →](https://DrMarioDeSean411-arch.github.io/nist-frte-facial-recognition-bias/frte_accuracy_equity_scatter.html) | FNMR vs. Gini / Max/GeoMean with Pareto frontier showing the structural equity floor |
| 5 | [Compounding harm Sankey →](https://DrMarioDeSean411-arch.github.io/nist-frte-facial-recognition-bias/frte_compounding_harm_sankey.html) | Causal flow from biased policing → skewed training data → elevated FMR → more surveillance |

---

## Visualization 1 — Operational False Positive Calculator

**File:** `frte_false_positive_calculator.html`

**What it does:** Translates per-comparison false match rates (FMR) into expected false positive counts for a database of N people using the binomial approximation FPIR ≈ N × FMR (Grother, NISTIR 8429, Annex B, eq. 32). Users set the database size with a slider and select a demographic group to see expected false match counts for three NIST-evaluated algorithms side by side.

**Key finding shown:**

| Algorithm | E. European male (35–50) | W. African female (65–99) | Fold difference |
|---|---|---|---|
| qazsmartvisionai_003 | 0.00006 | 0.02150 | 358× |
| recognito_001 | 0.00001 | 0.01878 | 1,878× |
| innovatrics_014 | 0.00004 | 0.01104 | 276× |

At a database of 10,000: an Eastern European male produces ~0 expected false matches with recognito_001; a West African female produces ~188.

**Methodological note:** The simplified binomial approximation is appropriate for illustration. Actual 1:N FPIR depends additionally on cross-demographic FMR, database composition, and search volume (NISTIR 8429, Annex B, eqs. 33–37). For algorithms that stabilize the tail of the impostor distribution, FPIR may not scale linearly with N.

---

## Visualization 2 — FMR Demographic Heatmap

**File:** `frte_fmr_heatmap.html`

**What it does:** Renders the full false match rate matrix as a color-coded grid — 5 demographic regions × 5 age groups — for three algorithms and both sexes. Color encodes FMR magnitude; darker red means more false matches. Users toggle algorithm and sex; clicking any cell shows the FMR value and an operational interpretation (expected false matches per 10,000 searched, fold-difference from the Eastern European male baseline).

**Key finding shown:** The lightest cells (near-zero FMR) cluster in Eastern Europe across all age groups. The darkest cells (FMR ≥ 0.01) cluster in West Africa, particularly for females aged 65–99. The spatial pattern is structurally stable across all three algorithms.

**Data:** FMR values extracted from NIST FRTE published log₁₀ FMR heatmaps (Figure 1 and Annex A, NISTIR 8429). Threshold set per algorithm at nominal FMR = 0.00001 overall on the visa-border dataset.

---

## Visualization 3 — Algorithm Submission Timeline & Bias Persistence

**File:** `frte_bias_persistence_timeline.html`

**What it does:** Plots all 1,368 NIST FRTE algorithm submissions as a scatter timeline from 2017 to 2026. Each dot is one algorithm; red = West African female (65–99) is the worst-performing demographic for that algorithm; blue = some other group is worst. Three dashed vertical lines mark the Gender Shades publication (Feb 2018), NISTIR 8280 (Dec 2019), and NISTIR 8429 (Jul 2022). A per-year table shows submission counts, developer counts, and estimated percentage of algorithms with West African female 65+ as worst demographic.

**Key finding shown:** The column of red dots does not thin out after any of the three publication markers. Algorithms submitted in July 2025 display the same demographic performance hierarchy documented in 2018 — across independent companies with different architectures and training pipelines.

**Methodological note:** Y-axis positions are jittered using a deterministic pseudo-random seed for legibility only and carry no data value. Per-year worst-demographic percentages are conservative interpolations from published NIST FRTE demographic differential tables (updated 2025-12-11) and the stated finding in NIST (2026) that the pattern appears in >95% of evaluated algorithms across 2024–2025 submissions.

---

## Visualization 4 — Accuracy vs. Equity Scatter

**File:** `frte_accuracy_equity_scatter.html`

**What it does:** Plots NIST FRTE algorithms with overall FNMR on the X-axis (accuracy) and demographic inequity on the Y-axis. Users toggle between two inequity metrics: the Gini coefficient (SAIC/DHS measure, NISTIR 8429 eqs. 13–14) and the Max/GeoMean FNMR ratio (NIST's preferred leading measure, eqs. 10–11). A Pareto frontier marks the boundary of the best-achievable accuracy–equity tradeoff. An amber band marks the observed floor for the highest-performing submissions. A teal dashed line marks ideal parity.

**Key finding shown:** The most accurate algorithms (FNMR 0.0013–0.0016) cluster at Gini 0.07–0.10 and Max/GeoMean 1.05–1.15. Neither metric approaches its ideal value (0.00 and 1.00 respectively). The floor has not moved from 2022 to 2025 despite improved overall accuracy. This directly counters the argument that better training data will close the equity gap.

**Data:** FNMR overall, Gini, and Max/GeoMean values taken from NIST FRTE demographic differential table (columns as documented in NISTIR 8429 §4). Algorithms shown are a representative selection covering the full accuracy range of publicly evaluated submissions.

---

## Visualization 5 — Compounding Harm Sankey

**File:** `frte_compounding_harm_sankey.html`

**What it does:** A Sankey flow diagram tracing the causal feedback loop described in Chapter 3: racially biased policing produces skewed mugshot databases → skewed databases become training data → training data bias produces elevated FMR for Black and Brown faces → elevated FMR generates more false investigative leads → more stops and arrests produce more mugshots. Node widths scale to cited statistics. Clicking any node opens a citation panel with the supporting source.

**Key finding shown:** The bias documented in NIST's FMR data is not a self-contained technical failure. It is one node in a closed feedback loop grounded in documented policing disparities. Technical improvements to algorithms address one node; the loop itself remains intact.

**Methodological note:** This diagram illustrates a causal argument made in the scholarly literature. Node width scaling uses cited statistics (Black Americans 5× more likely to be stopped; 56% of incarcerated population vs. 32% of U.S. population; 4× more likely to be arrested for cannabis possession). Causal arrows represent relationships documented across multiple sources, not experimentally established direct causation. The diagram's methodology note is embedded in the visualization footer.

---

## Shared Methodological Notes

**Race proxy:** NIST uses country of birth as a proxy for race across all visualizations. This proxy is imperfect — it ignores within-country ethnic variation and transnational ancestry — but is the best available metadata in the FRTE dataset (Grother, 2022, §4.1).

**Threshold:** Set per algorithm to achieve nominal FMR = 0.00001 overall on the visa-border dataset (NISTIR 8429, §4).

**FNMR dataset:** False negative results computed from 1,442,511 genuine scores produced by comparing high-quality visa-like portraits with medium-quality primary inbound airport immigration photos (NISTIR 8429, §4.1).

**FMR dataset:** False positive results computed from a subset of 195 billion impostor scores comparing disjoint sets of 442,019 and 441,517 high-quality visa-like portraits (NISTIR 8429, §4.1).

**No dependencies:** All five visualizations are self-contained HTML files with no external CDN calls at runtime (Chart.js is bundled via cdnjs at build time). They work fully offline and pass GitHub Pages CSP without configuration. All support automatic dark mode via `prefers-color-scheme`.

---

## Data Sources

Grother, P. (2022). *Face Recognition Vendor Test (FRVT) Part 8: Summarizing Demographic Differentials*. NISTIR 8429. National Institute of Standards and Technology. https://doi.org/10.6028/NIST.IR.8429

National Institute of Standards and Technology. (2026). *FRTE 1:1 Verification — Demographic Differentials* [online table, updated 2025-12-11]. https://pages.nist.gov/frvt/html/frvt11.html

Buolamwini, J., & Gebru, T. (2018). Gender shades: Intersectional accuracy disparities in commercial gender classification. *Proceedings of Machine Learning Research, 81*, 1–15.

Grother, P., Ngan, M., & Hanaoka, K. (2019). *Face recognition vendor test (FRVT) part 3: Demographic effects*. NISTIR 8280. National Institute of Standards and Technology. https://doi.org/10.6028/NIST.IR.8280

Hinton, E., & Cook, D. (2021). The mass criminalization of Black Americans: A historical overview. *Annual Review of Criminology, 4*, 261–286.

Garvie, C., Bedoya, A., & Frankle, J. (2016). *The perpetual line-up: Unregulated police face recognition in America*. Georgetown Law Center on Privacy & Technology. https://www.perpetuallineup.org

---

## Files

| File | Description |
|---|---|
| `frte_false_positive_calculator.html` | Viz 1: operational false positive calculator |
| `frte_fmr_heatmap.html` | Viz 2: FMR demographic heatmap |
| `frte_bias_persistence_timeline.html` | Viz 3: algorithm submission timeline & bias persistence |
| `frte_accuracy_equity_scatter.html` | Viz 4: accuracy vs. equity scatter with Pareto frontier |
| `frte_compounding_harm_sankey.html` | Viz 5: compounding harm Sankey diagram |
| `README.md` | This file |
| `SETUP.md` | Windows Command Prompt deployment guide |

---

## Citation

```
@misc{nist_frte_viz_2026,
  title        = {NIST FRTE Facial Recognition Demographic Bias: Interactive Visualizations},
  author       = {DrMarioDeSean411-arch},
  year         = {2026},
  howpublished = {\url{https://github.com/DrMarioDeSean411-arch/nist-frte-facial-recognition-bias}},
  note         = {Five interactive visualizations based on NIST FRTE 1:1 Verification demographic differential data, 2017--2026}
}
```

---

## License

MIT License. All data visualized is from publicly available NIST government publications and peer-reviewed literature cited above.
