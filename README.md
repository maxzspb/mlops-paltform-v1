# Enterprise MLOps Platform: Architecture & Components

This repository implements a Kubernetes-native Machine Learning Operations (MLOps) platform provisioned on AWS, designed for Continuous Training (CT) and automated, reliable model deployment.

## System Architecture

### 1. Feature Management
* Centralized feature engineering and serving are strictly managed by **Feast**.
* **Redis** is utilized for low-latency online feature retrieval, while **PostgreSQL/MySQL** handles offline historical data. Real-time data ingestion is supported via **Apache Kafka**.

### 2. Pipeline Orchestration
* End-to-end ML workflows (including PyTorch training) are orchestrated using **Kubeflow Pipelines (KFP)** via Directed Acyclic Graphs (DAGs).
* **MLflow** serves as the central experiment tracker and model registry, handling artifact versioning and model signatures to ensure strict reproducibility.

### 3. Model Serving
* High-performance, multi-framework inference is powered by an integration of **KServe**, **NVIDIA Triton Inference Server**, and custom **FastAPI** fallbacks.
* Serving components dynamically fetch registered model artifacts from **MinIO/S3** storage at inference time.

### 4. Observability and Monitoring
* Technical and system-level metrics are scraped and visualized using the **Prometheus**, **Grafana**, and **Loki** stack.
* Out-of-band data drift and model performance decay are tracked using **Evidently AI**, featuring its own dedicated monitoring containers and alerting rules.

### 5. GitOps and Infrastructure
* The foundational AWS infrastructure (EKS, RDS, S3, ElastiCache) is fully provisioned as code using **Terraform**.
* The deployment lifecycle follows a declarative GitOps flow managed by **ArgoCD**, with **GitHub Actions** automating CI pipelines and manifest validation via Kustomize.
