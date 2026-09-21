This repository contains my flashcard collection. I use
[hashcards](https://github.com/eudoxia0/hashcards), which stores cards in plain
text and schedules reviews with spaced repetition. The aim is to remember
things worth knowing, and understand them well enough to use them.

# Format

Cards live under `cards/`, grouped by topic. Each Markdown file is a **deck**;
its filename without `.md` is its **deck name**. For example,
`cards/computers/kafka.md` is the deck `kafka`. Follow the existing organization
and use lowercase filenames.

**Basic cards** have a question and an answer:

```markdown
Q: What's Kafka's consumer model: push or pull?
A: Pull.
```

Both sides can span multiple lines, and support Markdown lists and code blocks.
Separate cards with blank lines; horizontal rules (`---`) are optional.

**Cloze cards** hide the text in square brackets:

```markdown
C: [Partitions] are Kafka's unit of parallelism and ordering.
```

Each bracketed passage creates a separate prompt. Hashcards normally shows
only one of these related prompts per session, so one doesn't give away another.

**Term-definition notation** creates cloze prompts in both directions:

```markdown
T: Temporal locality
D: Recently accessed data is likely to be accessed again soon.
```

Math uses `$...$` inline and `$$...$$` for blocks. Shared TeX macros belong in
`cards/macros.tex`. Images and audio use `![](path/to/file)`, with paths relative
to the deck; `@/` makes a path relative to `cards/` instead.

For less common features, consult the
[Hashcards documentation](https://github.com/eudoxia0/hashcards#readme).

# Rules for Writing Cards

## Understand Before Memorizing

Don't turn an explanation you haven't understood into something to recite.
Find the missing idea and start there. Build difficult concepts from questions
whose answers already make sense.

## Keep Cards Atomic

Ask one narrow question with a brief answer. I should know exactly what I
remembered or forgot. Split a sprawling answer into smaller cards; keep an
overview question when recalling how the pieces fit together is useful.

## Make the Answer Unambiguous

A card should make sense months later, without the paragraph it came from.
Include the context needed to identify the answer. If a claim depends on a
version, configuration, or assumption, say so. Attribute uncertain findings
to their source rather than presenting them as settled facts.

## Make Every Word Earn Its Place

Longer answers mean longer reviews. Prefer a precise phrase to a paragraph.
Keep my supplied answers as written unless you can shorten them without losing
meaning. If something seems wrong, flag it and propose a correction.

## Ask in Both Directions

When the reverse question has a clear answer, ask it too. Knowing a definition
and recognizing the thing it describes are both useful:

```markdown
Q: What's temporal locality?
A: Recently accessed data is likely to be accessed again soon.

Q: What locality principle describes accessing the same data again soon?
A: Temporal locality.
```

## Connect the Ideas

Ask about examples, intuition, contrasts, and applications as well as names
and definitions. Choose useful connections to what I already study. Different
questions should offer different ways into an idea, not merely repeat it.

## Be Selective

Choose material that matters to my interests and work. An article is not a
checklist of sentences to memorize. Every new card asks for future attention;
give it a reason to be here.

# Working with the Collection

Run commands from the repository root. Always use `cards` as the collection
path so review history stays in one place.

After adding or editing cards, validate them and inspect the card count:

```sh
hashcards check cards
hashcards stats cards --format=json
```

Fix format errors before calling the work done, and report the result. The
check catches collection problems; it doesn't fact-check the answers.

To study all cards or a single deck:

```sh
hashcards drill cards
hashcards drill cards --from-deck=kafka
```

Use `--card-limit=20 --new-card-limit=5` for a smaller session. Hashcards opens
at `http://localhost:8000`. Finish the session or click End to save progress
before stopping it. Restart to load edited cards.

Cards are identified by their contents, so editing a card resets its progress.
Avoid cosmetic rewrites. Review history lives in `cards/hashcards.db`, which
is tracked in Git so progress can move between computers. Finish the session
and stop Hashcards before committing the database. SQLite journal, WAL, and
SHM files are temporary and remain ignored; checkpoint any WAL before taking
a snapshot. Do not reset the database unless I ask.

Pull before studying on another computer, then commit and push saved progress
before switching back. Study on one computer at a time: Git cannot merge
independent changes to the database.

# Scope and Git

Do the requested work without adding unrelated files or documentation.
Commit and push only when asked. Use `karanchawla`'s GitHub credentials.

Reference: [Augmenting Long-term Memory](https://augmentingcognition.com/ltm.html).
