---
title: 'Concepts — "One Size Fits All": An Idea Whose Time Has Come and Gone'
paper: 2026-fits-all
---

# Prerequisite Concepts

Concepts and background knowledge assumed by the paper. Each entry gives a brief
explanation and a pointer to learn more.

## OLTP (Online Transaction Processing)

**What it is:** Database workloads characterized by many short, concurrent transactions that read and write small amounts of data — typical of operational systems (point-of-sale, order entry, banking). Requires fast writes, row-level locking, and ACID guarantees.

**Why it matters here:** OLTP is the workload that traditional RDBMS architectures were originally designed and optimized for. The paper argues this optimization makes them a poor fit for other workloads.

**Learn more:** [Wikipedia — OLTP](https://en.wikipedia.org/wiki/Online_transaction_processing)

## Data Warehousing / Star Schema

**What it is:** A data warehouse consolidates data from multiple operational systems for analytical querying. Star schemas organize warehouse data around a central fact table (transactions) joined to dimension tables (products, stores, time). Queries are complex, ad-hoc, and read-heavy.

**Why it matters here:** The paper uses data warehousing as its first example of a workload that already forced vendors to maintain a separate engine (bitmap indexes, materialized views, column stores) behind the "one size fits all" marketing fiction.

**Learn more:** [Wikipedia — Star schema](https://en.wikipedia.org/wiki/Star_schema) · [Kimball Group](https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/kimball-techniques/dimensional-modeling-techniques/)

## Stream Processing

**What it is:** Processing continuous, potentially unbounded data flows in real time, as opposed to querying data at rest. Data is pushed through a processing pipeline rather than pulled from storage. Key concepts include sliding windows, event time, and out-of-order handling.

**Why it matters here:** Stream processing is the paper's primary case study. The inbound (push) processing model is architecturally incompatible with the outbound (store-then-query) model of traditional DBMSs, accounting for the 178x performance gap.

**Learn more:** [Streaming 101 — Tyler Akidau](https://www.oreilly.com/radar/the-world-beyond-batch-streaming-101/) · [Apache Flink docs](https://flink.apache.org/what-is-flink/flink-architecture/)

## Column Store vs Row Store

**What it is:** Row stores place all attributes of a record contiguously on disk (write-optimized). Column stores place all values of a single attribute contiguously (read-optimized). Column stores excel at analytical queries that scan few columns over many rows.

**Why it matters here:** The paper identifies column-store architecture as probably the biggest architectural difference between warehouse and OLTP engines. Stonebraker went on to co-found Vertica, a column-store database, based on this insight.

**Learn more:** [Wikipedia — Column-oriented DBMS](https://en.wikipedia.org/wiki/Column-oriented_DBMS) · [C-Store paper (Stonebraker et al.)](https://dl.acm.org/doi/10.5555/1083592.1083658)

## Inbound vs Outbound Processing

**What it is:** Two fundamental data processing models. Outbound: store data first, then query it (traditional DBMS). Inbound: data streams are pushed through standing queries/operators in memory, with storage optional. The paper introduces these terms to explain why stream processing engines are architecturally different from DBMSs at their core.

**Why it matters here:** This is the paper's central architectural distinction. Outbound engines use pull-based query execution over stored data; inbound engines use push-based processing of data in flight. The difference is not a feature gap but a fundamental design inversion.

**Learn more:** See Sections 4.1 of this paper (Figures 4 and 5).

## Client-Server vs Embedded Architecture

**What it is:** Client-server DBMSs run the database engine in a separate process from application code, providing security isolation for untrusted clients. Embedded systems run DBMS logic and application code in the same address space, eliminating process-switch overhead.

**Why it matters here:** Stream processing applications are embedded systems written by trusted developers. The client-server protection boundary adds overhead with no benefit, contributing to the performance gap. StreamBase embeds BerkeleyDB in-process for ~10x improvement over client-server mode.

**Learn more:** [Wikipedia — Embedded database](https://en.wikipedia.org/wiki/Embedded_database)

## High Availability in Streaming Systems

**What it is:** Techniques for keeping stream processing applications running 24/7 with minimal downtime. Traditional DBMS log-based crash recovery takes seconds to minutes — unacceptable for real-time streams. Process-pair (Tandem-style) failover to hot standbys is preferred. Many streaming applications can tolerate weaker recovery guarantees (some lost or duplicated tuples).

**Why it matters here:** The paper argues that HA requirements further differentiate streaming engines from traditional DBMSs, as ACID recovery mechanisms are both too slow and too rigid for many streaming workloads.

**Learn more:** [Hwang et al. — High-Availability Algorithms for Distributed Stream Processing (ICDE 2004)](https://dl.acm.org/doi/10.5555/977401.978189)
