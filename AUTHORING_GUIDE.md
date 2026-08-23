# MAT Education: Notebook Authoring Guide

The standard every notebook in the Michigan Traders curriculum must meet. Read this before building or editing any notebook. Builders live in `~/.cache/mat-build/`; deliverables live in `~/claude-workspace/MAT Education/` (git repo → `github.com/zyrxun/MAT-Education`, public).

---

## 1. Teaching bar (the most important rule)

**Teach thoroughly. Never dump, never rush.** Specifically:

- **Narrate every worked example.** Before each demo cell, a markdown cell explains *what it does* and *what to notice in the output*. After a surprising result, interpret it. (Bad: a timing cell with no explanation. Good: "the next cell squares a million numbers two ways, a Python loop vs NumPy, and times each; here's why NumPy wins.")
- **Teach each function/method individually.** Don't list six constructors and show one example. Give each its own short markdown (what it does, signature, when to use it) + its own small example + interpreted output.
- **One idea per cell.** Prefer several small focused cells over one dense cell. Split "1-D indexing", "2-D indexing", and "the view/copy trap" into separate beats.
- **Build up before the exercise.** By the time the learner hits a `# TODO`, everything they need has been explained.
- Audience = intermediate (knows Python + basic stats) but is *learning* the material. Write for someone smart who hasn't seen this tool. Friendly, concrete, finance-framed.

Rule of thumb: a teaching section is usually **2–4 markdown cells + 2–4 small teach cells**, then the exercise trio. NB01 has ~32 teach cells across 10 topics. That's the density to match.

---

## 2. The interactive format (Guided + Your-Turn), two-file split

Every topic follows: **explain → worked example (output baked) → ✏️ Your turn → self-check**. No inline solutions.

Each module ships as a **pair** built from ONE source file:

| File | Role | TODOs | Checks | Inline solutions |
|---|---|---|---|---|
| `NN_Title.ipynb` | exercise (start here) | blank | present, cleared | none |
| `NN_Title_SOLUTIONS.ipynb` | answer key | filled | executed, ✅ baked | none |

The answer-key title ends with `· *ANSWER KEY*`.

---

## 3. Builder mechanics

One builder script per notebook (`build_numpy.py`, `build_pandas.py`, …) using these helpers:

- `md(text)`: markdown cell (role `md`).
- `teach(code)`: worked-example code cell (role `teach`); outputs get baked.
- `yourturn(title, task)`: markdown prompt headed `### ✏️ Your turn: {title}`.
- `exercise(stub, solution)`: code cell that differs by MODE (role `exercise`).
- `check(code)`: `assert`-based self-check ending in `print("✅ Correct!", ...)` (role `check`).
- `reveal(solution)`: **no-op** (kept so call sites are harmless; inline answers are gone).

Each builder takes `MODE` in {`student`, `solution`} and writes straight into the repo folder (`…/NN_Title.ipynb` or `…/NN_Title_SOLUTIONS.ipynb`). Cell dicts need an `id` (nbformat 4.5) and `metadata.mat_role`.

**Hard gotcha:** never end a markdown string with a `"` right before the closing `"""` (triple-quote collision → SyntaxError). Rephrase so the last char isn't a quote. Never put `"""` inside a code-cell string.

### Build + validate pipeline

`run_pipeline.py [name-substring …]` does, per module:
1. Build `solution` → execute with nbconvert (**no** `--allow-errors`) → assert **every** check printed `✅` (count passes == count checks). This is the committed answer key, outputs baked.
2. Build `student` → execute with `--allow-errors` (bakes `teach` outputs; checks error harmlessly) → strip `outputs`/`execution_count` from `exercise` and `check` cells. This is the committed exercise file.

Run e.g. `python3 ~/.cache/mat-build/run_pipeline.py pandas`. Both files for a module are regenerated together, so they can never drift.

### Environment

Durable venv at `~/.cache/mat-build/venv` (numpy, pandas, matplotlib, jupyter, nbconvert). Python 3.14 has no system sci-stack; recreate the venv if missing. **Do NOT use `/tmp`**. It gets purged between sessions.

---

## 4. Notebook house structure

1. Title block: `# Michigan Traders: Module N` / `# {Title} · *interactive workbook*`, then Series/Level/Format lines and a short "why this matters" paragraph.
2. **How to use** (the 4-step loop; point to the `_SOLUTIONS` companion).
3. **Setup** cell (imports, seeded RNG; explain the synthetic data if any).
4. Numbered topics, each = teaching beats + exercise trio.
5. **Cheat sheet** table.
6. **Stretch goals** (open-ended, "bring to the next meeting").
7. **What's next** + official docs links.

---

## 5. Conventions

- **Foundations (01–02)** run in a plain Jupyter kernel. **QuantConnect modules (03+)** are paste-into-LEAN algorithm code + runnable `QuantBook` research cells. They will NOT run in a local kernel, so those exercises are code-reading / predict-the-output / fill-in-the-pillar (can't always `assert`-validate). Validate QC syntax against live docs (snake_case PEP8 API: `set_start_date`, `on_data`, `set_holdings`; enums UPPER_SNAKE e.g. `Resolution.DAILY`).
- **US equities/ETFs only** (SPY, AAPL, TLT, …). No crypto/futures/options.
- **Seed every RNG** so numbers are reproducible and checks are stable.
- **pandas 3.0**: `resample("ME")` not `"M"`. End a teach cell on a bare DataFrame for a nice HTML table; use `display()` for multiple tables.

---

## 6. Git hygiene

- Repo is nested in the workspace; push to `origin main`.
- **Never `git add -A`.** Add curriculum files by explicit name. Richard keeps personal working copies in the folder (`*Richard*`, `*workthrough*`, `*copy*`, `my-work/` are gitignored) that must never be uploaded.
- Commit messages end with the `Co-Authored-By: Claude Opus 4.8` trailer.

---

## 7. Definition of done (per module)

- [ ] Answer key validates: every self-check prints `✅` (pipeline asserts this).
- [ ] Exercise file: `teach` cells have baked output; `exercise`/`check` cells cleared; **0** inline `<details>`.
- [ ] Teaching meets the §1 bar (narrated demos, per-function explanations).
- [ ] Both files committed by explicit name and pushed.
