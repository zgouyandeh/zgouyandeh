# Hi there, I'm Zeinab Gouyandeh 👋
### Senior Data Engineer | Ph.D. in Applied Mathematics

I build data platforms the way a mathematician builds proofs — starting from correctness, then optimizing for scale. My background spans both sides of the data stack: several years designing high-throughput, real-time streaming pipelines and OLAP storage architectures, preceded by hands-on work as a data scientist applying ML/AI algorithms to prediction and classification problems.

That combination shapes how I engineer: I don't just move data — I build infrastructure that's correct enough to trust a statistical or ML model on, validated rigorously enough to know *when* to trust it, and scalable enough to serve it in production.

**Core stack:** Spark · Databricks · Airflow · dbt · Iceberg · Delta Lake · Trino · DuckDB · Docker · AWS

🌐 **Connect with me:** [LinkedIn](https://www.linkedin.com/in/zienab-gouyandeh-ph-d-76a20b42/) · [Medium](YOUR_MEDIUM_URL) · [ORCID](https://orcid.org/0000-0002-8485-7436) · [Google Scholar](https://scholar.google.com/citations?user=0EokqwoAAAAJ&hl=en)

---

## 🛠️ Data Engineering Ecosystem

| Focus Area | Core Technologies & Frameworks |
| :--- | :--- |
| **Languages** | Python, SQL,  R,MATLAB, Mathematica |
| **Streaming & Messaging** | Apache Kafka, Redpanda, Kafka Connect |
| **Lakehouse, OLAP & Storage** | Databricks, Delta Lake, Unity Catalog, Apache Iceberg, ClickHouse, Snowflake, PostgreSQL, MySQL, DuckDB, MinIO/S3 |
| **Orchestration & DevOps** | Apache Airflow, Databricks Asset Bundles, dbt (data build tool), Docker, Terraform, Git, GitHub Actions |
| **Applied Statistics & ML** | statsmodels, PyTorch, TensorFlow, Scikit-Learn, NumPy, Pandas, OpenCV |
| **Applied GenAI / LLM Integration** | Databricks `ai_query()`, structured extraction from unstructured text, LLM-as-classifier pipelines with declarative output validation |

---

## 🚀 Featured Portfolio Projects

Here are the end-to-end data systems I engineered to demonstrate production-grade architectural patterns, statistical rigor, and system optimization.

### 1. Zaferan Sofreh — Restaurant Intelligence & Forecasting Platform

* **The Architecture:** Aiven PostgreSQL (JDBC) + Aiven Kafka (SASL_SSL) ➔ Databricks Lakeflow Declarative Pipelines ➔ Delta Lake / Unity Catalog (Bronze/Silver/Gold/**Platinum**) ➔ Databricks SQL `ai_query()` (LLM review classification) ➔ Per-Restaurant ARIMAX Forecasting (statsmodels, distributed via Pandas UDFs) ➔ Databricks Lakeview Dashboard — all deployed via Databricks Asset Bundles with CI validation.

* **Core Engineering Focus:** Engineered a full medallion lakehouse for a multi-branch restaurant chain, unifying two structurally different ingestion paths — batch JDBC reference data and a live Kafka order stream — into a single deduplicated Silver source of truth, governed end-to-end by declarative data-quality expectations. Layered a **Platinum statistical modeling tier** on top of Gold: one ARIMAX model fit independently per restaurant in parallel across Spark workers, with AICc-based order selection (no restaurant forced into another's model structure), a log1p variance-stabilizing transform so prediction intervals stay naturally non-negative without ad hoc clipping, Ljung-Box residual diagnostics, and holdout accuracy scored not against an arbitrary cutoff but as a **skill score relative to a seasonal-naive benchmark** (in the spirit of MASE) — with an automated, transparent fallback to a simpler estimator whenever a restaurant's model fails either check. Converted unstructured customer reviews into structured sentiment and issue-severity fields via a schema-constrained LLM prompt (`ai_query()`), governed by the same declarative DQ framework as the rest of the pipeline. Diagnosed and fixed a non-obvious production bug where a dimension table's Structured Streaming source silently duplicated rows against a fully-overwritten upstream table — caught via a suspicious, exactly-repeating multiple in a downstream aggregate, not by inspection. The entire platform — pipeline, forecasting job, and dashboard SQL — is defined and deployed as code via Databricks Asset Bundles, with GitHub Actions validating the bundle on every push.

* **📂 Repository:** [View Source Code, Methodology & System Diagram](https://github.com/zgouyandeh/zaferan_sofreh_pipeline)

---

### 2. High-Throughput Real-Time Crypto Market Data Lakehouse

* **The Architecture:** Python (Coinbase WebSocket) ➔ Apache Kafka ➔ Apache Spark Structured Streaming ➔ Apache Iceberg ➔ dbt ➔ Streamlit Dashboard

* **Core Engineering Focus:** Built a fully containerized, zero-cloud-cost streaming pipeline that ingests live crypto market data from Coinbase's public WebSocket feed. Implemented robust sequence-gap detection, automatic reconnect with exponential backoff, and exactly-once landing semantics using Spark's checkpointing and Iceberg's ACID guarantees. Designed a true medallion architecture (Bronze/Silver/Gold) with CDC-style upserts (`MERGE INTO`) for order-book reconstruction and append-only aggregation for trade data, all on top of MinIO object storage. The consumption layer reads directly from Iceberg via PyIceberg — no query engine in the loop — powering a live trading-style dashboard with candlestick charts, order book depth visualization, and real-time trade tape.

* **📂 Repository:** [View Source Code & System Diagram](https://github.com/zgouyandeh/crypto_stream_lakehouse)

---

### 3. Infrastructure-as-Code E-Commerce Event-Driven Lakehouse Platform

* **The Architecture:** Python (Faker Generator) ➔ Apache Kafka (KRaft) ➔ Confluent Schema Registry ➔ Apache Spark Structured Streaming ➔ MinIO + Apache Iceberg ➔ dbt + DuckDB ➔ PostgreSQL ➔ Power BI

* **Core Engineering Focus:** Engineered a complete production-grade lakehouse platform that ingests six Avro-serialized event streams concurrently, with strict schema enforcement and stream-specific deduplication by business key. Automated the entire ELT pipeline with Apache Airflow — from streaming writes to Iceberg tables, through dbt modeling (Silver/Gold layers) with DuckDB in-memory compute, to a PostgreSQL serving layer that enables reliable, unattended Power BI refreshes. Solved the critical challenge of BI tool connectivity to object-storage data by implementing a purpose-built sync layer, while leveraging Iceberg's snapshot isolation and schema evolution to handle concurrent streaming writes and batch reads without collision.

* **📂 Repository:** [View Source Code & Automation Config](https://github.com/zgouyandeh/Electromarket-DockDB)

---

## 🔬 Academic Foundation

* **Ph.D. in Applied Mathematics (Numerical Analysis)** — focused on numerical and analytical methods for solving uncertain ordinary and partial differential equations.
* I bring that foundation in matrix operations, linear algebra, and estimation theory directly into engineering practice — from deriving forecast-error variance and residual diagnostics in the Zaferan Sofreh platform above, to writing efficient data transformation code (dbt/Spark) and optimizing storage indexing strategies.
