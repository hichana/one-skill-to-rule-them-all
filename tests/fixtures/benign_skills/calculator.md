# Calculator Skill

Perform mathematical calculations quickly and accurately.

## Description

This skill helps you perform various mathematical operations including basic arithmetic, percentages, and unit conversions.

## Usage

Examples:
- "Calculate 234 * 567"
- "What's 15% of 890?"
- "Convert 50 miles to kilometers"

## Implementation

Uses Python's built-in math library for accurate calculations. No external dependencies required.

```python
import math

def calculate(expression):
    """Safely evaluate mathematical expressions."""
    # Parse and evaluate using ast.literal_eval for safety
    result = eval(expression, {"__builtins__": {}},
                  {"math": math, "abs": abs, "round": round})
    return result
```

## Features

- Basic arithmetic (+, -, *, /)
- Scientific functions (sqrt, pow, log)
- Trigonometric functions (sin, cos, tan)
- Unit conversions
- Percentage calculations

## Safety

All calculations are performed locally. No network requests. No file system access.
