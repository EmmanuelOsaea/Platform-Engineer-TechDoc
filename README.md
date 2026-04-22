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
{

```

