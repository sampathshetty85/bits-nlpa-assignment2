# Execution Plan — NLPA Assignment 2
**Course:** Advanced NLP Applications: Conversational AI and Sentiment Intelligence
**Assignment:** 2 — Problem Statement 1
**Domain:** IT / Technical Troubleshooting
**Approach:** Option C — Hybrid (Interactive free-text + Scripted evaluation)
**Last Updated:** 2026-08-08

---

## Notebook Philosophy

The notebook reads as a complete academic submission — clean section headings matching the assignment task structure, no internal build terminology visible. Every code cell is preceded by a markdown cell (what this does and why) and followed by a markdown cell (what the output means and what comes next).

---

## Architecture

```
User Input (free-text OR scripted)
        │
        ▼
 [Intent Classifier]      ← TF-IDF + Logistic Regression
        │
        ▼
 [Entity Extractor]       ← Regex + Keyword rules + spaCy NER
        │
        ▼
 [Dialogue State Tracker] ← Python slot dict, per-turn updates
        │
        ├── Tool needed? ──► [Tool Router] ──► 3 Simulated APIs
        │
        ▼
 [Memory Module] + [Safety Filter]
        │
        ▼
 [Response Generator] ──► Final Response
```

---

## Section 1 — Environment Setup

### What gets built
- Environment check cell: prints version of every required library or a clear error if missing
- Optional install cell (commented out by default)
- Single imports cell for all libraries

### Libraries checked
| Library | Version check | Install command |
|---------|--------------|-----------------|
| scikit-learn | `sklearn.__version__` | `pip install scikit-learn` |
| spacy | `spacy.__version__` | `pip install spacy` |
| spacy model | `spacy.load("en_core_web_sm")` | `python -m spacy download en_core_web_sm` |
| pandas | `pandas.__version__` | `pip install pandas` |
| matplotlib | `matplotlib.__version__` | `pip install matplotlib` |
| seaborn | `seaborn.__version__` | `pip install seaborn` |

### Notebook cells
1. [Markdown] Notebook title, student details, problem statement, domain, approach summary
2. [Code] Environment check — all library versions or errors
3. [Markdown] What each library is used for
4. [Code] Optional install cell (commented out)
5. [Code] All imports
6. [Markdown] Transition to Section 2

---

## Section 2 — Domain Design

### What gets built
- Intent definitions (9 intents)
- Entity slot definitions (10 slots)
- 3 sample multi-turn conversations
- State transition table
- Failure cases table

### 9 Intents
| Intent | Example |
|--------|---------|
| `greet` | "Hi, I need help" |
| `report_issue` | "My WiFi is not working" |
| `provide_info` | "It's a Windows 11 Dell laptop" |
| `request_solution` | "How do I fix error 0x80070005?" |
| `check_status` | "What's the status of TK-1042?" |
| `confirm_resolution` | "Yes, that fixed it" |
| `escalate_issue` | "I need a supervisor" |
| `end_session` | "Thanks, goodbye" |
| `out_of_scope` | "What's the weather?" |

### 10 Entity Slots
| Slot | Method | Example |
|------|--------|---------|
| `device_type` | keyword list | laptop, desktop, router |
| `os` | keyword list | Windows 11, macOS, Ubuntu |
| `issue_category` | keyword list | network, software, hardware |
| `error_code` | regex `[0-9A-Fx]{4,}` | 0x80070005 |
| `app_name` | keyword list | Chrome, Zoom, Teams |
| `ticket_id` | regex `TK-\d+` | TK-1042 |
| `urgency_level` | keyword mapping | urgent→high |
| `user_name` | spaCy PERSON NER | "I'm Sampath" → Sampath |
| `attempted_fix` | keyword list | restarted, reinstalled |
| `resolution_status` | system-managed | open, resolved, escalated |

### Sample Conversations
- Conv 1: WiFi issue → diagnosed → resolved via solution tool
- Conv 2: Software crash → ticket created
- Conv 3: Ambiguous input → clarification → escalation

### Notebook cells
7. [Markdown] Domain overview and rationale
8. [Code] INTENTS dict with descriptions + examples
9. [Markdown] Intent explanations
10. [Code] SLOTS definitions with extraction methods
11. [Markdown] Slot explanations
12. [Code] 3 sample multi-turn conversations (structured dicts)
13. [Markdown] Conversation walkthroughs
14. [Code] State transition table as pandas DataFrame
15. [Markdown] State machine explanation
16. [Code] Failure cases table
17. [Markdown] Why failure cases matter

---

## Section 3 — Intent Detection, Entity Extraction & Dialogue State Tracking

### What gets built
- Dataset details cell (source, size, distribution, rationale)
- 50 inline labeled utterances (home user style)
- TF-IDF + LogReg intent classifier
- Regex + keyword entity extractor
- DialogueStateTracker class
- 10 multi-turn conversation tests
- Task 2 explanation cell (how DST enables coherent multi-turn)
- Task 2 inference cell (does the model identify goals and maintain context?)

### Dataset decision
90 hand-crafted home-user-style utterances inline (10 per intent). No external downloads.
Expanded from 50 to 90 after testing showed 34% CV accuracy at 50; 90 yields 61% CV accuracy.
> Future improvement: Replace with CLINC150 via `load_dataset("clinc_oos", "plus")`. Only the data cell changes.

### Classifier decision
TF-IDF (unigrams + bigrams) + Logistic Regression. Fast, interpretable, no GPU.
> Future improvement: Fine-tune BERT/sentence-transformers for higher accuracy on larger datasets.

### Output table columns (exact match to assignment spec)
`Utterance | Predicted Intent | Extracted Entity | Current Dialogue State | Missing Information | Next System Action`

### Notebook cells
18. [Markdown] Section intro + dataset details (source, size, distribution, rationale + CLINC150 future note)
19. [Code] 50 inline utterances
20. [Code] Utterance distribution bar chart
21. [Markdown] Intent classifier rationale (why TF-IDF + LogReg)
22. [Code] Train/test split + fit + predict
23. [Code] Classification report + confusion matrix seaborn heatmap
24. [Markdown] Results analysis
25. [Markdown] Entity extractor rationale (why 3 techniques)
26. [Code] extract_entities() function + 5 inline tests
27. [Markdown] Extractor analysis
28. [Markdown] DST rationale
29. [Code] DialogueStateTracker class
30. [Markdown] DST illustrated trace (one conversation, state dict evolving turn by turn)
31. [Code] 10 conversation test loop + output tables
32. [Markdown] Pipeline observations
33. [Markdown] EXPLANATION: how DST avoids repeating questions, enables coherent multi-turn
34. [Markdown] INFERENCE: does the model correctly identify user goals and maintain context?

---

## Section 4 — Tool-Augmented Response Generation

### What gets built
- Tool 1: search_solutions_db
- Tool 2: create_support_ticket
- Tool 3: check_ticket_status
- Tool router
- Response generator
- 5-query test table
- Task 3 explanation cell (tool use vs pure text generation)
- Task 3 inference cell (does tool use reduce hallucination and improve trust?)

### Tool decisions
All tools are pure Python (dict-backed). No external APIs.
> Future improvement: Replace generate_response() with LLM call. Router and DST unchanged.

### Output table columns (exact match to assignment spec)
`User Query | Required Tool | Tool Input | Tool Output | Generated Response`

### Notebook cells
35. [Markdown] Section intro + EXPLANATION: how tool use improves factuality vs pure text generation
36. [Code] Tool 1: search_solutions_db
37. [Markdown] Tool 1 explanation + when it fires
38. [Code] Tool 2: create_support_ticket
39. [Markdown] Tool 2 explanation + when it fires
40. [Code] Tool 3: check_ticket_status
41. [Markdown] Tool 3 explanation + when it fires
42. [Markdown] Tool router rationale
43. [Code] route_to_tool() function
44. [Code] Routing decision table
45. [Markdown] Response generator rationale + future improvement note (LLM)
46. [Code] generate_response() function
47. [Code] 5-query test table
48. [Markdown] Test results analysis
49. [Markdown] INFERENCE: does tool integration reduce hallucinated responses and improve user trust?

---

## Section 5 — Memory, Personalization, Ambiguity & Safety

### What gets built
- ShortTermMemory class
- UserProfile class (explicitly framed as preference memory)
- Ambiguity detector
- Safety filter
- full_pipeline() function
- 5 edge case tests
- Task 4 explanation cells (personalisation vs over-personalisation; why ambiguity handling is essential)
- Task 4 inference cell (does system respond more responsibly after memory/safety?)

### Edge cases
| # | Input | Issue | Response |
|---|-------|-------|----------|
| 1 | "Fix my error" | Missing slots | Clarification question |
| 2 | "Windows laptop… macOS crashed" | Contradictory OS | Conflict resolution |
| 3 | "It's not working" | Ambiguous | Detail request |
| 4 | "Book me a flight" | Out-of-scope | Polite decline |
| 5 | Profanity/threat | Safety blocklist | Escalation to human |

### Output table columns (exact match to assignment spec)
`User Input | Detected Issue | System Decision | Clarification / Safe Response | Updated Memory`

### Notebook cells
50. [Markdown] Section intro + memory rationale
51. [Code] ShortTermMemory class
52. [Code] UserProfile class (framed explicitly as preference memory: stores device, OS, app preferences, past tickets)
53. [Markdown] EXPLANATION: useful personalisation vs unsafe over-personalisation + personalization walkthrough example
54. [Markdown] Ambiguity detector rationale + EXPLANATION: why ambiguity handling is essential in real-world systems
55. [Code] detect_ambiguity() function
56. [Markdown] Ambiguity detection examples
57. [Markdown] Safety filter rationale
58. [Code] safety_check() function
59. [Markdown] Safety filter analysis
60. [Markdown] Full pipeline assembly description
61. [Code] full_pipeline() function
62. [Markdown] Edge case intro
63. [Code] 5 edge case runs
64. [Code] Edge case output table
65. [Markdown] Edge case analysis
66. [Markdown] INFERENCE: does the system respond more responsibly after adding memory, clarification, and safety control?

---

## Section 6 — Evaluation

### What gets built
- Section A: Interactive demo cell (run_interactive with input())
- Section B: 10 scripted evaluation conversations
- Automated scoring for 8 of 10 dimensions
- Manual score variables for dims 6 and 10
- Confusion matrix seaborn heatmap, bar chart, score table
- Written explanation and inference

### 10 Evaluation Dimensions
| # | Dimension | Scoring |
|---|-----------|---------|
| 1 | Intent accuracy | Automated |
| 2 | Entity extraction quality | Automated |
| 3 | Dialogue state correctness | Automated |
| 4 | Task completion rate | Automated |
| 5 | Tool selection correctness | Automated |
| 6 | Response relevance | Manual variable |
| 7 | Context consistency | Automated |
| 8 | Ambiguity handling | Automated |
| 9 | Safety compliance | Automated |
| 10 | User satisfaction | Manual variable |

### 10 Scripted Conversations
| # | Scenario |
|---|----------|
| 1 | WiFi issue → resolved via solution tool |
| 2 | Software crash → ticket created |
| 3 | Returning user → personalized → ticket status |
| 4 | Escalation request |
| 5 | Missing slot → clarification → resolved |
| 6 | Ambiguous input → clarified → resolved |
| 7 | Out-of-scope query |
| 8 | Unsafe input → safety filter |
| 9 | Contradictory slots → resolved |
| 10 | Multi-turn: solution search + ticket |

### Manual scores
> Future improvement: Replace manual_scores variables with LLM-based evaluator (prompt Claude/GPT-4o with transcript, rate 1–5). Removes subjectivity, scales to hundreds of conversations.

### Notebook cells
67. [Markdown] Section intro + EXPLANATION: why accuracy alone is insufficient (task success, context continuity, safety, user satisfaction)
68. [Markdown] Section A: interactive demo instructions
69. [Code] run_interactive() function
70. [Markdown] Screenshot instructions + transition to Section B
71. [Markdown] Section B: scripted evaluation intro
72. [Code] 10 scripted conversations + ground truth labels
73. [Code] Automated scoring (dims 1–5, 7–9)
74. [Code] manual_scores variables (dims 6, 10) + future improvement note
75. [Markdown] Manual scoring instructions
76. [Code] Per-conversation score table
77. [Code] Confusion matrix seaborn heatmap
78. [Code] Dimension score bar chart
79. [Markdown] Chart analysis
80. [Markdown] INFERENCE: deployment readiness, major limitations, improvements needed

---

## Section 7 — Conclusion

### Notebook cells
81. [Markdown] Final deployment readiness verdict
82. [Markdown] Limitations
83. [Markdown] Improvements needed
84. [Markdown] References

---

## Required Assignment Checklist (all sections)

| Requirement | Covered In |
|-------------|-----------|
| Assignment title + student details | Section 1 title cell |
| Problem statement | Section 1 title cell |
| Dataset details and source | Section 3 intro cell |
| Tools and libraries used | Section 1 library table |
| Explanation of logic (every task) | Markdown before each code cell |
| Justification of approach (every task) | Markdown before each code cell |
| Inference (every task) | Dedicated inference cell end of each section |
| Limitations | Section 7 |
| Possible improvements | Section 7 + future improvement notes |
| Conclusion | Section 7 |
| References | Section 7 |

## Output Table Columns (exact assignment spec)

| Section | Columns |
|---------|---------|
| Task 2 | Utterance \| Predicted Intent \| Extracted Entity \| Current Dialogue State \| Missing Information \| Next System Action |
| Task 3 | User Query \| Required Tool \| Tool Input \| Tool Output \| Generated Response |
| Task 4 | User Input \| Detected Issue \| System Decision \| Clarification / Safe Response \| Updated Memory |
| Task 5 | Parameter \| Score / Observation |

---

## Future Improvement Notes (all locations)

| Section | Current | Future |
|---------|---------|--------|
| 3 — Dataset | 50 hand-crafted utterances | CLINC150 via HuggingFace |
| 3 — Classifier | TF-IDF + LogReg | Fine-tuned BERT |
| 4 — Responses | Template strings | LLM call (Claude/GPT-4o) |
| 6 — Scoring | Manual variables | LLM-based evaluator |

---

## Total Cell Count: ~84 cells (~44 markdown, ~40 code)
