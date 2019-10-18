# Adding New Data Sources to Spark SQL

From the live coding session by Jacek Laskowski, a Spark contributor

## Setup

- Spark 2.12
- Java8

## Concepts

- `Data source API` is a pluggable abstraction of Spark SQL. Native in Spark.
- Some built-in data sources are parquet, kafka, avro, json, etc.
- Supporting both batch and streaming. This extensibility depends on the Spark releases too.
- The core API is `DataFrameReader`. Describe the input with `DataFrameReader.load()`.

The usage in the end will look like

```scala
spark.read.format("new_data_type").load
spark.read.format("new_data_type").load.explain // try this
```

We need to register a new `DataSource` as we create.

## Implementation of new DataSource

To be continued in the 2nd part of the talk, but I did not make it :)

```scala
def lookupDataSource(provider: String, ....) ....
```

## NOTE

- Need to create a class extending `DataSourceRegister`.
- Implements `def shortName()` representing the qualified name of the new data source.
- Need to create `resources/META-INF.services`
- Inside that folder, store the class with file named as `org.apache.spark.sql.sources.DataSourceRegister`
- 
