# Creator of Scikit-Learn 

Vision from Gaël Varoquaux, RIA

## Main focuses of the creator's team

Speeding up the process with following techniques

- Better batch processing
- More seamless integration with Joblib
- Smarter access to memory in C, not always exhaustively iterating the bytes
- Benefits more from Python3.8
- Avoid unnecessary memory copy during numpy serialisation (PEP 574)
- More distribution over network via `cloudpickle`

Introduction or advices
- Advice to use more often of `sklearn.impute.SimpleImpute`
- Conditional impute with `sklearn.impute.ImperativeImputer` (v.0.21)
- In case missing values are needed for prediction, use `ImperativeImputer(add_indicator=True)`
- Dealing with huge mispelled dataset with word distance encoding (one to many) as features
