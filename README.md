# Functional Data Analysis of Vegetation Index Curves

Each of 200 locations has a year of satellite Enhanced Vegetation Index (EVI) readings, taken at 46 eight-day intervals. Rather than treat those as 46 separate numbers, this project treats each location's year as one smooth curve and asks: what does a typical growing season look like, when does greening speed up and slow down, and in which ways do locations differ from each other?

Individual course project, Functional Data Analysis, University of Glasgow (2026). Written in R with the `fda` package.

## Method

1. **Smoothing with a roughness penalty.** B-spline basis with a knot at every observation, penalising curvature (second derivative). The smoothing parameter λ was chosen by generalised cross-validation (GCV) over a grid from 10⁻⁴ to 10⁶.
2. **Smoothing with a harmonic acceleration penalty.** This penalty leaves a pure annual sine and cosine cycle unpenalised and only penalises departures from it, which suits data where the yearly rhythm is the signal rather than noise. λ again chosen by GCV.
3. **Derivatives.** First and second derivatives of the smoothed curves give the rate and acceleration of greening through the year.
4. **Functional PCA**, first unpenalised, then with a roughness penalty on the components, to find the main ways the curves vary between locations.
5. **Interpretation** by plotting the mean curve plus and minus two standard deviations of each component.

## Results

From the submitted report:

- **Seasonal shape.** EVI is low and flat from January to April (0.2–0.3), rises sharply through May, peaks at 0.5–0.8 from June to August and falls steeply through October.
- **Smoothing.** GCV chose λ = 5.6 for the curvature penalty (effective degrees of freedom 40.9) and λ = 1,000 for the harmonic acceleration penalty (26.6), which kept the summer peak while removing more short-term noise.
- **Derivatives.** The fastest green-up is around May and the fastest decline around October; the second derivative marks the start and end of the growing season.
- **Principal components.** Three smoothed components explain 85.2% of the variation:
  - FPC 1 (45.6%): height of the summer peak
  - FPC 2 (33.4%): how green a location stays in winter
  - FPC 3 (6.2%): timing of the growing season, earlier versus later green-up
- **Penalised vs unpenalised.** Without a penalty, the third component picked up a sharp spike and irregular wiggles, which looked like noise. Penalising the components removed this and made all three interpretable.

`evi_fda.qmd` is my working notebook for the analysis. The submitted report came from a later revision that isn't in this repository, so the wording and a few smoothing choices differ slightly, and re-running the notebook may not reproduce every number above exactly.

## How to run

```r
install.packages(c("fda", "tidyverse"))
```

Then render `evi_fda.qmd` with Quarto from the repository root (PDF output needs a LaTeX installation, for example `tinytex`).

## Files

```
evi_fda.qmd         Analysis notebook (smoothing, GCV, derivatives, FPCA)
data/evi_data.csv   200 locations × 46 EVI readings (columns Day4 ... Day364)
```
