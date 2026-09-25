# ai-wettschein.de – public prediction track record

Every football prediction published on [ai-wettschein.de](https://ai-wettschein.de/), frozen at kick-off and scored after the final whistle. Nothing is deleted, nothing is picked afterwards: the misses stay in the file next to the hits.

Most tipster sites show you the wins. This repository exists for the opposite reason. Our model produces calibrated probabilities, and we say openly that it **does not beat the betting market**. If you want to check whether the probabilities are any good, this is the raw material.

## Current numbers

<!-- summary:start -->
_Settled predictions up to 2026-09-24. Lower log loss and Brier are better._

| Competition | Matches | Favourite correct | Log loss (1X2) | Brier (1X2) | vs. guessing (1.099) |
|---|---:|---:|---:|---:|---|
| **All** | 126 | 50.8% | 0.983 | 0.588 | better |
| La Liga | 28 | 57.1% | 0.897 | 0.534 | better |
| Serie A | 20 | 45.0% | 1.027 | 0.610 | better |
| Premier League | 20 | 45.0% | 1.122 | 0.687 | worse |
| 2. Bundesliga | 18 | 50.0% | 1.063 | 0.638 | better |
| Bundesliga | 18 | 50.0% | 0.911 | 0.541 | better |
| Ligue 1 | 18 | 55.6% | 0.929 | 0.545 | better |
| UEFA Nations League A | 4 | 50.0% | 0.871 | 0.527 | better |
<!-- summary:end -->

Guessing one third for every outcome scores a log loss of 1.099. Below that, a forecast carries information; above it, it did worse than guessing for that sample. Small samples swing a lot – a few hundred matches are needed before these numbers mean much.

## Files

| File | Content |
|---|---|
| [`data/predictions.csv`](data/predictions.csv) | One row per settled prediction |
| [`data/summary.csv`](data/summary.csv) | Monthly scores per competition |

### `predictions.csv`

| Column | Meaning |
|---|---|
| `match_id` | Stable id of the match |
| `competition`, `season` | e.g. `Bundesliga`, `2026/27` |
| `kickoff_utc` | Kick-off in UTC |
| `home`, `away` | Team names as shown on the site (German) |
| `p_home`, `p_draw`, `p_away` | Model probabilities for the 90-minute result |
| `p_over_2_5`, `p_btts` | Probability of more than 2.5 goals / both teams scoring |
| `exp_goals_home`, `exp_goals_away` | Expected goals from the model |
| `frozen_at_utc` | Time of the last update before kick-off – this is the forecast being scored |
| `goals_home`, `goals_away` | Final score after 90 minutes |
| `outcome` | `home`, `draw` or `away` |
| `favourite_correct` | 1 if the most likely outcome happened |
| `log_loss_1x2` | −ln(probability given to the actual outcome) |
| `brier_1x2` | Sum of squared errors over the three outcomes |

## How the forecasts are made

- **Club leagues** (Bundesliga, 2. Bundesliga, Premier League, La Liga, Serie A, Ligue 1): a Dixon-Coles goal model with time-weighted matches, fitted daily, then calibrated against the betting market. Each forecast is a full score distribution, from which every market follows. Method in detail: <https://ai-wettschein.de/methode/>
- **UEFA Nations League A**: World Football Elo ratings turned into a goals model, calibrated on all League A matches 2018–2024.
- A forecast updates until kick-off and is then frozen. The frozen version is what you see here.

Bookmaker odds are deliberately not part of this dataset.

## Limitations

- The sample is small and starts in September 2026. One bad weekend moves the monthly numbers visibly.
- A good log loss is not a betting edge. Beating the bookmakers requires beating their prices, and our own backtests say we don't.
- Nothing here is betting advice.

## Updates

The files are regenerated automatically every week from the site's prediction ledger. The monthly write-ups live at <https://ai-wettschein.de/ki-bilanz/>.

## Licence

Data under [CC BY 4.0](LICENSE). Please credit **ai-wettschein.de** and link to <https://ai-wettschein.de/> when you use it.

---

**Kurz auf Deutsch:** Jede Prognose von ai-wettschein.de, beim Anpfiff eingefroren und nach dem Spiel abgerechnet, auch die Fehlgriffe. Die monatliche Auswertung steht unter <https://ai-wettschein.de/ki-bilanz/>. Sportwetten ab 18, Glücksspiel kann süchtig machen – Hilfe unter [check-dein-spiel.de](https://www.check-dein-spiel.de/).
