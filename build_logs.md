# Build Log — NLPA Assignment 2

**Notebook:** nlpa_assignment2.ipynb
**Domain:** IT Technical Troubleshooting
**Started:** 2026-08-08

---

## Log Format

Each entry records:
- Date
- Section built
- Cells added (count and type)
- Decisions made or changed
- Issues encountered
- Status: ✅ Complete | 🔄 In Progress | ⏳ Pending | ❌ Blocked

---

## Build Status

| Section | Status | Cells Built | Notes |
|---------|--------|-------------|-------|
| 1. Environment Setup | ✅ Complete | 8 | Env check, optional install, imports, spaCy load |
| 2. Domain Design | ✅ Complete | 16 | Intents, slots, 3 conversations, state transitions, failure cases + explanation + inference cells |
| 3. NLU Pipeline | ✅ Complete | 17 | Utterances, classifier, heatmap, extractor, DST, 10-conv test, explanation + inference cells |
| 4. Tool Generation | ⏳ Pending | 0 | — |
| 5. Memory & Safety | ⏳ Pending | 0 | — |
| 6. Evaluation | ⏳ Pending | 0 | — |
| 7. Conclusion | ⏳ Pending | 0 | — |

---

## Decisions Log

| Date | Decision | Reason |
|------|----------|--------|
| 2026-08-08 | Domain: IT Technical Troubleshooting | Richer datasets, clearer slot boundaries, realistic tool simulation |
| 2026-08-08 | Approach: Option C (Hybrid) | Interactive demo for screenshots + scripted eval for locked notebook outputs |
| 2026-08-08 | Classifier: TF-IDF + LogReg | No GPU, interpretable, appropriate for 50-sample dataset |
| 2026-08-08 | Dataset: 90 inline utterances (10 per intent) | Expanded from 50 after testing showed 34% CV accuracy; 90 utterances yields 61% CV |
| 2026-08-08 | No future improvement notes in notebook | Keep evaluator-facing notebook clean; future notes stored in plan.md only |
| 2026-08-08 | Response gen: template-based | Predictable, auditable, appropriate for task-oriented dialogue |
| 2026-08-08 | Manual scores for dims 6 & 10 | Require human judgement; LLM eval noted as future improvement |
| 2026-08-08 | No phase/step references in notebook | Academic submission style; internal build follows phases |

---

## Build Entries

<!-- Entries will be added below as each section is built -->

### 2026-08-08 — Section 3 Post-Build: Bug Fixes + Accuracy Improvements
- **Bugs fixed (4):**
  1. `user_name` — spaCy tagged "WiFi" as PERSON; fixed by running name-phrase regex first, NER as fallback with tech-word exclusion set
  2. `user_name` — `re.IGNORECASE` on capture group matched "running" as a name; fixed by removing IGNORECASE from the capture pattern
  3. `error_code` — digits in ticket ID (e.g. TK-1042) matched as error code; fixed by stripping ticket IDs from text before running error code regex
  4. `device_type` — "macbook" stored raw; fixed by adding `DEVICE_MAP` to normalise macbook→laptop, pc/computer→desktop
- **Dataset expanded:** 50 → 90 utterances (10 per intent, perfectly balanced)
- **Accuracy improvement:** CV mean 34% → 61%, pipeline test accuracy 64% → 73%
- **Notebook cleaned:** All future improvement notes removed from notebook cells; kept in plan.md only
- **Markdown updated:** Extractor analysis and results analysis cells updated to reflect fixed outputs and 90-utterance numbers

### 2026-08-08 — Section 3: Intent Detection, Entity Extraction & Dialogue State Tracking
- **Cells added:** 17 (9 markdown, 8 code)
- **What was built:**
  - Section intro + dataset rationale (90 utterances, 10 per intent, home-user style)
  - 90 inline home-user-style utterances (10 per intent × 9 intents) + distribution table
  - Distribution bar chart (steelblue, count labels, rotated x-axis)
  - Classifier rationale markdown (why TF-IDF + LogReg at this scale)
  - TF-IDF + LogReg classifier code (80/20 split, stratified, classification report)
  - Seaborn confusion matrix heatmap
  - Results analysis markdown (reading the matrix, test-set size caveat)
  - Entity extractor rationale markdown (3 techniques explained)
  - `extract_entities()` function (keyword dicts, OS longest-match, regex, spaCy NER + fallback) + 5 test cases
  - Extractor analysis + DST rationale markdown
  - `DialogueStateTracker` class (slot accumulation, contradiction detection, next_action logic)
  - DST illustrated trace table (Conversation 1, turn-by-turn state evolution)
  - 10-conversation scripted test loop + per-conversation output tables (exact spec column names)
  - Pipeline observations markdown
  - EXPLANATION: how DST enables coherent multi-turn interaction (3 mechanisms)
  - INFERENCE: model goal identification and context maintenance assessment
- **Status:** ✅ Complete

### 2026-08-08 — Section 1: Environment Setup
- **Cells added:** 7 (4 markdown, 3 code)
- **File created:** nlpa_assignment2.ipynb
- **What was built:**
  - Notebook title cell with student details table, problem statement, domain rationale, approach summary, and notebook structure table
  - Section intro markdown: library table + two-step setup description
  - Auto-install + verify cell (3-step): detects missing packages, installs via pip, downloads spaCy model, verifies all imports — works on any environment
  - Markdown explaining how to read the 3-step output and what to do if install fails
  - All-imports cell: stdlib, pandas, matplotlib, seaborn, sklearn, spacy — with confirmation print
  - Markdown transitioning to Section 2
- **Change from original plan:** Replaced commented-out manual install cell with a fully automatic install+verify cell. No manual steps required.
- **Status:** ✅ Complete
