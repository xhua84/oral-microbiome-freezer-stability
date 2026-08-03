# Oral Microbiome Freezer Stability Study

This repository contains the end-to-end R Markdown workflow and generated manuscript outputs for an oral microbiome freezer stability analysis. The workflow evaluates buccal cell/oral wash and saliva microbiome profiles across baseline, 1 month, 12 months, and 5 years of freezer storage.

## View the report

- [Open the published HTML report](https://xhua84.github.io/oral-microbiome-freezer-stability/)
- [View the rendered HTML in this repository](manuscript_end_to_end.html)
- [View the rendered PDF](manuscript_end_to_end.pdf)

## Repository contents

All paths below are relative to the repository root.

- `manuscript_end_to_end.rmd` - source R Markdown workflow for the manuscript tables and figures.
- `analysis_sample_table_304.csv` - analysis-ready sample table with metadata, alpha diversity metrics, PCoA coordinates, genus relative abundances, and CLR values.
- `distance_*_study_235.txt` - five study-sample beta diversity distance matrices used by the PERMANOVA analysis.
- `result_adonis2.csv` - precomputed PERMANOVA / Adonis2 R2 summary.
- `result_ICC_Storage_Time_KingFisher_*.txt` - precomputed ICC and 95% CI summaries for buccal cells and saliva.
- `result_ICC_Storage_Time_long.csv` - long-format ICC output used for plotting.
- `Figure 1.png` through `Figure 4.png` - generated main manuscript figures.
- `Supplemental Figure 1.*` through `Supplemental Figure 4.png` - generated supplemental figures.
- `index.html` - GitHub Pages entry point; this is the same rendered report as `manuscript_end_to_end.html`.

## Analysis summary

The workflow uses 304 analysis samples, including 235 study samples and 69 QC samples. It summarizes read depth, alpha diversity, genus-level relative abundance, beta diversity, PERMANOVA explained variance, and intraclass correlation coefficients.

Key analyses include:

- alpha diversity metrics: observed ASVs, Shannon index, Pielou's evenness, and Faith's phylogenetic diversity
- beta diversity metrics: Bray-Curtis, Jaccard, unweighted UniFrac, generalized UniFrac, and weighted UniFrac
- PERMANOVA model: `distance ~ Source_Material + Specimen_Storage_Time + Subject_ID`
- ICC comparisons of 1 month, 12 months, and 5 years against baseline for diversity metrics and selected genera

## Reproduce the report

The default R Markdown parameters recompute the Adonis2 and ICC results and regenerate the figure files in place.

```sh
Rscript -e "rmarkdown::render('manuscript_end_to_end.rmd', output_format = 'html_document')"
Rscript -e "rmarkdown::render('manuscript_end_to_end.rmd', output_format = 'pdf_document')"
```

To render from the precomputed result files instead of recalculating Adonis2 and ICCs, run:

```sh
Rscript -e "rmarkdown::render('manuscript_end_to_end.rmd', params = list(run_icc_calculation = FALSE, run_adnois = FALSE, icc_iterations = 100L))"
```

The parameter name `run_adnois` matches the current R Markdown source.

## R requirements

The workflow uses these R packages:

```r
install.packages(c(
  "dplyr", "forcats", "ggplot2", "knitr", "lme4", "patchwork",
  "performance", "rmarkdown", "scales", "tibble", "tidyr", "vegan"
))
```

PDF rendering also requires a LaTeX installation with `xelatex`, such as TinyTeX or MiKTeX.

## Notes

- Raw sequencing reads are not included in this repository.
- Running the R Markdown workflow with default parameters rewrites generated figures and result tables in the working directory.
