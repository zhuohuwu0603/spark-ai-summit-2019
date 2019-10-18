# Koalas
  - Abstraction over background SparkJob, with 100% Pandas syntaxes, similar structures (df, series)
  - Row indexing is compromised internally
  - Row indexing is less than 15% identical to Pandas
  - Compatible with spark specific functions e.g. to_parquet(), read_parquet(), to_csv(), etc.
  - Compatible with matplotlib out-of-the-box
  - Special join types cannot yet be specified
  - `InteralFrame` is immutable and wrapped by pyspark, made used by Koalas
  - Any mutable operations on python side will result in creating multiple `InternalFrame` to carry changes.
  - Bi-weekly releases

# Koalas Future Releases
  - Rolling, window functions `WILL BE` supported, and more will be added 
  - Categorical data types `WILL BE` supported

## Use cases
  - Virgin Hyperloop migrated from Pandas to Koalas, obtaining 10x faster with less than 1% of code changes
  - Same overhead with pyspark (they did run benchmark)

## Snippets

```python
# Converting pandas dataframes to Koalas dataframe
kdf = ks.from_pandas(pandas_df)

# Converting from native Spark dataframe
kdf = spark_df.to_koalas()

# Convert koalas dataframe to np
array = kdf.to_numpy()

# Inspect basic stats of columns (mean, min, max, etc)
kdf.describe()

# Read from parquet
kdf = ks.read_parquet('PATH')

# NOT SUPPORTED, causing AnalysisError
kdf['c'] = kdf_another['a'] + kdf_another['b']

```