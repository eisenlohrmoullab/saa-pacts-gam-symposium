# saa-pacts-gam-symposium

Talks and vignettes from the CLEAR Lab's symposium at the Society for Ambulatory
Assessment 2026, Vienna, Austria.

## PACTS scaling and cyclic GAMMs: a walkthrough

**[Read it here, figures and all →](pacts_gamm_walkthrough.md)** — renders right in GitHub,
nothing to install.

Also available as a [styled web page](https://eisenlohrmoullab.github.io/saa-pacts-gam-symposium/pacts_gamm_walkthrough.html)
(collapsible code blocks), and as the runnable source,
[`pacts_gamm_walkthrough.Rmd`](pacts_gamm_walkthrough.Rmd).

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

### The three copies

| File | What it is |
|---|---|
| [`pacts_gamm_walkthrough.md`](pacts_gamm_walkthrough.md) | Rendered output with the figures inline. GitHub displays it directly, so this is the one to click if you just want to read it. |
| [`docs/pacts_gamm_walkthrough.html`](docs/pacts_gamm_walkthrough.html) | The full styled version, with collapsible code blocks and every figure embedded in the single file. Read it on [the web page](https://eisenlohrmoullab.github.io/saa-pacts-gam-symposium/pacts_gamm_walkthrough.html); GitHub serves `.html` as raw source rather than rendering it. |
| [`pacts_gamm_walkthrough.Rmd`](pacts_gamm_walkthrough.Rmd) | The source. Knit it or step through the chunks. |

All three come from the same `.Rmd` and the same seed, so the numbers agree.

## Running it yourself

Everything is simulated inline, so there is no dataset to download and no data-access
step. One file, and about 15 seconds to knit once you are set up.

### Step 1 — install, before the session

This is the only step that needs the network, and it is the one that goes wrong on
conference wifi. Do it at home. In the R Console:

```r
install.packages("remotes")
remotes::install_github("eisenlohrmoullab/menstrualcycleR")
```

That is everything. `mgcv` ships with R, and `ggplot2` comes along as a `menstrualcycleR`
dependency. Be aware that on a fresh R installation this pulls roughly 60 packages and
takes minutes, not seconds. If it offers to update other packages, answering "none" is
fine. No compiler is needed on Windows or macOS.

Check it worked:

```r
library(menstrualcycleR)
packageVersion("menstrualcycleR")   # 0.1.6 or newer
```

### Step 2 — get the file

Easiest, and it lands the file in your working directory rather than in Downloads:

```r
download.file(
  "https://raw.githubusercontent.com/eisenlohrmoullab/saa-pacts-gam-symposium/main/pacts_gamm_walkthrough.Rmd",
  "pacts_gamm_walkthrough.Rmd"
)
```

If you would rather click: open
[`pacts_gamm_walkthrough.Rmd`](pacts_gamm_walkthrough.Rmd) above, then use the **download
raw file** button at the top right of the file view. Or take the whole repo with **Code →
Download ZIP**, unzip it, and open `saa-pacts-gam-symposium.Rproj`, which sets the working
directory for you.

### Step 3 — run it

Open `pacts_gamm_walkthrough.Rmd` in RStudio. Then either:

- **Follow along chunk by chunk** — the green ▶ button at the top right of each grey code
  block, working down the document. This is what we will do in the session.
- **Run the whole thing** — the **Knit** button, or `rmarkdown::render("pacts_gamm_walkthrough.Rmd")`
  in the Console, which produces the HTML page in about 15 seconds.

The document is self-contained: it simulates its own diary data, so there is nothing to
download, no data-access agreement, and nothing that can fail for lack of a file.

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
