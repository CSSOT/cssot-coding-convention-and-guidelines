# CSSOT's PYTHON CODING CONVENTION (FastAPI & AI)

![Contributors](https://img.shields.io/github/contributors/CSSOT/cssot-coding-convention-and-guidelines?style=for-the-badge) ![Forks](https://img.shields.io/github/forks/CSSOT/cssot-coding-convention-and-guidelines?style=for-the-badge) ![Stars](https://img.shields.io/github/stars/CSSOT/cssot-coding-convention-and-guidelines?style=for-the-badge) ![Issues](https://img.shields.io/github/issues/CSSOT/cssot-coding-convention-and-guidelines?style=for-the-badge) ![Pull Requests](https://img.shields.io/github/issues-pr/CSSOT/cssot-coding-convention-and-guidelines?style=for-the-badge)

---

## Introduction

Defines coding conventions for writing clean, maintainable, and production-ready **Python** code in **FastAPI** projects and **AI development** environments at **CSSOT**.

---

## Table of Contents

- [CSSOT's PYTHON CODING CONVENTION (FastAPI \& AI)](#cssots-python-coding-convention-fastapi--ai)
  - [Introduction](#introduction)
  - [Table of Contents](#table-of-contents)
  - [1. Basic Rules](#1-basic-rules)
  - [2. Naming](#2-naming)
  - [3. Function \& Class Declaration](#3-function--class-declaration)
  - [4. Type Hinting](#4-type-hinting)
  - [5. Code Formatting](#5-code-formatting)
  - [6. Quotes](#6-quotes)
  - [7. Spacing \& Indentation](#7-spacing--indentation)
  - [8. Imports](#8-imports)
  - [9. FastAPI Specific Rules](#9-fastapi-specific-rules)
  - [10. AI/ML Specific Rules](#10-aiml-specific-rules)
  - [11. Exception Handling](#11-exception-handling)
  - [12. Folder \& File Structure](#12-folder--file-structure)
  - [13. Configuration \& Environment](#13-configuration--environment)
  - [14. Logging](#14-logging)
  - [15. Testing](#15-testing)
  - [Change Log](#change-log)

---

## 1. Basic Rules

- Follow **PEP 8** as the primary style guide.
- Use **Python 3.10+** for modern syntax (e.g., `match` statements).
- Keep functions short and do one thing.
- Prefer readability over cleverness.

---

## 2. Naming

| Entity        | Convention      | Example               |
|---------------|-----------------|------------------------|
| Variable      | `snake_case`    | `stock_data`, `user_id` |
| Function      | `snake_case()`  | `get_stock_price()`     |
| Class         | `PascalCase`    | `PricePredictor`        |
| Constant      | `UPPER_CASE`    | `MAX_RETRIES`           |
| Module/File   | `snake_case.py` | `data_loader.py`        |

---

## 3. Function & Class Declaration

- Group related utility functions inside modules.
- Use docstrings for all public functions/classes.
- Keep class methods under 30–50 lines each.

---

## 4. Type Hinting

- Always use type hints for function arguments and return types.
```python
def fetch_data(symbol: str, date: datetime) -> dict:
    ...
```

- Use `Optional[...]`, `List[...]`, `Dict[...]`, `Union[...]` from `typing`.

---

## 5. Code Formatting

- Use **Black** for formatting and **isort** for import sorting.
- Line length: 88 characters.
- One statement per line.

---

## 6. Quotes

- Use **double quotes** `"` for strings consistently.
- Use **single quotes** `'` only when quoting inside another string.

---

## 7. Spacing & Indentation

- Use **4 spaces** per indentation level.
- Add blank lines between function/class definitions.
- No extra whitespace at line ends.

---

## 8. Imports

Import order should follow this structure:
```python
# Standard libraries
import os
import json

# Third-party libraries
import numpy as np
import fastapi

# Internal modules
from app.routes import router
from utils.logger import setup_logger
```

Avoid wildcard imports.

---

## 9. FastAPI Specific Rules

- Separate routes, schemas, services, and models into distinct modules.
- Use `APIRouter` for modular route design.
- Use `pydantic.BaseModel` for request and response validation.
- Use dependency injection where possible.
- Handle all HTTP exceptions using `HTTPException`.

---

## 10. AI/ML Specific Rules

- Keep training scripts (`train.py`, `evaluate.py`) isolated.
- Use `configs/` for hyperparameters and experiment setups.
- Organize models into `models/` and datasets into `data/`.
- Save checkpoints in `checkpoints/`.
- Log metrics using `TensorBoard`, `MLflow`, or `Weights & Biases`.
- Avoid hardcoding paths — use `config.yaml` or `argparse`.

---

## 11. Exception Handling

- Use `try/except` blocks around API calls, I/O, model inference.
- Log all exceptions with context.
- Return user-friendly messages from API endpoints.
```python
try:
    result = model.predict(data)
except ValueError as e:
    logger.error(f"Prediction failed: {e}")
    raise HTTPException(status_code=400, detail="Invalid input data")
```

---

## 12. Folder & File Structure

For **FastAPI**:
```
app/
├── main.py
├── api/
│   ├── routes/
│   ├── schemas/
│   └── services/
├── core/        # config, logging, utils
├── models/      # SQLAlchemy or pydantic models
├── tests/
```

For **AI/ML**:
```
project/
├── data/
├── models/
├── notebooks/
├── scripts/
│   ├── train.py
│   └── eval.py
├── configs/
├── utils/
├── checkpoints/
```

---

## 13. Configuration & Environment

- Use `.env` and `python-dotenv` for environment variables.
- Store secrets securely, never commit to version control.
- Centralize config in a `config.py` or `yaml`.

---

## 14. Logging

- Use Python’s `logging` module or `loguru`.
- Log in a standard format with timestamp, level, and message.
```python
[2025-07-24 10:00:00] [INFO] Starting model training...
```

---

## 15. Testing

- Use `pytest` for testing.
- Place tests in `tests/` with same structure as `app/` or `scripts/`.
- Write tests for:
  - API routes (with `TestClient`)
  - Model inference
  - Data transformation pipelines

---

## Change Log

- Jul 24, 2025 - Initial Python (FastAPI & AI) coding convention release