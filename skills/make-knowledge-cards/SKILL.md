---
name: make-knowledge-cards
description: Convert a pasted article or a local Markdown/TXT file into a deck of 5-8 knowledge cards. Each card covers exactly one knowledge point and contains a title, the core knowledge, a concise explanation, and an example or self-test question. Use when the user wants to turn reading material, notes, essays, tutorials, or documentation into study cards, revision cards, flashcards-style notes, spaced-repetition notes (without generating an Anki package), or a quick knowledge summary. Do not use for web URLs, PDFs, images, GUI work, or when the user asks for an Anki import file.
license: MIT
---

# Make Knowledge Cards

Turn an article into a small deck of **5-8 knowledge cards** optimized for review and recall.

## Inputs

Accept either:

1. **Pasted text** supplied directly in the conversation.
2. **A local file** with a `.md` or `.txt` extension. Read it with the file tools before processing.

Decline and explain the limitation when given:

- A web URL (no fetching or scraping).
- A PDF, image, Office document, or other binary format.
- A request to build an Anki package or a graphical interface.

Offer to proceed if the user pastes the text or converts the file to Markdown/TXT.

## What a Knowledge Point Is

A knowledge point is one self-contained, reusable unit of understanding, for example:

- A concept or definition ("what X is")
- A principle, rule, or mechanism ("why/how X works")
- A method, procedure, or formula ("how to do X")
- A distinction or comparison ("X vs Y")
- A fact or figure worth remembering

The same subject may legitimately yield several cards when they make different assertions — for example "how to do X" (procedure) and "why X works" (principle) are distinct points.

These are NOT knowledge points and must not become cards:

- The article's topic or title alone
- A transitional sentence, opinion label, filler, or rhetorical remark
- An anecdote whose only role is illustration (it may still be used as a card's example)
- A restatement of a point already captured in another card

## Workflow

### Step 1: Read the source completely

Read the full text before writing anything. Do not skim and start cards in the same pass. Identify structure: thesis, sections, arguments, definitions, steps.

### Step 2: Extract candidate knowledge points

List candidate points from the source in order of importance:

1. Points required to understand the article's core message come first.
2. Supporting points that materially add understanding come next.
3. Drop minor details, examples-without-general-value, and repetitions.

Mark duplicates and near-duplicates (the same point restated in different words) and keep at most one card for each.

### Step 3: Select and de-duplicate

Select candidates until reaching the natural limit:

- Target **5-8 cards**.
- Quality dominates count. Fewer than 5 cards is correct when the source contains fewer than 5 distinct, genuinely important points. State this briefly when it happens instead of padding.
- Never split one point across cards, never merge two distinct points into one card.
- **Causal chains:** a link in a causal chain earns its own card only when it is independently statable and independently testable (its own condition, mechanism, or result with content the source actually develops). If the links are just clauses of a single atomic assertion with no independent content, keep one card.
- **Tight sets:** items that are mutually exclusive options within one mechanism (e.g., the commands of a single interactive menu) belong together in one card; independent concepts get separate cards.

### Step 4: Write each card

Write the four fields for every card:

- **Title** — the name of the knowledge point; a short noun phrase, not the article title. English: 2-8 words. Chinese and similar languages: a concise phrase of roughly 4-16 characters.
- **Core knowledge** — one or two sentences stating the point directly, as an assertion the learner can remember. Tight enumerations (e.g., a fixed command set) may use one longer sentence with separators.
- **Explanation** — 2-4 sentences clarifying meaning, mechanism, or why it matters. Use the source's own reasoning.
- **Example / Self-test** — exactly one of:
  - an example taken from or directly grounded in the source, **preferred whenever the source provides one concrete enough to illustrate the point**, or
  - a single self-test question whose answer is the card's core knowledge.
  - Self-test questions must be open (not answerable with bare "yes/no"). Never print the answer after the question — the card's Core knowledge already is the answer, and appending it destroys the recall attempt.

### Step 5: Self-check before delivering

Verify every card against all of the following:

- One card = exactly one knowledge point.
- Every claim is traceable to the source; no outside facts, no invented examples.
- No two cards repeat the same point.
- Each card has all four fields with real content.
- The deck has 5-8 cards unless the source genuinely supports fewer.
- Examples are concrete; self-test questions are open and carry no printed answer.

Fix any failure before presenting the result. Do not show the internal candidate list or this checklist unless asked.

## Source Fidelity Rules (hard constraints)

- Use only information present in the provided text.
- Do not add background, statistics, citations, or "helpful" facts from general knowledge.
- Preserve the source's claims; do not soften, strengthen, correct, or take sides on them.
- Reproduce the source title as written, including apparent typos; do not silently fix them.
- When a term is undefined in the source, do not invent a definition — either omit the card or quote the source's usage.
- **Language:** card content follows the source language (Chinese article -> Chinese card content; English article -> English). The fixed structural labels ("Knowledge Cards", "Card N", and the four field names) stay in English in every language so the schema stays stable and matches the JSON keys; only their values are localized.

## Output Format

Where to write:

- An **explicitly requested output path or filename always wins**.
- Otherwise, for a file input, write next to the source (or the working directory) as `<source-basename>-cards.md`.
- For pasted text, present the deck directly in the reply; save a file only if the user asks.

Use this exact Markdown structure. When the source supports fewer than 5 cards, insert one short italic note directly under the Source line:

```markdown
# Knowledge Cards: [article title or topic]

Source: [file name or "pasted text"] · N cards

*[Only when N < 5: one short sentence explaining the source contains fewer distinct points.]*

## Card 1: [Title]

- **Core knowledge:** ...
- **Explanation:** ...
- **Example / Self-test:** ...

## Card 2: [Title]
...
```

If the user explicitly asks for machine-readable output (e.g., for later import elsewhere), use this JSON instead of Markdown:

```json
{
  "source": "sample.md",
  "card_count": 2,
  "note": "Omit this field when there are 5-8 cards; otherwise one short sentence explaining why the deck is shorter.",
  "cards": [
    {
      "title": "...",
      "core_knowledge": "...",
      "explanation": "...",
      "example_or_self_test": "..."
    },
    {
      "title": "...",
      "core_knowledge": "...",
      "explanation": "...",
      "example_or_self_test": "..."
    }
  ]
}
```

## Edge Cases

- **Very short source** (e.g., a single paragraph): produce however many points it truly contains, even if only 1-4; add the one-line note under the Source line instead of padding.
- **Opinion / narrative text with few teachable points**: extract the arguments or lessons that are actually stated; do not manufacture concepts.
- **List-heavy article**: apply the tight-sets rule from Step 3 — coupled options in one card, independent concepts in separate cards.
- **Contradictions inside the source**: represent the source faithfully (e.g., note competing claims); do not resolve them by guessing.
- **Unclear / corrupted text**: point out the unreadable sections and ask for clarification rather than guessing content.
