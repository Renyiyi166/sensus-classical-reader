# Sensus Classical Reader

An interactive MVP reader for Latin texts. The first version focuses on an
immersive reading experience: click a word, inspect its morphology, read a
sentence-level syntax explanation, and connect the passage to a small historical
case file.

## What Is Implemented

- Next.js + TypeScript source in `app/`, `lib/`, and `data/`.
- A zero-dependency static preview at `preview.html` for environments without a
  package manager.
- Two curated Latin samples:
  - Caesar, `Commentarii de Bello Gallico 1.1`
  - Cicero, `In Catilinam 1.1`
- Editable annotation data model in `data/passages.json`.
- Prototype pipeline scripts:
  - `scripts/annotate_latin.py` emits token candidates from raw Latin.
  - `scripts/generate_ai_explanations.py` builds reviewable AI prompt payloads.
  - `scripts/validate_data.py` validates the MVP JSON shape.

## Run The Preview

This workspace currently has Node installed, but no `npm`, `pnpm`, `yarn`, or
`bun`. The static preview works immediately:

```bash
python3 -m http.server 4173
```

Then open:

```text
http://127.0.0.1:4173/preview.html
```

## Run The Next.js App

After installing a package manager, install dependencies and start the app:

```bash
npm install
npm run dev
```

Then open:

```text
http://localhost:3000
```

## Data Model

`data/passages.json` is intentionally human-editable. The core objects are:

- `Passage`: work metadata, source notes, sentences, and context.
- `Sentence`: Latin text, English translation, syntax explanation, dependencies.
- `Token`: form, lemma, part of speech, morphology, local meaning, grammar note,
  etymology, and confidence.
- `ContextCard`: people, places, historical events, timeline, and map pins.

The long-term pipeline should keep human-reviewed morphology separate from AI
explanation layers, so generated prose can be revised without corrupting the
linguistic base data.

## Validation

Run:

```bash
python3 scripts/validate_data.py
```

The validator checks required fields, duplicate IDs, sentence tokens,
dependencies, maps, timelines, and context cards.
