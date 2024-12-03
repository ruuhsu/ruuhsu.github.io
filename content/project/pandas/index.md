---
title: Introduction to Pandas
date: 2024-12-01
# external_link: https://leetcode.com/studyplan/introduction-to-pandas/
tags:
  - Python
  - Leetcode
  - Coding-Practice
---

# Pandas: A Flexible and Powerful Library for Data Analysis and Manipulation

Pandas is a go-to library for handling and analyzing data in Python, offering labeled data structures such as DataFrames. Whether you're cleaning messy data or performing complex transformations, Pandas makes data manipulation simple and efficient.

---

## Getting Started: Creating a DataFrame from a List

A **DataFrame** is a core structure in Pandas that represents tabular data. Here's how you can create a DataFrame from a list of lists in Python:

```python
from typing import List
import pandas as pd

def createDataframe(student_data: List[List[int]]) -> pd.DataFrame:
    """
    Create a DataFrame from a list of student data.

    Args:
        student_data (List[List[int]]): A list of lists where each inner list contains [student_id, age].

    Returns:
        pd.DataFrame: A DataFrame with 'student_id' and 'age' as columns.
    """
    colnames = ["student_id", "age"]
    try:
        result = pd.DataFrame(student_data, columns=colnames)
        return result
    except ValueError as e:
        raise ValueError("Input data does not match the expected column structure.") from e
