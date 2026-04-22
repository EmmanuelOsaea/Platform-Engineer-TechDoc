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
