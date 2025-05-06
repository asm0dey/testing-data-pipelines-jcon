---
# You can also start simply with 'default'
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://cover.sli.dev
# some information about your slides (markdown enabled)
title: Himalayan Peaks of Testing Data Pipelines
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
# open graph
# seoMeta:
#  ogImage: https://cover.sli.dev
aspectRatio: 16/9
layout: cover
# real width of the canvas, unit in px
canvasWidth: 800
lineNumbers: true
selectable: true
colorSchema: dark
---

# Himalayan Peaks

of Testing Data Pipelines

Kseniia Tomak, HelloFresh

Pasha Finkelshteyn, BellSoft

---
layout: image-contain-right
image: '/asm0dey.jpg'
backgroundPosition: right
backgroundSize: cover
---
# `whoami`

<v-clicks>

- <div v-after>Pasha Finkelshteyn</div>
- Dev <noto-v1-avocado /> at BellSoft
- ≈10 years in JVM. Mostly <logos-java /> and <logos-kotlin-icon />
- And <logos-spring-icon />
- <logos-twitter /> asm0di0
- <logos-mastodon-icon /> @asm0dey@fosstodon.org

</v-clicks>

---
layout: image-contain-right
image: '/ktomak.jpg'
backgroundPosition: right
backgroundSize: cover
---
# `whoami`

<v-clicks>

- <div v-after>Kseniia Tomak</div>
- Data Engineering Manager at HelloFresh
- ≈14 years of Engineering experience. Mostly Data engineering
- <logos-linkedin-icon /> ksenia-tomak

</v-clicks>

---
layout: image
image: /etl.png
backgroundSize: 60%
---

<style>
.center-title {
  text-align: center;
  margin-top: .2em;
}
</style>

# Data processing { .center-title }

---
layout: image
image: /datalake.svg
backgroundSize: contain
---

# Data Lake?

---
layout: image
image: /pipeline.svg
backgroundSize: contain
---

---

# Who needs pipelines

* Data Scientists
* Data Analytics
* Data Engineers
* POs
* any data-driven person

---
layout: fact
---

# It has to be tested

---
layout: statement
---

# Pyramid of testing?

---
layout: image-contain-right
image: /pyramid.png
backgroundSize: contain
backgroundPosition: right
---

# Pyramid of testing?

---
layout: image
image: /real_pyramid.webp
---

---
layout: image
image: /unit.svg
backgroundSize: 40%
---

# Pyramid of testing. Unit.

---
layout: image
image: /datalake_bronze.svg
backgroundSize: contain
---

# Bronze→Silver pipeline

---
layout: image-contain-right
image: /typical_pipeline.svg
class: bg-right
---

# Typical pipeline

```java {all|1-4|6-7|8|9-10}
StructType schema = new StructType(new StructField[]{
    new StructField("pk", DataTypes.LongType, false, Metadata.empty()),
    new StructField("aa", new DataTypes.StringType(), false, Metadata.empty())
})

spark.read
    .schema(schema)
    .csv(/* path */)
    .map(/* mapper */)
    .show() // terminal operation
```

---

# Unit testing of pipeline

What may we test here?

A pipeline should transform data correctly!

*Correctness is a business term*

---
layout: image
image: /fakes.svg
backgroundSize: containa
class: bg-right
---

# Let's paste fakes!

Fake input data

Reference data at the end of the pipeline

---

# Tools
<div></div>

[holdenk/spark-testing-base](https://github.com/holdenk/spark-testing-base) ← Tools to run tests

[MrPowers/spark-daria](https://github.com/MrPowers/spark-daria) ← tools to easily create test data

---

### When Should You Run It?

# On CI/CD

---

# Things we usually forget to test

* empty source/target datasets,
* one scenario per one test

---

# Things we usually forget after unit testing

100% coverage doesn't mean that we're 100% safe

---
layout: image
image: /component.svg
backgroundSize: 70%
---

# Component testing (or Integration testing)

---
layout: image
image: https://d33wubrfki0l68.cloudfront.net/a661dbbe55be3e9cb77889f24835a44c6daf53c2/ce0aa/logo.png
backgroundSize: cover
---

---
layout: image
image: /pipeline_docker.svg
backgroundSize: contain
---

# Testcontainers

---

# Testcontainers

Supported languages:

* Java (and compatibles: Scala, Kotlin, etc.)
* LocalStack Module (AWS)
* Python
* Go
* Node.js
* Rust
* .NET

---

# Testcontainers

```java {all|2,1|5-8,4|10-11|12|14-15|16-18}
@Testcontainers
class PostgreSQLIntegrationTest {

    @Container
    static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:17-alpine")
        .withDatabaseName("testdb")
        .withUsername("testuser")
        .withPassword("testpass");

    @Test
    void testDatabaseOperations() throws Exception {
        String jdbcUrl = postgres.getJdbcUrl();
        // omit here...
        try (Connection connection = DriverManager.getConnection(jdbcUrl, username, password);
             Statement statement = connection.createStatement()) {
                 ResultSet resultSet = statement.executeQuery("SELECT email FROM users WHERE id = 1");
                 assertTrue(resultSet.next());
                 assertEquals("test@example.com", resultSet.getString("email"));
        }
    }
}
```
---

### When Should You Run It?

# On CI/CD (but you might want to consider file changes in specific directories)

---

# Things we usually forget to test

* reruns (idempotency),
* data duplicates (inside/outside batch),
* incremental runs logic,
* metastore data (partitions, parameters, etc)

---
layout: image
image: /real_systems.svg
backgroundSize: contain
---

---
layout: image
image: /azure_pipeline.svg
backgroundSize: contain
---

---

# Real systems

<div></div>

Why are component tests not enough?

* vendor lock tools (DB, processing, etc.)
* external error handling
* access to data
* overall integration (especially for platform tooling)

---
layout: image
image: /real_data.svg
backgroundSize: contain
---

---
layout: image
image: sampling.svg
backgroundSize: contain
---

# Real data

<div></div>

Get data samples from prod

Anonymize it

---
layout: image
image: /reference_pipeline.svg
backgroundSize: 80%
---

# Compare to reference

---

# Comparison example

| **gender** | **reference** | **id** | **match** |
|--------|-----------|----|-------|
| m      | m         | 1  | true  |
| f      | c         | 2  | false |
| u      | u         | 3  | true  |
| c      | c         | 4  | true  |
| m      | f         | 5  | false |

---
layout: center
---

# Real data

<div></div>

Deploy full data backup on stage env, anonymize it 🤑

---
layout: statement
---

# In usual testing you won't trust your code

---
layout: statement
---

# In pipeline testing you won't trust both your code and your data

---

# Real data expectations

<div></div>

Test:

✅ no data

✅ valid data

❓ invalid data

❓ illegal data format

---

# Real data expectations. Tools:

* [great expectations](https://greatexpectations.io/)
* [Deequ](https://github.com/awslabs/deequ)

---

# Real data expectations

* profilers
* constraint suggestions
* constraint verification
* metrics
* metrics strores

---

```python {all|4-9|11-15|18}
from pyspark.sql.types import Row, StructureType
from datetime import datetime

schema = {
    "type": "struct",
    "fields": [
        {"name": "Id", "type": "long", "nullable": False, metadata: {}},
        {"name": "SaleDate", "type": "timestamp", "nullable": False, metadata: {}},
        {"name": "Country", "type": "string", "nullable": False, metadata: {}},]}

table_rows = [
    Row(1, datetime(2025, 1, 1, 10, 0, 0), "DE"),
    Row(2, datetime(1000, 1, 1, 10, 0, 0), "NL"),
    Row(3, datetime(2023, 1, 1, 10, 0, 0), "NO"),
    Row(4, datetime(2024, 1, 1, 10, 0, 0), ""),
]

sample_df = spark.createDataFrame(table_rows, StructType.fromJson(schema))
```

---

# Great Expectations

```json {all|2-6|8|9,11,12}
{
  "result": {
    "element_count": 4,
    "unexpected_count": 2,
    "unexpected_percent": 50.0,
    "partial_unexpected_list": ["NO", ""]
  },
  "success": false,
  "expectation_config": {
    "kwargs": {
      "column": "Country",
      "value_set": ["DE", "NL"]
    }
  }
}
```

---

# Java Deequ

```java {all|1|3|5|7|7,8|9-11|12|14,15}
SparkSession spark = SparkSession.builder().config(sparkConf).getOrCreate();

Dataset<Row> data = spark.sqlContext().read().parquet("/some/path/dataset");

Check check = new Check(spark, CheckLevel.Error(), "Review Check");

VerificationSuite checkResult = new VerificationSuite(spark)
    .onData(data)
    .addCheck(check
        .isComplete("Country")
        .isContainedIn("Country", List.of("DE", "NL")))
    .run();

verificationDf = VerificationResult.checkResultsAsDataFrame(spark, checkResult);
verificationDf.show()
```

---

# Java Deequ. Results

| **constraint** | **constraint_status** | **constraint_message** |
|-- |-- |-- |
| CompletenessConstraint | Success | |
| ComplianceConstraint | Failure |  Value: 0.5 does not meet the constraint requirement! |

---

# Real data ecpectations. Use cases

<v-clicks>

* pre-ingestion and post-ingestion data validaton
* before pipeline development
* monitoring and alerting

</v-clicks>

---
layout: image
image: /monitoring.svg
backgroundSize: contain
---

---

# Monitoring

Why?

* The only REAL testing is production
* Data tends to change over time

---

# Monitoring

What?

<v-clicks>

* data volumes
* time (SLAs)
* dead letter queue monitoring
* service health
* business metrics

</v-clicks>
---

# Monitoring

How?

<v-clicks>

* use Listeners
* Data Aggregators
* Airflow (Dagster, Prefect, etc)

</v-clicks>

---
layout: statement
---

# Data pipeline is always a DAG
## Monitoring should visualize it

---
layout: image
image: /airbnb.webp
backgroundSize: contain
---

# Monitoring visualization

<div></div>

[Visualizing data timelines](https://medium.com/airbnb-engineering/visualizing-data-timeliness-at-airbnb-ee638fdf4710) { .color-black }

---

# End-to-End tests

Compare with reports, old DWH

Multiple dimensions:

* data
* data latency
* performance, scalability

---
layout: image
image: /performance.svg
backgroundSize: contain
---

---
layout: image-contain-right
image: /performance_tests.svg
---

# Performance Tests

<br/>

* start with SLO
* test your initial data load

---
layout: image
image: /real_prod.svg
backgroundSize: 80%
---

# Real prod

<img/>

Run a parallel job with a different sink

---
layout: image
image: /costs.svg
backgroundSize: contain
---

---

# Summary

<v-clicks>

*    Testing pipeline is like testing code
*    Testing pipelines is not like testing code
*    Pipeline quality is not only about testing
*    Sometimes testing outside of production is tricky

</v-clicks>

---
layout: center
---

# Thanks!
# Questions? 🙇

<div></div>

@asm0di0

@if_no_then_yes
