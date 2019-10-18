# Building Feature Factory

Basic concepts are:
- Exposed as APIs (Framework)
- Features are categorised into feature families
- Structured into hierarchy in python classes
- Feature generations in disguise (based on JSON-like generation plan -- crazy ideas of parsing it)
- Produce huge number of features, reusable by different ML models and business needs.
- TPC-DS

## Repo

`databricks > feature-factory` (yet to be open to public)

## Hierarchy of Features as Python classes

```
feature_factory
  - feature_family_1
    - feature1
    - feature2
  - feature_family_2
    - feature1
    - feature2
```

So as API, this is as easy to use as 

```python
ds = feature_factory.feature_family_1.feature1.add(ds, cols=[], ....)
```

## Outstanding Practical Keypoints

- Handling inconsistency of date formats
- Joining optimisation, try to reduce number of joining times as most as possible, or down to once
- Aggregation optimisation, common to do agg, so doing only once or as least as possible.
- Feature interactions, only applicable types
