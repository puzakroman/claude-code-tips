# How to Cut Claude Code Costs in Half — Without Losing Quality

![Opus + Context7 + Sonnet](cover.png)

When you use Claude Code daily, the token bill adds up fast. Most developers just pick the most powerful model — Opus 4.7 — and don't think twice. But if you do the math, it turns out you can pay half as much while keeping the same code quality. Here's how.

## Same Price — Different Bill

Opus 4.7 and Opus 4.6 cost the same: $5 per million input tokens and $25 per million output tokens. Sonnet 4.6 is cheaper: $3 and $15 respectively.

| Model | Input / 1M tokens | Output / 1M tokens |
|---|---|---|
| Opus 4.7 | $5 | $25 |
| Opus 4.6 | $5 | $25 |
| Sonnet 4.6 | $3 | $15 |

At first glance, Opus 4.7 and 4.6 are the same price. But the bill for the same task will be different. The reason is how these models "read" text.

## The Tokenizer: A Hidden Multiplier

Opus 4.7 got a new tokenizer. It splits text into more tokens — and that changes everything.

Here are the numbers from Anthropic's documentation: one million Opus 4.6 tokens covers 750 thousand words, while one million Opus 4.7 tokens covers only 555 thousand. That means Opus 4.7 uses 1.8 tokens per word instead of 1.3. The difference is **35%**.

This means that at the same price *per token*, the same task on Opus 4.7 costs a third more. Not because the model is pricier — but because it consumes more billing units for the same work.

Sonnet 4.6 uses the same tokenizer as Opus 4.6 — so their tokens "weigh" the same.

## The Idea: Let One Think, Let the Other Write

In construction, an architect doesn't lay bricks. They design the building, and the crew builds it. The architect costs more, but their work is 20-30% of the project. The rest is done by the builders.

The same principle works with Claude models. Opus excels at planning, decomposition, and architectural decisions. But for writing a specific function or fixing a bug with clear instructions, Sonnet does just as well.

What if Opus thinks and breaks the task into steps, while Sonnet writes the code?

## Does Quality Suffer?

This was the key question. The answer came from benchmarks.

**SWE-bench Verified** — a standard test where a model autonomously fixes real GitHub bugs. Sonnet 4.6 with extended thinking scores **76.4%**. That's higher than both Opus models without extended thinking. The model that costs half as much writes code *better* — when given a clear task.

But there's a nuance. On **Terminal-Bench 2.0** (agentic terminal coding) and **CursorBench** (IDE coding) — Opus 4.7 leads by a wide margin:

| Benchmark | Opus 4.7 | Opus 4.6 | Sonnet 4.6 |
|---|---|---|---|
| SWE-bench Verified | ~31%* | ~24%* | **76.4%** |
| Terminal-Bench 2.0 | **~62%** | ~48% | ~42% |
| CursorBench | **70%** | 58% | — |

*\*Opus — likely without extended thinking*

See the pattern? Opus wins where it needs to **independently think, navigate, and make decisions**. Sonnet wins where the task is **already formulated**. This maps perfectly to the "architect + executor" pattern: Opus does what it's best at (planning), and Sonnet does what it's best at (writing code from instructions).

In effect, we eliminate each model's weakness and keep only its strength.

## Extended Thinking vs Adaptive Thinking

An important nuance that affects the choice. Opus 4.7 **does not have extended thinking** — instead it got **adaptive thinking**. This is a different mechanism: the model decides how much to "think" based on complexity, without the ability to manually set a large thinking budget.

Opus 4.6 and Sonnet 4.6 have full **extended thinking** — you can allocate a large thinking budget, which produces noticeably better results on complex tasks. This is exactly why Sonnet 4.6 with extended thinking scores 76.4% on SWE-bench — significantly higher than Opus 4.7 with adaptive thinking.

This is another argument for Opus 4.6 as the planner: extended thinking enables deeper task analysis before delegating execution.

## Sonnet's Limitations as an Executor

Sonnet 4.6 has a **64k token** output limit (vs 128k for Opus 4.6 and 4.7). For most tasks this is sufficient — 64k tokens is roughly 48 thousand words of code. But if a task requires generating very large files in a single pass, Sonnet may fall short. In that case, break the task into smaller pieces or use Opus for large generations.

## Doing the Math

Take a task of 100 thousand words (input + output) and compare four configurations. The calculation uses a blended rate — a weighted price based on a typical 60% input / 40% output split.

### Scenario: Mostly Code Writing (25% Planning / 75% Execution)

| Configuration | Cost | Savings |
|---|---|---|
| Pure Opus 4.7 | $2.34 | — |
| Pure Opus 4.6 | $1.73 | **-26%** |
| Opus 4.7 + Sonnet 4.6 | $1.37 | **-42%** |
| **Opus 4.6 + Sonnet 4.6** | **$1.21** | **-48%** |

### Scenario: Complex Task with Deep Planning (50/50)

| Configuration | Cost | Savings |
|---|---|---|
| Pure Opus 4.7 | $2.34 | — |
| Pure Opus 4.6 | $1.73 | **-26%** |
| Opus 4.7 + Sonnet 4.6 | $1.69 | **-28%** |
| **Opus 4.6 + Sonnet 4.6** | **$1.39** | **-41%** |

Even in the worst-case scenario — a complex task where half the work is planning — the Opus 4.6 + Sonnet 4.6 configuration saves 41%.

Here's an interesting detail: at 50/50, Opus 4.7 + Sonnet 4.6 ($1.69) is barely cheaper than pure Opus 4.6 ($1.73). The expensive 4.7 tokenizer eats up nearly all the savings from cheap Sonnet. Past ~53% planning, pure Opus 4.6 becomes the cheaper option even without Sonnet.

## Real-World Savings by Task Type

Different tasks have different "thinking vs writing" ratios:

| Task Type | Planning | Execution | Savings vs Pure Opus 4.7 |
|---|---|---|---|
| New feature | 30-40% | 60-70% | **44-47%** |
| Simple bug fix | 20% | 80% | **~50%** |
| Complex bug fix | 70-80% | 20-30% | **32-35%** |
| Refactoring | 40-50% | 50-60% | **41-44%** |

Pay attention to bug fixes. You'd think fixing a bug is "execution." But in reality, finding the bug, understanding the root cause, and choosing a fix strategy — that's all planning. The actual fix is often 5-20 lines. For complex bugs (race conditions, architectural issues) planning is 70-80% of the work.

That's why the "architect + executor" pattern is most natural for bug fixes: Opus diagnoses, Sonnet treats.

## Context7: Current Documentation Instead of Hallucinations

There's one more piece that makes the architect + executor combo significantly stronger — Context7 MCP server.

The problem: every model has a knowledge cutoff. Opus 4.6 and Sonnet 4.6 know the world as of early 2025. If you're using a library that released a new major version, changed its API, or introduced a new feature — the model doesn't know about it. It will hallucinate the old API, write code that doesn't compile, and you'll spend extra tokens on iterations and fixes.

Context7 solves this. It's an MCP server that pulls up-to-date documentation for any library on the fly — React, Next.js, Prisma, Redis, Tailwind, Django, anything. When a model needs to write code using a library, it first fetches the current docs and only then writes.

### How This Changes the Equation

Without Context7, the workflow looks like this: Opus plans → Sonnet writes code with an outdated API → build fails → Opus re-analyzes the error → Sonnet rewrites. Two to three extra iterations — and each one costs tokens.

With Context7: Opus plans and checks the current docs → passes the task to Sonnet with accurate API references → Sonnet writes working code on the first try.

The effect is threefold:

1. **Fewer iterations.** No back-and-forth fixing outdated API calls. On projects with modern or fast-moving dependencies, this can cut 20-40% of wasted tokens.
2. **Better planning quality.** When Opus has access to current docs, its architectural decisions account for actual library capabilities — not what it remembers from training data.
3. **Higher first-pass accuracy for Sonnet.** A clear task plus correct API docs means Sonnet produces working code without guessing.

This is especially impactful for the Opus 4.6 + Sonnet 4.6 configuration. Extended thinking in Opus 4.6 combined with fresh documentation means deeper, more accurate planning. And Sonnet 4.6, already strong at execution with clear instructions (76.4% on SWE-bench), becomes even more reliable when those instructions include current API syntax.

In practice, Context7 doesn't just improve quality — it reduces cost further by eliminating the "hallucination tax": the extra tokens spent correcting mistakes from stale knowledge.

## How to Set It Up

The setup has three parts: the main model + a subagent executor + up-to-date documentation.

**Prerequisite:** Context7 requires Node.js 20+. Check your version with `node --version`. If Node.js is not installed or is below version 20, install a current version from [nodejs.org](https://nodejs.org/) or via a package manager (e.g. `sudo apt install nodejs` for Ubuntu/Debian, `brew install node` for macOS).

### Step 1: Global Claude Code Settings

Add the following to `~/.claude/settings.json` (create the file if it doesn't exist):

```json
{
  "model": "claude-opus-4-6",
  "enableAllProjectMcpServers": true
}
```

Two settings at once:
- `model` — sets Opus 4.6 as the main model. Opus 4.6 doesn't appear in the Claude Code model picker dropdown — you'll only see Opus 4.7, Sonnet 4.6, and Haiku 4.5 there. But it works perfectly when set through `settings.json`. It's a "legacy" model that Anthropic hid from the UI but didn't remove from the API.
- `enableAllProjectMcpServers` — tells Claude Code to automatically connect to MCP servers defined in the project without asking for confirmation each time.

If you already have a `settings.json` with other settings — add these two fields to the existing file, don't overwrite the rest.

### Step 2: code-writer Subagent on Sonnet

Create the `~/.claude/agents/` directory (if it doesn't exist) and the file `~/.claude/agents/code-writer.md`:

```bash
mkdir -p ~/.claude/agents
```

Contents of `~/.claude/agents/code-writer.md`:

```markdown
---
name: code-writer
description: Writes code based on clear specifications
model: sonnet
tools: Read, Edit, Write, Bash
---

You are a focused code-writing agent. Implement exactly what's described in the task, following existing patterns in the codebase. Do not plan or discuss architecture — just write the code.
```

Now when the main Opus 4.6 session decides to delegate code writing — it passes the task to the `code-writer` subagent running on Sonnet. Opus thinks, Sonnet writes — automatically.

This config is global across all Claude Code environments: VSCode, JetBrains, CLI, Desktop, and Web app.

### Step 3: Context7 for Current Documentation

Create a `.mcp.json` file **in the root of each project** where you want to use Context7:

```json
{
  "mcpServers": {
    "context7": {
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp@latest"]
    }
  }
}
```

After restarting Claude Code, both Opus and Sonnet will have access to current documentation for any library via Context7. When the model encounters a library call, it pulls the latest docs before writing code — no more guessing from training data.

### Alternative Approaches

If you don't want to set up subagents, there are simpler options:

**Manual switching via `/model`** — start on Opus, once it describes what to do — type `/model sonnet` and let Sonnet execute. Then `/model opus` to switch back. Zero setup, but you have to remember to switch.

**Plan mode** — press Shift+Tab to enter Plan mode on Opus. It analyzes and builds a plan without touching code. Once the plan is ready — switch to Sonnet and implement it.

**Claude Agent SDK** — for full control you can build your own orchestration via SDK. Opus as an orchestrator invokes Sonnet workers through the API. Maximum flexibility, but requires separate development effort.

## Summary

| Configuration | Relative Cost | Best For |
|---|---|---|
| Pure Opus 4.7 | 100% | Unlimited budget, maximum autonomy needed |
| Pure Opus 4.6 | 74% | Solid default choice, with extended thinking |
| Opus 4.7 + Sonnet 4.6 | 58-80% | Only makes sense for mostly execution tasks |
| **Opus 4.6 + Sonnet 4.6** | **52-59%** | **Optimal choice for daily work** |

The most powerful model isn't always the most efficient. The right combination of two models delivers the same quality at half the price. And with Context7 providing current documentation, the savings grow further — fewer iterations fixing hallucinated APIs means fewer tokens burned. An architect shouldn't lay bricks — and Opus shouldn't spend expensive tokens on what Sonnet handles just as well.

## Risks and Caveats

**Opus 4.6 is a legacy model.** Anthropic has already deprecated Opus 4.0 and Sonnet 4.0 with a retirement date of June 15, 2026. Opus 4.6 is still available but may be next in line for deprecation. Monitor Anthropic's updates and be ready to migrate to Opus 4.7 + Sonnet 4.6 (the subagent approach works with any main model).

**Sonnet 4.6 has a 64k output limit.** Sufficient for most tasks, but for generating very large files you'll need to break the task into parts.

**Adaptive thinking in Opus 4.7** may offset the token difference by solving tasks more efficiently in fewer iterations. Real savings may be lower than calculated if Opus 4.7 solves a task in 3 steps where Opus 4.6 needs 5.

---

*Calculations based on official Anthropic API pricing and documented tokenizer characteristics as of April 2026. Actual savings may vary depending on task specifics, prompt caching, and batch API usage.*
