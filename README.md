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
FIND candidates WITH skills CONTAINING 'JAVA', C# AND location = 'In-Office', 'Remote'
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
• 
