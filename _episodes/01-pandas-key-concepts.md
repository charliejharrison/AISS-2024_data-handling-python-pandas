---
source: Rmd
title: "Key Concepts in `pandas`"
teaching: 20
exercises: 2
questions:
- "What is `pandas` and why should I use it?"
objectives:
- "Explain the difference between Series and DataFrames."
- "Explain the limitations and basic features of Series DataFrames."
keypoints:
- "`pandas` is a powerful data manipulation library for Python."
- "`pandas` provides two main data structures: Series and DataFrames."
- "A `Series` is a one-dimensional array-like object."
- "A `DataFrame` is a two-dimensional array-like object."
---

{% include links.md %}

First things first, let's import `pandas`. 

```python
import pandas as pd
```

This gives us access to all the useful code in the `pandas` library. That's about 1500 files and 400,000 lines of Python, plus some high-performance bits written in Cython and C - so we just saved ourselves a lot of work. 

## Data types in `pandas`

On top of all the usual Python object types (like `str`, `int` and `float`), `pandas` has two main types of its own: `Series` and `DataFrame`. These are both _containers_ for labelled data.

A `Series` is a 1-dimensional container, similar to a list, with some extra functionality. We can create a `Series` from a `list`, a `range`, or a `dict`:

```python
series_from_list = pd.Series([1, 2, 3])
series_from_range = pd.Series(range(start=0, stop=1000, step=2))
series_from_dict = pd.Series({1: 'first', 2: 'second', 3: 'third'})
```

`Series` are intended to store data a single type. This means that in general, a `Series` should contain only, for example, text or numbers - not both. 

A `DataFrame` is essentially a collection of `Series`. You can think of a `DataFrame` as a table, like in Microsoft Excel, with `Series` for columns. 

```callout 
A `Series` is 1-dimensional - like a vector, or a column
A `DataFrame` is 2-dimensional - like a matrix, or a table
```
