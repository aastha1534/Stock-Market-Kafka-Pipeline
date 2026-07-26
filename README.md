# Real-Time Stock Market Data Pipeline using Apache Kafka & AWS

An end-to-end data engineering pipeline that streams stock market data in real time using Apache Kafka, hosted on AWS EC2, and made queryable through S3, Glue, and Athena.

## Project Overview
This project simulates a real-time stock market data feed and streams it through a Kafka producer-consumer architecture. The consumer writes incoming records to Amazon S3, where AWS Glue Crawler catalogs the data and Amazon Athena is used to run SQL queries directly on top of it — without needing to load it into a separate database.

## Architecture
![Architecture Diagram](docs/architecture.jpg)

**Flow:** Stock data (CSV) → Kafka Producer → Kafka Topic (on EC2) → Kafka Consumer → Amazon S3 → AWS Glue Crawler → Glue Data Catalog → Amazon Athena (SQL queries)

## Tech Stack
- **Language:** Python
- **Streaming:** Apache Kafka (Producer-Consumer model)
- **Cloud:** AWS EC2, S3, Glue Crawler, Glue Catalog, Athena
- **Libraries:** kafka-python, pandas, boto3, s3fs

## Repository Structure
```
stock-market-kafka-pipeline/
├── README.md
├── requirements.txt
├── .gitignore
├── data/
│   └── indexProcessed.csv        # Sample stock market dataset
├── notebooks/
│   ├── kafka_producer.ipynb      # Reads CSV, streams rows as Kafka messages
│   └── kafka_consumer.ipynb      # Consumes messages, writes to S3
└── docs/
    ├── architecture.jpg          # Pipeline architecture diagram
    ├── project_running.jpg       # Screenshot of the pipeline running
    └── kafka_setup_commands.txt  # EC2 + Kafka setup/CLI reference commands
```

## How It Works
1. **Producer** (`notebooks/kafka_producer.ipynb`) reads the stock dataset row by row and publishes each record as a message to a Kafka topic, simulating a live feed.
2. **Kafka broker**, set up on an AWS EC2 instance, handles the topic and message queue (setup steps in `docs/kafka_setup_commands.txt`).
3. **Consumer** (`notebooks/kafka_consumer.ipynb`) subscribes to the topic, reads incoming messages, and writes them to an S3 bucket in near real time.
4. **AWS Glue Crawler** scans the S3 bucket and automatically infers the schema, populating the **Glue Data Catalog**.
5. **Amazon Athena** queries the cataloged data directly using standard SQL — enabling ad-hoc analysis without a traditional database.

## Key Learnings
- Setting up and configuring a Kafka broker (Zookeeper + Kafka server) on a cloud EC2 instance, including public IP / advertised listener configuration.
- Building producer-consumer pipelines with `kafka-python`.
- Using AWS Glue Crawler to enable schema-on-read querying via Athena, avoiding manual ETL into a database.
- Understanding how streaming ingestion differs from batch ETL in a real analytics pipeline.

## Setup / How to Run
1. Launch an AWS EC2 instance and install Java + Kafka (commands in `docs/kafka_setup_commands.txt`).
2. Start Zookeeper and the Kafka server on the EC2 instance.
3. Create a Kafka topic.
4. Run `notebooks/kafka_producer.ipynb` to stream the dataset to the topic.
5. Run `notebooks/kafka_consumer.ipynb` to consume messages and write them to your S3 bucket.
6. Set up a Glue Crawler pointing to the S3 bucket, then query the cataloged table in Athena.

## Dataset
Sample stock index data (`data/indexProcessed.csv`) used to simulate a real-time feed.

## Author
**Aastha** — Data Analyst / Data Engineer (Fresher)
