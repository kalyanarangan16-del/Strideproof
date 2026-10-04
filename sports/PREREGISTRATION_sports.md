# Preregistration: Weekly Training Volume and 10K Improvement in Club Runners

**Project:** Reproducible Research Question & Analysis Preregistration
**Status:** Locked before any real-data analysis. Pipeline verified only on synthetic fixtures.
**Analysis seed:** 20260904

## 1. Research question
Among adult club runners, do those with higher weekly training volume improve their 10K time more over a 12-week training block than those with lower volume?

Falsifiable form: *High-volume runners have a higher median 10K improvement (%) than low-volume runners.*

## 2. Study design (PICO + window)
| Element | Definition |
|---|---|
| Population | Club runners aged 18-65 with a recorded 10K time at baseline and follow-up |
| Exposure / intervention | High training volume: top tertile of average weekly distance (km) |
| Comparator | Low training volume: bottom tertile (middle tertile excluded from the primary test) |
| Outcome | improvement_pct = (baseline_10k_sec - followup_10k_sec) / baseline_10k_sec x 100 (positive = faster) |
| Observation window | 10 to 14 weeks between baseline and follow-up 10K |

## 3. Hypotheses
- **H0:** median improvement(high) = median improvement(low)
- **H1:** median improvement(high) > median improvement(low) (one-sided, directional)
- **Falsification rule:** H1 is rejected if one-sided p >= 0.05 OR the median difference is <= 0.

## 4. Exclusion rules (applied in this order)
1. Missing baseline time, follow-up time, or weekly distance.
2. Implausible values: 10K time outside 1500-7200 s, or weekly distance <= 0 or > 200 km.
3. Duplicate athlete_id (keep the row with the most weeks observed).
4. Observation window outside 10-14 weeks.
5. Age outside 18-65.

The count removed at each step is logged to `reports/exclusion_log.csv`.

## 5. Transformations
- Outcome: improvement_pct as defined above.
- Volume tiers: tertiles of weekly_km, computed over the full analytic sample.
- No outlier removal beyond rule 2. Winsorising appears only as a robustness check.

## 6. Primary test and effect size
- **Test:** one-sided Mann-Whitney U (high > low).
- **Effect sizes:** Cliff's delta and difference in medians of improvement_pct, each with a 95% bootstrap CI (10,000 resamples, percentile).
- **Alpha:** 0.05.

## 7. Robustness checks (all reported, none replace the primary result)
1. Welch t-test on improvement_pct.
2. Top vs bottom quartile of volume.
3. improvement_pct winsorised at 1st/99th percentile.
4. Excluding the top 1% of weekly volume.
5. Within-level comparison (beginner / intermediate / advanced), averaged by level size.

## 8. Deviations
Any deviation after lock must be listed in `DEVIATIONS.md` with date and reason.
