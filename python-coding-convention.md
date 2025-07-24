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
- Use **Python 3.10+** to leverage modern syntax (e.g., `match` statements, union types `|`).
- Keep functions short and focused on a **single responsibility**.
- Prefer readability over unnecessary cleverness.

---

## 2. Naming

| Entity        | Convention      | Example                |
|---------------|-----------------|------------------------|
| Variable      | `snake_case`    | `stock_data`, `user_id` |
| Function      | `snake_case()`  | `get_stock_price()`     |
| Class         | `PascalCase`    | `PricePredictor`        |
| Constant      | `UPPER_CASE`    | `MAX_RETRIES`           |
| Module/File   | `snake_case.py` | `data_loader.py`        |
| Private Attribute | `_single_leading_underscore` | `self._connection` |
| Mangled Attribute | `__double_leading_underscore` | `self.__task_name` |

---

## 3. Function & Class Declaration

- Use docstrings for all public functions, classes, and methods.
- **Mandatory:** Use **Google Style** for docstrings to enable automated documentation generation.
- Keep class methods under 50 lines.

```python
class MyClass:
    """A brief summary of the class."""

    def my_method(self, arg1: str) -> bool:
        """A brief summary of the method.

        Args:
            arg1: Description of arg1.

        Returns:
            Description of the return value.
        """
```   
---

## 4. Type Hinting

- Always use type hints for function arguments and return types.
- Prefer modern type hint syntax from Python 3.10+ (**list**, **dict**, **int | None** instead of **List**, **Dict**, **Optional[int]**).
```python
# Good
def fetch_data(symbol: str) -> dict[str, float] | None:
    ...

# Avoid (older syntax)
from typing import Dict, Optional
def fetch_data(symbol: str) -> Optional[Dict[str, float]]:
    ...
```

---

## 5. Code Formatting

- Use Black for auto-formatting and isort for import sorting.
- Maximum line length: 88 characters.
- One statement per line.
- Highly Recommended: Use pre-commit hooks to automatically run Black, isort, and a linter (e.g., ruff or flake8) before each commit. This ensures consistent code style across the entire repository.

---

## 6. Quotes

- Use **double quotes** `"` for strings consistently.
- Use **single quotes** `'` only when quoting inside another string.

---

## 7. Spacing & Indentation

- Use 4 spaces per indentation level.
- Add 2 blank lines between top-level function and class definitions.
- Add 1 blank line between methods in a class.
- No trailing whitespace at the end of lines.

---

## 8. Imports

- Import order should follow this structure:
  ```python
  # 1. Standard libraries
  import json
  import os

  # 2. Third-party libraries
  import fastapi
  import numpy as np
  import pydantic

  # 3. Internal modules (local application)
  from app.routes import router
  from utils.logger import setup_logger
  ```

- Never use wildcard imports (from module import *).

---

## 9. FastAPI Specific Rules

- Separate routes, schemas, services, and models into distinct modules.
- Use **APIRouter** for modular route design.
- Use **pydantic.BaseModel** for request and response validation.
- Mandatory: Use **async def** for endpoints that perform I/O operations (e.g., database queries, external API calls) to leverage FastAPI's asynchronous capabilities.
- Maximize the use of FastAPI's Dependency Injection.
- Handle HTTP errors using **HTTPException** or a custom exception handler middleware.

---

## 10. AI/ML Specific Rules

- Keep training scripts (**train.py**, **evaluate.py**) isolated.
- Use a **configs/** directory for hyperparameters and experiment setups (e.g., **.yaml** files).
- Organize models in **models/** and datasets in **data/**.
- Recommended: Use versioning tools for data and models, such as DVC (Data Version Control).
- Log metrics using **TensorBoard**, **MLflow**, or **Weights & Biases**.
- Do not hardcode paths—use configuration files (**config.yaml**) or **argparse**.

---

## 11. Exception Handling

- Use try...except blocks for operations that can fail: API calls, I/O, model inference.
- Log all exceptions with full context.
- Return user-friendly error messages from API endpoints.
- Recommended: Create custom exceptions and handle them in a middleware layer to keep business logic (services) clean.

  ```python
  # services/prediction_service.py
  class InvalidInputError(ValueError):
      pass

  def run_prediction(data):
      if not data:
          raise InvalidInputError("Input data cannot be empty")
      # ... business logic ...

  # main.py
  @app.exception_handler(InvalidInputError)
  async def invalid_input_exception_handler(request: Request, exc: InvalidInputError):
      return JSONResponse(
          status_code=400,
          content={"message": f"Invalid Input: {exc}"},
      )
  ```

---

## 12. Folder & File Structure

For **FastAPI**:
```
app/
├── main.py
├── api/
│   ├── routes/
│   └── schemas/
├── core/
│   ├── config.py       # Pydantic settings
│   └── logging_config.py
├── db/                 # SQLAlchemy models, database session
├── services/           # Business logic
├── models/             # ML model loading/inference logic
├── tests/
.env
.gitignore
README.md
```

For **AI/ML**:
```
project/
├── data/               # Raw and processed data
├── notebooks/          # Jupyter notebooks for exploration
├── src/                # Source code for the project
│   ├── data_processing/
│   ├── training/
│   └── utils/
├── scripts/
│   ├── train.py
│   └── evaluate.py
├── models/             # Trained model artifacts
├── configs/            # Configuration files (YAML)
├── tests/
```

---

## 13. Configuration & Environment

- Use a .env file and the python-dotenv library to manage environment variables.
- Recommended: Use Pydantic's BaseSettings to load and validate configurations. This integrates perfectly with FastAPI.
- Never commit secrets (e.g., API keys, passwords) to version control. Add .env and other sensitive files to .gitignore.

  ```python
  # core/config.py
  from pydantic_settings import BaseSettings

  class Settings(BaseSettings):
      DATABASE_URL: str
      API_KEY: str
      LOG_LEVEL: str = "INFO"

      class Config:
          env_file = ".env"

  settings = Settings()
  ```
---

## 14. Logging

- Use Python’s `logging` module or `loguru`.
- Log in a standard format with timestamp, level, and message.
  ```python
  [2025-07-24 10:00:00] [INFO] Starting model training...
  ```
---

## 15. Testing

- Use pytest for writing and running tests.
- Place test files in the tests/ directory with a structure that mirrors the app/ or src/ directory.
- Mandatory: Write unit and integration tests for:
  - API routes (using FastAPI's TestClient).
  - Business logic in services.
  - Data transformation functions.
- Aim for a code coverage of > 80%. Use pytest-cov to measure it.

---

## Change Log

- Jul 24, 2025 - Initial Python (FastAPI & AI) coding convention release