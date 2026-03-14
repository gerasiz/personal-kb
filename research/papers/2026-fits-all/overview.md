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
