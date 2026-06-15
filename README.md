# NIST FRTE Facial Recognition Demographic Bias

An interactive calculator visualizing demographic disparities in facial recognition false match rates, based on empirically measured data from NIST's Face Recognition Technology Evaluation (FRTE) 1:1 Verification track.

---

## What This Is

Facial recognition algorithms produce two types of errors: false matches (the system incorrectly links two photos of different people) and false non-matches (the system fails to recognize the same person across two photos). NIST's ongoing FRTE evaluations document that these error rates are not distributed equally across demographic groups.

This repository provides an operational false positive calculator that translates per-comparison false match rates (FMR) into expected false positive counts for a database of N people, using the binomial approximation:

```
FPIR ≈ N × FMR
```

(Grother, NISTIR 8429, Annex B, eq. 32)

Users can adjust database size and select a demographic group to see how the same algorithm produces radically different expected false match counts depending on who is being searched.

---

## Key Findings Visualized

| Algorithm | E. European male (35–50) FMR | W. African female (65–99) FMR | Fold difference |
|---|---|---|---|
| qazsmartvisionai_003 | 0.00006 | 0.02150 | 358× |
| recognito_001 | 0.00001 | 0.01878 | 1,878× |
| innovatrics_014 | 0.00004 | 0.01104 | 276× |

At a database of 10,000 people:
- An Eastern European male (35–50) searched with recognito_001 produces an expected **0 false matches**
- A West African female (65+) searched with the same algorithm against the same database produces an expected **188 false matches**

These disparities are not anomalies. Across 95%+ of algorithms evaluated in the past two years, Eastern European males (ages 20–50) consistently appear in the best-performance categories and West African females (65–99) consistently appear in the worst — across independent developers, different architectural approaches, and different training pipelines (NIST, 2026).

---

## Live Dashboard

[**Launch Calculator →**](https://DrMarioDeSean411-arch.github.io/nist-frte-facial-recognition-bias/frte_false_positive_calculator.html)

---

## Files

| File | Description |
|---|---|
| `frte_false_positive_calculator.html` | Self-contained interactive calculator (no dependencies) |
| `README.md` | This file |
| `SETUP.md` | Windows Command Prompt deployment guide |

---

## Methodological Notes

**Approximation used:** The simplified binomial approximation FPIR ≈ N × FMR is appropriate for illustration. Actual 1:N false positive identification rates depend additionally on cross-demographic FMR, database composition, and search volume (see NISTIR 8429 Annex B, eqs. 33–37). For 1:N algorithms that stabilize the tail of the impostor distribution, FPIR may not scale linearly with N.

**Threshold:** Set per algorithm to achieve a nominal FMR of 0.00001 overall on the visa-border dataset.

**Race proxy:** NIST uses country of birth as a proxy for race. This proxy is imperfect — it ignores within-country ethnic variation and transnational ancestry — but is the best available metadata in the FRTE dataset (Grother, 2022, §4.1).

**FMR values for specific demographic subgroups** were extracted from NIST FRTE published results (table updated 2025-12-11). Mid-range estimates were used for demographic groups where exact published values were not available at the intersection of age group, sex, and region.

---

## Data Sources

Grother, P. (2022). *Face Recognition Vendor Test (FRVT) Part 8: Summarizing Demographic Differentials*. NISTIR 8429. National Institute of Standards and Technology. https://doi.org/10.6028/NIST.IR.8429

National Institute of Standards and Technology. (2026). *FRTE 1:1 Verification — Demographic Differentials* [online table, updated 2025-12-11]. https://pages.nist.gov/frvt/html/frvt11.html

Buolamwini, J., & Gebru, T. (2018). Gender shades: Intersectional accuracy disparities in commercial gender classification. *Proceedings of Machine Learning Research, 81*, 1–15.

Grother, P., Ngan, M., & Hanaoka, K. (2019). *Face recognition vendor test (FRVT) part 3: Demographic effects*. NISTIR 8280. National Institute of Standards and Technology. https://doi.org/10.6028/NIST.IR.8280

---

## Related Work

This calculator was developed in conjunction with Chapter 3 ("Facial Recognition as Racial Recognition") of an ongoing scholarly manuscript examining surveillance technology and racial bias. The chapter documents the NIST demographic disparity data in the context of deployment patterns, training data feedback loops, and the constitutional framing of algorithmic objectivity.

---

## Citation

```
@misc{nist_frte_calculator_2026,
  title        = {NIST FRTE Facial Recognition Demographic Bias: Operational False Positive Calculator},
  author       = {DrMarioDeSean411-arch},
  year         = {2026},
  howpublished = {\url{https://github.com/DrMarioDeSean411-arch/nist-frte-facial-recognition-bias}},
  note         = {Interactive visualization based on NIST FRTE 1:1 Verification demographic differential data}
}
```

---

## License

MIT License. Data visualized is from publicly available NIST government publications.
