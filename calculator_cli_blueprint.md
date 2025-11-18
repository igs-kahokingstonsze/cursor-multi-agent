## CLI Calculator Blueprint

### Overview
- Python 3.10 CLI that evaluates single-line expressions such as `12.5 + 3 * 2`.
- Accepts expressions with at least two numbers and one operator from `+ - * / % ** //`.
- Supports integers, floats, parentheses, and operator precedence.
- Handles invalid input, unsupported operators, arithmetic errors, and other edge cases with clear feedback.

### Project Structure
```
multi-agent/
└── calc_cli/
    ├── presentation/
    │   ├── __init__.py
    │   └── cli.py
    ├── logic/
    │   ├── __init__.py
    │   └── evaluator.py
    ├── tests/
    │   ├── __init__.py
    │   └── test_evaluator.py
    ├── pyproject.toml
    └── README.md
```

### Presentation Layer (`presentation/cli.py`)
- Greets user, shows instructions, and runs a REPL loop (or optional one-shot mode).
- Reads user input, trims whitespace, supports `exit`/`quit`.
- Delegates expression validation/evaluation to the logic layer.
- Displays results via `display_result` and errors via `display_error` without stack traces.
- Catches logic-layer exceptions (`CalculationError`, etc.) and recovers gracefully.

### Logic Layer (`logic/evaluator.py`)
- Validates expressions to ensure ≥2 operands and ≥1 allowed operator.
- Tokenizes safely, rejecting unsupported characters.
- Parses with precedence/parentheses (e.g., via `ast` inspection or shunting-yard).
- Evaluates and returns numeric results; raises domain-specific exceptions for invalid cases.
- Exception hierarchy: `CalculationError` base with `InvalidExpressionError`, `OperatorError`, `ZeroDivisionCalcError`, etc.

### Error & Edge Case Handling
- Empty or whitespace-only input → prompt again.
- Missing operands/operators → `InvalidExpressionError`.
- Unsupported characters/keywords → `InvalidExpressionError`.
- Division/modulo by zero → `ZeroDivisionCalcError`.
- Catch `OverflowError`, `ValueError`, `SyntaxError` and wrap as `CalculationError`.

### Testing Strategy
- Unit tests for all operators (`+ - * / % ** //`) with ints/floats.
- Precedence and parentheses scenarios.
- Invalid inputs (e.g., `"2 +"`, `"foo"`, `"+"`, `"2 2"`).
- Division-by-zero paths.
- Whitespace variations.
- CLI smoke test via `subprocess` to ensure REPL workflow.

### Tooling & Documentation
- `pyproject.toml` for metadata; optional extra (e.g., `rich`) for styling.
- `README.md` covering install, usage examples, error scenarios, future work.
- Optional helper commands (`make run`, `python -m calc_cli.presentation.cli`).

