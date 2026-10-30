# Data Engineering Roadmap — 24-Week Learning Notebook

**Target:** Build the skills to design, implement, operate, optimise, and explain production-grade data platforms using SQL, Python, PySpark, Microsoft Fabric, Azure Data Lake Storage, and selected Synapse Analytics capabilities.

**Recommended pace:** 24 weeks, 10–12 focused hours per week. Adjust the pace to your schedule; do not skip the deliverables or exit criteria.

**Learning rule:** Every topic follows this cycle: understand the concept → implement a small lab → apply it to a realistic data problem → test failure cases → explain your design and trade-offs.

> This notebook is a learning plan and project tracker, not a substitute for running the labs, reading current product documentation, and debugging real workloads. Product capabilities and licensing can change; use the official docs linked in the resources section as the source of truth.

## How to use this notebook

- Keep this file in a Git repository and commit your progress weekly.
- For each week, complete the concept checklist, lab, deliverable, and exit criteria.
- Save SQL scripts, Python/PySpark notebooks, pipeline screenshots or definitions, architecture diagrams, test results, and short design notes in your portfolio repository.
- Record at least three interview questions and answers each week.
- Never mark a topic complete merely because you watched a course. You should be able to build it again without following a tutorial.

## Your target skill profile

By the end, you should be able to:

1. Write and tune complex SQL; explain execution plans, indexes, statistics, and query trade-offs.
2. Select appropriate batch, streaming, ETL/ELT, lake, warehouse, and lakehouse patterns.
3. Design dimensional models and implement SCD Type 1 and Type 2.
4. Build and tune PySpark pipelines, explain Spark execution, shuffles, partitions, joins, and skew.
5. Build Fabric lakehouse and warehouse solutions using OneLake, Delta tables, notebooks, and semantic models.
6. Orchestrate reliable pipelines with parameters, loops, retries, concurrency controls, and incremental loads.
7. Implement metadata-driven ingestion, data quality, audit logging, and restart/replay behaviour.
8. Explain where Synapse dedicated/serverless SQL pools and Spark fit, including their limitations.
9. Secure, test, monitor, deploy, and document a data platform.
10. Demonstrate the above through a portfolio-grade, end-to-end project.

## Weekly time budget (10–12 hours)

| Activity | Hours | Expected result |
|---|---:|---|
| Concepts and documentation | 2 | Explain the why and when, not just definitions |
| Guided lab | 3 | Reproduce a small working example |
| Independent implementation | 3–4 | Build without copying a tutorial |
| Testing and troubleshooting | 1 | Deliberately test bad, duplicate, late, and changed data |
| Notes and interview practice | 1–2 | Explain decisions, trade-offs, and failure recovery |

## Environment and portfolio setup

Use a single Git repository, for example `data-engineering-portfolio/`:

```text
data-engineering-portfolio/
├── README.md
├── docs/
│   ├── architecture/
│   ├── decisions/
│   └── runbooks/
├── sql/
│   ├── fundamentals/
│   ├── performance/
│   └── warehouse/
├── python/
│   ├── ingestion/
│   └── tests/
├── pyspark/
│   ├── bronze/
│   ├── silver/
│   └── gold/
├── fabric/
│   ├── notebooks/
│   ├── pipelines/
│   └── semantic-model/
├── synapse/
├── data-quality/
└── evidence/
```

**Suggested tools:** VS Code, Git, Python 3, a virtual environment, Jupyter, a SQL engine (SQL Server or PostgreSQL), and a Fabric workspace with the required capacity/trial access. Use local PySpark for Spark fundamentals when Fabric capacity is unavailable. Confirm current Fabric licensing, trial eligibility, and costs before creating paid resources.

**Datasets:**
- AdventureWorks or WideWorldImporters for relational SQL and dimensional modelling.
- NYC Taxi public trip data or another large public Parquet dataset for Spark, partitioning, and query optimisation.
- Synthetic retail orders/customers/products as the main batch project, so you can control updates, duplicates, deletes, and schema changes.
- Synthetic IoT/device events for streaming, late-arriving events, windows, and KQL.

---

---

## 24-week curriculum

Each week has a concept block and a practical build. Keep your code and evidence in the portfolio repository.

---

### Week 1: Python, developer workflow, and testing foundations

**Concepts to learn**
Python for data work; Git; command line; environments; APIs; JSON; CSV; exceptions; logging; typing; unit tests.

**Hands-on lab**
Create a Python ingestion utility that calls a public or mock REST API, handles pagination and rate limits, validates JSON, writes raw files, logs run metadata, and has pytest tests. Add a README and Git workflow.

**Deliverable**
A tested, configurable ingestion script with structured logs, sample input/output, and a clean Git history.

**Interview checkpoint**
Can you explain idempotency, exception handling, API pagination, secrets handling, and unit versus integration tests?

**Exit checklist**
- [ ] I can explain the concepts in my own words.
- [ ] I completed the lab without blindly copying a tutorial.
- [ ] I tested at least one edge case or failure scenario.
- [ ] I committed code and evidence to Git.
- [ ] I can answer the interview checkpoint aloud.

---

### Week 2: SQL foundations

**Concepts to learn**
Relational concepts; SELECT; filtering; sorting; NULL semantics; joins (inner/left/right/full/cross/self); set operations; data types; keys and constraints.

**Hands-on lab**
Use AdventureWorks/WideWorldImporters. Write 30 queries covering multi-table joins, anti-joins, NULL handling, duplicate detection, and reconciliation. Create a small relational schema with PK/FK constraints.

**Deliverable**
A documented SQL kata folder, ER diagram, and query result evidence.

**Interview checkpoint**
What is the difference between INNER and LEFT JOIN? How do NULLs affect NOT IN? How do you find orphan and duplicate records?

**Exit checklist**
- [ ] I can explain the concepts in my own words.
- [ ] I completed the lab without blindly copying a tutorial.
- [ ] I tested at least one edge case or failure scenario.
- [ ] I committed code and evidence to Git.
- [ ] I can answer the interview checkpoint aloud.

---

### Week 3: Advanced SQL and stored procedures

**Concepts to learn**
CTEs and recursive CTEs; subqueries; correlated subqueries; window functions; ranking; running totals; conditional aggregation; date logic; views; stored procedures; transactions; error handling.

**Hands-on lab**
Build an order analytics package: top-N products per category, rolling 7-day sales, month-over-month growth, customer cohorts, deduplication using ROW_NUMBER, and a parameterised stored procedure for daily summary loads.

**Deliverable**
A reusable SQL reporting and load script with parameter validation and transaction/error handling.

**Interview checkpoint**
When would you use a CTE versus a temp table? Explain ROW_NUMBER, RANK, and DENSE_RANK. How do you make a stored procedure safe to rerun?

**Exit checklist**
- [ ] I can explain the concepts in my own words.
- [ ] I completed the lab without blindly copying a tutorial.
- [ ] I tested at least one edge case or failure scenario.
- [ ] I committed code and evidence to Git.
- [ ] I can answer the interview checkpoint aloud.

---

### Week 4: SQL performance and query optimisation

**Concepts to learn**
Execution plans; estimated vs actual rows; scans vs seeks; indexes; clustered/nonclustered indexes; covering indexes; statistics; cardinality estimation; sargability; parameter sniffing; locking; deadlocks; isolation levels.

**Hands-on lab**
Choose 3 slow queries. Capture baseline plans and timings, add or change indexes, rewrite predicates/joins, update statistics where appropriate, and compare logical reads and elapsed time. Document when an index makes writes slower.

**Deliverable**
A before/after tuning report with query plans, index rationale, timings, and limitations.

**Interview checkpoint**
Why can an index be ignored? What is a key lookup? What is parameter sniffing? Why is applying a function to a filtered column often harmful?

**Exit checklist**
- [ ] I can explain the concepts in my own words.
- [ ] I completed the lab without blindly copying a tutorial.
- [ ] I tested at least one edge case or failure scenario.
- [ ] I committed code and evidence to Git.
- [ ] I can answer the interview checkpoint aloud.

---

### Week 5: Data Engineering fundamentals and architecture

**Concepts to learn**
OLTP vs OLAP; ETL vs ELT; batch vs streaming; data lake vs warehouse vs lakehouse; structured/semi-structured/unstructured data; data lifecycle; file formats; schema-on-read vs schema-on-write; reliability and idempotency.

**Hands-on lab**
Draw architectures for (a) nightly retail reporting, (b) near-real-time device monitoring, and (c) a data science lakehouse. Select storage and compute for each and explain latency, cost, governance, and consistency trade-offs.

**Deliverable**
Three architecture diagrams and short architecture decision records explaining technology choices.

**Interview checkpoint**
When is a lakehouse preferable to a warehouse? When would you not use streaming? Explain ETL vs ELT and exactly-once claims.

**Exit checklist**
- [ ] I can explain the concepts in my own words.
- [ ] I completed the lab without blindly copying a tutorial.
- [ ] I tested at least one edge case or failure scenario.
- [ ] I committed code and evidence to Git.
- [ ] I can answer the interview checkpoint aloud.

---

### Week 6: Data modelling and dimensional warehousing

**Concepts to learn**
Normalisation basics; business process and grain; star vs snowflake; fact and dimension tables; additive/semi-additive/non-additive measures; surrogate/natural keys; role-playing dimensions; conformed dimensions; date dimension; SCD 0/1/2; late-arriving facts/dimensions.

**Hands-on lab**
Design a retail star schema: FactSales, FactReturns, DimCustomer, DimProduct, DimStore, DimDate. Declare grain, keys, measures, and relationships. Implement SCD1 and SCD2, including effective dates and current-row flag.

**Deliverable**
A dimensional model, DDL, sample load logic, and tests proving history is preserved correctly.

**Interview checkpoint**
Why must grain be decided first? Why use surrogate keys? How do facts resolve to the correct SCD2 dimension version?

**Exit checklist**
- [ ] I can explain the concepts in my own words.
- [ ] I completed the lab without blindly copying a tutorial.
- [ ] I tested at least one edge case or failure scenario.
- [ ] I committed code and evidence to Git.
- [ ] I can answer the interview checkpoint aloud.

---

### Week 7: PySpark foundations

**Concepts to learn**
SparkSession; DataFrames; schema inference vs explicit schema; read/write; select/filter/withColumn; casts; null handling; groupBy/agg; built-in functions; UDF trade-offs; Spark SQL; transformations and actions; lazy evaluation.

**Hands-on lab**
Use PySpark locally or in Fabric to read CSV, JSON, and Parquet; apply explicit schemas; clean invalid rows; aggregate sales; write Parquet and Delta where available. Compare a built-in expression with a Python UDF.

**Deliverable**
A notebook that is restartable, uses explicit schemas, and writes reproducible outputs.

**Interview checkpoint**
What is lazy evaluation? What triggers a job? Why are built-in Spark functions generally preferable to Python UDFs?

**Exit checklist**
- [ ] I can explain the concepts in my own words.
- [ ] I completed the lab without blindly copying a tutorial.
- [ ] I tested at least one edge case or failure scenario.
- [ ] I committed code and evidence to Git.
- [ ] I can answer the interview checkpoint aloud.

---

### Week 8: Spark execution model and internals

**Concepts to learn**
Driver; executors; tasks; jobs; stages; DAG; narrow vs wide transformations; shuffle boundaries; Catalyst optimiser; physical plans; Adaptive Query Execution (AQE); caching/persistence; broadcast joins.

**Hands-on lab**
Run explain plans for filter, aggregation, join, and repartition operations. Identify stages and shuffle exchanges. Compare plans with AQE enabled/disabled where your environment permits, and test caching only when reuse justifies it.

**Deliverable**
Annotated Spark plans and a short execution walkthrough from action to job/stage/task.

**Interview checkpoint**
Explain how a DataFrame action becomes tasks. What creates a shuffle? What can AQE change at runtime?

**Exit checklist**
- [ ] I can explain the concepts in my own words.
- [ ] I completed the lab without blindly copying a tutorial.
- [ ] I tested at least one edge case or failure scenario.
- [ ] I committed code and evidence to Git.
- [ ] I can answer the interview checkpoint aloud.

---

### Week 9: Spark joins, partitioning, and performance

**Concepts to learn**
Partitioning vs partitions; repartition vs coalesce; hash/range partitioning; broadcast vs sort-merge joins; data skew; salting; predicate pushdown; partition pruning; column pruning; small files; file sizing; memory and spill basics.

**Hands-on lab**
Create a skewed dataset and a large fact/small dimension join. Measure baseline, broadcast, repartition, and skew mitigation approaches. Write partitioned Parquet/Delta data and verify pruning with plans and metrics.

**Deliverable**
A Spark tuning lab with reproducible data generation, plans, timings, and a reasoned choice of partition strategy.

**Interview checkpoint**
Why can too many partitions hurt? When is broadcast unsafe? How do you diagnose skew and distinguish it from insufficient resources?

**Exit checklist**
- [ ] I can explain the concepts in my own words.
- [ ] I completed the lab without blindly copying a tutorial.
- [ ] I tested at least one edge case or failure scenario.
- [ ] I committed code and evidence to Git.
- [ ] I can answer the interview checkpoint aloud.

---

### Week 10: Microsoft Fabric platform fundamentals

**Concepts to learn**
Workspaces and capacity concepts; OneLake; Lakehouse vs Warehouse; notebooks; Spark environments; SQL analytics endpoint; Data Factory; semantic models; lineage; item permissions; when to choose each Fabric workload.

**Hands-on lab**
Create a development workspace and a Lakehouse. Upload sample files, create a notebook, create Delta tables, query through the SQL analytics endpoint, and build a basic semantic model/report. Record the end-to-end lineage.

**Deliverable**
A Fabric workspace with a small working lakehouse-to-semantic-model flow and an architecture note.

**Interview checkpoint**
When would you choose a Lakehouse versus Warehouse? What does OneLake unify? What is the role of a semantic model?

**Exit checklist**
- [ ] I can explain the concepts in my own words.
- [ ] I completed the lab without blindly copying a tutorial.
- [ ] I tested at least one edge case or failure scenario.
- [ ] I committed code and evidence to Git.
- [ ] I can answer the interview checkpoint aloud.

---

### Week 11: Fabric Lakehouse, Delta Lake, and medallion architecture

**Concepts to learn**
Bronze/Silver/Gold responsibilities; Delta transaction log; ACID; schema enforcement/evolution; MERGE/upsert; time travel; compaction; OPTIMIZE/V-Order concepts; VACUUM retention implications; table maintenance; append vs overwrite.

**Hands-on lab**
Implement Bronze raw ingestion, Silver typed/deduplicated/conformed data, and Gold business-ready aggregates. Add a new source column and handle schema evolution intentionally. Use MERGE for updates and deletes; test reruns and historical snapshots.

**Deliverable**
A medallion lakehouse with documented table contracts, MERGE logic, schema-change handling, and maintenance notes.

**Interview checkpoint**
What does the Delta log provide? What is the risk of unrestricted schema evolution? Why must VACUUM retention be considered carefully?

**Exit checklist**
- [ ] I can explain the concepts in my own words.
- [ ] I completed the lab without blindly copying a tutorial.
- [ ] I tested at least one edge case or failure scenario.
- [ ] I committed code and evidence to Git.
- [ ] I can answer the interview checkpoint aloud.

---

### Week 12: ADLS Gen2 and OneLake storage design

**Concepts to learn**
Hierarchical namespace; containers/filesystems; RBAC vs ACLs; managed identity; Parquet vs CSV vs JSON; compression; partition-folder strategy; small files; OneLake shortcuts; shortcut vs copy; external vs managed data.

**Hands-on lab**
Lay out raw/bronze/silver/gold paths for a retail dataset. Store Parquet partitioned by a sensible date key, inspect file sizes, and create a OneLake shortcut to a supported source. Test access with least-privilege identities where your environment allows.

**Deliverable**
A storage layout decision record, sample files, shortcut demo, and access-control matrix.

**Interview checkpoint**
When should you partition? When should you avoid high-cardinality partitions? What is the difference between RBAC and ACLs?

**Exit checklist**
- [ ] I can explain the concepts in my own words.
- [ ] I completed the lab without blindly copying a tutorial.
- [ ] I tested at least one edge case or failure scenario.
- [ ] I committed code and evidence to Git.
- [ ] I can answer the interview checkpoint aloud.

---

### Week 13: Fabric Data Factory and pipeline fundamentals

**Concepts to learn**
Pipeline activities; Copy activity; connections; gateways where needed; source/sink mapping; Dataflow Gen2 vs pipeline vs notebook; parameters; dynamic expressions; variables; dependencies; schedules and event triggers.

**Hands-on lab**
Build a parameterised pipeline to copy multiple source tables/files into Bronze. Parameterise source, destination, load mode, and folder. Use a metadata list with ForEach to process multiple entities.

**Deliverable**
A reusable ingestion pipeline with parameters, a multi-entity loop, and run-history evidence.

**Interview checkpoint**
When should transformation happen in Dataflow Gen2, Spark notebook, SQL, or pipeline orchestration?

**Exit checklist**
- [ ] I can explain the concepts in my own words.
- [ ] I completed the lab without blindly copying a tutorial.
- [ ] I tested at least one edge case or failure scenario.
- [ ] I committed code and evidence to Git.
- [ ] I can answer the interview checkpoint aloud.

---

### Week 14: Pipeline orchestration, reliability, and concurrency

**Concepts to learn**
ForEach; parallelism; activity dependencies; retries; retry conditions; timeout; failure paths; concurrency limits; back-pressure; alerting; idempotent activities; restart and replay; control tables.

**Hands-on lab**
Run multiple entities concurrently with a controlled degree of parallelism. Simulate a transient failure and a permanent data error. Implement retries only where appropriate, failure logging, and restart from the failed entity without duplicating data.

**Deliverable**
A resilient orchestration pipeline with documented retry policy, concurrency setting, recovery runbook, and failure tests.

**Interview checkpoint**
Why can aggressive parallelism make a pipeline slower? Which failures should be retried? How do you design restartability?

**Exit checklist**
- [ ] I can explain the concepts in my own words.
- [ ] I completed the lab without blindly copying a tutorial.
- [ ] I tested at least one edge case or failure scenario.
- [ ] I committed code and evidence to Git.
- [ ] I can answer the interview checkpoint aloud.

---

### Week 15: Incremental processing, CDC, watermarks, deduplication, and SCD

**Concepts to learn**
Full vs incremental load; source CDC concepts; change tracking vs timestamps; high-watermark tables; overlap windows; late and out-of-order updates; deletes/tombstones; deduplication keys; idempotent MERGE; SCD2 effective dating; replay and reconciliation.

**Hands-on lab**
Implement a control table storing last successful watermark per entity. Load only changed records, include a configurable lookback window, deduplicate by business key and change timestamp, apply MERGE, handle deletes, and prove safe reruns.

**Deliverable**
A reusable incremental load pattern with a control table, audit results, reconciliation, and restart/replay tests.

**Interview checkpoint**
What if records arrive late? What if a run fails after writing data but before updating the watermark? How do you avoid data loss and duplicates?

**Exit checklist**
- [ ] I can explain the concepts in my own words.
- [ ] I completed the lab without blindly copying a tutorial.
- [ ] I tested at least one edge case or failure scenario.
- [ ] I committed code and evidence to Git.
- [ ] I can answer the interview checkpoint aloud.

---

### Week 16: Metadata-driven ingestion and data quality framework

**Concepts to learn**
Configuration-driven design; metadata tables; source/target abstraction; schema contracts; reusable notebook/pipeline parameters; audit tables; data quality dimensions; completeness, uniqueness, validity, consistency, referential integrity, freshness; quarantine; severity and thresholds.

**Hands-on lab**
Create metadata tables for source, entity, load type, watermark column, keys, target, schedule, and active flag. Build a generic ingestion flow. Add quality rules, quarantine invalid records, emit run metrics, and fail or warn based on thresholds.

**Deliverable**
A mini ingestion framework that can onboard a new entity by configuration rather than copying pipeline logic.

**Interview checkpoint**
What belongs in metadata? How do you prevent a generic framework from becoming too abstract? How do you distinguish a warning from a blocking quality failure?

**Exit checklist**
- [ ] I can explain the concepts in my own words.
- [ ] I completed the lab without blindly copying a tutorial.
- [ ] I tested at least one edge case or failure scenario.
- [ ] I committed code and evidence to Git.
- [ ] I can answer the interview checkpoint aloud.

---

### Week 17: Azure Synapse Analytics: targeted architecture and SQL skills

**Concepts to learn**
Dedicated vs serverless SQL pool; distributed MPP concepts; distributions and data movement; workload management basics; Spark pools; pipelines; external data; OPENROWSET; external tables; CETAS; supported T-SQL differences and cost model.

**Hands-on lab**
Use serverless SQL to query Parquet/CSV in a data lake with OPENROWSET and create output with CETAS. Review dedicated pool table distribution and workload concepts conceptually or in an available sandbox. Compare the same workload with Fabric Warehouse/Lakehouse and document the trade-offs.

**Deliverable**
A Synapse comparison lab and architecture note showing when to use serverless, dedicated, Spark, or Fabric.

**Interview checkpoint**
Why is serverless SQL not the same as a provisioned warehouse? What does data movement mean in an MPP system? When is CETAS useful?

**Exit checklist**
- [ ] I can explain the concepts in my own words.
- [ ] I completed the lab without blindly copying a tutorial.
- [ ] I tested at least one edge case or failure scenario.
- [ ] I committed code and evidence to Git.
- [ ] I can answer the interview checkpoint aloud.

---

### Week 18: End-to-end performance and cost optimisation

**Concepts to learn**
File sizing; partition pruning; predicate/column pushdown; statistics; join strategy; skew; materialisation; caching; pipeline parallelism; warehouse query plans; Spark UI/metrics; capacity and workload cost; performance baselines.

**Hands-on lab**
Choose one slow SQL query, one Spark transformation, and one pipeline. Establish baseline metrics, change one variable at a time, and compare latency, data scanned, shuffle, file count, and cost/resource use where visible.

**Deliverable**
A cross-platform optimisation report with baseline, hypothesis, experiment, results, and recommendation.

**Interview checkpoint**
How do you prove an optimisation worked? Which metric would you inspect first for a Spark shuffle bottleneck or a pipeline bottleneck?

**Exit checklist**
- [ ] I can explain the concepts in my own words.
- [ ] I completed the lab without blindly copying a tutorial.
- [ ] I tested at least one edge case or failure scenario.
- [ ] I committed code and evidence to Git.
- [ ] I can answer the interview checkpoint aloud.

---

### Week 19: Production readiness: security, governance, CI/CD, and observability

**Concepts to learn**
Least privilege; Entra ID identities; managed identity; secrets; workspace/item/data permissions; row/column/object-level security; sensitivity labels; lineage; audit logs; Git integration; deployment pipelines; environment configuration; automated tests; alerts; SLAs/SLOs; runbooks.

**Hands-on lab**
Create dev/test/prod deployment stages or a simulated equivalent. Store code in Git, promote changes, parameterise environment-specific configuration, add data tests and pipeline alerts, and document a response procedure for a failed daily load.

**Deliverable**
A deployable project with branching/PR evidence, environment configuration, access matrix, alerts, and operational runbook.

**Interview checkpoint**
How should secrets be handled? What is the difference between workspace and data-level permissions? How do you roll back a bad pipeline release?

**Exit checklist**
- [ ] I can explain the concepts in my own words.
- [ ] I completed the lab without blindly copying a tutorial.
- [ ] I tested at least one edge case or failure scenario.
- [ ] I committed code and evidence to Git.
- [ ] I can answer the interview checkpoint aloud.

---

### Week 20: Streaming and Fabric Real-Time Intelligence

**Concepts to learn**
Event time vs processing time; event ingestion; Eventstream; Eventhouse/KQL database; KQL query basics; windows; late/out-of-order events; checkpoints; Structured Streaming concepts; exactly-once caveats; streaming vs micro-batch; monitoring and replay.

**Hands-on lab**
Generate IoT telemetry (device ID, event timestamp, metric, value). Ingest with Eventstream/Eventhouse where available, query time windows with KQL, detect threshold breaches, and compare with a Spark Structured Streaming design. Inject duplicates and delayed events.

**Deliverable**
A small streaming proof of concept plus a clear batch-vs-streaming design decision.

**Interview checkpoint**
When is Eventhouse appropriate? Explain event time vs processing time. How would you handle duplicate and late events?

**Exit checklist**
- [ ] I can explain the concepts in my own words.
- [ ] I completed the lab without blindly copying a tutorial.
- [ ] I tested at least one edge case or failure scenario.
- [ ] I committed code and evidence to Git.
- [ ] I can answer the interview checkpoint aloud.

---

### Week 21: Capstone: architecture, requirements, and source ingestion

**Concepts to learn**
Requirements gathering; source profiling; SLAs; data contracts; grain; medallion architecture; security boundaries; cost/latency trade-offs; architecture decision records.

**Hands-on lab**
Start the main Retail Analytics Platform. Ingest synthetic orders, customers, products, and stores from at least two source shapes (relational extract plus JSON/CSV files or API). Create source contracts, metadata, Bronze storage, audit logging, and a solution diagram.

**Deliverable**
Capstone architecture, source-to-target mapping, initial ingestion, metadata configuration, and working Bronze layer.

**Interview checkpoint**
Can you explain the requirements, data flow, failure scenarios, security boundaries, and why you selected each component?

**Exit checklist**
- [ ] I can explain the concepts in my own words.
- [ ] I completed the lab without blindly copying a tutorial.
- [ ] I tested at least one edge case or failure scenario.
- [ ] I committed code and evidence to Git.
- [ ] I can answer the interview checkpoint aloud.

---

### Week 22: Capstone: Silver transformations and incremental framework

**Concepts to learn**
Data cleansing; schema enforcement; deduplication; CDC simulation; watermarks; MERGE; SCD2; quarantine; data quality; recovery; replay.

**Hands-on lab**
Implement Silver tables and the reusable incremental framework. Add customer attribute changes, order updates, duplicate batches, late records, missing fields, and deletes. Demonstrate that a failed/repeated run does not corrupt the target.

**Deliverable**
Working incremental Silver processing with SCD2 history, quality gates, audit metrics, and automated tests.

**Interview checkpoint**
Walk through one update, one late record, one duplicate, and one failed-run recovery from source to target.

**Exit checklist**
- [ ] I can explain the concepts in my own words.
- [ ] I completed the lab without blindly copying a tutorial.
- [ ] I tested at least one edge case or failure scenario.
- [ ] I committed code and evidence to Git.
- [ ] I can answer the interview checkpoint aloud.

---

### Week 23: Capstone: Gold warehouse, semantic model, and optimisation

**Concepts to learn**
Fact/dimension loading; star schema; surrogate key lookup; semantic model relationships/measures; query optimisation; table maintenance; data reconciliation.

**Hands-on lab**
Create Gold FactSales and dimensions, implement SCD-aware key resolution, build a semantic model/report for revenue, order volume, returns, customer cohorts, and product trends. Optimise one Spark job and one analytical query.

**Deliverable**
A functioning Gold model and business-facing analytical experience, with reconciliation and tuning evidence.

**Interview checkpoint**
Explain the model grain, SCD resolution, metrics definitions, and performance improvements.

**Exit checklist**
- [ ] I can explain the concepts in my own words.
- [ ] I completed the lab without blindly copying a tutorial.
- [ ] I tested at least one edge case or failure scenario.
- [ ] I committed code and evidence to Git.
- [ ] I can answer the interview checkpoint aloud.

---

### Week 24: Capstone: production hardening, portfolio, and interviews

**Concepts to learn**
CI/CD; permissions; monitoring; operational runbooks; failure handling; capacity/cost; architecture review; technical storytelling; SQL/Spark/system design interviews.

**Hands-on lab**
Run a complete end-to-end load from a clean starting state. Demonstrate retry, replay, incremental change, schema change, invalid data quarantine, alerting, and deployment. Record a 10-minute demo and write a concise project case study.

**Deliverable**
A portfolio-ready GitHub project, architecture diagram, README, runbook, test evidence, demo, and interview story.

**Interview checkpoint**
Can you design the solution on a whiteboard, estimate data volume/latency, identify bottlenecks, and explain trade-offs without opening the tutorial?

**Exit checklist**
- [ ] I can explain the concepts in my own words.
- [ ] I completed the lab without blindly copying a tutorial.
- [ ] I tested at least one edge case or failure scenario.
- [ ] I committed code and evidence to Git.
- [ ] I can answer the interview checkpoint aloud.

---

## 3. Flagship projects and real-world use cases

Do not finish with disconnected exercises. The following projects demonstrate that you can combine the technologies into realistic systems.

### Project A — SQL analytics and dimensional warehouse

**Scenario:** A retailer needs reliable sales, returns, product, and customer reporting.

**Build:**
- Profile operational tables and define the business grain.
- Design a star schema with fact and dimension tables.
- Implement surrogate keys and SCD Type 1/2.
- Create incremental stored procedures and reconciliation queries.
- Tune critical queries with actual plans, indexes, and statistics.

**Proof of quality:** Show correct historical customer attributes, correct fact-to-dimension key resolution, rerun safety, and before/after query performance evidence.

### Project B — Fabric retail lakehouse and metadata-driven ingestion platform (main capstone)

**Scenario:** Multiple retail source systems deliver orders, customers, products, and stores at different frequencies and in different formats. The business needs a trusted daily sales model.

**Architecture:**

```text
Relational extracts / REST API / CSV / JSON
                  |
          Fabric Data Factory
         metadata-driven pipeline
                  |
          OneLake / Bronze
       immutable raw source data
                  |
         PySpark / Delta Lake
                  |
          Silver Delta tables
    clean + typed + deduplicated
      CDC + watermark + SCD2
                  |
       Gold dimensional model
       FactSales + dimensions
                  |
    Fabric Warehouse / SQL endpoint
                  |
       Semantic model / report
```

**Required engineering capabilities:**
- Configuration-driven onboarding of multiple entities.
- Full and incremental loads with per-entity watermarks.
- Idempotent MERGE/upsert and safe reruns.
- Duplicate, late, missing, and invalid record handling.
- SCD2 history and surrogate key lookups.
- Data quality rules, quarantine, audit logs, and reconciliation.
- Retries, bounded concurrency, alerts, and restart/replay runbooks.
- Git, deployment stages, environment-specific configuration, and least-privilege access.

**Acceptance tests:**
1. Run the pipeline twice with the same input; target counts and business results must not duplicate.
2. Update a customer's segment; the old SCD2 version must close and a new version must become current.
3. Add a source column; your schema-change policy must be explicit and tested.
4. Send a late record and a duplicate; the correct business state must remain.
5. Fail one entity; restart it without reloading successful entities unnecessarily.
6. Insert invalid records; they must be quarantined or fail the load according to the configured rule.
7. Reconcile source and target counts/totals and explain any expected differences.
8. Demonstrate who can access raw, curated, and reporting data.

### Project C — IoT streaming analytics (specialisation)

**Scenario:** Devices send telemetry continuously. Operations wants alerts and trend analysis.

**Build:** Generate timestamped device events; ingest through Eventstream or a Spark Structured Streaming proof of concept; store/query through Eventhouse and KQL where available; create time-window aggregations and threshold detection; handle duplicates and late events; document replay and checkpoint strategy.

**Decision exercise:** Explain why streaming is justified (latency and action requirements) or why a scheduled micro-batch would be simpler and cheaper.

## 4. Frameworks you should build yourself

### Metadata-driven ingestion framework

Minimum configuration fields:

| Field | Purpose |
|---|---|
| `source_system` | Logical source identity |
| `source_object` | Table, file, or API entity |
| `source_type` | SQL, file, API, or other connector |
| `target_table` | Destination table |
| `load_type` | Full, incremental, or CDC |
| `watermark_column` | Source change indicator |
| `primary_key_columns` | Business key for deduplication/MERGE |
| `partition_column` | Optional partition strategy |
| `schema_policy` | Strict, additive evolution, or controlled migration |
| `is_active` | Enable/disable an entity |
| `parallel_group` | Optional concurrency grouping |

Also build `PipelineRun`, `EntityRun`, `WatermarkState`, and `DataQualityResult` audit/control tables. Keep configuration separate from code, validate metadata before execution, and version the configuration changes.

### Data quality framework

Implement reusable checks for:
- Schema and data type conformance.
- Required fields and null thresholds.
- Uniqueness and duplicate keys.
- Accepted value/range/domain checks.
- Referential integrity between facts and dimensions.
- Freshness and volume anomaly checks.
- Source-to-target count and aggregate reconciliation.

Every check should produce a result with the rule ID, entity, run ID, evaluated count, failed count, severity, timestamp, and action. Define whether each failure blocks publication, quarantines records, or emits a warning.

## 5. What to prioritise for interviews

### SQL
- Joins, aggregations, windows, CTEs, deduplication, top-N per group.
- Query plans, indexing, statistics, sargability, transactions, and isolation.

### Data modelling and processing
- Grain, fact/dimension design, surrogate keys, SCD1/SCD2.
- Full vs incremental loads, watermarks, CDC, late data, idempotency, replay.

### Spark
- Lazy evaluation, job/stage/task, shuffle, partitioning, broadcast join, skew, AQE, file sizing.

### Microsoft Fabric / Azure
- Lakehouse vs Warehouse; OneLake and shortcuts; Delta; medallion layers.
- Pipelines vs Dataflows Gen2 vs notebooks; parameterisation; concurrency; retries.
- Security, Git, deployment, monitoring, cost and capacity.

### System design
Practise designing a platform for 100 GB/day, 1 TB/day, and a bursty streaming workload. For each, discuss ingestion, storage format, partitioning, schema evolution, data quality, recovery, security, observability, cost, and downstream consumers.

## 6. Readiness rubric — do not confuse completion with mastery

Score each area from 0 to 3:

- **0 — Unfamiliar:** Cannot explain it.
- **1 — Familiar:** Can describe it but cannot implement it independently.
- **2 — Working:** Can build a standard implementation and debug common failures.
- **3 — Production-ready:** Can select the right pattern, explain trade-offs, test edge cases, monitor it, recover failures, and tune it.

| Skill area | Target score | Evidence required |
|---|---:|---|
| SQL and query tuning | 3 | Query library, plans, benchmark report |
| Python and Git | 2–3 | Tested ingestion utility, clean commits |
| Data architecture | 3 | Architecture diagrams and decision records |
| Dimensional modelling | 3 | Star schema, SCD implementation, tests |
| PySpark and Spark internals | 3 | Working notebooks, annotated plans, tuning report |
| Fabric lakehouse and warehouse | 3 | Working medallion solution |
| Data Factory orchestration | 3 | Parameterised resilient pipelines |
| Incremental and CDC patterns | 3 | Watermark/control tables, idempotency and replay tests |
| Metadata and data quality | 3 | Generic framework and quality results |
| Synapse and ADLS | 2 | Comparison lab and storage/security design |
| Security, governance, CI/CD | 2–3 | Access matrix, deployment evidence, runbook |
| Streaming and KQL | 2 | Working proof of concept or clearly justified scope decision |
| Technical communication | 3 | Demo, README, interview walkthrough |

**Job-ready gate:** You should be able to build the main capstone from a clean repository, explain the design without notes, troubleshoot a deliberately broken run, and answer SQL/Spark/data modelling/system-design questions. A course completion certificate alone is not sufficient evidence.

## 7. Recommended official learning resources

Use these as the primary references. Read the relevant sections when you reach each module rather than trying to consume everything upfront.

### Microsoft Fabric and DP-700
- [DP-700 study guide — current skills measured](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/dp-700)
- [DP-700 certification and learning paths](https://learn.microsoft.com/en-us/credentials/certifications/exams/dp-700/)
- [Microsoft Fabric Data Engineering documentation](https://learn.microsoft.com/en-us/fabric/data-engineering/)
- [Fabric Lakehouse and Delta tables](https://learn.microsoft.com/en-us/fabric/data-engineering/lakehouse-and-delta-tables)
- [Delta table schema evolution in Fabric](https://learn.microsoft.com/en-us/fabric/data-engineering/delta-lake-schema-evolution)
- [OneLake shortcuts](https://learn.microsoft.com/en-us/fabric/onelake/onelake-shortcuts)
- [Fabric dimensional modelling](https://learn.microsoft.com/en-us/fabric/data-warehouse/dimensional-modeling-overview)
- [Fabric CI/CD workflow options](https://learn.microsoft.com/en-us/fabric/cicd/manage-deployment)
- [Fabric Real-Time Intelligence getting started](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/get-started-with-real-time-intelligence)
- [Fabric Data Factory documentation](https://learn.microsoft.com/en-us/fabric/data-factory/)

### Spark, storage, and Synapse
- [Apache Spark SQL programming guide](https://spark.apache.org/docs/latest/sql-programming-guide.html)
- [Apache Spark performance tuning](https://spark.apache.org/docs/latest/sql-performance-tuning.html)
- [Azure Data Lake Storage access control lists](https://learn.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-access-control)
- [Synapse SQL feature comparison](https://learn.microsoft.com/en-us/azure/synapse-analytics/sql/overview-features)

### Books worth using selectively
- *The Data Warehouse Toolkit* — Ralph Kimball and Margy Ross (dimensional modelling).
- *Fundamentals of Data Engineering* — Joe Reis and Matt Housley (platform architecture and operational thinking).
- *Designing Data-Intensive Applications* — Martin Kleppmann (distributed systems, consistency, streams, storage).

## 8. Direct answers to your questions

**Is everything included?** Your original 12 areas are included. The roadmap also adds Python, Git, testing, security, governance, CI/CD, monitoring, data contracts, reliability, and a focused streaming/KQL module. These are important for production work and current Fabric role expectations.

**Are concepts and hands-on projects included for every topic?** Yes. Each week has concepts, an independent lab, a deliverable, an interview checkpoint, and exit criteria. The three flagship projects combine the topics into realistic systems.

**Are real-world use cases included?** Yes. The main project is a retail analytics platform with multiple sources, medallion layers, incremental ingestion, CDC simulation, SCD2, quality checks, orchestration, semantic modelling, and production operations. The other projects cover SQL warehousing and IoT streaming.

**Can you rely solely on this notebook?** No—not responsibly. Treat it as your syllabus, tracker, and project specification. Mastery comes from implementing the work, reading the current official documentation, investigating failures, reviewing query and Spark plans, and explaining the trade-offs. Use the DP-700 study guide as a current Fabric skills cross-check, not as a substitute for broader data engineering practice.

---

## Weekly progress tracker

| Week | Status | Evidence / Git commit | Confidence (0–3) | Questions to revisit |
|---:|---|---|---:|---|

---

| 1 | Not started |  |  |  |

---

| 2 | Not started |  |  |  |

---

| 3 | Not started |  |  |  |

---

| 4 | Not started |  |  |  |

---

| 5 | Not started |  |  |  |

---

| 6 | Not started |  |  |  |

---

| 7 | Not started |  |  |  |

---

| 8 | Not started |  |  |  |

---

| 9 | Not started |  |  |  |

---

| 10 | Not started |  |  |  |

---

| 11 | Not started |  |  |  |

---

| 12 | Not started |  |  |  |

---

| 13 | Not started |  |  |  |

---

| 14 | Not started |  |  |  |

---

| 15 | Not started |  |  |  |

---

| 16 | Not started |  |  |  |

---

| 17 | Not started |  |  |  |

---

| 18 | Not started |  |  |  |

---

| 19 | Not started |  |  |  |

---

| 20 | Not started |  |  |  |

---

| 21 | Not started |  |  |  |

---

| 22 | Not started |  |  |  |

---

| 23 | Not started |  |  |  |

---

| 24 | Not started |  |  |  |