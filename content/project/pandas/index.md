---
title: Data Engineering Zoomcamp y2025 
date: 2024-12-20
# external_link: https://leetcode.com/studyplan/introduction-to-pandas/
tags:
  - Data Engineering
  - Docker
  - Coding-Practice
---

# Pandas: A Flexible and Powerful Library for Data Analysis and Manipulation

My learning journey following the data engineering courses led by DataTalk Club. (https://github.com/DataTalksClub/data-engineering-zoomcamp/tree/main)

---

## Pre-course set up

1. Download **Docker**
2. Set up **Google Cloud**
3. Watch **Pre-course FAQ**
4. Have a **Github account** and **CodeSpaces**

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
