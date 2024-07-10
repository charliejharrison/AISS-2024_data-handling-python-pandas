---
source: Rmd
title: "Key Concepts in pandas"
teaching: 20
exercises: 2
questions:
- "What is `pandas` and why should I use it?"
objectives:
- "Explain the difference between Series and DataFrames."
- "Explain the limitations and basic features of Series DataFrames."
keypoints:
- "pandas is a powerful data manipulation library for Python."
- "pandas provides two main data structures: Series and DataFrames."
- "A Series is a one-dimensional array-like object."
- "A DataFrame is a two-dimensional array-like object."
- "Elements in Series and DataFrames can be changed, but they cannot be added or removed - to do this you need to create a new Series or DataFrame. This is by design"
---

{% include links.md %}

## Data types in `pandas`

On top of all the usual Python data types, Pandas has two main data types of its own: `Series` and `DataFrame`.

```python
import pandas as pd

pd.Series([1, 2, 3, 4, 5])
```

A `Series` is a 1-dimensional container, similar to a list with some extra functionality. We can create a `Series` from a list:

```python
pd.Series([])
```

It is intended to store only data of the same type. There are ways around this (creating a `Series` with a generic type like `object`), but it's best to consider it as a general guideline.

 and a name.

All elements of a series must be of the same data type.

A `DataFrame` is a collection of `Series`.
