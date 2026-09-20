# Zeinab Gouyandeh

### Senior Data & Machine Learning Engineer · Ph.D. in Applied Mathematics

I build data and machine learning systems the way a mathematician builds proofs: start from correctness, then optimize for scale.

My background covers the full path from raw data to deployed model. I started in machine learning and applied mathematics, building forecasting, deep learning, computer vision, and optimization solutions, including neural and fuzzy neural networks, for industrial and consumer applications. I then moved into data engineering, designing high-throughput, real-time streaming pipelines and OLAP storage architectures.

That combination shapes how I engineer: infrastructure that is **correct enough to trust a model on**, **validated well enough to know when not to**, and **scalable enough to serve in production**. I hold ML systems to the same standard, with leak-safe training, grouped evaluation, and drift monitoring built in.

**Core stack:** Spark · Databricks · Kafka · Airflow · dbt · Iceberg · Delta Lake · Trino · DuckDB · PostgreSQL · Feast · XGBoost · PyTorch · Docker · AWS

**Connect:** [LinkedIn](https://www.linkedin.com/in/zienab-gouyandeh-ph-d-76a20b42/) · [Google Scholar](https://scholar.google.com/citations?user=0EokqwoAAAAJ&hl=en) · [ORCID](https://orcid.org/0000-0002-8485-7436) ·

---

## Tech Stack

| Area | Technologies |
| :--- | :--- |
| **Languages** | Python, SQL, R, MATLAB, Mathematica |
| **Streaming & Messaging** | Apache Kafka, Redpanda, Kafka Connect, Confluent Schema Registry |
| **Lakehouse, OLAP & Storage** | Databricks, Delta Lake, Unity Catalog, Apache Iceberg, ClickHouse, Snowflake, PostgreSQL, MySQL, DuckDB, MinIO / S3 |
| **Orchestration & DevOps** | Apache Airflow, dbt, Databricks Asset Bundles, Docker, Terraform, Git, GitHub Actions |
| **BI & Serving** | Power BI, Streamlit, Databricks Lakeview, FastAPI |
| **Feature Stores & MLOps** | Feast, Valkey (online store), point-in-time-correct training data, Optuna |
| **Applied Statistics & ML** | statsmodels, scikit-learn, XGBoost, SHAP, PyTorch, TensorFlow, NumPy, pandas, OpenCV; forecasting, drift detection, computer vision, deep and fuzzy neural networks |
| **Applied GenAI**| Databricks ai_query(), LangChain, LangGraph, RAG, structured extraction from unstructured text, LLM-as-classifier pipelines with declarative output validation |

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

### 2. Real-Time Drift Detection & Classification (Feature Store + ML)

**Stack:** Kafka · Feast · PostgreSQL · Valkey · XGBoost · SHAP · Optuna · scikit-learn · Streamlit · Plotly

**Architecture:**
`Synthetic drift generator` → `Kafka` → `Streaming statistics (PSI / KS / Wasserstein)` → `Feast (Postgres offline, Valkey online)` → `Gated XGBoost models` → `Streamlit console`

**Highlights**
- **Three answers per time window, in real time:** is there drift, how severe is it (0-100 score), and what kind (covariate, concept, label, novel category, or engagement shift).
- **Ground truth by construction:** a synthetic generator (~4.5M events across 60 episodes) injects one of five drift mechanisms at a known day and magnitude, so detector latency, false-alarm rate, and accuracy can be measured exactly.
- **Leak-safe ML:** Feast point-in-time-correct retrieval for training, labels kept in an offline-only feature view, and every split grouped by episode rather than by row.
- **Gated modeling:** a binary detector runs first, then severity and type models trained only on active windows, compared against an ungated baseline. Hyperparameters tuned with Optuna.
- **Statistical rigor:** PSI, Kolmogorov-Smirnov, and Wasserstein distance on a reference distribution fit once, with Benjamini-Hochberg FDR correction. Decision boundaries are learned, not hand-set thresholds.
- **Feature selection by agreement:** XGBoost importance, SHAP, and a correlation matrix had to agree before a feature was kept (12 candidates reduced to 5).
- **Debugging story:** a label-definition bug (a fixed 10-day window on permanent drift) held the classifier at AUC 0.67. Correcting the label alone raised it to **AUC 0.99, F1 0.95** with no change to features or model. Documented in the project README.
- **Live dashboard:** Streamlit views for episode exploration, live-window inference with explanations, and model performance.

**Repository:** [drift_detection_pilot](https://github.com/zgouyandeh/drift_detection_pilot)

---

### 3. Real-Time Crypto Market Data Lakehouse

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

### 4. E-Commerce Event-Driven Lakehouse (Infrastructure as Code)

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

## Academic Foundation

- **Ph.D. in Applied Mathematics (Numerical Analysis):** numerical and analytical methods for uncertain ordinary and partial differential equations. Publications on [Google Scholar](https://scholar.google.com/citations?user=0EokqwoAAAAJ&hl=en).

