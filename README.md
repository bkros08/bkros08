# Ben Kros

## Contact
📧 bkroslowitz@gmail.com

New York-based data scientist transitioning from biostatistics 
(MS, healthcare publications) into DS/quant roles. 
Building at the intersection of statistical rigor and applied ML. 

I care more about honest evaluation than impressive-looking numbers — several of the projects below report null or marginal results where the data didn't support a stronger claim.

## Featured Projects

### [mlb-betting-model](https://github.com/bkros08/mlb-betting-model)
XGBoost v2 model for MLB home-team win probability with walk-forward backtesting across two prospective hold-out seasons (2023, 2024).

- **Cal AUC:** 0.617 (2023) / 0.614 (2024)
- **Flat-stake ROI net of −110 vig:** +10.5% (2023) / +9.9% (2024), combined +10.2% across 4,918 bets
- Regime-aware calibration fix documented after investigating a 2024 coverage collapse
- Saved `best_model_v2.joblib` + `src/predict.py` CLI + live prediction tracker with Windows Task Scheduler automation

`scikit-learn` · `xgboost` · `pandas` · `pybaseball` + MLB Stats API · walk-forward validation · isotonic calibration · quarter-Kelly sizing

---

### [tcga-survival](https://github.com/bkros08/tcga-survival)
TCGA-BRCA breast cancer survival analysis: **classical statistical models vs ML survival models** on 896 patients with clinical + gene expression + mutation data.

- **Cox (stage + age):** C-index 0.755 [0.66, 0.84]
- **Random Survival Forest:** C-index 0.766 [0.66, 0.86]
- **XGBoost AFT:** C-index 0.699 [0.55, 0.81] — severe overfit (train C=1.00)
- **Honest finding:** ML adds no clinically meaningful improvement over stage + age on this cohort. Bootstrap CIs overlap almost entirely.

`lifelines` · `scikit-survival` · `xgboost` · GDC REST API · Cox PH · Schoenfeld residuals · Elastic Net Cox · walk-forward C-index · calibration analysis

---

### [quant-factor-model](https://github.com/bkros08/quant-factor-model)
Fama-French 5-factor + momentum equity model with ML alpha overlay. 495 S&P 500 stocks, 2010–2024.

- Per-stock time-series regressions, Fama-MacBeth with Newey-West HAC, rolling betas
- **Walk-forward Ridge / XGBoost / LightGBM** on ~20 features, 143 test months
- **Best honest spec** (Ridge LS + regime filter, net of 10 bps): Sharpe **0.54**, alpha +6.3% (t-stat 1.89, not significant at 5%)
- **Passive S&P 500 beats every active strategy** on risk-adjusted basis — the honest finding, consistent with documented post-2010 factor premium decay

`pandas-datareader` · `yfinance` · `statsmodels` · `scikit-learn` · `empyrical` · walk-forward validation · bootstrap CI · CAPM decomposition · transaction cost sensitivity

---

### [news-sentiment-polymarket](https://github.com/bkros08/news-sentiment-polymarket)
End-to-end NLP pipeline: GDELT news → HuggingFace transformers → MiniLM semantic retrieval over 15k Polymarket markets → FastAPI service.

- **Cardiff RoBERTa** (3-class, 76% neutral rate on news headlines) beats DistilBERT (binary, 84% high-confidence) for sentiment calibration
- **MiniLM cosine retrieval** brings article-to-market coverage from 14% (keyword) to 100% (semantic)
- **Null result:** sentiment does not significantly predict Polymarket resolution (AUC 0.527 [0.40, 0.64], cluster-bootstrapped, no category survives Bonferroni)
- **FastAPI service** with 4 endpoints, Pydantic validation, lifespan model loading

`transformers` · `torch` · `fastapi` · `uvicorn` · `gdeltdoc` · `py-clob-client` · cluster bootstrap · Bonferroni correction · LDA topic modeling

---

### [kraken_portfolio](https://github.com/bkros08/kraken_portfolio)
Python crypto portfolio tracker + ML-gated paper-trading engine for Kraken holdings.

- Live balance + trade history + OHLCV fetching via Kraken REST API
- Technical signals (SMA-20/50, RSI-14, Bollinger Bands), composite rebalancing score, Kelly-scaled sizing
- Walk-forward ML validation (LogReg, RandomForest, XGBoost) across 8 rolling windows with auto-refit on new data
- Paper-trading engine with explicit production guardrails (trade-size cap, daily spend limit, kill switch, ≥55% win rate + 10 closed trades gate before live)
- Vectorized backtesting, daily Task Scheduler snapshots, Jupyter reporting dashboard

`python-kraken-sdk` · `pandas` · `scikit-learn` · `xgboost` · Kelly criterion · walk-forward CV · paper-trading gate

---

## Recurring themes across these projects

- **Walk-forward validation** — every backtest uses chronological train/test splits; no shuffled k-fold on temporal data
- **Bootstrap confidence intervals** — cluster-bootstrapped where observations are not i.i.d. (e.g., multiple articles per market, multiple patients from a common cohort)
- **Multiple-testing correction** — Bonferroni applied across category-level tests when hypothesis count > 1
- **Transaction costs modeled explicitly** — −110 vig, bid/ask spreads, 10 bps round-trip where applicable
- **Honest null results reported** — when the data doesn't support an edge, the writeup says so rather than cherry-picking the window where it did

## Stack

`Python` · `pandas` / `numpy` · `scikit-learn` · `xgboost` · `lightgbm` · `statsmodels` · `transformers` · `torch` · `fastapi` · `lifelines` · `scikit-survival` · `jupyter` · `matplotlib` / `seaborn` · `git` · `Windows Task Scheduler`
