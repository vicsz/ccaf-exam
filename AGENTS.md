# CCA-F Practice Deck

A single self-contained `index.html` for studying the "Claude Certified Architect – Foundations" exam — Practice Mode, Test Mode, and a Cheat Sheet, all in one file. No build step, no dependencies, nothing else needs to be checked out to use it.

## Exam domains

1. Agentic Architecture & Orchestration
2. Tool Design & MCP Integration
3. Claude Code Configuration & Workflows
4. Prompt Engineering & Structured Output
5. Context Management & Reliability

## Adding a new question

Questions live directly inside `index.html`, in the JSON array inside `<script type="application/json" id="question-data">`. There's no separate source pipeline — edit that JSON in place.

When the user provides a new question:

1. Check it's not a duplicate (search existing questions for matching prompt/choices).
2. Match the exact shape of existing entries:

```json
{
  "id": "q0403",
  "title": "Short Title, No Spoilers",
  "domain": 1,
  "domainLabel": "Domain 1: Agentic Architecture & Orchestration",
  "task": "1.1",
  "taskLabel": "Task 1.1: <full task statement>",
  "question": "...",
  "scenario": "",
  "choices": [
    { "label": "A", "text": "..." },
    { "label": "B", "text": "..." },
    { "label": "C", "text": "..." },
    { "label": "D", "text": "..." }
  ],
  "correctAnswer": { "label": "B", "text": "..." },
  "explanation": "...",
  "wrongAnswers": {
    "A": { "answer": "...", "explanation": "..." },
    "C": { "answer": "...", "explanation": "..." },
    "D": { "answer": "...", "explanation": "..." }
  },
  "rulesOfThumb": ["..."],
  "distractorPatterns": ["..."]
}
```

- `id`: next sequential id after the current highest one in the file.
- `title`: short, no spoilers — never name the correct mechanism/technique in the title.
- `domain` / `task` / `taskLabel`: pick the closest domain from the list above, and reuse the `task`/`taskLabel` from an existing question that tests the same skill for consistency.
- `wrongAnswers`: one entry per incorrect choice, each with its own specific explanation — never reuse the same sentence across choices.
- `rulesOfThumb`: short, transferable "when X, do Y" lines. Portability test: would this line still be true and useful under a *different* question testing the same concept?
- `distractorPatterns`: only include if a wrong choice is a recognizable exam trap; omit the field otherwise.

After adding a question, tell the user: the correct answer, why it's correct, why each wrong choice is wrong, and any rules of thumb.

## Adding a cheat sheet entry

The Cheat Sheet tab has its own data near the top of the same `<script>` block:

- `CHEAT_SHEET` — an array of `{ domain, trigger, pattern }`. Keep both sides short (a handful of words) — this is a scan-at-a-glance reference, not prose.
- `CHEAT_RIGHT_ANSWERS`, `CHEAT_DISTRACTORS`, `CHEAT_LESS_IS_MORE` — flat arrays of short one-line callouts shown at the top of the tab.

When the user pastes new entries, match the existing tone/length and append them to the right array — don't rewrite existing entries unless asked.
