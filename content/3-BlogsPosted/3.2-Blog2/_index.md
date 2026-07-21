---
title: "Understanding Apache Celeborn on Amazon EMR"
date: 2026-07-20
weight: 2
pre: " <b> 3.2. </b> "
chapter: false
---

## Article information

| Field | Details |
| --- | --- |
| Topic | Big Data, Apache Spark, Amazon EMR, and cost optimization |
| Type | Research based on the AWS Big Data Blog |
| Publication platform | AWS Study Group on Facebook |
| Status | Published |
| Publication link | [View the post on Facebook](https://www.facebook.com/share/p/18VT4KGFLT/) |
| Research source | [High-performance Remote Shuffle Service on Amazon EMR with Apache Celeborn](https://aws.amazon.com/blogs/big-data/high-performance-remote-shuffle-service-on-amazon-emr-with-apache-celeborn/) |

## The Spark shuffle problem

The article examines **data shuffle** in large Apache Spark workloads. Shuffle redistributes data among executors for operations such as groupBy, join, reduceByKey, sorting, and key-based aggregation. It consumes network bandwidth, memory, and storage and can strongly affect job completion time.

## Limitations of traditional shuffle

### Data loss during Spot interruption

EC2 Spot Instances can reduce compute cost, but they may be interrupted. If an executor stores shuffle data on local disk, the data can disappear with the instance. Spark must recompute upstream stages, increasing runtime and reducing the expected savings.

### Over-provisioned storage

With a traditional External Shuffle Service, each worker needs enough local storage for possible shuffle output. Utilization is uneven, so the cluster may pay for storage that is not used effectively.

### Difficult scale-in

A node may finish its compute work but remain active because another task still needs its shuffle data. This coupling prevents timely scale-in and keeps nearly idle nodes running.

## Apache Celeborn

Apache Celeborn is an open-source **Remote Shuffle Service**. Rather than keeping shuffle data on Spark executors, it moves the data to a separate Celeborn cluster.

The main components are:

- **Leader/Primary:** manages metadata and cluster state.
- **Worker:** stores and serves shuffle data.
- **Client:** integrates with Spark and transfers data to Celeborn.

~~~text
Amazon EMR on EC2 / Amazon EMR on EKS
                    ↓
          Internal Network Load Balancer
                    ↓
         Apache Celeborn on Amazon EKS
             ├── Leader/Primary nodes
             └── Worker nodes
~~~

## Push-based shuffle

Spark executors push shuffle data to Celeborn Workers rather than requiring reducers to fetch data from many executors. This approach can:

- Reduce network connection fan-out during reads.
- Reduce dependence on executor local disks.
- Improve the ability to use Spot Instances.
- Allow Spark and Celeborn to scale independently.
- Improve Spark job resilience.

## Fault tolerance and configuration

Leader nodes can use Raft to maintain coordination state. Workers can use local NVMe for performance and replicate shuffle partitions across workers to reduce data-loss risk.

Important Spark settings include:

~~~properties
spark.shuffle.service.enabled=false
spark.shuffle.manager=org.apache.spark.shuffle.celeborn.SparkShuffleManager
spark.shuffle.sort.io.plugin.class=org.apache.spark.shuffle.celeborn.CelebornShuffleDataIO
spark.celeborn.master.endpoints=<NLB_DNS_NAME>:9097
spark.celeborn.client.push.replicate.enabled=true
spark.sql.adaptive.localShuffleReader.enabled=false
~~~

These values must be reviewed against the actual Spark, Celeborn, and EMR versions before use.

## Monitoring and observability

The source solution uses AWS Distro for OpenTelemetry to collect Prometheus metrics from Celeborn and visualize them with Grafana. Useful signals include active shuffles, push failures, worker status, leader changes, disk capacity, memory usage, JVM heap, and garbage collection.

## Connection to Team Task Management

My project does not use Amazon EMR or Apache Spark, but the separation principle still applies:

- The frontend is separated from EC2 and hosted on S3.
- Deadline checking is separated from the backend with Lambda.
- Data is separated from compute with Amazon RDS.
- Monitoring is treated as an architectural component.

## Lessons learned

- Big Data optimization is not only about selecting cheaper instances.
- Spot capacity is useful only when the system tolerates interruption.
- Separating compute from shuffle storage improves scaling flexibility.
- Higher reliability introduces cost and operational complexity.
- Monitoring should be designed from the start.
- Architecture must be evaluated against the real workload.

{{% notice info %}}
This is a research and analysis article based on the AWS Big Data Blog. I did not deploy a complete Apache Celeborn cluster in my personal project, so the article does not claim unverified performance results.
{{% /notice %}}
