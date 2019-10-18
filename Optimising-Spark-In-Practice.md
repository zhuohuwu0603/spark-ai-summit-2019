# Optimising Spark Practically

## Knowing the infrastructure
- Core count and speed
- Mem per core
- Disk type, physical speed
- Network speed and topology
- Data rate limit of the whole lake
- Cost per hr

## Inspection Tips
- Identifying biggest `spill` counts
- Try to minimise amount of data being scan (`lazy load`)
- Data bucketing is hard to make it right, always slowing down if poorly managed.
- Enforce `Hive Partition Pruning` improves the speed by triggering `lazy load`. (See Partition Filter)

## Side notes
- DeltaLake improves `lazy load` (Tied-in)
- Hive Partition NOT EQ to Spark Partitions
- Largest shuffle target size SHOULD BE less than 200 MB/partition
- Beware that some old (non-vectorised) UDFs cannot use Tungsten.

## Spark Partitioning Types
- Input - `spark.sql.files.maxPartitionBytes`, at the time of reading data
- Shuffle - `spark.sql.shuffle.partitions`, default to 200 as everyone knows and it is practically poor.
- Output  -  `repartition` or `coalesce` or `df.write.option("maxRecordsPerFile", NNN)`

To find the optimal Max input partition size, look at the following factors
- Input data size
- Number of cores

Reducing spills by
- Finding the optimal shuffle partition size

Do not use default 128MB default partition size when:
- UDFs
- Increment of parallelism
- Heavily nested data or repetitive data
- Explosion (or any generation of data per row per key, etc.) <-- ask for less memory to get some room for generated data later.

Example:
- Input: 15 GB
- Output: 1500 MB
- Cores: 10
- Each core will handle data at the size of 1.5GB, writing at 150 MB

## Adjusting Output Partitions
- `df.write.option("maxRecordsPerFile", nnnn)` 
- `df.coalesce().write`
- `df.repartition(nnn).write` - Do not repartition before joining, this causes shuffle
- `df.repartition(n, [col_1, ....])`
- `spark.sql.shuffle.partitions` - Let this handle number of partitions when shuffling (joining for example)
- `df.localCheckpoint().repartition(n).write` - Break the lineage, reset all shuffling partitions
- `df.localCheckpoint().coalesce(n).write`

## Balance data properly
- Identifying skew by looking at mean, 75th PR and max. Notice if mojority is on the left of curve leaving the large data on the right (minority.)

## Join Optimisation
- Broadcast join is the fastest (large v tiny)
- SortMergeJoins is the default and standard (large v large)
- `spark.sql.autoBroadcastJoinThreshold` default to 10MB
- Broadcast requries memory on the driver, risking out of mem if broadcasted data is too large.
- Drop duplicates before joining or grouping

## Skewed aggregation

Tackle with salt

```scala
df.withColumn("salt", lit(_____))
  .groupBy("key1", "key2", "salt")
  .agg(____)
  .drop("salt")
```

## Worth looking at
- ForkJoin (`new ForkJoinTaskSupport(new ForJoinPool(parallelism))` where `parallelism` is a function) 












