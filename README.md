# asya-samples

Sample actor mesh pipelines for [Asya](https://github.com/asyacore/asya).

This monorepo demonstrates how to build, compile, and deploy Asya flows
organized by pattern category. Each category has its own `Dockerfile` and
`skaffold.yaml` — one image per group, just like a real multi-team setup.

## Structure

```
.asya/                          # Central config (shared across all categories)
  config.yaml                   #   registry, templates, compiler paths
  config.compiler.rules.yaml    #   tenacity, asyncio.timeout extraction
  templates/                    #   actor.yaml, router.yaml, etc.
  flows/                        #   All compiled output lands here

src/
  control-flow/                 # Sequential, branching, loops, fan-out, composition
  compiler-sugar/               # Adapters, decorators, typed signatures, bare scripts
  agentic/                      # ReAct, multi-agent debate, human-in-the-loop
  resiliency/                   # Retry, error routing, try/except, timeouts
  text-improver/                # End-to-end example with real LLM calls
```

Each category contains:
- `flows/` — flow definitions (`flow_*.py`)
- `actors/` — actor handler implementations
- `Dockerfile` + `skaffold.yaml` — build config for the category image

## Categories

### control-flow (29 flows)

Core compiler patterns: if/elif/else branching, while loops (break, continue,
nested), fan-out (list literals, comprehensions, asyncio.gather), flow
composition, and payload mutations.

### compiler-sugar (13 flows)

Advanced compiler features: decorator extraction (@tenacity.retry, @timeout),
typed signatures (TypedDict, Dataclass, Pydantic), inline comment overrides,
bare script flows, and adapter patterns.

### agentic (19 flows)

Agentic AI patterns: ReAct tool loops, evaluator-optimizer, multi-agent debate,
human-in-the-loop with approval gates, orchestrator-workers, RAG pipelines,
voting ensembles, and hierarchical delegation.

### resiliency (13 flows)

Error handling and fault tolerance: try/except with multiple handlers, nested
error handling, finally blocks, retry loops, error routing, failover pipelines,
asyncio.timeout scopes.

### text-improver (1 flow)

End-to-end example ported from [asya-demo-kubecon2026](https://github.com/atemate/asya-demo-kubecon2026).
Uses real LLM calls (Gemini via LiteLLM) with research, generate, evaluate, and
polish actors in a while loop with quality scoring.

## Quick Start

```bash
# Compile a flow
asya flow compile src/control-flow/flows/flow_if_elif_else.py

# Compile with visualization
asya flow compile src/text-improver/flows/flow_text_improver.py --plot

# Build a category image
cd src/text-improver && skaffold build

# Deploy compiled manifests
asya k apply text-improver --context dev
```

## Requirements

- Python 3.13+
- [Asya CLI](https://github.com/asyacore/asya) (`pip install asya`)
- Docker (for building images)
- Skaffold (for container builds)
- A Kubernetes cluster with Asya installed (for deployment)
