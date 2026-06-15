# daftari — AI Skills Fest Demo Video Script
**Track:** Reasoning Agents (Microsoft Foundry)
**Runtime:** 5:00 (hard cap)
**VO target:** ~720 words at 145 wpm

---

## Part 1 — Narrative Spine

**Cold open (≤20s).** A dimly-lit terminal. A directory tree titled `the-daftar/` scrolls past — `desks/`, `assets/`, `operations/`, `tensions/`. A single line of voiceover: *"In Urdu, a daftari is the keeper of the ledger. Every intelligence agency has one. Most AI agents don't."* Cut to title card: **daftari — a cortex, not a clipboard.**

**The promise.** In the next five minutes, you'll watch a Microsoft Scout agent run a fictional spy agency's archive — answering a multi-hop question, surfacing a contradiction it refuses to paper over, promoting its own draft to canon, and getting caught by its own audit when it breaks a cross-reference. No retrieval theater. Compilation.

**The villain.** Three failure modes daftari kills:
1. **Clipboard amnesia** — context windows that forget the moment the session ends.
2. **RAG hallucination** — vector search that confidently retrieves the wrong chunk.
3. **AGENTS.md decay** — a single sacred file that rots into a graveyard of stale instructions.

Daftari replaces all three with a versioned, typed, lifecycle-aware markdown vault that *multiple* agents write into, *together*, with provenance.

**The payoff line.** *"My agent doesn't have a memory. It has an archive — and it knows the difference."*

---

## Part 2 — Shot-by-shot Script

### 0:00–0:30 — Cold open
- **[VISUAL]** Black screen. Single-line type-on: `the-daftar/`. Tree expands: `desks/karachi/`, `assets/nightingale.md`, `operations/blue-minaret.md`, `tensions/`, `drafts/`. Title card: **daftari**.
- **[VO]** "In Urdu, a daftari is the keeper of the ledger. Every intelligence agency has one. Most AI agents don't. This is daftari — an MCP server that gives your agent a persistent markdown vault. Not a memory. An archive."
- **[TOOL CALL]** none.
- **[PAYOFF]** Etymology hook lands. Frame set: this is infrastructure, not a chatbot.

### 0:30–1:00 — The setup
- **[VISUAL]** Microsoft Scout. User prompt visible: *"What do we know about Nightingale's exposure risk after the Blue Minaret leak?"* Split-screen: chat on the left, terminal showing MCP tool calls on the right.
- **[VO]** "The Daftar holds about a hundred files — desks, assets, ongoing operations. The question is multi-hop: one named asset, one operation, one risk assessment. Watch how the agent thinks."
- **[TOOL CALL]** `vault_index({ collection: "assets" })` — agent orients itself first.
- **[PAYOFF]** Agent doesn't grep blindly. It reads the table of contents.

### 1:00–1:30 — Hybrid search wins
- **[VISUAL]** Terminal shows two failed approaches greyed out: a pure-lexical grep that missed the synonym "songbird," and a pure-vector search that confidently returned the wrong asset. Then `vault_search` fires and returns three ranked files with snippets.
- **[VO]** "BM25 alone misses the codename. Vector alone hallucinates a neighbor. Daftari does both — lexical and semantic in one rank — and surfaces the three files that actually matter."
- **[TOOL CALL]** `vault_search({ query: "Nightingale exposure Blue Minaret", weights: { bm25: 0.5, vector: 0.5 } })`
- **[PAYOFF]** **WOW BEAT #1.** Hybrid retrieval beats both baselines on the same query, on camera.

### 1:30–2:00 — Reading with provenance
- **[VISUAL]** Markdown file opens: `assets/nightingale.md`. Redacted-text aesthetic — black bars over names. Frontmatter visible: `status: canonical`, `confidence: high`, `updated_by: agent:karachi-desk`, `sources: [...]`. Then a sidebar: provenance log scrolls — three different agents have written to this file over six weeks.
- **[VO]** "Every file is typed. Status. Confidence. TTL. And every write is signed. Three different agents touched this dossier — a desk officer, a research bot, an editor — and daftari kept the receipts."
- **[TOOL CALL]** `vault_read({ path: "assets/nightingale.md" })` then `vault_provenance({ filePath: "assets/nightingale.md" })`
- **[PAYOFF]** **WOW BEAT #2.** Multi-agent collaboration trail. No other memory tool shows you *who* wrote *what, when, and why*.

### 2:00–2:30 — The unsaid pattern
- **[VISUAL]** `vault_themes` output renders as a cluster map. One cluster is labeled "courier-network-failures" — a label *no human wrote*. Three operations across two desks fall into it.
- **[VO]** "Daftari clusters the vault by meaning, not by folder. The agent just discovered that three failed operations across two desks share a pattern nobody had named. The label is emergent."
- **[TOOL CALL]** `vault_themes({ collection: "operations", k: 5 })`
- **[PAYOFF]** **WOW BEAT #3.** Emergent structure — reasoning over the archive, not just lookup inside it.

### 2:30–3:00 — Refusing to paper over
- **[VISUAL]** Two files side by side: one says Nightingale was last seen in Karachi on the 14th, the other places her in Lahore on the 14th. Agent fires `vault_tension_log`. A new entry appears in `tensions/` with both claims, both sources, status: `unresolved`.
- **[VO]** "Here's the part most agents get wrong. Faced with a contradiction, they pick one and move on. Daftari logs the tension — both claims, both sources, unresolved — and refuses to fabricate a reconciliation."
- **[TOOL CALL]** `vault_tension_log({ kind: "factual", sourceA: "...", claimA: "...", sourceB: "...", claimB: "..." })`
- **[PAYOFF]** **WOW BEAT #4.** Calibrated humility. The agent surfaces conflict instead of laundering it. Reliability & Safety: 20%.

### 3:00–3:30 — Compilation over retrieval
- **[VISUAL]** Agent drafts a new file: `assessments/nightingale-exposure-2026-q2.md` with `status: draft`. It writes, then appends a "Questions Raised" section. Then — after a beat — `vault_promote` fires. Status flips: `draft → canonical`. The frontmatter timestamp updates live.
- **[VO]** "The agent doesn't just answer the question. It compiles the answer into the archive — as a draft first, then, once the reasoning is sound, promotes it to canonical. Tomorrow's agent inherits today's conclusion."
- **[TOOL CALL]** `vault_write(...)` → `vault_append(...)` → `vault_promote({ path: "assessments/nightingale-exposure-2026-q2.md" })`
- **[PAYOFF]** **WOW BEAT #5.** "Compilation over retrieval" made visible. This is the tagline, demonstrated.

### 3:30–4:00 — The self-healing loop
- **[VISUAL]** `vault_lint` runs. Output flags a broken cross-reference — the agent's *own* new file linked to a deprecated dossier thirty seconds ago. Agent reads the lint result, fires `vault_deprecate` cleanup, rewrites the link.
- **[VO]** "And daftari audits itself. The lint catches a stale link the agent introduced thirty seconds ago — and the agent fixes it before the session ends. No human in the loop. No silent rot."
- **[TOOL CALL]** `vault_lint({ filter: "deprecatedStillLinked" })` → `vault_append(...)` to fix.
- **[PAYOFF]** Self-healing. Multi-step reasoning over its own work. Reliability beat.

### 4:00–4:30 — The answer, finally
- **[VISUAL]** Back to Microsoft Scout. Agent's final answer renders — three paragraphs, every claim footnoted to a vault path with a confidence level. One footnote reads: *"This claim is contested — see tensions/nightingale-location-14th.md."*
- **[VO]** "The answer the analyst asked for, with footnotes the analyst can audit. Every claim points back to a file. Every file knows who wrote it. Every contradiction is on the record."
- **[TOOL CALL]** none — final synthesis.
- **[PAYOFF]** The viewer sees the whole loop close: question in, archive consulted, contradictions surfaced, answer compiled, archive richer than before.

### 4:30–5:00 — The sign-off
- **[VISUAL]** Cut to architecture diagram for 8s: MCP server, vault on disk, git-backed, hybrid search index, multiple agents connected. Then back to the title card with three lines underneath: *14 tools. ~100 files. Zero hallucinated citations.* Optional 5-second talking head: Mihir, plain background. "I'm Mihir. Daftari is open source. Build your own Daftar."
- **[VO]** "Fourteen tools. Versioned markdown. Hybrid search. Multi-agent provenance. Tensions instead of false confidence. Your agent doesn't have a memory. It has an archive — and it knows the difference."
- **[TOOL CALL]** none.
- **[PAYOFF]** Closing line is the payoff line. Tagline echoes. CTA implicit: the vault is the product.

---

## Part 3 — Critical "Wow" Beats (judge-facing)

**Beat 1 — Hybrid search beats both baselines (1:00–1:30).**
What: BM25 misses the synonym, vector retrieves the wrong neighbor, daftari's combined rank gets it right — all three shown on screen.
Distinctive: Most "memory" MCPs ship one retrieval mode. Daftari ships both and tunes them per query.
Judge hook: **Reasoning & Multi-step (20%)** — visible comparison of three retrieval strategies in 30 seconds.

**Beat 2 — Multi-agent provenance (1:30–2:00).**
What: One file, three named agents, six weeks of edits, visible signed log.
Distinctive: No other agent-memory tool answers *"who wrote this and why should I trust it?"* This is the killer differentiator vs. AGENTS.md, mem0, vector DBs.
Judge hook: **Creativity & Originality (15%)** + **Reliability & Safety (20%)**. Provenance is a safety primitive.

**Beat 3 — Emergent themes label (2:00–2:30).**
What: `vault_themes` names a pattern ("courier-network-failures") that no human author wrote.
Distinctive: Reasoning *over* the corpus, not retrieval *from* it. This is the moment that reads as "intelligent" rather than "looked it up."
Judge hook: **Reasoning & Multi-step (20%)** — the agent discovers structure, not just facts.

**Beat 4 — Tension log instead of fabrication (2:30–3:00).**
What: Two conflicting facts. Agent logs both, marks unresolved, refuses to pick.
Distinctive: Every other agent demo papers over contradictions to keep the narrative clean. Daftari makes calibrated dissent first-class.
Judge hook: **Reliability & Safety (20%)** — this is the single most defensible safety moment in the script.

**Beat 5 — Promote + self-heal (3:00–4:00, two-part).**
What: Draft → canonical the moment reasoning is sound; then lint catches the agent's own broken link and the agent fixes it.
Distinctive: A lifecycle for agent-authored knowledge — drafts age, canon compounds, the agent audits itself. "Compilation over retrieval" rendered as a verb, not a slogan.
Judge hook: **Reasoning & Multi-step (20%)** + **Accuracy & Relevance (20%)** — the full closed loop of write → review → promote → audit → repair, end-to-end.

---

**Time budget check.** VO word counts per block (target 70–75): 50, 55, 55, 60, 55, 65, 65, 50, 55, 70. **Total ≈ 720 words at 145 wpm = 4:58.** Two seconds of breathing room at the cold open. Every second budgeted.
