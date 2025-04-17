# FastAPI Backend with Automated Tests and GitHub Actions

This project demonstrates how to set up a basic FastAPI backend with automated API tests using Python and integrate those tests into a GitHub Actions CI/CD pipeline.

## Table of Contents

- [Overview](#overview)
- [Task 1: Set Up the FastAPI Server](#task-1-set-up-the-fastapi-server)
  - [Installation](#installation)
  - [Running the Server](#running-the-server)
- [Task 2: Writing Automated Tests for the API](#task-2-writing-automated-tests-for-the-api)
- [Task 3: Enhancing the Tests with Pytest](#task-3-enhancing-the-tests-with-pytest)
- [Task 4: Integrating Test Automation with GitHub Actions](#task-4-integrating-test-automation-with-github-actions)
- [Task 5: Expanding the Automation for Real-World Projects](#task-5-expanding-the-automation-for-real-world-projects)
- [Contributing](#contributing)
- [License](#license)

## Overview

This repository contains:

- A FastAPI server with three basic mathematical operations: **add**, **subtract**, and **multiply**.
- Automated tests using Python's `requests` library and `pytest`.
- A GitHub Actions workflow that automatically runs the tests when code is pushed or a pull request is made.

## Task 1: Set Up the FastAPI Server

### Installation

1. Clone the repository:

   ```sh
   git clone https://github.com/your-username/fastapi-tests-ci.git
   cd fastapi-tests-ci
   ```

2. Create a virtual environment (optional but recommended):

   ```sh
   python -m venv venv
   source venv/bin/activate  # On Windows use `venv\Scripts\activate`
   ```

3. Install the required dependencies:

   ```sh
   pip install fastapi uvicorn
   ```

### Running the Server

1. Create a file named `main.py` and add the following code:

   ```python
   from fastapi import FastAPI

   app = FastAPI()

   @app.get("/add")
   def add(a: int, b: int):
       return {"result": a + b}

   @app.get("/subtract")
   def subtract(a: int, b: int):
       return {"result": a - b}

   @app.get("/multiply")
   def multiply(a: int, b: int):
       return {"result": a * b}
   ```

2. Run the FastAPI server:

   ```sh
   uvicorn main:app --reload
   ```

3. Open your browser and visit:

   - `http://127.0.0.1:8000/docs` for the interactive API documentation.
   - `http://127.0.0.1:8000/add?a=5&b=3` to test the addition endpoint.

## Task 2: Writing Automated Tests for the API

Create a file named `test_api.py` and add the following tests:

```python
import requests

BASE_URL = "http://127.0.0.1:8000"

def test_add():
    response = requests.get(f"{BASE_URL}/add", params={"a": 5, "b": 3})
    assert response.status_code == 200
    assert response.json()["result"] == 8

def test_subtract():
    response = requests.get(f"{BASE_URL}/subtract", params={"a": 5, "b": 3})
    assert response.status_code == 200
    assert response.json()["result"] == 2

def test_multiply():
    response = requests.get(f"{BASE_URL}/multiply", params={"a": 5, "b": 3})
    assert response.status_code == 200
    assert response.json()["result"] == 15
```

## Task 3: Enhancing the Tests with Pytest

1. Install `pytest`:

   ```sh
   pip install pytest
   ```

2. Run the tests:

   ```sh
   pytest test_api.py
   ```

## Task 4: Integrating Test Automation with GitHub Actions

1. Create a `.github/workflows/test.yml` file and add:

   ```yaml
   name: Run API Tests

   on:
     push:
     pull_request:

   jobs:
     test:
       runs-on: ubuntu-latest

       steps:
         - name: Checkout repository
           uses: actions/checkout@v3

         - name: Set up Python
           uses: actions/setup-python@v3
           with:
             python-version: '3.9'

         - name: Install dependencies
           run: pip install fastapi uvicorn pytest

         - name: Run tests
           run: pytest test_api.py
   ```

2. Commit and push the changes:

   ```sh
   git add .
   git commit -m "Added GitHub Actions for automated testing"
   git push origin main
   ```

3. The GitHub Actions workflow will automatically run the tests on every push or pull request.

## Task 5: Expanding the Automation for Real-World Projects

To enhance this project for real-world applications, consider:

- Adding a **database** (e.g., PostgreSQL, SQLite, MongoDB).
- Implementing **authentication** (e.g., OAuth, JWT).
- Writing **integration tests** with `httpx` instead of `requests`.
- Deploying the FastAPI app using **Docker** and **CI/CD pipelines**.

## Contributing

Contributions are welcome! To contribute:

1. Fork the repository.
2. Create a new branch (`git checkout -b feature-branch`).
3. Commit your changes (`git commit -m "Added new feature"`).
4. Push to the branch (`git push origin feature-branch`).
5. Create a Pull Request.

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.

