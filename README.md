# MAT Education: Michigan Traders

A hands-on, notebook-based curriculum that takes members from the Python scientific stack to **researching, building, and backtesting systematic trading strategies on [QuantConnect](https://www.quantconnect.com/)**.

> **Michigan Traders** is a student-led organization focused on algorithmic trading, quantitative market research, and competitive trading strategy development, combining mathematics, statistics, computer science, finance, machine learning, and high-performance computing. This curriculum is the on-ramp.

---

## How the notebooks work

Every notebook is an **interactive workbook**, not a lecture. Each topic follows the same loop:

1. **Worked example**: a short concept + one runnable example (output already shown).
2. **✏️ Your turn**: a `# TODO` cell you fill in yourself.
3. **Self-check**: a cell you run that prints **`✅ Correct!`** or tells you exactly what's off.

Every module ships as a **pair of files**:

| File | What it is |
|---|---|
| `NN_Title.ipynb` | **Exercise version**, blanks to fill in. Start here. |
| `NN_Title_SOLUTIONS.ipynb` | **Answer key**, every exercise filled in, all self-checks passing. Check yourself *after* trying. |

> Working through it yourself? Duplicate the exercise notebook and edit your copy. Name it with `copy` in the title or drop it in a `my-work/` folder and it stays off GitHub automatically.

---

## Curriculum

| # | Module | Runs in | Status |
|---|--------|---------|--------|
| 01 | **NumPy for Quants** | Local Jupyter | ✅ Available |
| 02 | **Pandas for Financial Data** | Local Jupyter | ✅ Available |
| 03 | **The 5 Pillars of a QuantConnect Algorithm** | QuantConnect (LEAN) | ✅ Available |
| 04 | **The Research Environment & Working with Financial Data** | QuantConnect Research | ✅ Available |
| 05 | **Indicators, Signals & Strategy Design Patterns** | QuantConnect (LEAN) | ✅ Available |
| 06 | **Backtesting Mechanics & Performance Analytics** | QuantConnect (LEAN) | ✅ Available |
| 07 | **Statistical Signal Research (mean reversion & pairs)** | QuantConnect Research + LEAN | ✅ Available |
| 08 | **Machine Learning for Alpha** | QuantConnect Research + LEAN | ✅ Available |
| 09 | **Algorithm Framework, Risk Management & Competition Readiness** | QuantConnect (LEAN) | ✅ Available |

**Modules 01–02 (Foundations)** are pure Python. They run in any Jupyter kernel on your laptop. **Modules 03+** move to QuantConnect: the algorithm code runs inside the **LEAN engine** (the QuantConnect IDE or the LEAN CLI), and the research cells run in QuantConnect's **Research Environment** (`QuantBook`).

Modules 03–09 are **hybrid**. Cells marked 🔵 are QuantConnect code and are meant to be read here and run there. They are never executed locally. Everything else, including every exercise with a self-check, runs on your laptop against a simulated dataset that stands in for market data. You get the full build–run–check loop without waiting on a backtest.

Take them in order. Each module assumes the ones before it and nothing else: no concept, function, or piece of vocabulary appears in a worksheet before the module that teaches it.

---

## Quick start

### Foundations (notebooks 01–02): run locally

```bash
pip install numpy pandas matplotlib jupyter
jupyter notebook        # then open 01_NumPy_for_Quants.ipynb
```

That's it, no account, no keys. Run the cells top to bottom and do the exercises.

### Modules 03–09: local exercises

The local half of these notebooks needs two more libraries, both only from Module 07 onward:

```bash
pip install numpy pandas matplotlib jupyter statsmodels scikit-learn
```

`statsmodels` is used in **07** (OLS, the ADF test); `scikit-learn` in **08** (decision trees, cross-validation). Modules 03–06 need nothing beyond the foundations list.

> **pandas version.** These notebooks use the pandas ≥ 2.2 offset aliases: `resample("ME")`, `"QE"`, `"YE"` for month/quarter/year end. On pandas 2.0–2.1 those raise `ValueError`; use the old `"M"`, `"Q"`, `"A"` instead. On pandas 3.0 the old aliases are gone, so the new ones are the portable choice going forward. Check yours with `pd.__version__`.

### QuantConnect modules (03+)

1. Create a free account at [quantconnect.com](https://www.quantconnect.com/).
2. In a notebook's code cell that defines a `class ...(QCAlgorithm)`, copy it into a new project's `main.py` and click **Backtest**.
3. Research-environment cells (`qb = QuantBook()`) run inside QuantConnect's own notebook.

> A QuantConnect algorithm will **not** run in a plain local kernel. The trading API only exists inside LEAN. Each notebook makes clear which cells go where.

---

## Conventions used throughout

- **QuantConnect Python API is `snake_case`** (`self.set_start_date(...)`, `def on_data(self, data):`, `self.set_holdings(...)`), matching QuantConnect's current PEP8 API. The old `PascalCase` still works but isn't used here.
- **US equities / ETFs only** (SPY, AAPL, TLT, …), cleanest for learning. No crypto/futures/options.
- **Reproducible randomness**: every notebook seeds its RNG so your numbers match the comments.

---

## Repo layout

Every module is the same pair: `NN_Title.ipynb` (exercise) and `NN_Title_SOLUTIONS.ipynb` (answer key).

```
MAT Education/
├── 01_NumPy_for_Quants.ipynb                                    # exercise
├── 01_NumPy_for_Quants_SOLUTIONS.ipynb                          # answer key
├── 02_Pandas_for_Financial_Data.ipynb
├── 03_Five_Pillars_of_a_QuantConnect_Algorithm.ipynb
├── 04_Research_Environment_and_Financial_Data.ipynb
├── 05_Indicators_Signals_and_Strategy_Patterns.ipynb
├── 06_Backtesting_Mechanics_and_Performance.ipynb
├── 07_Statistical_Signal_Research.ipynb
├── 08_Machine_Learning_for_Alpha.ipynb
├── 09_Algorithm_Framework_Risk_and_Competition.ipynb
├── …                                                            # + _SOLUTIONS.ipynb for each
├── AUTHORING_GUIDE.md                                           # read before editing a notebook
├── TODO.md
└── README.md
```

> **Editing a notebook?** Read `AUTHORING_GUIDE.md` first. The notebooks are generated from builder scripts, so hand-edits to the `.ipynb` files are overwritten on the next build.

---

*Questions or improvements? Bring them to the research channel.*
