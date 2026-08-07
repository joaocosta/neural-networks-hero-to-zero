# Neural Networks: Zero to Hero

An [Obsidian](https://obsidian.md/)-compatible study vault for the **Neural Networks: Zero to Hero** lecture series.

> https://www.youtube.com/watch?v=VMj-3S1tku0&list=PLAqhIrjkxbuWI23v9cThsA9GvCAUhRvKZ&pp=0gcJCbYEgXWwhyeT

## Lectures

1. [[lectures/01-micrograd/notes|The spelled-out intro to neural networks and backpropagation: building micrograd]] — `not-started`
2. [[lectures/02-makemore-language-modeling/notes|The spelled-out intro to language modeling: building makemore]] — `not-started`
3. [[lectures/03-makemore-mlp/notes|Building makemore Part 2: MLP]] — `not-started`
4. [[lectures/04-makemore-activations-gradients-batchnorm/notes|Building makemore Part 3: Activations & Gradients, BatchNorm]] — `not-started`
5. [[lectures/05-makemore-backprop-ninja/notes|Building makemore Part 4: Becoming a Backprop Ninja]] — `not-started`
6. [[lectures/06-makemore-wavenet/notes|Building makemore Part 5: Building a WaveNet]] — `not-started`
7. [[lectures/07-build-gpt/notes|Let's build GPT: from scratch, in code, spelled out.]] — `not-started`
8. [[lectures/08-state-of-gpt/notes|State of GPT]] — `not-started`
9. [[lectures/09-gpt-tokenizer/notes|Let's build the GPT Tokenizer]] — `not-started`
10. [[lectures/10-reproduce-gpt-2/notes|Let's reproduce GPT-2 (124M)]] — `not-started`

## Study workflow

1. Open the next lecture's `notes.md`; use [[templates/lecture-notes|the lecture template]] when adding future lectures.
2. Confirm its title, video URL, source references, and status.
3. Take notes while watching the lecture. Put committed screenshots and diagrams in the lecture's `attachments/` directory.
4. Download the original transcript to `transcripts/raw.md`.
5. Deduplicate and clean it into `transcripts/clean.md`. Transcripts stay local and are ignored by Git.
6. Clone referenced repositories into the ignored `workspace/` directory. Push meaningful work to a personal fork and update [[sources|the source registry]] with its branch and tested commit.
7. Give `prompts/lecture-review.md` and the target lecture directory to a coding agent for an adaptive knowledge review.
8. Commit the generated dated review under the lecture's `reviews/` directory.
9. Change the lecture to `complete` only after explicitly accepting a `ready` verdict.

## Lecture layout

```text
lectures/NN-short-title/
├── notes.md                 # Authored notes and lecture metadata
├── attachments/             # Committed images and diagrams
├── transcripts/             # Ignored local transcript variants
│   ├── raw.md
│   └── clean.md
└── reviews/                 # Committed assessment history
    └── YYYY-MM-DD-HHMM.md
```

Transcript files should include provenance frontmatter such as the video URL, retrieval date, language, and whether the source captions were automatic or human-authored.

## Status values

- `not-started`
- `in-progress`
- `reviewing`
- `complete`

## Vault map

- [[sources]] — source repository registry
- [[prompts/lecture-review|Lecture review prompt]] — coding-agent assessment instructions
- [[templates/lecture-notes|Lecture notes template]]
- [[templates/review|Assessment record template]]

## Git policy

Commit authored notes, attachments, source references, and assessment history. Do not commit transcripts, disposable source checkouts, or Obsidian session state.
