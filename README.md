# Hi there, I'm Zeinab Gouyandeh 👋 
### Senior Data Engineer | Ph.D. in Applied Mathematics

I bridge the gap between rigorous computational logic and scalable data infrastructure. Over the past 3+ years, I have focused on designing, building, and optimizing high-throughput real-time streaming pipelines, robust OLAP storage architectures, and automated infrastructure platforms. 

With a strong foundation in numerical analysis and machine learning frameworks, I build data platforms that are not only highly scalable but also optimized for analytical performance and mathematical correctness.

🌐 **Connect with me:** [LinkedIn Profile](https://www.linkedin.com/in/zienab-gouyandeh-ph-d-76a20b42/) | [Medium Blog](YOUR_MEDIUM_URL) | [OrcID](https://orcid.org/0000-0002-8485-7436)|[Google Scholar](https://scholar.google.com/citations?user=0EokqwoAAAAJ&hl=en)
---

## 🛠️ Data Engineering Ecosystem

| Focus Area | Core Technologies & Frameworks |
| :--- | :--- |
| **Languages** | Python, SQL, C++, Scala, Java, Go, R, MATLAB |
| **Streaming & Messaging** | Apache Kafka, Redpanda, Kafka Connect |
| **OLAP & Storage Layers** | ClickHouse, Snowflake, PostgreSQL, MySQL, DuckDB, Apache Iceberg, MinIO/S3 |
| **Orchestration & DevOps** | Apache Airflow, dbt (data build tool), Docker, Terraform, Git, GitHub Actions |
| **Machine Learning & Math** | PyTorch, TensorFlow, Scikit-Learn, NumPy, Pandas, OpenCV |

---

## 🚀 Featured Portfolio Projects

Here are the end-to-end data systems I engineered to demonstrate production-grade architectural patterns, scalability, and system optimization.

### 1. High-Throughput Real-Time Crypto Market Data Lakehouse

* **The Architecture:** Python (Coinbase WebSocket) ➔ Apache Kafka ➔ Apache Spark Structured Streaming ➔ Apache Iceberg ➔ dbt ➔ Streamlit Dashboard

* **Core Engineering Focus:** Built a fully containerized, zero-cloud-cost streaming pipeline that ingests live crypto market data from Coinbase's public WebSocket feed. Implemented robust sequence-gap detection, automatic reconnect with exponential backoff, and exactly-once landing semantics using Spark's checkpointing and Iceberg's ACID guarantees. Designed a true medallion architecture (bronze/silver/gold) with CDC-style upserts (`MERGE INTO`) for order-book reconstruction and append-only aggregation for trade data, all on top of MinIO object storage. The consumption layer reads directly from Iceberg via PyIceberg—no query engine in the loop—powering a live trading-style dashboard with candlestick charts, order book depth visualization, and real-time trade tape.

* **📂 Repository:** [View Source Code & System Diagram](https://github.com/zgouyandeh/crypto_stream_lakehouse)

---

### 2. Infrastructure-as-Code E-Commerce Event-Driven Lakehouse Platform

* **The Architecture:** Python (Faker Generator) ➔ Apache Kafka (KRaft) ➔ Confluent Schema Registry ➔ Apache Spark Structured Streaming ➔ MinIO + Apache Iceberg ➔ dbt + DuckDB ➔ PostgreSQL ➔ Power BI

* **Core Engineering Focus:** Engineered a complete production-grade lakehouse platform that ingests six Avro-serialized event streams concurrently, with strict schema enforcement and stream-specific deduplication by business key. Automated the entire ELT pipeline with Apache Airflow—from streaming writes to Iceberg tables, through dbt modeling (Silver/Gold layers) with DuckDB in-memory compute, to a PostgreSQL serving layer that enables reliable, unattended Power BI refreshes. Solved the critical challenge of BI tool connectivity to object-storage data by implementing a purpose-built sync layer, while leveraging Iceberg's snapshot isolation and schema evolution to handle concurrent streaming writes and batch reads without collision.

* **📂  Repository:** [View Source Code & Automation Config](https://github.com/zgouyandeh/Electromarket-DockDB).



---

## 🔬 Academic Foundation
* **Ph.D. in Applied Mathematics (Numerical Analysis)** – Focused on numerical and analytical methods for solving uncertain ordinary and partial differential equations. 
* *I heavily leverage my background in matrix operations, linear algebra, and algorithmic optimization to write highly efficient data transformation code (dbt/Spark) and optimize storage indexing strategies.*
