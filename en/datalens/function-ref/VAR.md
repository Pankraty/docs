---
editable: false
sourcePath: en/_api-ref/datalens/function-ref/VAR.md
---

# VAR



#### Syntax {#syntax}

{% list tabs %}

- Standard

  ```
  VAR( value )
  ```

- Extended

  ```
  VAR( value
       [ FIXED ... | INCLUDE ... | EXCLUDE ... ]
       [ BEFORE FILTER BY ... ]
     )
  ```

{% endlist %}

#### Description {#description}
Returns the statistical variance of all values in an expression based on a selection from the population.

**Argument types:**
- `value` — `Fractional number | Integer`


**Return type**: `Fractional number`

#### Example {#examples}

```
VAR([Profit])
```


#### Data source support {#data-source-support}

`Materialized Dataset`, `ClickHouse 19.13`, `Microsoft SQL Server 2017 (14.0)`, `MySQL 5.6`, `Oracle Database 12c (12.1)`, `PostgreSQL 9.3`, `YDB`.
