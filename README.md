# 04.02-build-workflow: MEDIUM — “Does my application code work?”

### Goal

Validate:

* imports
* application logic
* test failures when code is wrong

This is **real CI**, not just a smoke test.

---

## Instructions

1. Add application code
2. Add tests that import it
3. Use the same workflow structure

---

## Repo structure

```
.
├── app.py
├── test_app.py
├── requirements.txt
└── .github/
    └── workflows/
        └── build.yml
```

---

## Code

### `app.py`

```python
def add(a, b):
    return a + b
```

### `test_app.py`

```python
from app import add

def test_add():
    assert add(2, 3) == 5
```

### `requirements.txt`

```txt
pytest
```

---

## Workflow: `.github/workflows/build.yml`

```yaml
name: Build

on:
  push:
    branches: [ "main" ]
  pull_request:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-python@v5
        with:
          python-version: "3.11"

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt

      - name: Run tests
        run: pytest
```

---

## What this proves

* Your app code is importable
* Logic errors break the build
* Pull requests are validated

