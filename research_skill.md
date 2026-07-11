---
name: paper-analysis
description: Analyze, summarize, or extract knowledge from research papers (ML, AI agents, LLMs, RAG, memory systems, robotics, or any technical paper). Use this skill whenever the user shares a paper (PDF, arXiv link, or text) and asks to read it, summarize it, break it down, take notes on it, evaluate whether it's worth implementing, or fill out a paper-reading template. Also use when the user asks "is this paper relevant to my research", "can I productionize this", "explain this paper to me", or wants a structured literature-review note for any academic paper.
---

# Research Paper Knowledge Extraction

Turn a research paper into a structured, engineering-focused knowledge note using a proven multi-pass reading strategy and a comprehensive extraction template.

There are two parts to this skill:

1. **How to approach the paper** — the reading strategy below. Follow it in order; it prevents wasted effort on papers that don't deserve a deep read.
2. **What to extract** — the full template in `references/extraction-template.md`. Read that file when producing the final note.

---

## Part 1: How to Approach a Research Paper

Never read a paper linearly from page 1 to the end on the first attempt. Use the three-pass method, with an engineering filter between passes. Each pass has an exit ramp — most papers should be abandoned or downgraded before Pass 3.

### Pass 0 — Triage (2 minutes)

Before reading anything in depth, answer:

- Who wrote it and where was it published? (Lab reputation and venue are weak but useful priors.)
- How old is it, and has it been superseded? For fast-moving areas (agents, RAG, LLM training), an 18-month-old paper may already be obsolete — check citations and follow-up work.
- Is there code? A paper with a maintained repo is worth roughly 2x one without, for engineering purposes.
- Does the abstract's claimed contribution overlap with the reader's actual problems?

**Exit ramp:** If the answer to the last question is no, file it under "Future Reference" in the metadata table and stop.

### Pass 1 — The Skeleton Read (10–15 minutes)

Read ONLY, in this order:

1. Title, abstract, introduction
2. Section and subsection headings (skim everything else)
3. **Every figure and table, including captions** — authors put their best evidence here; figures often explain the method better than the prose
4. The conclusion
5. Skim the references, mentally ticking off ones already known

After Pass 1, be able to answer the five Cs:

- **Category** — What type of paper is this? (new method, measurement/benchmark, systems, position paper)
- **Context** — Which prior work is it built on? What's the baseline it fights?
- **Correctness** — Do the assumptions seem valid at a glance?
- **Contributions** — What are the claimed contributions?
- **Clarity** — Is it well written enough to be worth a deeper pass?

**Exit ramp:** Fill in only sections 1–3 of the template (Executive Summary, Core Contribution, Problem Statement) and stop if the paper is interesting-but-not-relevant. That's a legitimate, complete outcome.

### Pass 2 — The Comprehension Read (~1 hour)

Read the whole paper, but still skip proofs and heavy derivations. While reading:

- Rewrite the core idea in your own words as you go — if a paragraph can't be paraphrased, that's a flag to re-read, not skip.
- For every figure/plot: check axes, error bars, and whether the comparison is fair (same compute? same data? tuned baselines?).
- Mark every unread reference that seems load-bearing — these go in Section 19 (References Worth Reading).
- Write down every point of confusion immediately in Section 24 (Questions While Reading). Confusion is data: it's either a gap in the reader's background or a gap in the paper.
- Actively hunt for what the authors _don't_ say: missing baselines, cherry-picked benchmarks, "we leave X to future work" (often means X failed).

After Pass 2, be able to summarize the paper's main thrust with supporting evidence to another engineer in five minutes (this becomes Section 5, Key Idea).

**Exit ramp:** Most papers end here. Fill the full template except Sections 7–9 (algorithm walkthrough, math, component breakdown) which can stay high-level.

### Pass 3 — The Virtual Re-implementation (2–5 hours, rare)

Only for papers rated "Must Read" that will be built on or reproduced. Mentally (or actually) re-implement the paper:

- Make the same assumptions as the authors and try to reconstruct the method from scratch. Every place where reconstruction diverges from the paper reveals either a hidden assumption or an innovation worth noting in Section 23 (Engineering Notes).
- Work through every equation: assign meaning to each variable, and answer "why does this term exist?" — this fills Section 8.
- Challenge every design decision: "what if this component were removed/replaced?" Compare against the ablations in Section 16; anything they didn't ablate is a research opportunity for Section 25.
- If code exists, read it. Papers and their code frequently disagree; the code is the truth.

### Cross-cutting reading habits

- **Read adversarially, extract generously.** Assume claims are overstated until the evidence supports them, but actively look for reusable ideas even in flawed papers. A paper can be wrong overall and still contain one great trick.
- **Numbers before narrative.** Trust tables over prose. If the prose says "significantly outperforms" and the table says +0.4%, believe the table.
- **Always ask "what did this cost?"** Accuracy gains that require 10x compute, proprietary data, or brittle pipelines usually score low on Production Value.
- **Anchor to your own problems.** After every section, ask Andrew Ng's four questions: What did the authors try to accomplish? What were the key elements? What can I use myself? What references should I follow?
- **Batch by topic.** Reading 3–5 papers on the same topic in one week compounds understanding far more than reading them months apart; shared baselines and notation start to become visible.

---

## Part 2: Producing the Extraction Note

Once the appropriate pass is complete, read `references/extraction-template.md` and fill it out. Rules for filling it:

1. **Match depth to the pass reached.** Pass 1 → sections 1–3 + metadata + verdict. Pass 2 → everything except deep math. Pass 3 → the full template.
2. **Never copy the abstract or any text verbatim.** Every section must be a paraphrase in plain engineering English. If it reads like the paper, rewrite it.
3. **Section 10 (AI Agent Relevance) is the highest-priority section** for this user — always complete it thoughtfully, even for papers outside the agents space. Explain _exactly how_ an idea maps to agent memory, planning, tool use, retrieval, etc., or state clearly that it doesn't map.
4. **Be concrete in scores.** Every /10 rating in Sections 2 and 27 needs a one-line justification. "7/10 — novel retrieval trick but only tested on toy benchmarks" beats a bare number.
5. **Record actual numbers** in Section 15, with baseline vs. proposed side by side. Note whether improvements come with error bars/significance tests; if not, say so.
6. **The cheat sheet (Section 29) is written last** and must stand alone — someone who reads only that page should get 80% of the value.
7. **Honest verdicts.** "Should I Build It? → No" is a perfectly good outcome. The template's job is to save future time, not to make every paper look valuable.

### Output format

Produce the filled template as a Markdown file (`<short-paper-name>-notes.md`) unless the user asks for another format. Preserve the template's section numbering so notes are comparable across papers. Keep Mermaid diagrams for architecture (Section 6) — one clear flowchart beats three paragraphs.

If the user provides only a link or title, fetch/search for the paper first. If the paper can't be fully accessed, say so explicitly and mark unfilled sections as "not available from accessible text" rather than guessing.
