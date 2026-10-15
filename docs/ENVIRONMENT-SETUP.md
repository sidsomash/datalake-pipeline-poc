# Local Development Environment Guide
### How to Set Up Language Runtimes, Packages, and Local Development Workflows

---

## 1. Overview

This guide explains how to set up your local development environment for the Online Retail Lakehouse project.

### Tools Used

- **mise** for managing Python runtime versions
- **uv** for dependency management and virtual environments
- **Python 3.12** as the project runtime
- **Git** for source control
- **VS Code** (recommended) for development

---

## 2. Development Architecture

### ✔ mise

mise is responsible for installing and managing the Python runtime used by the project.
Install Mise 

Install Python 3.12 globally:

```bash
mise use -g python@3.12
```

Verify installation:

```bash
python --version
```

Expected output:

```text
Python 3.12.x
```

---

### ✔ uv

uv is responsible for:

- Creating virtual environments
- Installing dependencies
- Managing dependency versions
- Generating lock files
- Reproducing environments across machines

The project tracks dependencies using:

```text
pyproject.toml
uv.lock
```

---

## 3. First-Time Setup

### Clone the Repository

```bash
git clone <repository-url>
cd datalake-pipeline-poc
```

### Create a Virtual Environment

```bash
uv venv
```

This creates a local `.venv` directory.

---

### Activate the Virtual Environment

#### Windows PowerShell

```powershell
.venv\Scripts\Activate.ps1
```

#### Mac/Linux

```bash
source .venv/bin/activate
```

You should now see:

```text
(.venv)
```

at the beginning of your terminal prompt.

---

### Install Project Dependencies

If dependency files already exist:

```bash
uv sync
```

This command installs the exact package versions defined in:

```text
pyproject.toml
uv.lock
```

---

## 4. Initial Project Dependencies

Install the core lakehouse dependencies:

```bash
uv add pyspark delta-spark pandas pyarrow jupyter duckdb
```

This command automatically:

- Installs packages into the virtual environment
- Updates `pyproject.toml`
- Updates `uv.lock`

Not necessary if you've already run uv sync with the most recent and up to date pyproject.toml and uv.lock.

---

### Package Usage

#### PySpark

Used for:

- Data ingestion
- Data transformations
- Spark SQL
- Aggregations
- Window functions
- DataFrame operations

```bash
uv add pyspark
```

---

#### Delta Lake

Used for:

- Bronze/Silver/Gold storage layers
- ACID transactions
- MERGE operations
- Slowly Changing Dimensions (SCD Type 2)

```bash
uv add delta-spark
```

---

#### Pandas

Used for:

- Reading Excel files
- Data profiling
- Lightweight local analysis
- Initial ingestion workflows

```bash
uv add pandas
```

---

#### PyArrow

Used for:

- Parquet file support
- Spark ↔ Pandas interoperability
- Efficient serialization

```bash
uv add pyarrow
```

---

#### Jupyter

Used for:

- Data exploration
- Validation checks
- SQL prototyping
- Debugging transformations

```bash
uv add jupyter
```

---

#### DuckDB

Used for:

- Local analytical SQL development
- Query prototyping
- Fast validation of transformations

```bash
uv add duckdb
```

---

## 5. Airflow Installation

Airflow should be installed after the Bronze, Silver, and Gold pipelines are working locally.

Install Airflow when beginning orchestration development:

```bash
uv add apache-airflow
```

Airflow will orchestrate:

```text
extract_raw
    ↓
load_bronze
    ↓
transform_silver
    ↓
validate_silver
    ↓
transform_gold
    ↓
publish_gold
```

---

## 6. Daily Development Workflow

### Activate Environment

#### Windows

```powershell
.venv\Scripts\Activate.ps1
```

#### Mac/Linux

```bash
source .venv/bin/activate
```

---

### Pull Latest Changes

```bash
git pull
```

---

### Sync Dependencies

Before starting work, ensure your environment matches the repository:

```bash
uv sync
```

---

### Run Python Code

```bash
uv run python main.py
```

---

### Launch Jupyter

```bash
uv run jupyter notebook
```

or

```bash
uv run jupyter lab
```

---

## 7. Dependency Management

### Add a Dependency

```bash
uv add <package-name>
```

Example:

```bash
uv add great-expectations
```

---

### Add a Development Dependency

```bash
uv add --dev pytest
```

---

### Remove a Dependency

```bash
uv remove <package-name>
```

Example:

```bash
uv remove duckdb
```

---

### Upgrade Dependencies

```bash
uv lock --upgrade
uv sync
```

---

### View Installed Packages

```bash
uv pip list
```

---

### View Dependency Tree

```bash
uv tree
```

---

## 8. Source Control Requirements

Whenever dependencies are added, removed, or updated, commit the following files:

```text
pyproject.toml
uv.lock
```

Example:

```bash
git add pyproject.toml uv.lock
git commit -m "Update project dependencies"
```

This allows other developers to recreate the exact same environment using:

```bash
uv sync
```

---

## 9. Recommended Project Structure

```text
datalake-pipeline-poc/
│
├── .venv/
├── pyproject.toml
├── uv.lock
├── main.py
│
├── src/
│   ├── bronze/
│   ├── silver/
│   ├── gold/
│   ├── validation/
│   ├── airflow/
│   └── common/
│
├── notebooks/
│   ├── exploration.ipynb
│   ├── validation.ipynb
│   └── debugging.ipynb
│
├── docs/
│
├── data/
│   ├── raw/
│   ├── bronze/
│   ├── silver/
│   └── gold/
│
└── tests/
```

---

## 10. New Developer Setup Checklist

After cloning the repository:

```bash
git clone <repository-url>
cd datalake-pipeline-poc
uv venv
uv sync
```

Activate the environment:

```powershell
.venv\Scripts\Activate.ps1
```

Verify installation:

```bash
uv pip list
```

Open the project:

```bash
code .
```

Select the interpreter:

```text
.venv/Scripts/python.exe
```

You are now ready to begin development.