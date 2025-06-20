# WordleBOT

This project contains a small Wordle solver created as a coursework assignment for the Machine Learning course at National Chengchi University (NCCU) in 2021. The objective is to guess the secret Wordle word as quickly as possible. The solver evaluates all candidate words and chooses the one with the highest information gain, measured using Shannon entropy.

## Repository structure

```
.
├── data
│   └── answer.py          # List of 5‑letter English words used as dictionary
├── wordle
│   ├── simulation.py      # Entropy-based solver
│   ├── wordle.py          # Command line Wordle game
│   ├── simulation.ipynb   # Notebook version of the solver
│   └── wordle_dev.ipynb   # Notebook used during development
└── requirements.txt       # Python dependencies
```

## Installation

1. Install Python (3.7 or later is recommended).
2. Install the required packages:

```bash
pip install -r requirements.txt
```

`requirements.txt` lists `scipy` and `pandas` as dependencies.

## Usage

### Running the solver

1. Change into the `wordle` directory so that the relative import of `data/answer.py` works correctly:

```bash
cd wordle
python simulation.py
```

2. The default answer is set to `"biddy"` in `simulation.py`. Modify the `ans` variable if you want to test other words. The script prints the best guess and hint at each step until the word is solved.

### Playing manually

You can also play a console version of Wordle against a random word from the dictionary:

```bash
cd wordle
python wordle.py
```

The program prompts for guesses and returns a hint string where `2` means a correct letter in the correct position, `1` means the letter exists in the word but in a different position, and `0` means the letter does not appear in the target word.

## Algorithm overview

The solver first builds a table of comparisons between all possible pairs of words. For each candidate guess the distribution of hint patterns is computed and scored using Shannon entropy. The word with the highest entropy is selected as the next guess. After each guess the candidate table is filtered using the returned hint, significantly reducing the search space. This process repeats until the correct word is found.

Key functions can be found in `wordle/simulation.py`, including `best_entropy` and `sub_table` which compute the entropy and filter the table respectively.

```python
    best = -1
    best_w = ''
    for idx, key in enumerate(table):
        entro_score = entropy(table[key].value_counts().values/len(table), base=2)
        if entro_score > best:
            best = entro_score
            best_w = key
    return best_w
```

## Data

The dictionary of valid answers is stored in `data/answer.py` as a Python list named `dictionary`. There are 2315 five‑letter words, starting with entries such as `"aback"`, `"abase"`, `"abate"` and ending with `"young"`, `"youth"`, `"zebra"`, `"zesty"`, `"zonal"`.

## License

This code was written for educational purposes as part of a course assignment and is provided as‑is.
