# Zeinab Gouyandeh

### Senior Data Engineer · Ph.D. in Applied Mathematics

I build data platforms the way a mathematician builds proofs: start from correctness, then optimize for scale.

My background covers both sides of the data stack. I have spent several years designing high-throughput, real-time streaming pipelines and OLAP storage architectures, and before that I worked as a data scientist applying ML/AI algorithms to prediction and classification problems. That combination shapes how I engineer: infrastructure that is **correct enough to trust a model on**, **validated well enough to know when not to**, and **scalable enough to serve in production**.

**Core stack:** Spark · Databricks · Kafka · Airflow · dbt · Iceberg · Delta Lake · Trino · DuckDB · PostgreSQL · Docker · AWS

**Connect:** [LinkedIn](https://www.linkedin.com/in/zienab-gouyandeh-ph-d-76a20b42/) · [Google Scholar](https://scholar.google.com/citations?user=0EokqwoAAAAJ&hl=en) · [ORCID](https://orcid.org/0000-0002-8485-7436) · [Medium](YOUR_MEDIUM_URL)

---

## Tech Stack

| Area | Technologies |
| :--- | :--- |
| **Languages** | Python, SQL, R, MATLAB, Mathematica |
| **Streaming & Messaging** | Apache Kafka, Redpanda, Kafka Connect, Confluent Schema Registry |
| **Lakehouse, OLAP & Storage** | Databricks, Delta Lake, Unity Catalog, Apache Iceberg, ClickHouse, Snowflake, PostgreSQL, MySQL, DuckDB, MinIO / S3 |
| **Orchestration & DevOps** | Apache Airflow, dbt, Databricks Asset Bundles, Docker, Terraform, Git, GitHub Actions |
| **BI & Serving** | Power BI, Streamlit, Databricks Lakeview, FastAPI |
| **Applied Statistics & ML** | statsmodels, scikit-learn, PyTorch, TensorFlow, NumPy, pandas, OpenCV |
| **Applied GenAI** | Databricks `ai_query()`, structured extraction from unstructured text, LLM-as-classifier pipelines with declarative output validation |

---

## Featured Projects

End-to-end data systems built to demonstrate production-grade architecture, statistical rigor, and system optimization. Datasets in the portfolio projects are synthetic or public.

### 1. Zaferan Sofreh: Restaurant Intelligence & Forecasting Platform

**Stack:** Aiven PostgreSQL · Aiven Kafka · Databricks Lakeflow Declarative Pipelines · Delta Lake / Unity Catalog · `ai_query()` · statsmodels · Lakeview · Databricks Asset Bundles · GitHub Actions

**Architecture:**
`PostgreSQL (JDBC) + Kafka (SASL_SSL)` → `Bronze` → `Silver` → `Gold` → `Platinum (statistical models)` → `Lakeview dashboard`

**Highlights**
- **Unified ingestion:** merged batch JDBC reference data and a live Kafka order stream into one deduplicated Silver source of truth, governed by declarative data-quality expectations.
- **Statistical Platinum tier:** one ARIMAX model per restaurant, fit in parallel across Spark workers via Pandas UDFs.
  - AICc-based order selection, so no restaurant is forced into another's model structure.
  - `log1p` variance-stabilizing transform, keeping prediction intervals non-negative without ad hoc clipping.
  - Ljung-Box residual diagnostics, with an automated fallback to a simpler estimator when a model fails a check.
  - Holdout accuracy reported as a **skill score against a seasonal-naive benchmark** (in the spirit of MASE), not against an arbitrary cutoff.
- **LLM enrichment:** turned unstructured customer reviews into structured sentiment and issue-severity fields with a schema-constrained `ai_query()` prompt, validated by the same DQ framework as the rest of the pipeline.
- **Debugging:** found and fixed a silent duplication bug where a dimension table's Structured Streaming source re-read a fully overwritten upstream table. It surfaced through a suspicious, exactly repeating multiple in a downstream aggregate, not by inspection.
- **Everything as code:** pipeline, forecasting job, and dashboard SQL are deployed with Databricks Asset Bundles, with CI validation on every push.

**Repository:** [zaferan_sofreh_pipeline](https://github.com/zgouyandeh/zaferan_sofreh_pipeline)

---

### 2. Real-Time Crypto Market Data Lakehouse

**Stack:** Python · Coinbase WebSocket · Kafka · Spark Structured Streaming · Apache Iceberg · MinIO · dbt · PyIceberg · Streamlit · Docker

**Architecture:**
`Coinbase WebSocket` → `Kafka` → `Spark Structured Streaming` → `Iceberg (Bronze/Silver/Gold)` → `dbt` → `Streamlit`

**Highlights**
- **Reliable ingestion:** sequence-gap detection and automatic reconnect with exponential backoff.
- **Exactly-once landing** through Spark checkpointing and Iceberg's ACID guarantees.
- **Medallion design:** CDC-style upserts (`MERGE INTO`) for order-book reconstruction and append-only aggregation for trades.
- **Direct consumption:** the dashboard reads from Iceberg through PyIceberg with no query engine in the loop, powering candlestick charts, order-book depth, and a live trade tape.
- Fully containerized and runs at zero cloud cost.

**Repository:** [crypto_stream_lakehouse](https://github.com/zgouyandeh/crypto_stream_lakehouse)

---

### 3. E-Commerce Event-Driven Lakehouse (Infrastructure as Code)

**Stack:** Python (Faker) · Kafka (KRaft) · Schema Registry · Spark Structured Streaming · MinIO · Apache Iceberg · dbt · DuckDB · Airflow · PostgreSQL · Power BI

**Architecture:**
`Faker generator` → `Kafka + Schema Registry` → `Spark Structured Streaming` → `Iceberg on MinIO` → `dbt + DuckDB (Silver/Gold)` → `PostgreSQL serving layer` → `Power BI`

**Highlights**
- **Six Avro event streams** ingested concurrently, with strict schema enforcement and stream-specific deduplication by business key.
- **Automated ELT in Airflow:** streaming writes to Iceberg, dbt modeling with in-memory DuckDB compute, and a PostgreSQL serving layer.
- **Reliable BI refresh:** a purpose-built sync layer solves Power BI connectivity to object-storage data, enabling unattended refreshes.
- **Concurrency:** Iceberg snapshot isolation and schema evolution let streaming writes and batch reads run without collisions.

**Repository:** [Electromarket-DockDB](https://github.com/zgouyandeh/Electromarket-DockDB)

---

### In Progress: Drift Detection & Adaptive Feature Platform

A feature-store platform (Feast, Kafka, PostgreSQL, Valkey) with a statistical drift-detection engine (KS, PSI, Wasserstein, FDR correction), benchmarked on a synthetic drift generator with known ground truth. Feature selection with XGBoost importance, SHAP, and correlation analysis, plus a documented label-definition bug and its fix. *Repository coming soon.*

---

## Academic Foundation

- **Ph.D. in Applied Mathematics (Numerical Analysis):** numerical and analytical methods for uncertain ordinary and partial differential equations. Publications on [Google Scholar](https://scholar.google.com/citations?user=0EokqwoAAAAJ&hl=en).
- I bring that foundation in linear algebra, numerical methods, and estimation theory into engineering practice, from forecast-error variance and residual diagnostics in the Zaferan Sofreh platform to efficient transformation code in dbt and Spark.
