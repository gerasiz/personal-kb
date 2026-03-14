---
title: '"One Size Fits All": An Idea Whose Time Has Come and Gone'
authors: [Michael Stonebraker, Ugur Cetintemel]
year: 2005
venue: "ICDE Workshop (later published as invited paper)"
url: "https://cs.brown.edu/~ugur/fits_all.pdf"
date_analyzed: 2026-03-14
tags: [papers, databases, architecture, specialization, stream-processing, data-warehousing]
tweet: "https://x.com/dominiktornow/status/2031301110955421922"
---

# "One Size Fits All": An Idea Whose Time Has Come and Gone

## Summary

Stonebraker and Cetintemel argue that the traditional RDBMS architecture — originally designed for OLTP business data processing — is no longer suitable as a universal database engine. They demonstrate that workloads like data warehousing, stream processing, text search, sensor networks, and scientific databases have fundamentally different requirements that demand specialized engines. Using a financial-feed stream processing benchmark, they show a specialized engine (StreamBase) outperforming a commercial RDBMS by two orders of magnitude (160,000 vs 900 messages/sec). The paper predicts the DBMS market will fragment into a collection of independent, domain-specific database engines.

## Key Ideas

- **"One size fits all" is a market fiction** — Major DBMS vendors maintained a single code line for cost, compatibility, sales, and marketing reasons, but were already shipping effectively different engines (OLTP bottom vs warehouse bottom) behind a common SQL parser.
- **Inbound vs outbound processing** — Traditional DBMSs use "outbound" (store-then-query) processing. Stream applications need "inbound" (push-through) processing where data is processed in-flight without mandatory storage. This fundamental architectural difference cannot be bridged by bolting triggers onto an outbound engine.
- **Correct primitives matter** — Stream processing requires time windows, sliding aggregates, timeout/slack parameters for late or out-of-order data. These have no analogue in SQL over stored tables.
- **Seamless integration of DBMS + application logic** — Embedded systems (like feed processors) don't need client-server protection boundaries. Running DBMS logic, control flow, and application code in one address space eliminates costly process switches.
- **Column stores for warehouses** — Row-oriented storage is write-optimized (good for OLTP); column-oriented storage is read-optimized (good for analytical queries over wide tables). These are fundamentally different architectures.
- **Rethink system software factoring** — The traditional decomposition into DBMS + message bus + application server is a historical artifact. Stream processing engines need all three "under one roof" to avoid per-message boundary-crossing overhead.

## Method / Approach

The paper is an argumentative/position paper rather than a formal experimental study. It builds its case through:

1. **Data warehousing analysis** — showing OLTP and warehouse workloads already require different indexing (B-tree vs bitmap), different optimizations (materialized views, star schema tactics), and different storage representations (row vs column).
2. **Stream processing case study** — a real financial-feed alarm application specified by a mutual fund company, implemented in both StreamBase and a commercial RDBMS, measuring throughput on identical hardware.
3. **Architectural analysis** — systematic examination of why the performance gap exists (inbound vs outbound, primitives, address-space integration, HA mechanisms, synchronization).
4. **Survey of other domains** — sensor networks, text search (Google/GFS), scientific databases, XML databases — each with characteristics that make traditional RDBMS a poor fit.

## Results

- **StreamBase: 160,000 msg/sec vs RDBMS: 900 msg/sec** on the feed alarm application (same hardware: 2.8GHz Pentium, 512MB RAM, single SCSI disk). A ~178x performance gap.
- **Linear Road benchmark:** traditional RDBMS nearly an order of magnitude slower than a purpose-built stream engine.
- The performance gap stems from multiple compounding architectural mismatches, not a single bottleneck.
- The paper predicts fragmentation into specialized engines for: OLTP, warehousing, stream processing, text search, sensor networks, scientific data, and XML.

## Insights & Takeaways

- **Prophetic accuracy.** Written circa 2005, nearly every prediction came true: column stores (Vertica, founded by Stonebraker), stream processing (Kafka, Flink, Spark Streaming), specialized search (Elasticsearch), time-series databases (InfluxDB, TimescaleDB), and document stores (MongoDB). The DBMS market did fragment dramatically.
- **The "common parser" pattern persists.** Today's trend of SQL-on-everything (Presto/Trino, dbt, KSQL) echoes the paper's observation that vendors preserved a common front-end parser over divergent engines.
- **Relevance to AI/LLM era.** The "one size fits all" critique maps onto current debates about monolithic vs specialized AI systems. Just as no single DBMS architecture could serve all workloads optimally, no single model architecture or prompting strategy may serve all AI tasks.
- **System factoring is underappreciated.** The section on rethinking how system software is decomposed into products (Section 6) is arguably the paper's most forward-looking insight — the traditional app-server / message-bus / DBMS split was indeed disrupted by systems like Kafka, serverless platforms, and modern cloud-native architectures.
- **Limitations.** The paper's benchmark is a single application on a single machine; it doesn't address the operational complexity and cost of maintaining multiple specialized engines. The "one size fits all" approach has real organizational advantages that the paper underweights.

## Citation

> Michael Stonebraker, Ugur Cetintemel. "'One Size Fits All': An Idea Whose Time Has Come and Gone." *ICDE Workshop*, 2005. [PDF](https://cs.brown.edu/~ugur/fits_all.pdf)

---

## Appendix: Related Discussion — "One Size Fits All" Meets the Prompt-as-Platform Thesis

This section connects Stonebraker & Cetintemel's paper with ideas from Dominik Tornow's
tweet threads on LLM-driven system design.

- **Paper link:** <https://x.com/dominiktornow/status/2031301110955421922>
- **Thread link:** <https://x.com/dominiktornow/status/2028515744682508730>

### Tornow's Key Ideas

#### The Prompt Is the Platform

The core thesis: when you give an LLM a prompt, you are not just asking a question — you
are defining a platform. The prompt specifies the behavior, constraints, and interface of
a system. In this framing, the prompt replaces traditional platform configuration,
middleware setup, and even some application code. The LLM becomes the universal runtime
and the prompt becomes the specialization layer.

#### Platforms on Demand

Rather than building and maintaining bespoke platforms for each use case, LLMs enable
"platforms on demand" — you describe what you need, and the model assembles the right
combination of capabilities at inference time. This collapses the traditional build/deploy
cycle into a single prompt-response interaction. Each prompt effectively instantiates a
specialized, ephemeral platform.

#### Specification and Verification Harness

Tornow emphasizes that LLMs need formal specification and verification to be trustworthy
system components. The idea is to pair LLM-generated behavior with a spec/verification
harness: the LLM proposes actions, and a formal layer checks them against constraints
before execution. This separates the creative/generative capability (LLM) from the
correctness guarantee (formal methods), combining the flexibility of natural language with
the rigor of formal verification.

#### Durable Execution Example

Durable execution — where workflow state survives failures and can be replayed
deterministically — serves as a concrete example of this pattern. Traditional durable
execution requires specialized frameworks (Temporal, Restate). In the prompt-as-platform
model, you could describe a durable workflow in a prompt, have the LLM generate the
execution plan, and use a verification harness to ensure the plan satisfies durability
and idempotency properties. The infrastructure becomes implicit rather than explicit.

### Connection to "One Size Fits All"

Stonebraker's paper argued that the monolithic RDBMS would fragment into specialized
engines because no single architecture could serve all workloads optimally. Twenty years
later, Tornow's thesis suggests LLMs might *reverse* this fragmentation — not by building
one system that does everything, but by making specialization cheap and dynamic.

#### Parallels

| Stonebraker (2005) | Tornow (2026) |
|---|---|
| One RDBMS can't serve all workloads | One platform can't serve all use cases |
| Solution: specialized engines per domain | Solution: LLM generates specialized behavior per prompt |
| Common SQL parser over divergent engines | Common natural-language interface over dynamic capabilities |
| System factoring is a historical artifact | Platform boundaries are historical artifacts |
| Stream processing needs inbound architecture | Each task needs its own "architecture" assembled on demand |

#### Tensions

- **Stonebraker's insight was about *deep* architectural specialization** — column stores vs row stores, inbound vs outbound processing. These are not superficial differences. Can an LLM truly generate deeply optimized, domain-specific systems from prompts, or will prompt-generated systems be "good enough" generalists?
- **The verification gap.** Stonebraker's specialized engines were hand-optimized and extensively tested. Tornow's spec/verification harness is the proposed answer to this for LLM systems, but it remains an open challenge — especially for performance-critical workloads where "correct but slow" is not acceptable.
- **Operational complexity.** Stonebraker underweighted the cost of maintaining many specialized engines. Tornow's prompt-as-platform model addresses this by making specialization ephemeral — but shifts complexity to the prompt engineering and verification layers.

#### Synthesis

The deepest connection is about **system factoring**. Stonebraker's Section 6 questioned
whether the traditional decomposition of system software (DBMS + message bus + app server)
was optimal. Tornow extends this question to a more radical conclusion: if an LLM can
generate the right specialized behavior for each task, then perhaps no fixed factoring is
needed at all. The "platform" becomes whatever the prompt says it is, verified against
whatever formal properties the use case demands.

This is "one size fits all" turned inside out: instead of one rigid system trying to serve
every workload, you have one flexible system that *becomes* a different specialized system
for each workload. Whether this actually delivers the deep architectural optimization that
Stonebraker showed matters (178x performance gaps) remains the key open question.
