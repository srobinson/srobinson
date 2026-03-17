# do not repeat

I build infrastructure that makes AI agents waste fewer tokens and remember more.

**[Helioy](https://helioy.com)** is context engineering for AI agents. The thesis: larger context windows don't solve the problem. Better representations do. Code as indexed structure. Documentation as scoped sections. Decisions as distilled knowledge. Identity as geometric memory. Every token earns its place.

## helix

A unified context API. One interface between your agents and every context source that matters.

```
helix recall <query>     # curated context from all sources
helix save <content>     # distill and store knowledge
helix conflicts          # surface overlaps for resolution
```

Both reads and writes pass through an LLM-powered proxy for curation. Tantivy handles candidate retrieval. The proxy shapes responses to token budget. Your agent never touches individual backends.

## The adapters

helix draws from four context sources. Each transforms a type of raw context into a representation that maximizes signal per token.

**[context-matters](https://github.com/srobinson/context-matters)** -- Structured context store. Facts, decisions, patterns, and trade-offs persisted across agent lifetimes. SQLite + FTS5, hierarchical scopes (global > project > repo > session), BLAKE3 content deduplication. Rust.

**[attention-matters](https://github.com/srobinson/attention-matters)** -- Organizational identity through geometric memory on the S3 hypersphere. Quaternion positions, SLERP drift along geodesics, phasor interference between conscious and subconscious manifolds, Kuramoto phase coupling. Conservation laws keep it grounded: total mass M=1, coupling K_CON + K_SUB = 1. The geometry does the thinking. Rust.

**[markdown-matters](https://github.com/srobinson/mdcontext)** -- Structural intelligence for markdown. Section-aware indexing, hybrid search (BM25 + semantic), 80%+ token compression. Your agent gets relevant sections under a token budget, not entire directories. TypeScript + Effect.

**[frontmatter-matters](https://github.com/srobinson/fmm)** -- Code as indexed structure. Export maps, import graphs, dependency topology, file outlines. 5 structural queries replace 30+ file reads. Rust + tree-sitter.

## The tools

**[helioy-bus](https://github.com/srobinson/helioy-bus)** -- Inter-agent messaging. SQLite registry, file-based mailboxes, tmux nudges. Direct, role-based, and broadcast addressing. No central daemon. Coordination through shared state.

**[nancyr](https://github.com/srobinson/nancyr)** -- Multi-agent orchestrator. Context routing between agents, token budgets, adaptive coordination. Rust.

**[helioy-plugins](https://github.com/srobinson/helioy-plugins)** -- Claude Code plugin layer. Skills, MCP servers, hooks. The glue.

## The architecture

```
Agent  -->  helix  -->  Proxy LLM  -->  Adapters
                            |
                       curates both
                       reads and writes
                            |
              +-------------+-------------+
              |             |             |
        context-matters  attention-   markdown-    frontmatter-
        (decisions)      matters     matters       matters
                         (identity)  (docs)        (code)

Agent  <->  helioy-bus  <->  Agent
                 |
               nancy
            (orchestration)
```

## The theory

Helioy's architecture came from asking what origin-of-life research, autocatalytic closure theory, and thermodynamics teach us about building autonomous systems.

The core insight from Stuart Kauffman's work: a system becomes self-sustaining when its components form closed loops of mutual production. No single component catalyzes itself, but the set collectively catalyzes its own existence. There's a critical threshold of diversity below which nothing sustains and above which closure becomes inevitable.

**Five capabilities form the minimum autocatalytic set for agency:** OBSERVE, DELIBERATE, ACT, EVALUATE, REMEMBER. Remove any one and the system either collapses or drifts into error catastrophe. Removing OBSERVE, DELIBERATE, or ACT causes immediate collapse. Removing EVALUATE or REMEMBER causes gradual degradation: Eigen's error catastrophe, where behavioral complexity exceeds the system's ability to self-correct. In a multi-agent system, these capabilities distribute across specialists that catalyze each other through three message types: SUBSTRATE (upward, compressed observations), FRAME (downward, interpretive context that reshapes processing), and REPAIR (lateral, error correction).

Context rot is the thermodynamic constraint. Empirically validated across every frontier model: performance degrades as context grows, effective capacity is 50-75% of advertised limits, and coherent prose can hurt more than help. The engineering response is compression at every boundary. Each adapter transforms raw context into a representation that maximizes signal per token. The proxy LLM curates both reads and writes because agents cannot be trusted to compress well.

The theoretical framework lives in [`docs.llm/`](https://github.com/srobinson/helioy-identity/tree/main/docs.llm): twelve documents tracing the path from Von Neumann's constructor duality through Kauffman's autocatalytic sets, Eigen's error threshold, the compression problem, and context rot, to their concrete implementation in Helioy's architecture.

## What's next

### The system genome

The system has a genome: agent definitions, skills, prompts, MCP server code, context configuration, orchestration patterns. Each is independently modifiable. Context is what flows through the system. The genome is what shapes it.

```
UNIT TYPE        EXAMPLE                     MUTATION COST
---------------------------------------------------------------
Prompt           System prompt for agent     Low:  text change, instant deploy
Skill            analyze_blast_radius        Med:  may break workflows
Agent definition Role, constraints, persona  Med:  changes behavior globally
MCP endpoint     /helix/recall handler       High: code change, affects all agents
Context config   Token budget, eviction      Med:  system-wide behavior change
Orchestration    Warroom team composition    High: changes multi-agent dynamics
```

### The CRITIC

A process that observes system-level signals, diagnoses which genome unit is responsible for observed behavior, proposes targeted mutations, tests them in controlled conditions, and selects improvements.

The CRITIC operates on the genome, not on messages. Every human correction is a fitness signal that tells you which genome unit to mutate. Wrong approach points to the agent definition. Wrong tool use points to skills or plugins. Wrong reasoning points to the prompt. Wrong context points to helix configuration. The correction type localizes the mutation.

The CRITIC uses attention-matters as the fitness function. Mutations must align with values, not just improve metrics. The owner shapes identity in AM. The CRITIC evaluates mutations against that identity. The system evolves toward the owner's vision, not just toward whatever produces good numbers.

```
OBSERVE    Collect system-level signals: task outcomes,
           token economics, human corrections, context quality
     |
DIAGNOSE   Trace observed behavior to a genome unit
     |
HYPOTHESIZE  Propose a targeted mutation
     |
TEST       Run the mutation in controlled conditions
     |
MEASURE    Did it improve the target metric without
           degrading others?
     |
SELECT     Keep, discard, or refine
     |
RECORD     Store the result regardless of outcome
```

The trajectory: owner IS the critic (now). Owner WITH critic tooling: systematic observation, controlled testing, evolution history. CRITIC proposes, owner approves. CRITIC evolves autonomously within guardrails, high-risk mutations still require approval.

### The agent as self-evolver

Agents are not passive consumers of infrastructure. They have primitives. They are upstream. They should experiment with their own tooling, context queries, and work patterns within a session, measure outcomes, and adjust.

Four evolution levels run simultaneously:

```
Level 1  Agent self-evolution       fast, local, full authority
         The agent experiments with how it queries helix,
         which tools it calls, how it decomposes tasks.
         Discovers what works. Deposits those discoveries.

Level 2  Context evolution          cross-session, curated
         Agent knowledge deposits improve future agents.
         The proxy LLM quality-controls the write path.
         Better deposits produce better recalls produce
         better deposits.

Level 3  System evolution           deliberate, tested
         The genome: skills, prompts, configs, code.
         CRITIC proposes mutations from aggregated agent
         assessments. Owner approves. Changes propagate
         system-wide.

Level 4  Identity evolution         owner exclusive
         attention-matters. The geometric manifold that
         shapes the fitness landscape for everything else.
         The owner curates the terrain. The system evolves
         to thrive on it.
```

The evolutionary process is itself autocatalytic. Agent self-experimentation feeds assessments upward. Context quality improves. The CRITIC evolves the system genome based on aggregated assessments. The owner shapes the fitness landscape through AM. Improved identity and system genome produce better agents who produce better assessments.

The gap between "agents that use tools" and "agents that evolve their own tooling" is the gap between a pipeline and a living system.
