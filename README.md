# ER Stress and UPR-Related Transcript Expression in Prostate Cancer

An exploratory, **probe-level analysis** of publicly available prostate tissue microarray data using Python, pandas, and Matplotlib.

## Research question

How do transcript signals associated with the unfolded protein response (UPR) differ between prostate tumor and benign tissue samples? How much confidence can we place in individual microarray probes, particularly when several probes are annotated to the same gene?

The analysis focuses on six UPR-related genes: **ERN1** (IRE1α), **EIF2AK3** (PERK), **ATF6**, **HSPA5** (BiP/GRP78), **ATF4**, and **DDIT3** (CHOP).

## Dataset

- **Source:** [NCBI GEO GSE32571](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE32571) — *Gene signatures in prostate cancer linked to the Gleason score*.
- **Samples:** 59 prostate tumor and 39 benign tissue samples (98 total).
- **Platform:** [GPL6947](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GPL6947), Illumina HumanHT-12 V3.0 expression beadchip.
- **Measurements:** 48,652 microarray probes with processed, quantile-normalized **log₂ signal intensities**. These are **not RNA-seq counts**.

The notebook downloads expression data, sample metadata, probe annotations, and per-sample detection information directly from GEO.

## Methods

1. Load and inspect the expression matrix with **pandas**.
2. Identify tumor and benign samples from GEO metadata and visualize sample composition with **Matplotlib**.
3. Map Illumina probe IDs to gene symbols using the original platform annotation.
4. Compare median probe-level signals and visualize their distributions and descriptive group differences.
5. Examine **inter-probe Pearson correlations** for genes represented by multiple probes.
6. Retrieve Illumina **detection p-values** and calculate the percentage of samples in which each probe's signal is detected above background (threshold: p < 0.05).
7. Inspect the available sample metadata for information needed to reconstruct matched patient samples.

## Main observations

- **ATF4:** All three annotated probes have higher median signals in tumor samples. Two A-type probes correlate reasonably well (**r = 0.736**) and are detected in more than 90% of samples in each group. The I-type probe correlates weakly with those probes and has less consistent detection (**69.2% benign; 81.4% tumor**). These findings warrant probe-level investigation, not an isoform-specific biological claim.
- **ERN1:** Its two probes show weak negative correlation (**r = −0.215**). One is detected in every sample, while the other is detected in only **33.3% of benign** and **27.1% of tumor** samples. Their signals are therefore not averaged.
- **Other UPR genes:** ATF6, DDIT3, and HSPA5 probes are detected in all samples; the EIF2AK3 probe is detected in approximately 80% of samples in each group. Descriptive tumor–benign differences are displayed in the notebook.

**These are exploratory observations, not statistically validated differential-expression findings.**

## Interpretation and limitations

- The original study includes benign and tumor tissues from some of the same patients. The sample metadata examined here do not provide a reliable patient-pairing map; consequently, **no paired significance test is reported**.
- The historical GPL6947 probe annotation may require updated transcript mapping and cross-hybridization checks.
- Detection p-values assess whether an array signal is distinguishable from background; they are **not p-values for tumor-versus-benign differences**.
- Transcript signal intensity does not directly establish activation of IRE1α, PERK, ATF6, or the UPR pathway. Protein-level and functional measurements would be needed for such conclusions.

Potential next steps include resolving patient matching, validating probe sequences against current transcript references, and selecting a statistically appropriate comparison model.

## Reproduce the analysis

Open [`01_GSE32571_UPR_analysis.ipynb`](01_GSE32571_UPR_analysis.ipynb) in Jupyter or Google Colab and run the cells in order.

**Requirements:** Python 3, `pandas`, `matplotlib`, and an internet connection to download public GEO files. No separate local dataset is needed.

## Data attribution

The dataset and platform annotations belong to their original contributors and are available through [NCBI GEO](https://www.ncbi.nlm.nih.gov/geo/). Illumina probe-type and detection conventions are described in the [GenomeStudio Gene Expression Module documentation](https://support.illumina.com/content/dam/illumina-support/documents/documentation/software_documentation/genomestudio/genomestudio-2011-1/genomestudio-gx-module-v1-0-user-guide-11319121-a.pdf).
