# 🛤️ Railroad — Contributor Onboarding

Welcome to **🛤️ Railroad - Laying the Tracks for Text Transformation Grounded in Tradition**. This document is your entry point regardless of your background. Read it before opening any other file in the project.

---

## The Problem We Are Solving

Large language models are surprisingly poor at translating, adapting, or teaching classical Buddhist texts reliably. The reason is not that they lack knowledge — most modern LLMs have ingested enormous amounts of Buddhist scholarship. The reason is that classical texts are densely ambiguous in ways that require the commentary tradition to resolve, and LLMs cannot access that tradition reliably at generation time.

A single Sanskrit compound can carry three or four grammatically valid readings. A Pāli technical term like _cetanā_ or _phassa_ has a precise scope within Abhidhamma analysis that ordinary psychological vocabulary collapses. A verse may be syntactically incomplete without the previous one. Commentators across centuries have worked out exactly how to read each of these — but that knowledge is scattered across texts in multiple languages, written for specialist audiences, and unavailable to a model trying to generate a fluent English translation in a single pass.

The standard workaround is to feed the LLM more material — upload the commentaries to NotebookLM, attach PDFs to a chat session, use retrieval-augmented generation. This does not work well. The context is too large, too unstructured, and the model still has to do the interpretive work at generation time, from scratch, every time. Every translation of every verse starts from zero. Nothing accumulates.

Most current efforts focus on building more powerful models and agents — better engines, smarter vehicles. 🛤️ Railroad takes a different approach: **lay the tracks first.** The tracks are a structured, openly licensed, openly accessible reference resource that resolves the interpretive ambiguities of each text word by word, verse by verse, grounded in the classical commentary tradition. Once the tracks are laid, any engine can run on them — any model, any agent, any transformation type, any target language.

---

## What 🛤️ Railroad Is

🛤️ Railroad is a methodology, implemented as a structured Obsidian vault, for building an **open interpretive infrastructure** for classical Buddhist texts — and using it to generate tradition-grounded text transformations.

The infrastructure is built one passage at a time. For each verse or analytical unit, a structured **context package** is compiled from the classical commentary tradition and stored as a machine-readable file. The package resolves every significant ambiguity in the passage — which edition, which compound reading, which syntactic structure, which sense of each term — and cites the specific commentary passage that determines each decision. By the time the package is complete, all interpretive work is done and sourced. An LLM generating a translation, lesson, or adaptation from it does stylistic work only — it does not decide what _dharmakāya_ means in this verse, or how Prajñākaramati reads the compound. That is already in the package.

A context package for a single verse contains:

- The source text in original language and transliteration
- A morphological table: every token tagged with lemma, part of speech, inflection, and compound analysis — sourced to the specific commentary passage that determines the reading
- A UCCA syntactic tree mirroring the grammatical interpretation of the commentaries
- A semantic gloss table: every token with its active sense ID, an English gloss, and a list of senses the commentary explicitly excludes for this passage
- Interpretive notes: what each relevant commentary says is happening in this passage, paraphrased in English with citations

The full collection of context packages for a text is the track. It is openly licensed and unrestricted — a shared foundation that any project can build on. A Swahili translation, a children's adaptation, a scholarly edition, and a video script can all run on the same track, drawing on the same tradition-grounded interpretive decisions, without any of them having to redo the philological work from scratch.

---

## The Two Texts We Are Working On

### Bodhicaryāvatāra (BCA)

Śāntideva's 8th-century Sanskrit guide to the bodhisattva path. Ten chapters, approximately 900 verses. Primary source language: Sanskrit. Key commentaries: Prajñākaramati (Sanskrit, 11th c.), Kunzang Pelden and Minyak Kunzang Sönam (Tibetan). Reference format: `BCA_CC.VVV` (e.g. `BCA_06.033`).

### Abhidhamma Piṭaka

The seven Pāli treatises of the Abhidhamma basket of the Pāli Canon. Primary source language: Pāli. Key commentaries: Buddhaghosa's Atthasālinī, Sammohavinodanī, and Pañcappakaraṇa (Pāli, 5th c.); Anuruddha's Abhidhammattha Saṅgaha (Pāli, 11th c.). Reference format: `[Treatise]_[section].[unit]` (e.g. `Ds_001`, `Kv_001.001`).

---

## The Folder Structure

```
Railroad/
├── CLAUDE.md                  ← schema and rules for the LLM
├── index.md                   ← master index of texts, commentaries, sense IDs
├── log.md                     ← append-only audit trail
│
├── 0-Project/                   ← you are here
├── 1-Human-Sources/             ← ground truth — never edited by the LLM
├── 2-Authoritative-Context/     ← structured interpretation — LLM-compiled
├── 4-Wiki/                      ← cross-text concept knowledge — LLM-compiled
└── 3-Text-Transformations/      ← generated outputs — translations, lessons etc.
```

### `1-Human-Sources/` — Ground Truth

Everything human scholars have produced: root texts, editions, translations in any language, commentaries, subcommentaries, and secondary literature. The LLM reads this folder but never writes to it. It is the sole source of authority for every claim made anywhere else in the project.

Each text has its own subfolder structured as:

```
[text]/
├── root-text-[lang].md        ← primary source file
├── editions/                  ← other editions, named [edition-name]-[lang]/
├── translations/              ← translations, named [author]-[lang]/
├── commentaries/              ← named [commentary-name]-[lang]/
├── subcommentaries/           ← named [subcommentary-name]-[lang]/
└── secondary-literature/      ← named [author]-[lang]/
```

Language tags follow ISO 639-1: `-sk` Sanskrit, `-pi` Pāli, `-bo` Tibetan, `-en` English, `-fr` French, `-de` German, etc.

### `2-Authoritative-Context/` — Structured Interpretation

The heart of 🛤️ Railroad. This is where commentary interpretation is extracted and reformatted as structured, machine-readable context packages. Each verse or analytical unit gets a package file containing all five layers of analysis. Text-local concept pages collect all attestations of a term within one text's commentary tradition.

The LLM writes here. Every field it writes must cite a specific file in `1-Human-Sources/`. If it cannot cite a claim, it leaves the field blank.

### `4-Wiki/` — Cross-Text Knowledge

Concept pages that aggregate across all texts in the project. A page for _dharma (cosmic order)_ collects how that sense operates in the BCA, the Abhidhamma, and any other text we add. The LLM writes here, citing `2-Authoritative-Context/` files only — never `1-Human-Sources/` directly.

### `3-Text-Transformations/` — Generated Outputs

Translations, adaptations, lessons, reading plans, video scripts, and any other content generated using the authoritative context packages. Each output file records which context package(s) it was generated from. This makes every transformation auditable: if a translation choice is questioned, you can trace it back through the context package to the specific commentary passage that motivated it.

---

## The Citation Chain

Every claim in this project traces back to a human source through a strict chain:

```
1-Human-Sources/  →  2-Authoritative-Context/  →  4-Wiki/
                                           →  3-Text-Transformations/
```

No link in this chain may be skipped. `4-Wiki/` never cites `1-Human-Sources/` directly. `3-Text-Transformations/` records which `2-Authoritative-Context/` packages it used.

This chain is what makes 🛤️ Railroad outputs reliable and human-verifiable. A scholar can always check any claim in a generated translation back to the commentary passage that grounds it.

---

## The Two Contributor Roles

🛤️ Railroad requires two complementary types of expertise. You may bring both, or just one.

### Domain Specialists

Philologists, Buddhist scholars, and text experts. Your role is:

- Evaluating and correcting LLM-generated analysis in `2-Authoritative-Context/`
- Adding new source materials to `1-Human-Sources/`
- Determining unit boundaries and commentary groupings
- Signing off on fields before their status moves from `draft` to `complete`
- Flagging interpretive divergences between commentaries

You do not need to know how to prompt LLMs or write code. You need to be able to read the commentary tradition and assess whether the structured analysis accurately reflects it. See `0-Project/roles/domain-specialists/` for detailed guidance.

### AI Skills Designers

Contributors who build and maintain the LLM workflows. Your role is:

- Writing and refining ingest prompts that extract analysis from commentaries
- Designing and testing transformation prompts for different output types
- Building lint passes that check citation coverage and consistency
- Maintaining the transclusion resolver script
- Developing Dataview queries for coverage tracking

You do not need to read Sanskrit or Pāli. You need to understand the schema (CLAUDE.md) and be able to evaluate whether LLM outputs conform to it. See `0-Project/roles/ai-skills-designers/` for detailed guidance.

---

## Key Concepts

### Authoritative Context

A context package for a single verse or analytical unit. Contains all the information needed to resolve the interpretive ambiguities of that passage, sourced to specific commentary passages. This is what gets passed to the LLM at generation time instead of raw commentary material.

### Disambiguation Stack

The five layers of analysis in each context package, each resolving a different type of ambiguity:

|Layer|What it resolves|
|---|---|
|Source text|Which edition, which variant readings|
|Morphological|Token segmentation, compound analysis, inflection|
|Syntactic (UCCA)|Sentence structure, argument relations|
|Semantic gloss|Which sense of each term is active; which senses are excluded|
|Interpretive notes|What the commentaries say is happening|

### Sense IDs

A disambiguation ID for each term, following Wikipedia disambiguation style: `term (disambiguating phrase)`. For example:

- `dharma (cosmic order)` — the moral-cosmic framework of the BCA narrative
- `dharma (Abhidhamma phenomenon)` — a mind-object in Abhidhamma analysis
- `cetanā (volition)` — mental intention as defined by Buddhaghosa

The disambiguating phrase is always English, even when the term is Sanskrit or Pāli. Sense IDs are registered in `4-Wiki/` and used consistently across all files.

### Divergence Flags

When commentaries disagree on any analytical decision, the field is flagged with ⚑. Divergences are never flattened to a false consensus — they are recorded explicitly. This is one of the most important features of 🛤️ Railroad: the commentary tradition often contains genuine disagreement, and that disagreement is itself historically significant.

### UCCA Trees

Universal Conceptual Cognitive Annotation trees represent the syntactic and semantic structure of a verse as understood by the commentaries. They use ASCII tree format:

```
H: Scene
├── A: Participant
│   └── bodhisattvaṃ namaskṛtya
└── P: Process
    └── pravakṣyāmi
```

Nodes are labeled with UCCA categories (H Scene, P Process, A Participant, etc.). The tree mirrors the commentarial reading, not a generic parse.

### Unit Boundaries

The atomic unit of analysis is the smallest syntactically complete passage defensible by any commentary. A verse is the default unit for the BCA; Abhidhamma units vary by treatise. When commentaries group verses differently, the finest granularity takes precedence and coarser groupings are recorded as metadata.

---

## What 🛤️ Railroad Is Not

**Not an AI model or agent.** 🛤️ Railroad is infrastructure, not an engine. It does not generate translations or adaptations on its own. It lays the track that makes AI-generated transformations reliable. The LLM is the vehicle; the context packages are the track.

**Not a translation archive.** The `3-Text-Transformations/` folder contains generated outputs, not authoritative translations. Outputs are grounded in the tradition but are not themselves scholarly editions.

**Not a commentary database.** `1-Human-Sources/` stores commentaries but 🛤️ Railroad is not designed for searching or browsing raw commentary material. That is what existing digital humanities tools do. 🛤️ Railroad's value is in the structured extraction layer compiled from that material.

**Not a chatbot.** The LLM is a formatting and compilation agent, not an interpreter. Every interpretive decision is made by the human commentary tradition and recorded in `2-Authoritative-Context/`. The LLM executes; it does not decide.

**Not finished.** Coverage is incremental. A verse package with `status: draft` means the LLM has attempted a layer but it has not been reviewed by a domain specialist. Only `status: complete` fields should be trusted for generation.

---

## Your Next Steps

**If you are a domain specialist:**

1. Read `0-Project/editorial-guidelines/` — especially the transliteration conventions and how to handle commentarial divergence
2. Read `0-Project/roles/domain-specialists/` — how to evaluate and correct LLM output
3. Browse one complete verse package in `2-Authoritative-Context/` to see the schema in practice before contributing
4. Contact the project coordinator to be assigned a text section to review

**If you are an AI skills designer:**

1. Read `CLAUDE.md` in full — the schema the LLM works from
2. Read `0-Project/technical-documentation/` — Obsidian setup, Claude Code configuration, the resolver script
3. Read `0-Project/roles/ai-skills-designers/` — ingest and transformation prompt design
4. Run the lint pass on an existing section to understand the coverage map before building new workflows

**If you bring both skill sets:** Start with `CLAUDE.md` and one complete verse package. The schema is the foundation everything else builds on.

---

## Questions and Contributions

All structural decisions about the schema, folder conventions, and reference systems are documented in `CLAUDE.md`. If you believe a decision needs revisiting, raise it with the project coordinator before making changes — the schema is shared infrastructure and changes cascade through every file in the project.

All contributions to `2-Authoritative-Context/` require a human-sources citation. All contributions to `4-Wiki/` require an authoritative-context citation. No exceptions.