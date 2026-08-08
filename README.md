# NLPA Assignment 2 — Task-Oriented Conversational AI System

**Course:** Advanced NLP Applications: Conversational AI and Sentiment Intelligence
**Program:** M.Tech in Artificial Intelligence & Machine Learning — BITS Pilani (Sem 3)
**Assignment:** Problem Statement 1

---

## Overview

This project implements an end-to-end **task-oriented conversational AI system** for the domain of **IT Technical Troubleshooting**. The system can understand user intent across multiple dialogue turns, maintain and update dialogue state, call simulated external tools to complete tasks, handle ambiguous or unsafe inputs, and evaluate conversation quality across 10 dimensions.

The implementation is delivered as a fully self-contained Jupyter notebook (`nlpa_assignment2.ipynb`) that runs in any standard Python 3.8+ environment — no GPU, no external dataset downloads required.

---

## Domain

**IT Technical Troubleshooting** — users interact with the system to report device, software, network, or account issues. The system diagnoses the problem, retrieves solutions, raises support tickets, and escalates to a human agent when needed.

- **9 intents:** greet, report\_issue, provide\_info, request\_solution, check\_status, confirm\_resolution, escalate\_issue, end\_session, out\_of\_scope
- **10 entity slots:** device\_type, os, issue\_category, error\_code, app\_name, ticket\_id, urgency\_level, user\_name, attempted\_fix, resolution\_status
- **3 simulated tools:** solution knowledge base, support ticket creator, ticket status checker

---

## Notebook Structure

| Section | Content |
|---------|---------|
| 1 | Environment Setup — auto-install, imports, version verification |
| 2 | Domain Design — intents, slots, sample conversations, state transitions, failure cases |
| 3 | Intent Detection, Entity Extraction & Dialogue State Tracking |
| 4 | Tool-Augmented Response Generation |
| 5 | Memory, Personalization, Ambiguity & Safety |
| 6 | Evaluation — 10 conversations × 10 quality dimensions |
| 7 | Conclusion — limitations and improvements |

---

## Tech Stack

| Library | Purpose |
|---------|---------|
| `scikit-learn` | TF-IDF vectorizer, Logistic Regression classifier, evaluation metrics |
| `spacy` (`en_core_web_sm`) | Named Entity Recognition for user name extraction |
| `pandas` | Structured output tables |
| `matplotlib` | Evaluation bar charts |
| `seaborn` | Confusion matrix heatmap |
| `re`, `json` | Regex slot extraction, state storage |

---

## Approach

**Hybrid (Option C):**
- **Interactive mode** — fully functional chatbot accepts free-text input in real time
- **Scripted evaluation mode** — 10 pre-defined conversations run automatically with all outputs captured in the notebook

**Classifier:** TF-IDF (unigrams + bigrams) + Logistic Regression — fast, interpretable, no GPU required.
**Dataset:** 90 hand-crafted home-user-style utterances (10 per intent) — fully inline, no downloads.
**5-fold CV accuracy:** 61%

---

## Running the Notebook

```bash
# Install dependencies (auto-handled by the notebook's first code cell)
pip install scikit-learn spacy pandas matplotlib seaborn
python -m spacy download en_core_web_sm

# Launch Jupyter
jupyter notebook nlpa_assignment2.ipynb
```

Run all cells top-to-bottom. The environment setup cell handles any missing packages automatically.

---

## Files

| File | Description |
|------|-------------|
| `nlpa_assignment2.ipynb` | Main assignment notebook |
| `NLP_APPL_A2_PS1.docx` | Original problem statement |
| `plan.md` | Detailed execution plan |
| `build_logs.md` | Build progress and decisions log |
| `CLAUDE.md` | Project constraints and build guidance |
