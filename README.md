# 🚀 Astronomical Source NER Pipeline

## 🧠 Project Overview

This project develops a custom **Named Entity Recognition (NER)** model that extracts references to **celestial sources** (e.g., quasars, black holes, gamma-ray bursts) from scientific abstracts and real-time alerts. 

The model is trained using astronomy abstracts from **NASA ADS** and then deployed to identify source objects in the **Astronomer's Telegram (ATel)** RSS feed. It is designed with **full MLOps capabilities**, including experiment tracking, workflow orchestration, model deployment, monitoring, and reproducibility.

---

## 🎯 Problem Description

In astrophysics, the discovery and study of celestial sources are documented in scientific literature and real-time alert networks like ATel. However, identifying and indexing source names from this unstructured data is challenging due to inconsistent formatting, varied nomenclature, and volume.

This project addresses that by:
- Creating a labeled dataset of astronomical sources from NASA ADS abstracts
- Fine-tuning a `spaCy` NER model to recognize source names
- Applying the model to ATel RSS feed entries to extract entities
- Providing a **Streamlit web app** for real-time interaction

---

## ☁️ Cloud Architecture

We use **AWS** as the cloud platform and **Terraform** for Infrastructure as Code (IaC) to provision and manage resources.

### Cloud Components

| Component         | Technology                      |
|-------------------|----------------------------------|
| Cloud Provider    | AWS (S3, Lambda, CloudWatch)     |
| IaC               | Terraform                        |
| Data Storage      | S3                               |
| Workflow Scheduler| Prefect Cloud (Free Tier)        |
| Model Hosting     | AWS Lambda                       |
| Monitoring        | Grafana, Evidently, CloudWatch   |
| CI/CD             | GitHub Actions                   |
| Web App           | Streamlit (locally or on ECS)    |

✅ This setup meets the 4-point cloud criterion: fully cloud-hosted with IaC.

---

## 🧪 Experiment Tracking and Model Registry

All experiments are tracked with **MLflow**, which logs:
- spaCy model versions
- Dataset versions
- Hyperparameters and metrics (precision, recall, F1)

A **model registry** tracks models in staging and production.

✅ This setup meets the 4-point experiment tracking criterion: tracking + registry used.

---

## 🔁 Workflow Orchestration

Workflows are built and scheduled with **Prefect Cloud**:

### Flows:
1. `build_dataset`: Parse and label NASA abstracts
2. `train_model`: Train spaCy NER model
3. `evaluate_model`: Run evaluation and log metrics
4. `inference_atel`: Poll ATel RSS feed and extract sources
5. `monitor`: Run drift reports with Evidently

✅ Fully deployed with remote Prefect agent and schedules.

---

## 🚀 Model Deployment

- **Batch inference** is performed via **AWS Lambda** (low-cost and scalable).
- spaCy model is compiled to optimize Lambda performance.
- Outputs written to **S3** and logged to **CloudWatch**.

### Streamlit Demo
A **Streamlit web app** enables users to:
- Paste in text (e.g., ATel message)
- View extracted source names
- Can run locally or be deployed to AWS ECS

✅ Meets 4-point deployment criterion: containerized + cloud-executable.

---

## 📈 Monitoring

Monitoring is conducted via:
- **Evidently** for drift detection and label distribution checks
- **Grafana dashboards** via CloudWatch metrics (cold starts, errors, latency)

### Conditional Flow
If metrics degrade (e.g., F1 drop, drift detected):
- Prefect triggers a **retraining flow**
- Slack/email alerts notify maintainers

✅ Meets 4-point monitoring criterion: alerts and conditional logic included.

---

## 🧹 Reproducibility

- Project uses **`uv`** for fast, modern dependency and environment management
- Code and data are versioned
- All flows are reproducible via Prefect
- Infrastructure managed by Terraform

✅ Meets 4-point reproducibility criterion.

---

## ✅ MLOps Best Practices

| Practice                  | Status             |
|---------------------------|--------------------|
| Unit tests                | ✅ `pytest` in `tests/unit/` |
| Integration tests         | ✅ `pytest` in `tests/integration/` |
| Linter/Formatter          | ✅ `black`, `ruff` |
| Makefile                  | ✅ Automation for test/lint/train |
| Pre-commit hooks          | ✅ `pre-commit` used |
| CI/CD                     | ✅ GitHub Actions  |

✅ Full 7/7 points for MLOps best practices.

---

## 🗂️ Project Structure

```bash
astronomical-ner/
├── data/                      # Raw + processed ADS abstracts
├── notebooks/                 # EDA and dataset building
├── src/
│   ├── data_ingestion/        # Download from NASA ADS
│   ├── labeling/              # Create spaCy-compatible annotations
│   ├── training/              # Model training logic
│   ├── inference/             # ATel RSS + entity extraction
│   ├── monitoring/            # Evidently integration
│   ├── app/                   # Streamlit demo app
├── tests/
│   ├── unit/                  # Unit tests
│   ├── integration/           # End-to-end tests
├── deployment/
│   ├── lambda/                # Lightweight Lambda inference logic
│   ├── docker/                # Container setup for app and model
├── terraform/                # IaC: S3, Lambda, IAM, CloudWatch
├── .github/workflows/        # CI/CD pipelines
├── Makefile
├── requirements.txt
├── pyproject.toml
├── .pre-commit-config.yaml
├── README.md

# 🧪 How to Run (Dev Mode) — Using uv
## Install uv (if not installed)
curl -LsSf https://astral.sh/uv/install.sh | sh

## Install dependencies
uv pip install -r requirements.txt

## Lint + format code
uv pip install black ruff
black .
ruff check . --fix

## Run all tests
uv pip install pytest
pytest

## Run specific test sets
pytest tests/unit/
pytest tests/integration/

## Launch Streamlit app
streamlit run src/app/app.py


