---
title: "How I Built a Content Operating System With Claude Code (Technical Breakdown)"
source: "https://www.linkedin.com/pulse/how-i-built-content-operating-system-claude-code-technical-mike-bayly-sclke/?trackingId=Q44B6M%2Fo4u6oMp4LPX7auA%3D%3D"
author:
published: 2001-04-07
created: 2026-04-13
description:
tags:
  - "clippings"
---
The first time the linter (an automated quality checker) ran for me in late 2025, it scored a draft post across eight quality gates in seconds. It identified six violations with exact locations and fix suggestions, repaired every one, re-linted itself, and hit 38 out of the 40 grade score developed. My job was to read the output from Claude Code and decide whether it matched the intent of what I'd briefed AI on in the first place for the content piece, and make sure it hit with my preferred tone.

That experience changed how I think about content production using AI.

Not because the new system was impressive (although it genuinely was), but because it revealed the gap between how most people use AI for content and what becomes possible when the approach shifts from conversation to infrastructure that compounds.

Drawing on [Gareth Flynn](https://www.linkedin.com/in/garethflynn/) 's recent article where he raised [Larridin's AI proficiency model](https://larridin.com/blog/ai-proficiency-maturity-model#the-ai-proficiency-and-maturity-framework), Gareth identified five AI cohorts: non-users, experimenters, regular users, power users, and **system builders**.

System builders represent 1-3% of the workforce. The productivity gap between those cohorts is not linear, and it is exponential at the top between system builders and the power user tier. At this level of capability, the difference is not prompting skill but the depth of human expertise encoded into the system and the infrastructure around it.

[Tim Rayner](https://www.linkedin.com/in/tim-rayner-superesque/) calls this **cognitive archaeology**: the deliberate excavation of tacit knowledge, the unspoken expertise and decision-making patterns that experts carry but rarely articulate. Not better prompts, just better encoding of what is already known.

What follows is a walkthrough of what a system builder's content infrastructure looks like in production: a content operating system that:

1. Programmatically enforces quality
2. Stress-tests arguments before publication, and
3. Feeds performance data back into itself so every piece is informed by everything that came before.

### The Content Pipeline: Five Stages That Separate Thinking From Writing

The thing most people call an "AI content system" is a prompt and a prayer. The thing that actually compounds and improves is closer to a **production pipeline** with enforcement, memory, and feedback loops.

Content in this system moves through five discrete stages, each with gate checks, before anything gets published. Ideas get structured and stress-tested before a single word is drafted.

The stages run as reusable [**skills**](https://theaicorner.co.nz/p/claude-skills-are-so-good-i-m-rebuilding-all-of-my-workflows) [in Claude Code](https://theaicorner.co.nz/p/claude-skills-are-so-good-i-m-rebuilding-all-of-my-workflows). Think of skills like saved playbooks for the AI. For example, one skill might check whether a LinkedIn post is too long, too wordy, or missing a clear hook, then suggest fixes. These are instruction files the system follows on command, not custom software I coded.

1. **Capture** ingests sources verbatim: articles, transcripts, voice notes, and research flow in with metadata attached, with no processing and no summarising.
2. **Ideas** runs an eight-phase extraction process with a specificity gate. Generic ideas get rejected at this stage. The system scores each one against tension, evidence availability, and funnel alignment before it advances.
3. **Structure** builds the full argument architecture: thesis, tension, pillars, belief change mapping across six stages, and story pairing. This is where the skeleton takes shape, and where most of the intellectual work happens, with the human heavily involved.
4. **Challenge** runs five reader archetypes against the structure. A sceptic, a competitor, a time-poor executive, a technical expert, and a been-burned-before buyer each generate objections that get absorbed into the architecture before drafting begins.
5. **Production** drafts, lints, fixes, and re-lints in an autonomous loop until the content passes both a programmatic linter and an AI critique layer.

The challenge here is not producing one good post. It is producing five posts a week, a newsletter, and derivative content while maintaining the same voice and quality bar for months. That is where manual processes break, and pipelines like this exist because compounding requires consistency that human discipline alone cannot sustain at scale.

To be clear about what is automated and what is not:

- I choose which ideas advance.
- I confirm the architecture, direct the stress-test mitigations, and make the final editorial call on what gets published.
- The system handles enforcement and memory.
- I handle direction and (although cringe to say it) 'taste'.

### The Linter: Programmatic Quality & Rule Enforcement

If skills are the playbooks that guide the process from the beginning, the linter is the quality checker at the end. In this case, the linter is not using an LLM to make judgement calls. It is a standard Python script that applies eight fixed rules to every draft, scores each one, and reports exactly what failed. That makes the feedback consistent, measurable, and repeatable.

The content linter is a **Python script** that Claude Code wrote to my specifications. It runs eight passes against every piece of content, scoring each from one to five, with a target of **40 out of 40**. This moves us from subjective, non-determinstic feedback, to deterministic scoring against measurable rules. Examples of the checks and balances it runs:

1. **Auto-fail detection** checks **136 banned phrases** across 19 categories: AI slop, corporate jargon, hedge words, weak preambles, false voice patterns, and punctuation violations. Any single violation fails the entire pass.
2. **Hook quality** scores against a formula combining specificity, tension, stakes, and reader relevance.
3. **Structural integrity** verifies framework presence, belief change architecture, story pairing, and evidence for major claims.
4. **Sentence mechanics** targets the sentence structure and average sentence length I prefer, within specific ranges. It catches staccato rhythm, one of the most common forms of AI slop, that reads like a TED script and flags dense runs of long sentences that lose momentum.
5. **Voice alignment** measures against a **JSON voice profile**: a machine-readable specification of my voice with banned words, rhythm rules, and tone targets. It also carries 12 documented failure patterns the system has learned to avoid.
6. **Evidence and credibility** checks that every claim has a source. No statistics float without attribution, and examples must be specific rather than hypothetical.
7. **Formatting** enforces scannability. Single-sentence paragraph density, arrow lists instead of bullets, and bold density between 15-25 instances per thousand words.
8. **Polish** catches UK spelling, punctuation consistency, and how many times specific conversational markers appear.

The part most content teams skip when working with AI: on top of the eight programmatic passes, a multi-dimension AI critique layer runs independently. It generates multiple alternative hooks with ratonale and compares them against the original. It asks whether someone else could have written this without my experience to confirm the content piece is more unique than not.

All gates must pass before content reaches me for review.

The thing is that the linter measures compliance with my preferences. 'Voice' is a system of constraints. Those 136 auto-fail phrases are the accumulated result of months of publishing, reviewing, and encoding my preferences and what works through reader feedback into a machine-readable format and rules.

### The Feedback Loop: Every Post Makes the Next One Better

The linter enforces quality, whereas the feedback loop improves the definition of quality over time.

A **LinkedIn analytics scraper** skill runs on a scheduled cron job, one of many automated jobs on an always-on Mac Mini. It pulls impressions, reactions, comments, and reposts, then matches posts in my Obsidian knowledge vault to published articles by date and title similarity, analysing performance and engagement by funnel stage, hook type, format, and post length.

Those findings flow as append-only updates into **17 foundations files** that the production engine loads before every content drafting cycle. The voice DNA profile, audience personas, a hook workshop with 12 formulas and 28 proven examples, LinkedIn structural patterns, and a persuasion pipeline for conversion content.

The **common-issues log** is where the system's self-correction becomes tangible. It holds 12 named failure patterns. For example:

- RALPH\_OVER\_BOLD tracks excessive formatting.
- RALPH\_MISSING\_BRIDGE catches contrarian claims stated without explaining the mechanism behind them.
- RALPH\_INCOMPLETE\_PAYOFF flags insights that stop at observation without translating into a business outcome.

Each pattern has a root cause, a prevention rule, and the date it was first observed. Every production run checks this file before drafting a post, and when new patterns emerge they get appended automatically to the relevant files.

This is about creating not just a self-enforcing loop, but a proper self-correcting one. Ultimately the feedback loop is what separates a static system from one that compounds.

- Posts that score well on the linter and perform well with the audience validate the rules.
- Posts that score well but underperform trigger a review of the rules themselves.

### The Wiki: Knowledge That Compounds Across Everything

The final layer is a persistent **knowledge base** inspired by [Andrej Karpathy's approach](https://x.com/karpathy/status/2039805659525644595) to compounding knowledge. It holds hundreds ofpages across two types:

1. Concept pages (covering themes and frameworks), and
2. Entity pages (covering people, companies, and products).

It is powered by a local search engine running **BM25 keyword search** (finds exact word matches) and **semantic vector search** (finds similar meaning, even with different wording) through an MCP connector (a bridge that lets Claude Code connect to external tools and data sources).

The net result is every article published, every source captured, and every transcript mined feeds back into the wiki, building a compounding knowledge corpus that can be queried to uncover relationships between concepts, companies, people, source material, and recurring performance patterns.

- The content pipeline queries it before structuring each new piece, pulling evidence, finding contradictions that become content angles, and identifying gaps that become content opportunities.
- Performance data from LinkedIn analytics feeds back into concept pages as empirical evidence.
- A daily automated sync captures new content.
- A weekly health check flags contradictions, orphan pages, and stale claims.

This is not a reference library. It is a compounding asset where published articles strengthen concept pages, performance data validates claims, and contradictions surface future ideas.

### What This Means (and Where to Start)

The challenge most people assume is technology, but it is not.

This system was built by me, a knowledge worker, not a software engineer.

- Claude Code wrote the Python linter.
- The skills are markdown instruction files, written by Claude Code.
- The knowledge layer is plain markdown and JSON in an Obsidian vault synced to Google Drive, and skills replicated in Github as back up.

Nothing is locked into any platform, and the expertise survives any tool change as it's all modular and can be worked with in other tools, like OpenAI's Codex.

It was also not built in a week either. The first version was a foundations file and a drafting prompt, and over time, each piece was added when the previous one was working. The linter came later, the analytics loop after that, and the wiki after that.

The hard part was never technical ability. It was clarity about my own voice, my standards, and my workflow, and codifying scoring systems to ensure AI can determine a pass/fail against standards.

How to set up a system this week:

1. **Capture everything**, including what you hate as much as what you love. Banned phrases, lazy thinking you reject, and perspectives you find dishonest shape the system as much as the positive inputs.
2. Build a **foundations folder** with voice rules, structural preferences, and the evidence you return to. This is the minimum viable version of everything described above.
3. Wire one **feedback loop**: even a manual review of what performed well last month, written into a file the system reads next cycle, changes the trajectory.
4. Add **one piece per week**. In a month it becomes a system, and in three months it compounds.

A normal week now looks like this:

1. Drop in an idea or a transcript.
2. The pipeline structures it with my guidance, stress-tests the argument, drafts against foundations and messaging pattersn, lints programmatically, and presents something to review.
3. Edit, confirm, and publish.
4. Analytics run on schedule, and foundational documents update themselves.

The job is taste and direction, and the system handles enforcement and memory. Build a foundations folder this week and start there.