# saa-pacts-gam-symposium

Talks and vignettes from the CLEAR Lab's symposium at the Society for Ambulatory
Assessment 2026, Vienna, Austria.

## PACTS scaling and cyclic GAMMs: a walkthrough

**[Read the rendered walkthrough →](https://eisenlohrmoullab.github.io/saa-pacts-gam-symposium/pacts_gamm_walkthrough.html)**
· source: [`pacts_gamm_walkthrough.Rmd`](pacts_gamm_walkthrough.Rmd)

Prepared by Tory Eisenlohr-Moul, PhD, and adapted from the larger `menstrualcycleR`
vignette, ["Getting started with `menstrualcycleR`"](https://eisenlohrmoullab.github.io/menstrualcycleR/articles/menstrualcycleR-overview.html),
written by Anisha Nagpal, PhD. Claude Code was utilized to polish the vignette and ensure
appropriate length.

It asks one question, *does positive affect track the menstrual cycle?*, and answers it
twice, problem-first:

| Step | What it does |
|---|---|
| 0 | Shows the input data: one row per person per day, with the `menses` and `ovtoday` anchors (50 simulated people, roughly 3 cycles each) |
| 1 | Fits a GAMM on forward day count, and gets a flat line at F = 0.65, p ≈ .57 |
| 2 | Rescales cycle time with `pacts_scaling()`, since day 14 is a different cycle phase in an 18- than in a 34-day cycle |
| 3 | Plots the same observations on both time axes, coloured by days to ovulation, to show where the alignment differs |
| 4 | Refits the same GAMM on PACTS cycle time, recovering a periovulatory peak at F = 18.6, p < .001, with individual `fs` smooths behind it |

The pair is the point. The people, the outcome, and the model structure are identical
across Steps 1 and 4; only the definition of cycle time changes. Misalignment does not
produce an obviously wrong answer. It produces a clean null that one would believe.

### Running it yourself

Everything is simulated inline, so there is no dataset to download and no data-access
step. One file, three packages, about 15 seconds to knit.

```r
install.packages(c("mgcv", "ggplot2"))                    # mgcv ships with base R already
remotes::install_github("eisenlohrmoullab/menstrualcycleR")
rmarkdown::render("pacts_gamm_walkthrough.Rmd")
```

If you are following along live, install `menstrualcycleR` **before** the session.
That step needs the network, and conference wifi is where these things go wrong.

## Citing

PACTS is implemented in [`menstrualcycleR`](https://github.com/eisenlohrmoullab/menstrualcycleR)
([docs](https://eisenlohrmoullab.github.io/menstrualcycleR/)). The method paper, which
`citation("menstrualcycleR")` also returns, is:

> Nagpal, A., Schmalenberger, K. M., Barone, J. C., Mulligan, E., Stumper, A., Knol, L.,
> Failenschmid, J., Kiesner, J., Peters, J. R., & Eisenlohr-Moul, T. A. (2025). Studying the
> menstrual cycle as a continuous variable: Implementing phase-aligned cycle time scaling
> (PACTS) with the `menstrualcycleR` package. *Psychoneuroendocrinology*, *181*, 107584.
> https://doi.org/10.1016/j.psyneuen.2025.107584

The models are fitted with `mgcv` (Wood, 2017), which should be cited alongside it.
