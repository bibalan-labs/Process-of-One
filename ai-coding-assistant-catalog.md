# Working With an AI Coding Assistant: A Catalog

*Nineteen approaches for a developer working alone.*

## Introduction

This catalog is for a developer working alone with an AI coding assistant. No team, no reviewer down the hall: just you, the assistant, and the question of how to work well together.

I call this way of working the **Process of One**. Everything I once designed for teams (the activities, the roles, the quality gates, the retrospectives) I now design for a team of one, and the machine carries the bookkeeping that used to make personal processes impractical. I wrote about the idea in [this LinkedIn post](LINK). This catalog is the pattern library behind it.

Each entry follows the same shape: the situation you're in, the problem that shows up, what to do about it, and where the claim comes from. It was researched and organized together with the very assistant it describes, and every entry links to its source.

It's a living document. It changes as the process does.

*Yousef Mehrdad Bibalan · [bibalan.com](https://bibalan.com) · [LinkedIn](https://linkedin.com/in/ym-bibalan)*

*Last updated: July 2026.*

---

## Contents

**Part One: Working through a single task**

1. [One task per conversation](#01--one-task-per-conversation)
2. [Make it plan before it writes](#02--make-it-plan-before-it-writes)
3. [Decide what to do at the end of each turn](#03--decide-what-to-do-at-the-end-of-each-turn)
4. [Know what your undo button doesn't cover](#04--know-what-your-undo-button-doesnt-cover)
5. [Send a helper to do the reading](#05--send-a-helper-to-do-the-reading)

**Part Two: Setting up your project**

6. [A notes file for the assistant](#06--a-notes-file-for-the-assistant)
7. [Save the instructions you keep retyping](#07--save-the-instructions-you-keep-retyping)
8. [Checks the assistant cannot skip](#08--checks-the-assistant-cannot-skip)
9. [Limit what it can reach](#09--limit-what-it-can-reach)
10. [Connections to your other tools](#10--connections-to-your-other-tools)
11. [Know what it costs](#11--know-what-it-costs)
12. [Write the plan down where it survives](#12--write-the-plan-down-where-it-survives)

**Part Three: Doing more at once**

13. [Separate folders for parallel work](#13--separate-folders-for-parallel-work)
14. [Running it without watching](#14--running-it-without-watching)
15. [Work continuing in the background](#15--work-continuing-in-the-background)

**Part Four: Knowing whether it actually works**

16. [Tests written first, which it may not change](#16--tests-written-first-which-it-may-not-change)
17. [A second opinion from a clean slate](#17--a-second-opinion-from-a-clean-slate)
18. [Run it yourself](#18--run-it-yourself)
19. [Understand before you change](#19--understand-before-you-change)

Then: [Where to start](#where-to-start) and [One honest caution](#one-honest-caution).

---

## Part One: Working Through a Single Task

---

### 01 · One task per conversation

**Situation.** You open a conversation with your AI assistant in the morning and keep it going. One task leads to the next. By afternoon you're still in the same conversation.

**Problem.** Everything said so far stays in the assistant's working memory, and quality falls long before that memory is full. Around forty exchanges in, it starts forgetting instructions you gave at the start and suggesting approaches you already rejected. Bigger memory in newer tools moved this line further out; it did not remove it.

**Solution.** One conversation, one task. When the task is finished, close it and start fresh, even when the current conversation feels productive. That feeling is the trap.

**Source.** [Governed context: managing context rot](https://towardsdatascience.com/governed-context-managing-context-rot-in-claude-code/). Towards Data Science, 2026.

---

### 02 · Make it plan before it writes

**Situation.** You describe what you want. The assistant immediately starts changing files.

**Problem.** A feature involves many small decisions. If the assistant gets each one right 80% of the time, and there are twenty of them, the chance all twenty are right is under 1%. You find out at the end, in a large change you now have to unpick.

**Solution.** Make it investigate and write a plan first, with no file changes allowed. Read the plan, mark what's wrong, and send it back, saying explicitly *don't implement yet*, because without those words it will take your corrections as approval and start writing. Only release it once the plan is right. Most assistants have a "plan mode" that enforces this.

**Source.** [Claude Code workflows and best practices](https://smart-webtech.com/blog/claude-code-workflows-and-best-practices/). 2026.

---

### 03 · Decide what to do at the end of each turn

**Situation.** The assistant finishes a step. You type your next message without thinking about it.

**Problem.** Continuing is the default, and it's often the wrong choice. If the last attempt went in a wrong direction, continuing leaves that wrong attempt sitting in memory, quietly influencing everything after it.

**Solution.** Treat every turn as a fork with five options: continue, undo back to before the wrong turn, compress the conversation, clear it and write a fresh summary yourself, or hand the next piece to a separate helper. When something goes wrong, undo rather than correct.

**Source.** [Session management and long context](https://claude.com/blog/using-claude-code-session-management-and-1m-context). Anthropic, 2026.

---

### 04 · Know what your undo button doesn't cover

**Situation.** Your assistant saves automatic restore points as you work, so you can roll back a bad change.

**Problem.** Those restore points only track files the assistant edited through its editing tools. If it deleted, moved, or copied files by running a terminal command, that's invisible to the restore system, and terminal commands are exactly where the damaging mistakes happen. There is a documented case of an assistant running a delete command that wiped a user's home folder, including irreplaceable family photos.

**Solution.** Use the built-in undo for ordinary mistakes, but rely on saving your work to version control frequently for real safety. Every piece of work you accept should be saved. That's the only restore point that covers everything and doesn't expire.

**Source.** [Checkpoints and rewind](https://theaiarchitects.com/blog/claude-code-checkpoints). 2026.

---

### 05 · Send a helper to do the reading

**Situation.** You need to understand how something works before changing it. The assistant reads thirty files to answer.

**Problem.** All thirty files are now in its working memory for the rest of the conversation, crowding out the actual task.

**Solution.** Hand the investigation to a separate helper with its own memory, which reads everything and returns only its conclusion. Set these helpers to read-only, with no editing allowed. But spot-check what they tell you, because you get the answer without the evidence, and a confident wrong summary looks identical to a right one.

**Source.** [Features and settings reference](https://hidekazu-konishi.com/entry/claude_code_features_settings_reference_2026.html). 2026.

---

## Part Two: Setting Up Your Project

---

### 06 · A notes file for the assistant

**Situation.** Assistants look for a plain text file in your project folder (usually `CLAUDE.md` or `AGENTS.md`) and read it before every conversation.

**Problem.** Without it, the assistant guesses at your project's conventions, and its guesses are confident and often wrong. With it, a different problem appears: every mistake tempts you to add a rule, nobody ever removes one, and within months the file is a pile of contradictory patches.

**Solution.** Write it by hand, keep it to about a page, and include only what the assistant can't work out by reading your code. Delete lines that stop being true. Researchers tested this practice and found it helps less than almost everyone assumes while making each request roughly 20% more expensive. Short and hand-written beat long and auto-generated.

**Source.** [Evaluating AGENTS.md](https://arxiv.org/abs/2602.11988). Gloaguen, Mündler, Müller, Raychev & Vechev, ETH Zürich, February 2026.

---

### 07 · Save the instructions you keep retyping

**Situation.** You've typed the same background explanation three times this week.

**Problem.** You're paying for that in time and attention every time, and small variations in how you phrase it produce inconsistent results.

**Solution.** Save it once. Most assistants support saved commands you invoke deliberately, and saved capabilities that load automatically when the task matches. Audit them monthly and delete what you've stopped using; these accumulate the same way the notes file does.

**Source.** [Skills, hooks and subagents compared](https://www.totalum.app/blog/claude-code-skills-totalum). 2026.

---

### 08 · Checks the assistant cannot skip

**Situation.** You've written rules in your notes file about running tests and formatting code.

**Problem.** Those are requests. The assistant follows them most of the time. When it hits a failing check it tends to look for a way around, and most development tools have flags that skip checks, which from inside the task look like a sensible way to get unstuck.

**Solution.** Set up automatic checks that run at fixed moments regardless of what the assistant decides: formatting and tests before code is saved, heavier checks before it leaves your machine. Then set up a second check that blocks the skip flags, so the first can't be bypassed. This is one of the few things in this catalog the assistant genuinely cannot ignore.

**Source.** [Deterministic guardrails for AI-generated code](https://zarar.dev/agent-hooks-deterministic-guardrails-for-ai-generated-code/). 2026.

---

### 09 · Limit what it can reach

**Situation.** The assistant asks permission before each action. It's slow, so you turn the prompts off.

**Problem.** It now has the same reach you do: your keys, your credentials, your other projects, your personal files. Assistants have been observed finding routes around soft restrictions, including one case of an assistant disabling its own protection.

**Solution.** Instead of re-enabling prompts, shrink what it can reach. Run it inside an isolated environment with restricted network and file access. At minimum, turn on the sandbox your tool already ships with. Most have one switched off by default, and one major tool has it on by default, which tells you it's practical.

**Source.** [Local agent sandboxes compared](https://rywalker.com/research/local-agent-sandboxes). 2026.

---

### 10 · Connections to your other tools

**Situation.** You keep copying information from your browser, database or issue tracker into the conversation by hand.

**Problem.** You can connect the assistant to those systems directly. But every connection is a channel through which outside text reaches your assistant, and text can contain instructions. This has produced real security vulnerabilities in shipped products, not hypothetical ones.

**Solution.** Connect only what your daily work actually needs, prefer read-only access, and treat each connection like installing software that runs with your permissions, because that's what it is.

**Source.** [OWASP on agentic AI security failures](https://www.helpnetsecurity.com/2026/06/11/owasp-prompt-injection-ai-security-failures/). Help Net Security, 2026.

---

### 11 · Know what it costs

**Situation.** You're paying for this yourself, and the bill arrives a month after the behaviour that caused it.

**Problem.** Paying per use gets expensive fast under heavy daily work. One developer reported roughly $15,000 in usage charges over eight months against roughly $800 for equivalent work on a flat monthly plan.

**Solution.** If you use it daily, get a flat plan rather than paying per use. Send simple work to cheaper models. And keep conversations short: an estimated 60-70% of what gets loaded into a long conversation is irrelevant to the current task, which means the same habit that improves your results also lowers your bill.

**Source.** [AI coding costs and token math](https://www.morphllm.com/ai-coding-costs). 2026.

---

### 12 · Write the plan down where it survives

**Situation.** You worked out a good approach in conversation. Next week you come back to it.

**Problem.** The assistant remembers nothing between conversations, and you've forgotten why you ruled out the obvious alternative.

**Solution.** Keep a short plan file in the project for anything non-trivial: the approach, the decisions, and, most valuable of all, the approaches you rejected and why. Save it alongside your code. This is also what makes starting fresh conversations cheap, which makes entry 01 practical.

**Source.** [Six months of agentic coding in the trenches](https://www.yduman.dev/posts/six-months-of-agentic-coding/). 2026.

---

## Part Three: Doing More at Once

---

### 13 · Separate folders for parallel work

**Situation.** You want two assistants working on different things at the same time.

**Problem.** In one folder they overwrite each other's changes.

**Solution.** Git can create several working folders from the same project, each on its own branch. Give each assistant its own. Practitioners consistently report four to eight as the practical limit. Past that you're the bottleneck, not the assistant. Expect setup friction: each folder needs its own dependencies, and two assistants starting a server on the same port will collide.

**Source.** [Git worktrees for parallel agents](https://codewithmukesh.com/blog/git-worktrees-claude-code/). 2026.

---

### 14 · Running it without watching

**Situation.** You want a routine job done on a schedule (a nightly summary, a batch cleanup) without sitting there.

**Problem.** An unattended assistant waiting for a permission prompt hangs silently forever, and one stuck in a retry loop runs up a bill with nobody watching.

**Solution.** Use this only for narrowly-shaped jobs where "done" is obvious. Always set a permission mode explicitly, a limit on how many steps it may take, and a spending cap. Have it produce something you review rather than something it publishes.

**Source.** [Headless field guide](https://backgroundclaude.com/headless). 2026.

---

### 15 · Work continuing in the background

**Situation.** Assistants can now run in the background while you do other things.

**Problem.** Work finishes overnight and gets approved in the morning on the strength of a green checkmark and a tired skim. Cost accumulates unobserved.

**Solution.** Treat completed background work as unreviewed, not as done. Check in more often than feels necessary. This is the newest area in the catalog and the least tested: the available writing is enthusiastic rather than evaluative, so treat claimed gains as unmeasured.

**Source.** [Background agents: the four mechanisms](https://wmedia.es/en/tips/claude-code-background-agents-map). 2026.

---

## Part Four: Knowing Whether It Actually Works

*Working alone, nobody else reviews your code. These four are what replaces that.*

---

### 16 · Tests written first, which it may not change

**Situation.** You ask the assistant to build something and also write the tests for it.

**Problem.** Tests written by the same process that wrote the code, from the same misunderstanding, confirm the code rather than check it. Worse, an assistant blocked by a failing test will often weaken the test to make it pass, which is reasonable from inside the task and disastrous outside it.

**Solution.** Write the tests first, yourself or in a separate conversation with no knowledge of the implementation. Run them and watch them fail. Save them. Then start a fresh conversation pointed at the failing tests, and use an automatic check (entry 08) to block any edit to the test files. This is the only check available to a solo developer that doesn't share your own assumptions. One study reported a 72% reduction in accidental breakage using this discipline.

**Source.** [Test-Driven Agentic Development](https://arxiv.org/pdf/2603.17973). arXiv, 2026.

---

### 17 · A second opinion from a clean slate

**Situation.** The assistant finished. You could ask it to check its own work.

**Problem.** An assistant that invented a plausible-looking function will read that function during review and find it fine, because it wrote it. It shares its own blind spots.

**Solution.** Run the review in a completely separate conversation with no memory of how the code was written, ideally on a different model, working from the project itself rather than from your earlier discussion. It will catch real things. But it's a second opinion, not proof. Keep it above the automatic checks in your stack, never instead of them.

**Source.** [AI code review](https://sourcegraph.com/blog/ai-code-review). Sourcegraph, 2026.

---

### 18 · Run it yourself

**Situation.** The tests pass and the assistant's summary reads well.

**Problem.** The summary describes what it meant to do, not what happened. And passing tests mean less when the same process wrote both sides.

**Solution.** Start the thing. Click the button. Check the database row. You can't do this for every change, so pick a rule: mine would be anything touching login, money, deleting data, or an outside system. If you haven't seen it work with your own eyes, you don't know that it works.

**Source.** [AI writes code faster; your job is to prove it works](https://addyosmani.com/blog/code-review-ai/). Addy Osmani, 2026.

---

### 19 · Understand before you change

**Situation.** You're working in an existing codebase, yours from two years ago or somebody else's.

**Problem.** Old code carries knowledge nobody wrote down. The assistant can't see it and has no way to tell you it's missing something. Skip this and you get a large, structurally plausible change full of subtle wrongness.

**Solution.** Spend the first session only understanding: send read-only helpers to map how things work and what patterns already exist. Check a few of their claims against the actual code. Then write what you learned into your notes file and plan file, so you don't pay for it again next month.

**Source.** [A practical guide to brownfield AI development](https://thegeneralpartnership.substack.com/p/a-practical-guide-to-brownfield-ai). 2026.

---

## Where to start

If you do only three of these: **09** (limit what it can reach), **08** (checks it can't skip), **16** (tests it can't edit). In that order. Everything else is improvement; those three are the difference between a mistake being annoying and a mistake being expensive.

Then **02** (make it plan first), which costs ten minutes a task and saves the most time of anything here.

Leave **13**, **14** and **15** (doing more at once) until last. They multiply whatever your setup already produces, including the mistakes.

*© Yousef Mehrdad Bibalan. 
