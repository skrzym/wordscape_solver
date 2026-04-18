# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A Python prototype for solving the Wordscape mobile word game. The project scrapes a wordscapes answer site to build a word list, then filters that list by available letters to find valid plays, and uses a Board class to represent word placement on a grid.

All logic lives in two Jupyter notebooks — there is no module structure, test suite, build system, or CI configuration.

## Running the Notebooks

```bash
jupyter notebook
```

Dependencies (not declared in any requirements file — install manually):

```bash
pip install requests beautifulsoup4 pandas numpy jupyter
```

## Architecture

### WordscapeSolverNotebook.ipynb

The main prototype. Contains two independent concerns:

**1. Web scraping pipeline**
- Fetches pages from an external wordscapes answer site using `requests` + `BeautifulSoup`
- Extracts answers from `<li>` elements with `aria-label` attributes
- Applies a 0.25s delay between requests to avoid rate-limiting
- Deduplicates and writes results to `wordscapes_word_list.csv` (~8,247 unique words from ~70,652 raw answers)

**2. Word-matching engine**
- `get_letter_frequencies(letters)` — builds a `{char: count}` frequency dict from available letters
- `is_word_subset_of_letters(word_dict, letters_dict)` — validates a word can be formed from available letters by comparing frequency dicts
- `possible_words(letters, word_list)` — top-level filter; returns all words from the CSV that can be formed from the given letters

### BoardClassExperimenting.ipynb

Experiments with a `Board` class representing a 12×12 grid (numpy array, `-` as empty cell).

- `add_word(word, start_position, direction)` — places a word horizontally (`0`) or vertically (`1`)
- `grid_validity_check()` — scans all rows and columns, extracts character runs longer than 1 character as "placed words"
- `get_surrounding_cell_details(r, c)` — returns the 3×3 neighborhood around a cell (intended for future auto-placement logic, currently incomplete)

The `board_builder` function and automatic placement logic are stubs/TODOs.

## Key Conventions

- Snake_case for functions and variables; PascalCase for classes
- Frequency dictionaries (`{char: count}`) are the core data structure for letter-matching logic
- Numpy 2D arrays for grid state; pandas for CSV I/O
- No error handling or input validation — this is exploratory prototype code
