---
editable: false
sourcePath: en/_api-ref/datalens/function-ref/QUARTER.md
---

# QUARTER



#### Syntax {#syntax}


```
QUARTER( datetime )
```

#### Description {#description}
Returns the number of the quarter (from `1` to `4`) of the year of the specified date `datetime`.

**Argument types:**
- `datetime` — `Date | Datetime`


**Return type**: `Integer`

#### Example {#examples}

```
QUARTER(#2019-01-23#) = 1
```


#### Data source support {#data-source-support}

`ClickHouse 21.8`, `Files`, `Google Sheets`, `Microsoft SQL Server 2017 (14.0)`, `MySQL 5.7`, `Oracle Database 12c (12.1)`, `PostgreSQL 9.3`, `Yandex Documents`, `YDB`.
