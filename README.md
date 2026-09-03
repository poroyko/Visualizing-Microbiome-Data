# Microbiome Visualization Tutorial

A self-contained Jupyter notebook tutorial on visualizing microbiome
abundance data in Python — from single-taxon boxplots to community-wide
ordination, temporal dynamics, and co-occurrence networks. Every
visualization is implemented as a documented, reusable function, applied to
a reproducible synthetic dataset, and paired with a discussion of what the
plot shows, the biological question it answers, and its limitations.

The tutorial follows a minimal-ink, black-and-white design philosophy
throughout: groups are told apart using marker shape, fill, and line style
rather than color wherever possible, so every figure remains legible after
grayscale printing or photocopying.

## Table of Contents

- [Why microbiome data is hard to visualize](#why-microbiome-data-is-hard-to-visualize)
- [What's included](#whats-included)
- [The ten visualizations](#the-ten-visualizations)
- [Repository structure](#repository-structure)
- [Getting started](#getting-started)
- [The mock dataset](#the-mock-dataset)
- [Design philosophy](#design-philosophy)
- [License](#license)

## Why microbiome data is hard to visualize

Microbiome abundance data combine several statistical and visual
challenges that make off-the-shelf plotting approaches frequently
inadequate. This tutorial's introduction addresses each of these directly,
and every subsequent section revisits the ones relevant to that
visualization:

- **Compositionality.** Sequencing-based abundance data are almost always
  *relative*, summing to a fixed total per sample. An increase in one
  taxon's proportion mechanically forces a decrease in others, even absent
  any real biological change in those other taxa — a constraint that plain
  correlation, PCA, and Euclidean distance all implicitly violate if
  applied naively.
- **Sparsity and high dimensionality.** Real datasets often measure
  hundreds to thousands of taxa, most absent or near-zero in most samples.
  Taxon-by-taxon visualizations stop scaling quickly, motivating
  dimensionality reduction (ordination) and aggregate summaries (heatmaps,
  networks).
- **Relative vs. absolute abundance.** Without a spike-in control or
  absolute quantification, "abundance" means proportion of the community,
  not organism count. A taxon can appear to decline in a plot purely
  because other taxa increased.
- **Between-sample and within-sample variation.** Composition varies
  enormously between subjects, but also fluctuates within a subject from
  replicate to replicate. Group means without any indication of spread can
  be seriously misleading.
- **Categorical vs. longitudinal designs.** Static comparisons (does
  composition differ between groups?) and longitudinal questions (how does
  composition change over time within subjects?) call for different chart
  types, and this tutorial covers both.
- **Univariate vs. multivariate visualization.** Some questions concern one
  taxon at a time; others concern the whole community simultaneously. A
  thorough analysis needs both views, not just one.
- **Statistical inference alongside visualization.** A visual difference
  is not evidence of a real effect without an appropriate test; a p-value
  disconnected from effect size can just as easily mislead. This tutorial
  treats visualization and inference as inseparable throughout.

## What's included

One notebook, **`microbiome_visualization_tutorial.ipynb`**, structured as
a continuous, pedagogical walkthrough:

**Concept → biological question → statistical considerations → Python
implementation → visualization → interpretation**, repeated for each of
the ten visualization techniques below, applied to one coherent synthetic
dataset (a stable *Control* arm and a dynamic *Treatment* arm, sampled
repeatedly over eight weeks).

## The ten visualizations

| # | Visualization | Biological question | Section |
|---|---|---|---|
| 1 | **Boxplot** | Does *this specific taxon* differ between groups? | 3.1 |
| 2 | **Boxplot + statistical inference** (Mann-Whitney U, Benjamini-Hochberg FDR) | Is that difference statistically robust once corrected for multiple testing? | 3.2 |
| 3 | **Static PCoA** (Bray-Curtis ordination) | Does *overall community composition* differ between groups? | 3.3 |
| 4 | **Stacked bar chart** | What does the whole community look like, taxon by taxon, at each sampling occasion? | 5 |
| 5 | **Alluvial (stream) plot** | How does composition flow and shift continuously over time? | 6 |
| 6 | **Longitudinal line plot with error bars** | How does one taxon change over time, and how much do individual subjects vary? | 7 |
| 7 | **Temporal PCoA with group trajectories** | How does the whole community move through compositional space over time? | 8 |
| 8 | **Heatmap** (log-scaled, optionally clustered) | What are the abundance patterns across many taxa and time points at once? | 9 |
| 9 | **Volcano plot** | Which taxa show a large *and* statistically robust change between two conditions? | 10 |
| 10 | **Co-occurrence network** | Which taxa rise and fall together, or in opposition? | 11 |

Each technique is implemented as one or more standalone functions with
full docstrings (parameters, return values, and the reasoning behind key
design choices), so they can be lifted directly into your own analysis
alongside the tutorial content.

## Repository structure

```
microbiome-visualization-tutorial/
├── README.md
└── microbiome_visualization_tutorial.ipynb
```

## Getting started

### Requirements

The notebook depends only on standard, widely available packages —
deliberately avoiding niche dependencies so it runs in most existing
Python environments without extra setup:

```
numpy
pandas
matplotlib
seaborn
scipy
networkx
```

Benjamini-Hochberg multiple-testing correction is implemented directly in
the notebook (rather than imported from `statsmodels`), so no additional
statistics package is required.

### Installation

```bash
git clone https://github.com/<your-username>/microbiome-visualization-tutorial.git
cd microbiome-visualization-tutorial
pip install numpy pandas matplotlib seaborn scipy networkx
jupyter notebook microbiome_visualization_tutorial.ipynb
```

### Running

Run all cells from top to bottom in a fresh kernel — the notebook is
fully self-contained and generates its own dataset with a fixed random
seed (`RANDOM_SEED = 42`), so every table and figure is exactly
reproducible.

## The mock dataset

Rather than requiring a real (and typically restricted or large)
microbiome dataset, the tutorial simulates one: ten bacterial taxa
(`A`–`J`), two arms — a **Control** arm whose composition fluctuates only
due to noise, and a **Treatment** arm in which five taxa steadily decline
while five others proportionally expand — each with eight subjects sampled
repeatedly across eight weeks. This gives every visualization something
real to show: a static between-group difference (Section 3), a genuine
group-by-time interaction (Section 4 onward), and enough subjects and taxa
to make significance testing and multiple-testing correction meaningful.

## Design philosophy

Every figure follows Edward Tufte's **data-ink principle**: maximize the
share of ink that represents data, minimize the rest. In practice, this
means:

- Groups are distinguished by **marker shape, fill, and line style**
  instead of color wherever two or a few groups need to be told apart.
- **Chart junk is minimized** — background gridlines, redundant legends,
  and unnecessary borders are removed by default; where a legend would
  only exist to decode a category, data is labeled directly instead.
- Where full color removal genuinely isn't practical — heatmaps, and
  stacked bar/alluvial charts with ten categories — the notebook says so
  explicitly rather than pretending otherwise, and uses a grayscale ramp
  as the least-ink alternative that still works.

## License

MIT Copyright (c) 2026 Valeriy Poroyko
