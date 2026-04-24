# 1. AST Optimization example
DSL input(Example):
```
SELECT candidates WHERE experience is > 12 AND experience is > 6
```
# Step 1: Parse into AST(simplified)
```
{
"type": "SELECT",
"table": "candidates",
"where": {
"type": "AND",
"conditions": [
{"field": "experience", "operator": ">", "value": 12},
{"field": "experience", "operator": ">", "value": 6},
]
}
}
```

# Step 2: AST Optimization - Remove redundant condition
• This example shows that since ```experience > 6``` . The former condition which is ```experience > 12`` is impertinent.

# Optimized AST:
```
{
"type": "SELECT",
"table": "candidates",
"where": {
  "field": "experience"
  "operator": ">",
  "value": 12
  }
  }
```

# Step 3: Generate SQL
```
SELECT * FROM candidates WHERE experience > 12;
```

# 2. Intermediate Representation (IR) and SQL Generation
# DSL input:
```
FIND candidates WITH skills CONTAINING 'Java', C# AND location = 'In-Office', 'Remote'
```

# Step 1: Convert DSL to IR(abstract form)
```
{
"operation": "filter",
"table": "candidates",
"filters": [
   {
 "field": "skills",
 "operator": "contains-all",
 "value": ["Java", "C#"]
},
{
"field": "location",
"operator": "in",
"value": ["In-office", "Remote"]
}
]
}
```

# Step 3: Generate SQL with bind variables (for security and reuse)
```
SELECT * FROM candidates WHERE skills LIKE '%' || ? || '%' AND location = ?;
```

# Bind variables
```
["Java", "C#", "In-office", "Remote"]
```

# 3. Cost-Based Optimization Example
# Scenario:

• DSL query requires merging or attaching ```candidates``` and ```applications``` tables.

• ```candidates``` has 5 million rows, ```applications``` has 15,000 rows.

• Index on ```applications.candidate_id```.

# Step 1: Generate two possible SQL queries
# Option A: 
```
SELECT c.*, a.* FROM candidates c JOIN applications a ON c.id = a.candidate_id WHERE a.status = 'pending';
```

# Option B: 
```
SELECT a.*, c.* FROM applications a JOIN candidates c ON a.candidate_id = c.id WHERE a.status = 'pending';
```

# Step 2: Cost estimation
So, based on the example provided from my research Option B is cheaper due to it begining from smaller ```applications``` table filtered by status then merges ```candidates```.

# Step 3: Choose Option B for final SQL generation
# 4. SQL Template Caching
# Common DSL pattern:
```
FIND candidates WHERE skills CONTAINING 'Java', 'C#' AND location IN ('In-office', 'Remote')
```
# Template:
```
SELECT * FROM candidates
WHERE skills LIKE '%' || ? || '%'
  AND skills LIKE '%' || ? || '%'
  AND location IN (?, ?);
```

# Implementation:
• It entails multiple ```skills LIKE``` condition.

• Also, multiple locations in an ```IN``` clause.

# 5. Parallel Compilation
# Scenario:

• Multiple DSL queries arrive simultaneously.

# Implementation(Java example):

```

```

# Implementation(C# example):

```

```

# 6. Dialect-Specific Adaptation

# Example: PostgreSQL JSON query optimization
# DSL:
```

```
# PostgreSQl SQL:
```

```

# MySQl SQL:

```

```



