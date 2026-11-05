## Quick orientation

This repository contains a small, focused tool to parse a Signal chat export (Markdown) into a CSV summarising daily morning/evening "Dienst" records and counts of eggs ("Ei/Eier"). The main working file is `ParseChat.ipynb` (a runnable notebook). See `README.md` for background and limitations.

Keep the guidance below short and concrete so an AI coding assistant can make small, safe edits or add features.

## What this project does (big picture)

- Input: a Signal export `chat.md` (expected to live next to `ParseChat.ipynb`).
- Processing: notebook `ParseChat.ipynb` contains Python code that 1) preprocesses the markdown, 2) uses regex to extract timestamped messages, 3) normalises messages into day/service buckets and counts eggs, and 4) writes `chat_parsed.csv`.
- Output: `chat_parsed.csv` and `chat_parsed.xlsx` (CSV is the canonical output).

Key code locations:
- `ParseChat.ipynb` — the entire parsing pipeline; treat as the primary implementation.
- `README.md` — project purpose and human-facing notes.

## Conventions and important patterns

- The project is notebook-first. Edits are safe in the notebook cells (Python). If extracting to .py, preserve function names shown in the notebook: `preprocess_chat`, `detect_service`, `adjust_date_by_weekday`, `extract_eggs_laid`, `extract_eggs_members_and_mask`, `mask_mitgenommen_segments`.
- Regex-driven parsing is the core. Be conservative changing regexes — add unit tests or small sample inputs to verify behavior. Example regex constants in the notebook:
  - `MSG_REGEX = re.compile(r"\[(.*?)\]\s(.*?):\s*(.*?)(?=\n\n\[|$)", re.S)`
  - `RE_EGGS_NUMERIC`, `RE_EGGS_WORDS`, `RE_MEMBER_GIVE`, `RE_MITGEN`
- Locale: code attempts to set `locale.setlocale(locale.LC_TIME, "de_DE.UTF-8")` to produce German weekday names. Don't remove this unless replacing with an explicit mapping.
- Author replacement: the notebook uses `preprocess_chat(..., author=AUTHOR)` and replaces `Me` with the author string. Keep this behaviour unless you intentionally change the import/export semantics.

## Typical developer workflows (how to run / debug)

- Run interactively: open `ParseChat.ipynb` in Jupyter or VS Code and run the cells. The notebook reads `chat.md` relative to the notebook directory.
- Quick non-notebook run: you can extract the notebook code to a `.py` script and run `python parse_chat.py` once created. The notebook writes `chat_parsed.csv` to the repo root.
- If you need to test regex changes, create a minimal `tests/fixtures/sample_chat.md` with a few representative messages and run the parsing cells against it.

## Integration points & inputs to check

- Input file: `chat.md` — ensure the exported format from `signal-export` matches the `MSG_REGEX` pattern: `[YYYY-MM-DD HH:MM:SS] Person: Message`.
- Media links and reactions are removed by `preprocess_chat` — if `signal-export` changes its markdown, update `preprocess_chat` accordingly.

## Examples of repo-specific edits an AI might make

- Add a parameter to `ParseChat.ipynb` to accept a custom `FILE_PATH` via `argparse` or environment variable.
- Add unit tests: create `tests/test_parsing.py` that imports the parsing functions (extracted into a small module) and runs a few assertions for numeric and word-based egg counts.
- Improve weekday correction: extend `WEEKDAY_HINTS` mapping if you see new abbreviations in messages.

## Safety checks before committing changes

- Run the notebook cells locally to ensure `chat_parsed.csv` still produces sensible rows.
- If changing regexes, include fixture inputs demonstrating the old and new matches so reviewers can verify behaviour.

## Files to reference when making changes

- `ParseChat.ipynb` — parsing logic and constants (primary).
- `README.md` — project goals and human constraints.
- `chat_parsed.csv` — example output; use as a quick smoke-check after changes.

## If you need more context

- Ask for a sample `chat.md` if not present. The repo expects a Signal export; different export versions change markdown details.

---

If this looks good, I can: 1) extract the notebook parsing functions into a `parse_chat.py` module and add a simple test harness, or 2) add a `tests/` directory with fixtures — tell me which you'd prefer.
