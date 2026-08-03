# SAA 2026 — PACTS scaling and cyclic GAMMs

Hands-on walkthrough from the CLEAR Lab's symposium at the Society for Ambulatory
Assessment, Vienna. It asks one question, *does positive affect track the menstrual
cycle?*, and answers it twice on two different definitions of cycle time.

**Only want to read it?** → **[the walkthrough, figures and all](pacts_gamm_walkthrough.md)**,
or the [styled version](https://eisenlohrmoullab.github.io/saa-pacts-gam-symposium/pacts_gamm_walkthrough.html).
Nothing to install.

## Start here

**1. Install — please do this before the session.** It is the only step that needs the
network, and conference wifi is where it goes wrong. In the R Console:

```r
install.packages("remotes")
remotes::install_github("eisenlohrmoullab/menstrualcycleR")
```

That is everything: `mgcv` ships with R and `ggplot2` comes along as a dependency. On a
fresh R installation this pulls around 60 packages and takes minutes rather than seconds.
If it offers to update other packages, answering "none" is fine. No compiler needed.

Confirm it worked with `library(menstrualcycleR)`.

**2. Get the file.** This drops it in your working directory rather than in Downloads:

```r
download.file(
  "https://raw.githubusercontent.com/eisenlohrmoullab/saa-pacts-gam-symposium/main/pacts_gamm_walkthrough.Rmd",
  "pacts_gamm_walkthrough.Rmd"
)
```

Or open [`pacts_gamm_walkthrough.Rmd`](pacts_gamm_walkthrough.Rmd) above and use the
**download raw file** button.

**3. Run it.** Open it in RStudio and work down the document with the green ▶ at the top
right of each code chunk, which is what we will do together. **Knit** runs the whole thing
in about 15 seconds.

There is no dataset to fetch: the document simulates its own diary, so nothing can fail
for want of a file.

## What it does

| Step | |
|---|---|
| 0 | The input data: one row per person per day, with the `menses` and `ovtoday` anchors (50 simulated people, ~3 cycles each) |
| 1 | Fits a GAMM on forward day count, and gets a flat line at F = 0.65, p ≈ .57 |
| 2 | Rescales cycle time with `pacts_scaling()`, since day 14 is a different cycle phase in an 18- than in a 34-day cycle |
| 3 | Plots the same observations on both time axes, coloured by days to ovulation |
| 4 | Refits the same GAMM on PACTS cycle time, recovering a periovulatory peak at F = 18.6, p < .001, with individual `fs` smooths behind it |

The pair is the point. The people, the outcome, and the model structure are identical
across Steps 1 and 4; only the definition of cycle time changes. Misalignment does not
produce an obviously wrong answer. It produces a clean null that one would believe.

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

Prepared by Tory Eisenlohr-Moul, PhD, and adapted from the larger `menstrualcycleR`
vignette, ["Getting started with `menstrualcycleR`"](https://eisenlohrmoullab.github.io/menstrualcycleR/articles/menstrualcycleR-overview.html),
written by Anisha Nagpal, PhD. Claude Code was utilized to polish the vignette and ensure
appropriate length.
