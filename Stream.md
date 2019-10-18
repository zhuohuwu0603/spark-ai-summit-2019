# Streaming

Setup
- Kafka
- Spark structured streaming app (introduced in Spark 2.0) as a reader of Kafka
- Spark streaming keep writing into files
- Huge I/O throughput which does not fit inside memory definitely (many tiny messages)
- This underutilises the cluster (mostly they remain idle)

Downsides of Structured Streaming
- Does not support stateful approach (required local aggregation manually)
- Checkpoint on S3 is not natively seamless

## RDR (Raw Data Repository)

- Storing topic messages on S3 as parquets, partitioined by date
- Kafka generates RDR files on S3, Spark runs as consumer reading from RDR

## Futher references

medium.com/nmc-tech


