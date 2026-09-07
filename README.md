# Searching for the Core of Conspiracy Belief Systems: The Relationship Between Specific Beliefs and Generic Mentality

Replication materials for the paper "Searching for the Core of Conspiracy Belief Systems: The Relationship Between Specific Beliefs and Generic Mentality"
(*European Journal of Social Psychology*; Arturo Bertero, Matt Williams, Federico Vegetti and Moreno Mancosu).

The paper asks whether conspiracy mentality and specific conspiracy beliefs are empirically
distinguishable (H0), and whether the association structure is better represented with
conspiracy mentality accounting for the covariance among specific beliefs (H1) or the reverse
(H2), using psychological network models across three datasets, 17 country samples and 29,938
respondents.

This repository reproduces every number, table and figure in the article and the Supplement.

---

## Quick start

```r
# 1. Clone the repository and open Conspiracy_Belief_Systems.Rproj in RStudio.
#    All paths use here::here(), so nothing needs to be edited.

# 2. Install the packages (the scripts do this themselves via pacman, but you can do it up front)
install.packages("pacman")
pacman::p_load(tidyverse, here, psych, lavaan, EGAnet, qgraph, mgm, bootnet, huge,
               sjPlot, patchwork, reshape2, magick, haven, janitor, rio, conflicted,
               doParallel, foreach, knitr, summarytools, visdat, stargazer, labelled,
               readxl, openxlsx, cocron, lme4, broom.mixed, kableExtra, semPlot,
               countrycode)

# 3. Knit the scripts in Processing/ in numerical order (1, 2, 3, 4, 5, 6, 7, 8).
```

Scripts 2 to 7 all start from `Input/Secondary/master_datasets.rds`, which is included,
so they run out of the box and can be run individually. Script 1 is the only one that touches
the raw Study 2 and Study 3 files, which are not redistributed here. See
**[Data sources](#data-sources)** for how to obtain them.

---

## Repository structure

```
Input/                       data and the intermediate objects built from it
  SUSPECTS.rds                 Study 1: SUSPECTS comparative survey, 8 countries
  CCRS/                        Study 3: NOT INCLUDED, see "Data sources" below
    CCRS.csv                     obtain from Harvard Dataverse
  Enders/                      Study 2: NOT INCLUDED, see "Data sources" below
    Qualtrics 2021.dta           obtain from Harvard Dataverse
  Secondary/                   objects produced by the scripts (see "Long-running steps")
    master_datasets.rds          the three analysis datasets, harmonised
    boot_H1_master.rds           1,000 non-parametric bootstraps, H1 networks
    boot_H2_master.rds           1,000 non-parametric bootstraps, H2 networks
    boot_case_master.rds         1,000 case-dropping bootstraps, all networks
    boot_alt_master.rds          the same H1/H2 bootstraps under two alternative tuning settings
    monte_carlo_results.rds      1,000 equal-item Monte Carlo iterations per country
    figure1_layouts.rds          frozen node coordinates for Figure 1; delete to redraw

Processing/                  the analysis, in the order it should be run
  1_Data_Managment.Rmd    harmonises the three datasets
  2_Networks_H0.Rmd       H0: EGA, CFA, reliability, item-wording tables
  3_Networks_H1.Rmd       H1 networks and bootstraps
  4_Networks_H2.Rmd       H2 networks, bootstraps, and the article figures
  5_Robustness.Rmd        equal-item Monte Carlo, case-dropping bootstraps
  6_Estimator_Robustness.Rmd  repeats H1 and H2 under two alternative tuning settings
  7_Reporting_Checks.Rmd  re-derives every number reported in the paper
  8_Export_Submission.Rmd     writes Output/Submission, every file under its article name

Output/                      everything the scripts produce
  H0/ H1/ H2/                  country networks and composite figures per hypothesis
  Robustness/                  centrality profiles, stability panels, Monte Carlo
  Wording/                     item-wording tables
  Difference_Tests/            Table S9, the bootstrapped difference tests on Strength
  Estimator_Robustness/        Table S10, the same tests under two alternative tuning settings
  Reporting_Checks/            verification tables (see below)
  Figures/                     the four article figures, in two formats each
  Submission/                  every figure and table under the name the article gives it
```

---

## Where each figure and table comes from

### Article

All four are written by script 4, into `Output/Figures/`, in two formats:

| In the paper | Reading copy (PNG, 600 dpi) | Production copy (vector PDF) |
|---|---|---|
| Figure 1 | `Figure 1 - conspiracy belief systems in the U.S.png` | `Figure_1.pdf` |
| Figure 2 | `Figure 2 - Study 1 results.png` | `Figure_2.pdf` |
| Figure 3 | `Figure 3 - Study 2 results.png` | `Figure_3.pdf` |
| Figure 4 | `Figure 4 - Study 3 results.png` | `Figure_4.pdf` |

The PNGs are named after the captions; the PDFs follow the publisher's rule for production
artwork. Both use Wiley's 180 mm full-width canvas, so nothing is rescaled at typesetting and
the point sizes in the code are the ones that print. Figure 1 is 180 x 240 mm, the rest
180 x 220 mm.

### Supplement

| In the Supplement | File | Produced by |
|---|---|---|
| Table S1 | `Output/Wording/Table_S1_Wording_Study1.doc` | script 2 |
| Figures S1, S2 | `Output/H0/Composite_Figures/H0_Study1_Composite_A.png`, `_B.png` | script 2 |
| Table S2 | built from `Output/H0/Alpha_Reliability_Master.csv`, `CFA_Fit_Stats_Master.csv`, `Alpha_RMSEA_CIs.csv` | script 8 |
| Figures S3, S4 | `Output/H1/Composite_Figures/H1_Study1_Composite_A.png`, `_B.png` | script 3 |
| Figures S5, S6 | `Output/H2/Composite_Figures/H2_Study1_Composite_A.png`, `_B.png` | script 4 |
| Figure S7 | `Output/Robustness/Centrality_Profiles_Study1_SUSPECTS.png` | script 5 |
| Figures S8, S9 | `Output/Robustness/Stability/Panel_Study1_SUSPECTS_H1.png`, `_H2.png` | script 5 |
| Table S3 | `Output/Wording/Table_S3_Wording_Study2.doc` | script 2 |
| Figure S10 | `Output/H0/Composite_Figures/H0_Study2_Composite.png` | script 2 |
| Table S4 | `Output/H0/Robustness/Study2_Dimensionality_Check.doc` | script 2 |
| Table S5 | built from the same three `.csv` files, Study 2 rows | script 8 |
| Figure S11 | `Output/H1/Composite_Figures/H1_Study2_Composite.png` | script 3 |
| Figure S12 | `Output/H2/Composite_Figures/H2_Study2_Composite.png` | script 4 |
| Figure S13 | `Output/Robustness/Centrality_Profiles_Study2_ACTS.png` | script 5 |
| Figure S14 | `Output/Robustness/Stability/Panel_Study2_ACTS_All_Hypotheses.png` | script 5 |
| Table S6 | `Output/Wording/Table_S6_Wording_Study3.doc` (both panels) | script 2 |
| Figures S15, S16 | `Output/H0/Composite_Figures/H0_Study3_CMQ_Composite_A.png`, `_B.png` | script 2 |
| Table S7 | `Output/H0/Robustness/Table_S7_Canada_US_Comparison.doc` | script 2 |
| Table S8 | built from the same three `.csv` files, Study 3 rows | script 8 |
| Figures S17, S18 | `Output/H1/Composite_Figures/H1_Study3_CMQ_Composite_A.png`, `_B.png` | script 3 |
| Figures S19, S20 | `Output/H2/Composite_Figures/H2_Study3_CMQ_Composite_A.png`, `_B.png` | script 4 |
| Figure S21 | `Output/Robustness/Centrality_Profiles_Study3_CMQ.png` | script 5 |
| Figures S22, S23 | `Output/Robustness/Stability/Panel_Study3_CMQ_H1.png`, `_H2.png` | script 5 |
| Figure S24 | `Output/Robustness/MC/MC_Final_Hub_Probability.png` | script 5 |
| Figure S25 | `Output/Robustness/MC/MC_Final_Delta_Mean_Violin.png` | script 5 |
| Table S9 | `Output/Difference_Tests/Table_S9_Difference_Tests.doc` | script 7 |
| Table S10 | `Output/Estimator_Robustness/Table_S10_Estimator_Robustness.doc` | script 6 |

Tables S2, S5 and S8 are assembled by script 8 from three CSV files written by script 2:
`Alpha_RMSEA_CIs.csv` for the interval estimates, `Alpha_Reliability_Master.csv` for the item
counts and alphas, and `CFA_Fit_Stats_Master.csv` for the fit statistics.

---

## Checking the numbers in the text

`Processing/7_Reporting_Checks.Rmd` re-derives every quantity stated in the Results rather
than reading it off a figure. It writes five tables to `Output/Reporting_Checks/`, plus
Table S9 of the Supplement to `Output/Difference_Tests/`:

- `Check_Sample_Sizes.csv`: the analytic N per country and per study, after listwise deletion.
- `Check_EGA_Communities.csv`: the number of communities and the full item partition returned
  by EGA for each of the 17 country networks.
- `Check_Alpha_Fit_CIs.csv`: Cronbach's alpha with Feldt 95% intervals, and the two-factor CFA
  fit with the 90% RMSEA interval.
- `Check_Summary_Node_Ranks.csv`: for each of the 34 H1 and H2 country networks: the Strength
  rank of the summary node, whether its 95% bootstrap interval lies entirely above or below the
  intervals of all other nodes, and the same rank re-estimated with the alternative tuning
  criterion.
- `Check_Difference_Tests.csv`: the decision rule the paper applies to H1 and H2: bootstrapped
  difference tests on Strength between the summary node and every other node, in each of the 34
  networks, with the resulting verdict (supported / falsified / inconclusive).

On the decision rule: comparing point estimates is not a test, and comparing two marginal
bootstrap intervals is not one either, since two nodes can differ reliably while their intervals
overlap. Following Epskamp, Borsboom and Fried (2018), the difference is bootstrapped directly:
within each of the 1,000 bootstrap samples we take the difference in Strength between the summary
node and each other node, and read the 2.5th and 97.5th percentiles of that difference. A
hypothesis is supported only where the summary node is reliably more central than every other
node, and falsified where it is reliably less central than every other node.

The last file is a quick check on the tuning criterion. Every network in this repository,
drawn or analysed, is estimated with `mgm()` under EBIC (`lambdaGam = 0.25`) and the AND rule,
which are the settings `bootnet(default = "mgm")` applies internally. `Same_rank` compares the
summary-node rank against a re-estimation under cross-validation and the OR rule, and it is
`TRUE` in all 34 networks. Script 6 carries the full version of the same check, with
bootstraps and difference tests under two alternative specifications.

---

## The estimator

Every network in the paper is estimated the same way.

```r
mgm(data, type = rep("g", k), level = rep(1, k),
    lambdaSel = "EBIC", lambdaGam = 0.25, ruleReg = "AND")
```

These are the settings `bootnet(default = "mgm")` applies internally, so the networks in
Figure 1 and in Figures S3 to S6, S11, S12 and S17 to S20 are the networks on which the
Strength estimates, the bootstrap intervals and the difference tests are computed.

The nonparanormal transformation (`huge::huge.npn`) is applied before every H1 and H2
estimation, which is what Section 4.4 of the article describes. It is **not** applied to the
17 H0 networks in script 2: those figures are illustrative, and the H0 conclusion rests on
EGA and CFA rather than on them. Figure 1's H0 panel is built in script 4 and does use the
transformation, so it differs slightly from the same country's panel in Figures S1 and S2.

Edges are drawn
from `$pairwise$wadj` with `$pairwise$edgecolor_cb`, which is the colourblind-safe palette and
carries the sign of each edge.

The `mgm` package selects the tuning parameter by cross-validation instead. Script 6 repeats
the whole H1 and H2 analysis under cross-validation with both the AND and the OR rule, and
writes Table S10.

One check departs from this on purpose. The equal-item Monte Carlo in script 5 keeps the EBIC
tuning and switches to the OR rule, because those networks are built on half the items of the
H0 networks, and the looser rule retains weak edges that the AND rule discards. That matters
when items are ranked by summed edge weight. The two quantities it records, a hub identity and
a difference in mean Strength, do not enter any hypothesis test.

---

## The submission bundle

Script 8 writes `Output/Submission/`, in which every file carries the name the article or the
Supplement gives it: `Figure 1.png`, `Figure 1.pdf`, `Figure S1.png` through `Figure S25.png`,
and `Table S1.doc` through `Table S10.doc`. Nothing is recomputed there; the folder is a
renamed view of the outputs listed above, and the script fails loudly if any of them is
missing. `Output/Submission/_contents.txt` lists what was written.

---

## Long-running steps

Five steps are expensive, so their results are saved in `Input/Secondary/` and loaded by
default:

| Step | Where | Status in the script |
|---|---|---|
| H1 bootstraps (1,000 per country) | script 3 | commented out; `boot_H1_master.rds` is loaded instead |
| H2 bootstraps (1,000 per country) | script 4 | commented out; `boot_H2_master.rds` is loaded instead |
| case-dropping bootstraps | script 5 | commented out; `boot_case_master.rds` is loaded instead |
| H1/H2 bootstraps under two alternative tuning settings | script 6 | `RUN_BOOTSTRAPS <- FALSE`; `boot_alt_master.rds` is loaded instead |
| equal-item Monte Carlo (1,000 iterations x 17 datasets) | script 5 | **runs by default** and takes hours |

Uncomment the relevant block, or set `RUN_BOOTSTRAPS <- TRUE` in script 6, to re-run any of
them. Lower `nCores = 16` and `makeCluster(12)` to match your machine. Seeding is only partial, and
it is worth being precise about where. `set.seed(123)` fixes the deterministic steps. It does
not reach the parallel workers: neither `bootnet`'s cluster nor the `foreach` loop behind the
equal-item Monte Carlo calls `clusterSetRNGStream`, so bootstrap values and Monte Carlo
iterations differ in the last decimals from run to run and from machine to machine. No
reported ranking or verdict depends on that, but the Monte Carlo block also overwrites its own
`.rds` on every run, so delete or comment it if you want the shipped Figures S24 and S25 to
stay exactly as published. The network figures are reproducible: EBIC selection is deterministic, and
the one placement that is not, the spring layout of Figure 1, is frozen in
`Input/Secondary/figure1_layouts.rds`.

---

## Data sources

Study 1 is included here; **Studies 2 and 3 are not**, because they belong to other research
teams who distribute them themselves. Both are free to obtain from the Harvard Dataverse, and
the steps below produce exactly the two files `Processing/1_Data_Managment.Rmd` expects.

### Study 1: SUSPECTS (included)

The SUSPECTS comparative survey (Mancosu et al.). `Input/SUSPECTS.rds` holds only the variables
used here (country, the eight COMP-CB items, the five GCB-5 items and the two composite
scores), cleaned and with no missing values.

### Study 2: Enders et al., Qualtrics May 2021 survey (download required)

Enders, A., Farhart, C., Miller, J., Uscinski, J., Saunders, K., & Drochon, H. (2023). Are
Republicans and conservatives more likely to believe conspiracy theories? *Political Behavior*,
45(4), 2001-2024. https://doi.org/10.1007/s11109-022-09812-3

The archive is public and licensed CC0 1.0, so no permission request is needed.

1. Go to https://doi.org/10.7910/DVN/MMMYGJ (Harvard Dataverse).
2. In the **Files** tab, find `Qualtrics 2021`.
3. Under **Access File**, choose **Original File Format (Stata Binary)**. The `.tab` and the
   CSV export drop the value labels that `haven::read_dta()` expects.
4. Save it as `Input/Enders/Qualtrics 2021.dta`.

Script 1 uses ten variables, documented in the archive's `README.pdf`: `con1`-`con4` (the ACTS
conspiracy-mentality items) and the six specific-belief items on the 1-5 scale (`epstein`,
`lightbulbs`, `fda`, `soros`, `billgates`, `cellphone`). The other eleven specific-belief items
use a different response scale and are excluded, as explained in the paper.

### Study 3: Comparative Conspiracy Research Survey (download required)

Bordeleau, J.-N., Stockemer, D., Amengay, A., & Shamaileh, A. (2025). The Comparative Conspiracy
Research Survey (CCRS): A new cross-national dataset for the study of conspiracy beliefs.
*European Political Science*, 24(1), 1-11. https://doi.org/10.1057/s41304-023-00463-4

Licensed CC BY 4.0, but the depositors have **file access requests enabled**, so you may have
to click *Request Access* and wait for approval first. This is normally quick.

1. Go to https://doi.org/10.7910/DVN/VRRY9B (Harvard Dataverse).
2. Request access if prompted, then download `CCRS Dataset` as **Comma Separated Values**.
3. Save it as `Input/CCRS/CCRS.csv`.

Script 1 applies `janitor::clean_names()`, so the columns it uses (`id_q2`, `id_q33`,
`id_q33_1`-`id_q33_5`, `id_q36_1`-`id_q36_4`) are the snake_case forms of the Dataverse names.
`CCRS Codebook.pdf` and `CCRS Questionnaire.pdf` in the same archive document them.

### What is included instead

`Input/Secondary/master_datasets.rds`, the output of script 1: the three harmonised datasets
restricted to the items actually modelled (country, the mentality items, the specific-belief
items and the two composite indices). It is included so that every analysis, figure and table in
the paper and the Supplement reproduces without any download. Re-running script 1 after obtaining
the two raw files rebuilds it identically.

---

## Software

Analyses were run under R 4.4.2 with:

```
tidyverse  2.0.0     lavaan   0.6.20    mgm      1.2.14    sjPlot     2.8.16
here       1.0.1     EGAnet   2.2.1     bootnet  1.6       patchwork  1.3.2
psych      2.5.3     qgraph   1.9.8     huge     1.3.5     knitr      1.50
```

---

## Citation

Please cite the paper. A `CITATION.cff` will be added once the DOI is assigned.

## License

Code in `Processing/` is MIT (see `LICENSE`). The data in `Input/` remain governed by their
original sources: CC0 1.0 for Study 2, CC BY 4.0 for Study 3.

## Contact

Arturo Bertero, arturo.bertero@unimi.it
