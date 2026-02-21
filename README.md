# do not repeat

I build tools that make AI agents waste fewer tokens and remember more.

**[Helioy](https://helioy.com)** is an ecosystem for AI-augmented software engineering. The thesis is simple: context windows are the bottleneck. Every token should be high-value. Navigation is waste. So I'm building the infrastructure to eliminate it.

## What I'm building

**[attention-matters](https://github.com/srobinson/attention-matters)** — Geometric memory engine on the S³ hypersphere. Persistent recall for AI agents using quaternion drift, phasor interference, and Kuramoto coupling. One brain per developer, not per project. Rust.

**[fmm](https://github.com/srobinson/fmm)** — Structural metadata for source code. Auto-generated sidecars that tell LLMs what a file does without reading it. 80-90% fewer file reads. Rust + tree-sitter.

**[mdcontext](https://github.com/srobinson/mdcontext)** — Structural intelligence for markdown. Hybrid search (BM25 + semantic), section-level indexing, 80%+ token compression. TypeScript + Effect.

**[nancyr](https://github.com/srobinson/nancyr)** — Multi-agent orchestrator wrapping Claude Code CLI. Hub-and-spoke coordination, token budgets, adaptive TUI. Rust.

**[helioy-plugins](https://github.com/srobinson/helioy-plugins)** — Claude Code plugin bundling skills, MCP servers, and hooks. The glue that connects everything.

**[nancy](https://github.com/srobinson/nancy)** — The shell prototype that proved the architecture. Process supervision, token management, hook servers. Where it all started.

## The stack

Three libraries that each solve one problem well:

```
attention-matters  →  memory      (what happened before?)
fmm                →  code        (what does this file do?)
mdcontext          →  documents   (what does this doc say?)
```

One orchestrator that composes them:

```
nancyr  →  spawn agents, compile context, coordinate work, learn from outcomes
```

All exposed as MCP servers. All usable independently. All designed to make the next AI interaction cheaper and more accurate than the last.

## Principles

- **Ship working code.** Fix forward.
- **CLI wrapping over direct API.** Meet developers where they are.
- **One brain, not one brain per project.** Cross-pollination is a feature.
- **No magic numbers.** Constants derive from phi and pi. The math decides what matters.
- **Independent repos, not a monorepo.** More scalable, independent brands.
