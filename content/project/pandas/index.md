---
title: Data Engineering Zoomcamp y2025 
date: 2024-12-20
# external_link: https://leetcode.com/studyplan/introduction-to-pandas/
tags:
  - Data Engineering
  - Docker
  - Coding-Workshop
---

# Data Engineering Zoomcamp

My learning journey following the data engineering courses led by [DataTalk Club](https://github.com/DataTalksClub/data-engineering-zoomcamp/tree/main).

---

## Pre-course set up

1. Download **Docker** (2025/01/03 Note: Github Codespaces have built-in docker)
2. Set up **Google Cloud** (2025/01/03 Note: Set up right before the class for Google Cloud 3-months free trials)
3. Watch **Pre-course FAQ**
4. Have a **Github account** and **CodeSpaces**

## Pre-course resources

1. [Github Codespace Tutorial](https://www.youtube.com/watch?v=XOSUt8Ih3zA&list=PL3MmuxUbc_hKihpnNQ9qtTmWYy26bPrSb&index=2)
### Steps summarization to set up Codespace and start coding in VS code
- Open a new repository (Zoomcamp2025)
- Open codespace and create a new codespace under the created repository (Zoomcamp2025)
- Click the top left button and select 'Open in VS code Desktop'
- Make sure the 'Github Codespace' extension is installed in the VC code
- Open in integrated terminal
- Install 'jupyter notebook'
  ```python
  pip install juypter
- Docker create container
  ```python
  docker network create pg-network
  docker volume create --name dtc_postgres_volume_local -d local
  
  docker run -it \
  -e POSTGRES_USER="root" \
  -e POSTGRES_PASSWORD="root" \
  -e POSTGRES_DB="ny_taxi" \
  -v dtc_postgres_volume_local:/var/lib/postgresql/data \
  -p 5432:5432 \
  --network=pg-network \
  --name pg-database \
  postgres:13
- Docker open 'pgAdmin'
  ```python
  docker run -it \
  -e PGADMIN_DEFAULT_EMAIL="admin@admin.com" \
  -e PGADMIN_DEFAULT_PASSWORD="root"\
  -p 8080:80 \
  --network=pg-network \
  --name pgadmin \
  dpage/pgadmin4
  
- Done!

---

# [Module 1. 01-docker-terraform](https://github.com/DataTalksClub/data-engineering-zoomcamp/tree/main/01-docker-terraform)
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
