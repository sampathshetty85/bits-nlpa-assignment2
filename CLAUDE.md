# CLAUDE.md — NLPA Assignment 2

## Project Overview

This directory contains the Jupyter notebook for BITS Pilani M.Tech AI/ML Sem-3 NLPA Assignment 2.
The assignment builds an advanced task-oriented conversational AI system for the IT Technical Troubleshooting domain.

## Assignment File

- `NLP_APPL_A2_PS1.docx` — original problem statement
- `nlpa_assignment2.ipynb` — the notebook to build (target output)
- `plan.md` — full execution plan
- `build_logs.md` — build progress log

## Key Constraints

- Notebook must be fully self-contained — no external file downloads at runtime
- All 90 training utterances are hand-crafted inline in the notebook (10 per intent)
- Works in any standard Python 3.8+ Jupyter environment (local or hosted)
- No GPU required. No HuggingFace `datasets` library required.
- All cells must be executed with outputs visible before submission

## Tech Stack

| Library       | Purpose                          |
|---------------|----------------------------------|
| scikit-learn  | TF-IDF vectorizer, LogReg, metrics |
| spacy         | NER for user_name slot extraction |
| pandas        | Output tables                    |
| matplotlib    | Confusion matrix, bar charts     |
| seaborn       | Heatmap styling                  |
| re, json      | Regex extraction, state storage  |

## Notebook Output Style

- The notebook reads as a clean academic submission
- Section headings match the assignment task structure (1–7), not internal phase names
- Every code cell has a markdown cell before it (what + why) and after it (what the output means)
- No references to "phases", "steps" by number, or internal build terminology
- No future improvement notes in the notebook — these are kept in plan.md only

## Domain

IT Technical Troubleshooting — 9 intents, 10 entity slots, 3 simulated tools, full hybrid interactive + scripted evaluation.

## Build Order

Environment Setup → Domain Design → NLU Pipeline → Tool Generation → Memory/Safety → Evaluation → Conclusion

## Interaction Model

- Build one section at a time
- Pause after each section for human review before proceeding
- All code decisions explained before writing
