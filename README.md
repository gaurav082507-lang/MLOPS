# MLOps Pipeline

End-to-end machine learning operations repository covering data ingestion, model training, evaluation, deployment, and monitoring.

## 📋 Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
- [Pipeline Stages](#pipeline-stages)
- [Configuration](#configuration)
- [Testing](#testing)
- [CI/CD](#cicd)
- [Monitoring](#monitoring)
- [Contributing](#contributing)
- [License](#license)

## Overview

This repository implements a reproducible MLOps workflow for building, training, deploying, and monitoring machine learning models in production. It follows best practices for versioning, experiment tracking, and automated deployment.

**Key Features:**
- 🔄 Reproducible data and model pipelines
- 📊 Experiment tracking and model versioning
- 🚀 Automated CI/CD for model deployment
- 📈 Model performance monitoring and drift detection
- 🐳 Containerized environments for consistency
- ⚙️ Infrastructure-as-code for reproducible environments

## Architecture

```
Data Source → Data Validation → Feature Engineering → Training
     → Model Evaluation → Model Registry → Deployment → Monitoring
```

## Project Structure

```
.
├── data/
│   ├── raw/                # Immutable raw data
│   ├── processed/          # Cleaned/transformed data
│   └── external/            # Third-party data sources
├── notebooks/               # Exploratory analysis (not for production)
├── src/
│   ├── data/                 # Data ingestion & preprocessing scripts
│   ├── features/             # Feature engineering
│   ├── models/                # Training, prediction, evaluation
│   ├── pipelines/             # Orchestration (e.g., Airflow/Kubeflow DAGs)
│   └── utils/                  # Shared utilities
├── models/                     # Serialized trained models
├── configs/                    # YAML/JSON configuration files
├── tests/                      # Unit and integration tests
├── docker/                     # Dockerfiles and compose files
├── infra/                      # Terraform/CloudFormation/K8s manifests
├── .github/workflows/          # CI/CD pipelines
├── requirements.txt
├── Makefile
└── README.md
```

## Prerequisites

- Python 3.10+
- Docker & Docker Compose
- (Optional) Kubernetes cluster for deployment
- (Optional) DVC / MLflow for experiment tracking
- Cloud provider CLI (AWS/GCP/Azure), if applicable

## Installation

```bash
# Clone the repository
git clone https://github.com/<your-org>/<your-repo>.git
cd <your-repo>

# Create a virtual environment
python -m venv venv
source venv/bin/activate   # Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# (Optional) Install pre-commit hooks
pre-commit install
```

## Usage

### 1. Data Preparation
```bash
python src/data/make_dataset.py --input data/raw --output data/processed
```

### 2. Train a Model
```bash
python src/models/train.py --config configs/train_config.yaml
```

### 3. Evaluate a Model
```bash
python src/models/evaluate.py --model-path models/latest --data data/processed/test
```

### 4. Serve / Deploy
```bash
docker build -t ml-model-service -f docker/Dockerfile.serve .
docker run -p 8080:8080 ml-model-service
```

## Pipeline Stages

| Stage | Description | Tooling |
|---|---|---|
| Data Ingestion | Pull raw data from source(s) | Airflow / custom scripts |
| Validation | Schema & quality checks | Great Expectations / TFX Data Validation |
| Feature Engineering | Transform raw data into model-ready features | pandas / Feast |
| Training | Train and tune models | scikit-learn / PyTorch / XGBoost |
| Experiment Tracking | Log metrics, params, artifacts | MLflow / Weights & Biases |
| Model Registry | Version and stage models | MLflow Model Registry |
| Deployment | Serve models via API | FastAPI / TorchServe / KServe |
| Monitoring | Track drift, latency, performance | Prometheus / Grafana / Evidently |

## Configuration

All pipeline parameters are managed via YAML files in `configs/`. Example:

```yaml
model:
  name: xgboost_classifier
  params:
    max_depth: 6
    learning_rate: 0.1
    n_estimators: 300

data:
  train_path: data/processed/train.csv
  test_path: data/processed/test.csv

tracking:
  experiment_name: churn_prediction
  tracking_uri: http://localhost:5000
```

## Testing

```bash
# Run unit tests
pytest tests/unit

# Run integration tests
pytest tests/integration

# Run all tests with coverage
pytest --cov=src tests/
```

## CI/CD

Automated workflows (see `.github/workflows/`) handle:
- Linting & unit tests on every PR
- Model training & evaluation on merge to `main`
- Container build & push to registry
- Deployment to staging/production on tagged release

## Monitoring

Post-deployment monitoring tracks:
- **Data drift** — input distribution shifts vs. training data
- **Model performance** — accuracy/latency degradation over time
- **System health** — API uptime, response time, resource usage

Dashboards are available via Grafana at `http://<host>:3000` (default local setup).

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/my-feature`)
3. Commit your changes (`git commit -m 'Add my feature'`)
4. Push to the branch (`git push origin feature/my-feature`)
5. Open a Pull Request

Please ensure all tests pass and code is linted (`pre-commit run --all-files`) before submitting.

## License

This project is licensed under the [MIT License](LICENSE).
