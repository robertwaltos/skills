---
name: data-pipeline
description: |
  Design and implement production-grade data pipelines for ETL, streaming, and analytics workloads. Embodies expertise of a Principal Data Engineer with 15+ years building systems processing petabytes of data.
  Use when: user requests data ingestion, ETL/ELT pipelines, streaming architectures, data warehousing, or analytics infrastructure.
  Outputs: Pipeline architectures, transformation code, orchestration configs, data quality frameworks, and documentation.
  Keywords: ETL, ELT, data pipeline, Airflow, dbt, Spark, Kafka, streaming, batch processing, data warehouse, data lake, data quality
license: Complete terms in LICENSE.txt
---

# Data Pipeline Skill

**Expertise Level**: Principal Data Engineer (15+ years equivalent)

This skill embodies the methodology of elite data engineering teams at companies like Netflix, Spotify, and Airbnb—building pipelines that process petabytes reliably while maintaining data quality and freshness.

---

## Related Skills

- **[database-design](../database-design)**: Design source and destination databases
- **[api-design](../api-design)**: Build APIs to expose pipeline outputs
- **[devops-automation](../devops-automation)**: Deploy and monitor pipelines
- **[performance-optimization](../performance-optimization)**: Optimize pipeline throughput

---

## Core Philosophy

**Data is only valuable if it's reliable, fresh, and accessible.** Every pipeline decision should optimize for:

1. **Reliability**: Pipelines must handle failures gracefully
2. **Observability**: Know what's happening at every stage
3. **Data Quality**: Validate data at boundaries
4. **Scalability**: Design for 10x data growth
5. **Maintainability**: Future engineers must understand the code

---

## Architecture Selection

### Batch vs. Streaming Decision

```
┌─────────────────────────────────────────────────────────────────┐
│                    Latency Requirements                         │
├───────────────────────────┬─────────────────────────────────────┤
│   Real-time (<1 minute)   │   Near-real-time (1-15 min)         │
│   ━━━━━━━━━━━━━━━━━━━━━   │   ━━━━━━━━━━━━━━━━━━━━━━━━━━        │
│   • Kafka Streams         │   • Micro-batch Spark               │
│   • Apache Flink          │   • Kafka + batch consumers         │
│   • AWS Kinesis           │   • Cloud Dataflow                  │
│                           │                                     │
│   Use cases:              │   Use cases:                        │
│   • Fraud detection       │   • Dashboard updates               │
│   • Real-time analytics   │   • Operational reporting           │
│   • Live recommendations  │   • Event aggregations              │
├───────────────────────────┴─────────────────────────────────────┤
│                    Batch (15 min - 24 hours)                    │
│   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━   │
│   • Apache Spark          • dbt                                 │
│   • Apache Airflow        • Dagster                             │
│   • AWS Glue              • Prefect                             │
│                                                                 │
│   Use cases:                                                    │
│   • Data warehouse loads  • ML training data                    │
│   • Daily aggregations    • Historical analysis                 │
│   • Cross-system syncs    • Compliance reports                  │
└─────────────────────────────────────────────────────────────────┘
```

### Tool Selection Guide

| Tool | Best For | Avoid When |
|------|----------|------------|
| **Apache Spark** | Large-scale batch, ML pipelines | Small data, real-time |
| **dbt** | SQL transformations, analytics engineering | Complex Python logic |
| **Apache Airflow** | Workflow orchestration, DAGs | Streaming workloads |
| **Dagster** | Data-aware orchestration, testing | Simple cron jobs |
| **Apache Kafka** | Event streaming, pub/sub | Small scale, simple ETL |
| **Apache Flink** | True streaming, stateful processing | Batch-only workloads |
| **Fivetran/Airbyte** | Standard connectors, ELT | Custom transformations |
| **AWS Glue** | Serverless Spark, catalog | High control needs |

---

## Pipeline Design Patterns

### Pattern 1: ELT with dbt

```
┌─────────────────────────────────────────────────────────────────┐
│                      Data Sources                               │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐           │
│  │ Postgres │  │  MySQL  │  │ Salesforce│ │  API    │           │
│  └────┬────┘  └────┬────┘  └────┬────┘  └────┬────┘           │
└───────┼────────────┼────────────┼────────────┼─────────────────┘
        │            │            │            │
        ▼            ▼            ▼            ▼
┌─────────────────────────────────────────────────────────────────┐
│                    Ingestion (Fivetran/Airbyte)                 │
│              Raw data → Staging schema (append-only)            │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                    Data Warehouse (Snowflake/BigQuery)          │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │  raw.*          │  staging.*      │  marts.*            │   │
│  │  (source data)  │  (cleaned)      │  (business logic)   │   │
│  └─────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                    Transformation (dbt)                         │
│  staging → intermediate → marts → metrics                       │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                    Consumers                                    │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐           │
│  │ Looker  │  │ Tableau │  │ ML Models│ │Reverse  │           │
│  │         │  │         │  │         │  │  ETL    │           │
│  └─────────┘  └─────────┘  └─────────┘  └─────────┘           │
└─────────────────────────────────────────────────────────────────┘
```

### Pattern 2: Streaming with Kafka

```
┌─────────────────────────────────────────────────────────────────┐
│                    Event Producers                              │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐           │
│  │ Web App │  │ Mobile  │  │  IoT    │  │ Services│           │
│  └────┬────┘  └────┬────┘  └────┬────┘  └────┬────┘           │
└───────┼────────────┼────────────┼────────────┼─────────────────┘
        │            │            │            │
        ▼            ▼            ▼            ▼
┌─────────────────────────────────────────────────────────────────┐
│                    Apache Kafka                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │  events.clicks  │  events.orders  │  events.pageviews   │   │
│  │  (partitioned)  │  (partitioned)  │  (partitioned)      │   │
│  └─────────────────────────────────────────────────────────┘   │
│                    Retention: 7 days, Compaction: enabled       │
└─────────────────────────────────────────────────────────────────┘
        │                       │                       │
        ▼                       ▼                       ▼
┌───────────────┐    ┌───────────────┐    ┌───────────────┐
│ Flink Job     │    │ Kafka Connect │    │ Spark         │
│ (real-time    │    │ (sink to S3)  │    │ Streaming     │
│  aggregates)  │    │               │    │ (ML features) │
└───────────────┘    └───────────────┘    └───────────────┘
        │                       │                       │
        ▼                       ▼                       ▼
┌───────────────┐    ┌───────────────┐    ┌───────────────┐
│ Redis         │    │ Data Lake     │    │ Feature Store │
│ (dashboards)  │    │ (analytics)   │    │ (ML serving)  │
└───────────────┘    └───────────────┘    └───────────────┘
```

### Pattern 3: Lambda Architecture (Batch + Speed)

```
                    ┌─────────────────┐
                    │  Event Stream   │
                    └────────┬────────┘
                             │
              ┌──────────────┼──────────────┐
              │              │              │
              ▼              │              ▼
    ┌─────────────────┐     │    ┌─────────────────┐
    │   Speed Layer   │     │    │   Batch Layer   │
    │   (Streaming)   │     │    │   (Daily)       │
    │                 │     │    │                 │
    │ • Real-time     │     │    │ • Complete      │
    │ • Approximate   │     │    │ • Accurate      │
    │ • Last 24 hours │     │    │ • Historical    │
    └────────┬────────┘     │    └────────┬────────┘
             │              │             │
             │    ┌─────────┘             │
             │    │                       │
             ▼    ▼                       ▼
    ┌─────────────────┐         ┌─────────────────┐
    │  Real-time View │         │   Batch View    │
    │  (hot data)     │         │   (cold data)   │
    └────────┬────────┘         └────────┬────────┘
             │                           │
             └───────────┬───────────────┘
                         │
                         ▼
              ┌─────────────────────┐
              │   Serving Layer     │
              │   (Query merges     │
              │    both views)      │
              └─────────────────────┘
```

---

## dbt Implementation

### Project Structure

```
dbt_project/
├── dbt_project.yml
├── profiles.yml
├── packages.yml
│
├── models/
│   ├── staging/           # 1:1 with source tables
│   │   ├── stg_stripe__payments.sql
│   │   ├── stg_stripe__customers.sql
│   │   └── _stg_stripe__models.yml
│   │
│   ├── intermediate/      # Business logic building blocks
│   │   ├── int_payments__pivoted.sql
│   │   └── _int_models.yml
│   │
│   ├── marts/             # Business-facing tables
│   │   ├── core/
│   │   │   ├── dim_customers.sql
│   │   │   ├── fct_orders.sql
│   │   │   └── _core__models.yml
│   │   └── marketing/
│   │       ├── mrt_customer_segments.sql
│   │       └── _marketing__models.yml
│   │
│   └── metrics/           # Semantic layer
│       └── revenue.yml
│
├── macros/
│   ├── generate_schema_name.sql
│   └── cents_to_dollars.sql
│
├── tests/
│   └── assert_positive_values.sql
│
├── seeds/
│   └── country_codes.csv
│
└── snapshots/
    └── customers_snapshot.sql
```

### Model Examples

```sql
-- models/staging/stg_stripe__payments.sql
{{
    config(
        materialized='view',
        tags=['stripe', 'daily']
    )
}}

with source as (
    select * from {{ source('stripe', 'payments') }}
),

renamed as (
    select
        id as payment_id,
        customer_id,
        amount as amount_cents,
        currency,
        status,
        created as created_at,
        _fivetran_synced as synced_at
    from source
    where _fivetran_deleted = false
)

select * from renamed
```

```sql
-- models/marts/core/fct_orders.sql
{{
    config(
        materialized='incremental',
        unique_key='order_id',
        incremental_strategy='merge',
        on_schema_change='sync_all_columns',
        tags=['core', 'daily']
    )
}}

with orders as (
    select * from {{ ref('stg_shopify__orders') }}
    {% if is_incremental() %}
    where updated_at > (select max(updated_at) from {{ this }})
    {% endif %}
),

payments as (
    select * from {{ ref('int_payments__aggregated') }}
),

customers as (
    select * from {{ ref('dim_customers') }}
),

final as (
    select
        orders.order_id,
        orders.customer_id,
        customers.customer_segment,
        orders.order_date,
        orders.status,
        payments.total_amount_cents,
        {{ cents_to_dollars('payments.total_amount_cents') }} as total_amount_dollars,
        payments.payment_count,
        orders.created_at,
        orders.updated_at
    from orders
    left join payments using (order_id)
    left join customers using (customer_id)
)

select * from final
```

```yaml
# models/marts/core/_core__models.yml
version: 2

models:
  - name: fct_orders
    description: Order fact table with payment and customer information
    
    columns:
      - name: order_id
        description: Primary key
        tests:
          - unique
          - not_null
          
      - name: customer_id
        description: Foreign key to dim_customers
        tests:
          - not_null
          - relationships:
              to: ref('dim_customers')
              field: customer_id
              
      - name: total_amount_cents
        description: Total order amount in cents
        tests:
          - not_null
          - dbt_utils.accepted_range:
              min_value: 0
              
    tests:
      - dbt_utils.recency:
          datepart: day
          field: created_at
          interval: 1
```

### dbt Tests & Data Quality

```sql
-- tests/assert_order_total_matches_line_items.sql
-- Custom test: order totals should match sum of line items

with order_totals as (
    select
        order_id,
        total_amount_cents
    from {{ ref('fct_orders') }}
),

line_item_totals as (
    select
        order_id,
        sum(line_item_amount_cents) as calculated_total
    from {{ ref('fct_order_line_items') }}
    group by 1
),

mismatches as (
    select
        o.order_id,
        o.total_amount_cents,
        l.calculated_total,
        abs(o.total_amount_cents - l.calculated_total) as difference
    from order_totals o
    join line_item_totals l using (order_id)
    where abs(o.total_amount_cents - l.calculated_total) > 1  -- Allow 1 cent rounding
)

select * from mismatches
```

---

## Airflow Implementation

### DAG Structure

```python
# dags/daily_etl.py
from datetime import datetime, timedelta
from airflow import DAG
from airflow.operators.python import PythonOperator
from airflow.providers.amazon.aws.operators.s3 import S3CreateObjectOperator
from airflow.providers.snowflake.operators.snowflake import SnowflakeOperator
from airflow.utils.task_group import TaskGroup

default_args = {
    'owner': 'data-team',
    'depends_on_past': False,
    'email_on_failure': True,
    'email_on_retry': False,
    'retries': 3,
    'retry_delay': timedelta(minutes=5),
    'retry_exponential_backoff': True,
    'max_retry_delay': timedelta(hours=1),
}

with DAG(
    dag_id='daily_etl',
    default_args=default_args,
    description='Daily ETL pipeline',
    schedule_interval='0 6 * * *',  # 6 AM UTC
    start_date=datetime(2024, 1, 1),
    catchup=False,
    max_active_runs=1,
    tags=['production', 'daily'],
) as dag:
    
    # Task Group: Extract
    with TaskGroup(group_id='extract') as extract_group:
        extract_orders = PythonOperator(
            task_id='extract_orders',
            python_callable=extract_orders_from_api,
            op_kwargs={'date': '{{ ds }}'},
        )
        
        extract_customers = PythonOperator(
            task_id='extract_customers',
            python_callable=extract_customers_from_db,
            op_kwargs={'date': '{{ ds }}'},
        )
    
    # Task Group: Transform
    with TaskGroup(group_id='transform') as transform_group:
        clean_orders = PythonOperator(
            task_id='clean_orders',
            python_callable=clean_and_validate_orders,
        )
        
        enrich_customers = PythonOperator(
            task_id='enrich_customers',
            python_callable=enrich_customer_data,
        )
    
    # Task Group: Load
    with TaskGroup(group_id='load') as load_group:
        load_to_warehouse = SnowflakeOperator(
            task_id='load_to_warehouse',
            sql='sql/load_daily_data.sql',
            snowflake_conn_id='snowflake_default',
            params={'date': '{{ ds }}'},
        )
    
    # Task: Quality checks
    run_quality_checks = PythonOperator(
        task_id='run_quality_checks',
        python_callable=run_data_quality_checks,
        op_kwargs={'date': '{{ ds }}'},
    )
    
    # Dependencies
    extract_group >> transform_group >> load_group >> run_quality_checks
```

### Idempotent Task Design

```python
# Always design tasks to be idempotent (safe to re-run)

def load_orders_idempotent(execution_date: str, **context):
    """
    Idempotent load: delete then insert for the date partition.
    Safe to re-run multiple times.
    """
    snowflake_hook = SnowflakeHook(snowflake_conn_id='snowflake_default')
    
    # Delete existing data for this date (makes re-runs safe)
    delete_sql = f"""
        DELETE FROM warehouse.orders 
        WHERE order_date = '{execution_date}'
    """
    
    # Insert new data
    insert_sql = f"""
        INSERT INTO warehouse.orders
        SELECT * FROM staging.orders
        WHERE order_date = '{execution_date}'
    """
    
    with snowflake_hook.get_conn() as conn:
        conn.execute(delete_sql)
        conn.execute(insert_sql)
        conn.commit()
```

---

## Spark Implementation

### PySpark Pipeline

```python
# jobs/daily_aggregation.py
from pyspark.sql import SparkSession
from pyspark.sql import functions as F
from pyspark.sql.window import Window
from delta.tables import DeltaTable

def main():
    spark = SparkSession.builder \
        .appName("DailyAggregation") \
        .config("spark.sql.extensions", "io.delta.sql.DeltaSparkSessionExtension") \
        .config("spark.sql.catalog.spark_catalog", "org.apache.spark.sql.delta.catalog.DeltaCatalog") \
        .getOrCreate()
    
    # Read from source
    events_df = spark.read \
        .format("delta") \
        .load("s3://data-lake/events/")
    
    # Filter to processing date
    processing_date = spark.conf.get("spark.app.processingDate")
    
    events_filtered = events_df.filter(
        F.col("event_date") == processing_date
    )
    
    # Aggregations
    daily_metrics = events_filtered.groupBy(
        "user_id",
        "event_date"
    ).agg(
        F.count("*").alias("event_count"),
        F.countDistinct("session_id").alias("session_count"),
        F.sum(F.when(F.col("event_type") == "purchase", F.col("amount")).otherwise(0)).alias("total_revenue"),
        F.collect_set("event_type").alias("event_types")
    )
    
    # Add derived columns
    daily_metrics = daily_metrics.withColumn(
        "avg_events_per_session",
        F.col("event_count") / F.col("session_count")
    )
    
    # Window function: running totals
    window_spec = Window.partitionBy("user_id").orderBy("event_date").rowsBetween(Window.unboundedPreceding, Window.currentRow)
    
    daily_metrics = daily_metrics.withColumn(
        "cumulative_revenue",
        F.sum("total_revenue").over(window_spec)
    )
    
    # Write with merge (upsert)
    target_path = "s3://data-lake/metrics/daily_user_metrics/"
    
    if DeltaTable.isDeltaTable(spark, target_path):
        delta_table = DeltaTable.forPath(spark, target_path)
        
        delta_table.alias("target").merge(
            daily_metrics.alias("source"),
            "target.user_id = source.user_id AND target.event_date = source.event_date"
        ).whenMatchedUpdateAll() \
         .whenNotMatchedInsertAll() \
         .execute()
    else:
        daily_metrics.write \
            .format("delta") \
            .mode("overwrite") \
            .partitionBy("event_date") \
            .save(target_path)
    
    # Data quality checks
    row_count = daily_metrics.count()
    null_count = daily_metrics.filter(F.col("user_id").isNull()).count()
    
    if null_count > 0:
        raise ValueError(f"Found {null_count} rows with null user_id")
    
    print(f"Successfully processed {row_count} rows for {processing_date}")
    
    spark.stop()

if __name__ == "__main__":
    main()
```

### Spark Optimization Tips

```python
# Optimization 1: Broadcast small tables
from pyspark.sql.functions import broadcast

# Small dimension table (< 100MB)
dim_products = spark.read.parquet("s3://data-lake/dim_products/")

# Broadcast join
result = events_df.join(
    broadcast(dim_products),
    events_df.product_id == dim_products.product_id,
    "left"
)

# Optimization 2: Partition pruning
# Only read partitions needed
events_df = spark.read \
    .format("delta") \
    .load("s3://data-lake/events/") \
    .filter(F.col("event_date").between("2024-01-01", "2024-01-31"))

# Optimization 3: Repartition for skewed data
# If one key has much more data than others
events_df = events_df.repartition(100, "user_id")

# Optimization 4: Cache intermediate results
# If used multiple times
intermediate_df = events_df.groupBy("user_id").agg(...).cache()

# Optimization 5: Use appropriate file formats
# Parquet for analytics (columnar, compressed)
df.write.format("parquet").mode("overwrite").save(output_path)

# Delta for ACID transactions, time travel
df.write.format("delta").mode("overwrite").save(output_path)
```

---

## Streaming with Kafka

### Kafka Producer

```python
# producers/event_producer.py
from confluent_kafka import Producer
from confluent_kafka.schema_registry import SchemaRegistryClient
from confluent_kafka.schema_registry.avro import AvroSerializer
from confluent_kafka.serialization import SerializationContext, MessageField
import json

# Schema Registry setup
schema_registry_conf = {'url': 'http://schema-registry:8081'}
schema_registry_client = SchemaRegistryClient(schema_registry_conf)

# Avro schema
user_event_schema = """
{
    "type": "record",
    "name": "UserEvent",
    "namespace": "com.example.events",
    "fields": [
        {"name": "event_id", "type": "string"},
        {"name": "user_id", "type": "string"},
        {"name": "event_type", "type": "string"},
        {"name": "timestamp", "type": "long", "logicalType": "timestamp-millis"},
        {"name": "properties", "type": {"type": "map", "values": "string"}}
    ]
}
"""

avro_serializer = AvroSerializer(
    schema_registry_client,
    user_event_schema,
    lambda obj, ctx: obj  # Identity function for dict
)

# Producer config
producer_conf = {
    'bootstrap.servers': 'kafka:9092',
    'acks': 'all',  # Wait for all replicas
    'retries': 3,
    'linger.ms': 5,  # Batch messages
    'compression.type': 'snappy',
}

producer = Producer(producer_conf)

def delivery_report(err, msg):
    if err:
        print(f'Delivery failed: {err}')
    else:
        print(f'Message delivered to {msg.topic()} [{msg.partition()}]')

def produce_event(event: dict):
    producer.produce(
        topic='events.user',
        key=event['user_id'].encode('utf-8'),
        value=avro_serializer(
            event,
            SerializationContext('events.user', MessageField.VALUE)
        ),
        callback=delivery_report
    )
    producer.flush()
```

### Kafka Consumer (Flink)

```python
# flink_jobs/realtime_aggregation.py
from pyflink.datastream import StreamExecutionEnvironment
from pyflink.table import StreamTableEnvironment, EnvironmentSettings
from pyflink.table.window import Tumble

# Setup
env = StreamExecutionEnvironment.get_execution_environment()
env.set_parallelism(4)
env.enable_checkpointing(60000)  # Checkpoint every 60 seconds

settings = EnvironmentSettings.in_streaming_mode()
table_env = StreamTableEnvironment.create(env, environment_settings=settings)

# Kafka source table
table_env.execute_sql("""
    CREATE TABLE user_events (
        event_id STRING,
        user_id STRING,
        event_type STRING,
        event_time TIMESTAMP(3),
        properties MAP<STRING, STRING>,
        WATERMARK FOR event_time AS event_time - INTERVAL '5' SECOND
    ) WITH (
        'connector' = 'kafka',
        'topic' = 'events.user',
        'properties.bootstrap.servers' = 'kafka:9092',
        'properties.group.id' = 'flink-aggregation',
        'scan.startup.mode' = 'latest-offset',
        'format' = 'avro-confluent',
        'avro-confluent.url' = 'http://schema-registry:8081'
    )
""")

# Aggregation with tumbling window
table_env.execute_sql("""
    CREATE TABLE user_metrics_1min (
        window_start TIMESTAMP(3),
        window_end TIMESTAMP(3),
        user_id STRING,
        event_count BIGINT,
        unique_event_types BIGINT,
        PRIMARY KEY (window_start, user_id) NOT ENFORCED
    ) WITH (
        'connector' = 'upsert-kafka',
        'topic' = 'metrics.user.1min',
        'properties.bootstrap.servers' = 'kafka:9092',
        'key.format' = 'json',
        'value.format' = 'json'
    )
""")

# Real-time aggregation query
table_env.execute_sql("""
    INSERT INTO user_metrics_1min
    SELECT 
        TUMBLE_START(event_time, INTERVAL '1' MINUTE) AS window_start,
        TUMBLE_END(event_time, INTERVAL '1' MINUTE) AS window_end,
        user_id,
        COUNT(*) AS event_count,
        COUNT(DISTINCT event_type) AS unique_event_types
    FROM user_events
    GROUP BY 
        TUMBLE(event_time, INTERVAL '1' MINUTE),
        user_id
""")
```

---

## Data Quality Framework

### Great Expectations Setup

```python
# data_quality/expectations.py
import great_expectations as gx

# Initialize context
context = gx.get_context()

# Create expectations suite
suite = context.add_or_update_expectation_suite("orders_suite")

# Define expectations
expectations = [
    # Schema expectations
    gx.expectations.ExpectColumnToExist(column="order_id"),
    gx.expectations.ExpectColumnToExist(column="customer_id"),
    gx.expectations.ExpectColumnToExist(column="total_amount"),
    
    # Data type expectations
    gx.expectations.ExpectColumnValuesToBeOfType(
        column="order_id",
        type_="str"
    ),
    gx.expectations.ExpectColumnValuesToBeOfType(
        column="total_amount",
        type_="float"
    ),
    
    # Null expectations
    gx.expectations.ExpectColumnValuesToNotBeNull(column="order_id"),
    gx.expectations.ExpectColumnValuesToNotBeNull(column="customer_id"),
    
    # Value range expectations
    gx.expectations.ExpectColumnValuesToBeBetween(
        column="total_amount",
        min_value=0,
        max_value=100000
    ),
    
    # Uniqueness expectations
    gx.expectations.ExpectColumnValuesToBeUnique(column="order_id"),
    
    # Referential integrity (via custom expectation)
    gx.expectations.ExpectColumnDistinctValuesToBeInSet(
        column="status",
        value_set=["pending", "processing", "shipped", "delivered", "cancelled"]
    ),
    
    # Freshness expectation
    gx.expectations.ExpectColumnMaxToBeBetween(
        column="created_at",
        min_value={"$PARAMETER": "yesterday"},
        max_value={"$PARAMETER": "now"}
    ),
    
    # Volume expectation
    gx.expectations.ExpectTableRowCountToBeBetween(
        min_value=1000,
        max_value=1000000
    ),
]

# Add expectations to suite
for expectation in expectations:
    suite.add_expectation(expectation)

# Save suite
context.save_expectation_suite(suite)
```

### Data Quality Checks in Pipeline

```python
# dags/tasks/quality_checks.py
from great_expectations.checkpoint import Checkpoint
from great_expectations.core.batch import RuntimeBatchRequest
import great_expectations as gx

def run_quality_checks(table_name: str, date: str) -> dict:
    """Run data quality checks and return results."""
    
    context = gx.get_context()
    
    # Create batch request
    batch_request = RuntimeBatchRequest(
        datasource_name="snowflake_datasource",
        data_connector_name="runtime_connector",
        data_asset_name=table_name,
        runtime_parameters={
            "query": f"SELECT * FROM {table_name} WHERE date = '{date}'"
        },
        batch_identifiers={"batch_date": date}
    )
    
    # Run checkpoint
    checkpoint = Checkpoint(
        name=f"{table_name}_checkpoint",
        run_name_template=f"{table_name}_{date}",
        data_context=context,
        batch_request=batch_request,
        expectation_suite_name=f"{table_name}_suite",
        action_list=[
            {
                "name": "store_validation_result",
                "action": {"class_name": "StoreValidationResultAction"}
            },
            {
                "name": "update_data_docs",
                "action": {"class_name": "UpdateDataDocsAction"}
            },
            {
                "name": "send_slack_notification",
                "action": {
                    "class_name": "SlackNotificationAction",
                    "slack_webhook": "${SLACK_WEBHOOK}",
                    "notify_on": "failure"
                }
            }
        ]
    )
    
    results = checkpoint.run()
    
    # Check if passed
    if not results.success:
        failed_expectations = [
            result.expectation_config.expectation_type
            for result in results.list_validation_results()[0].results
            if not result.success
        ]
        raise ValueError(f"Quality checks failed: {failed_expectations}")
    
    return results.to_json_dict()
```

---

## Monitoring & Observability

### Pipeline Metrics

```python
# monitoring/metrics.py
from datadog import initialize, statsd
from functools import wraps
import time

initialize(statsd_host='localhost', statsd_port=8125)

def track_pipeline_metrics(pipeline_name: str):
    """Decorator to track pipeline metrics."""
    
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            start_time = time.time()
            
            try:
                result = func(*args, **kwargs)
                
                # Success metrics
                duration = time.time() - start_time
                statsd.timing(f'pipeline.{pipeline_name}.duration', duration * 1000)
                statsd.increment(f'pipeline.{pipeline_name}.success')
                
                # Row count if available
                if hasattr(result, 'row_count'):
                    statsd.gauge(f'pipeline.{pipeline_name}.row_count', result.row_count)
                
                return result
                
            except Exception as e:
                # Failure metrics
                duration = time.time() - start_time
                statsd.timing(f'pipeline.{pipeline_name}.duration', duration * 1000)
                statsd.increment(f'pipeline.{pipeline_name}.failure')
                statsd.increment(f'pipeline.{pipeline_name}.error.{type(e).__name__}')
                
                raise
        
        return wrapper
    return decorator

# Usage
@track_pipeline_metrics('daily_orders')
def process_daily_orders(date: str):
    # Pipeline logic
    pass
```

### Alerting Configuration

```yaml
# datadog/monitors.yml
monitors:
  - name: "Pipeline Failure - Daily Orders"
    type: metric alert
    query: "sum(last_5m):sum:pipeline.daily_orders.failure{*} > 0"
    message: |
      Pipeline daily_orders has failed!
      
      Check logs: https://logs.example.com/daily_orders
      Runbook: https://wiki.example.com/runbooks/daily_orders
      
      @slack-data-alerts @pagerduty-data-oncall
    tags:
      - pipeline:daily_orders
      - severity:critical
    
  - name: "Data Freshness - Orders"
    type: metric alert
    query: "avg(last_15m):avg:data.freshness.orders{*} > 3600"
    message: |
      Orders data is stale (>1 hour old)!
      
      Last update: {{value}} seconds ago
      Expected: < 3600 seconds
      
      @slack-data-alerts
    tags:
      - freshness:orders
      - severity:high

  - name: "Row Count Anomaly - Daily Orders"
    type: anomaly detection
    query: "avg(last_1d):sum:pipeline.daily_orders.row_count{*}"
    message: |
      Unusual row count detected for daily_orders
      
      Current: {{value}}
      Expected range: based on historical patterns
      
      @slack-data-alerts
```

---

## Pipeline Design Checklist

### Before Development

- [ ] Requirements documented (SLA, freshness, volume)
- [ ] Data sources identified and access granted
- [ ] Schema and data quality expectations defined
- [ ] Destination schema designed
- [ ] Backfill strategy planned
- [ ] Cost estimates reviewed

### During Development

- [ ] Idempotent design (safe to re-run)
- [ ] Incremental processing where possible
- [ ] Error handling and retries configured
- [ ] Logging and metrics added
- [ ] Data quality checks implemented
- [ ] Documentation written

### Before Production

- [ ] Testing with realistic data volumes
- [ ] Performance benchmarks met
- [ ] Monitoring dashboards created
- [ ] Alerting configured
- [ ] Runbook written
- [ ] On-call rotation established

### Post-Production

- [ ] SLA monitoring active
- [ ] Cost tracking enabled
- [ ] Regular data quality reports
- [ ] Periodic optimization reviews

---

## Final Step: Quality Assurance

Before deploying any pipeline:

1. **Backfill Test**: Run historical backfill to verify correctness
2. **Performance Test**: Validate with production-scale data
3. **Failure Test**: Simulate failures to verify recovery
4. **Quality Validation**: Verify all quality checks pass
5. **Documentation Review**: Ensure runbooks are complete

The pipeline is ready when it can handle 3x expected volume with 99.9% reliability and full observability.
