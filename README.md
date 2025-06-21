Project structure:

```
recipe-llm-project/
├── data/
│   ├── raw/                          # Raw dataset files (as obtained from source or initial ingestion)
│   ├── processed/                    # Processed/cleaned data files (ready for training, e.g., Parquet files)
│   └── profiling/                    # Data profiling outputs and scripts
│       ├── deequ_report.json         # JSON report of data quality checks
│       └── profiling_notebook.ipynb  # (Optional) Jupyter notebook for data analysis
├── model_training/
│   ├── fine_tune.py                  # Script to fine-tune LLaMA (could use HF Trainer or custom loop)
│   ├── lora_finetune.py              # (Optional) Script using LoRA for low-resource fine-tuning
│   ├── requirements.txt              # Python deps for training (transformers, datasets, mlflow, etc.)
│   ├── mlflow_pipeline.yml           # (Optional) MLflow project spec or configs
│   └── notebooks/                    # (Optional) notebooks for experimentation
│       └── exploration_eda.ipynb
├── model_card/
│   ├── model_card.py                 # Pydantic model definition for Model Card
│   └── recipe-llama-model-card.json  # Generated model card with details of v1.0
├── model_repository/                 # Directory to hold model artifacts for deployment
│   └── recipe-llama/
│       ├── 1/
│       │   └── model.onnx            # ONNX model file for version 1
│       └── config.pbtxt              # Triton model config
├── services/
│   ├── api/
│   │   ├── app.py                    # FastAPI app initialization
│   │   ├── routes.py                 # Defines API endpoints
│   │   ├── schemas.py                # Pydantic models (RecipeRequest, RecipeResponse, etc.)
│   │   ├── triton_client.py          # Utility for communicating with Triton (using Triton gRPC client)
│   │   ├── auth_dependency.py        # Dependency functions to verify JWT and roles
│   │   └── Dockerfile                # Dockerfile for API service
│   └── auth/
│       ├── auth_app.py               # FastAPI app for auth service
│       ├── models.py                 # SQLAlchemy models or in-memory store for users
│       ├── routes.py                 # Login, register endpoints
│       ├── security.py               # Password hashing and JWT token functions
│       └── Dockerfile                # Dockerfile for Auth service
├── k8s/                              # Kubernetes manifests for deploying the system
│   ├── deployment-api.yaml           # Deployment for API service + Service + HPA
│   ├── deployment-auth.yaml          # Deployment for Auth service + Service
│   ├── deployment-triton.yaml        # Deployment for Triton server + Service (ClusterIP for internal use)
│   ├── hpa-api.yaml                  # HorizontalPodAutoscaler for API (if not in deployment file)
│   ├── secret-jwt.yaml               # Secret for JWT signing key (mounted in auth and api)
│   ├── configmap-env.yaml            # Env variables (e.g., Triton server address) for pods
│   ├── flagger-canary-api.yaml       # Flagger Canary CR for API (blue-green deployment config)
│   ├── flagger-canary-triton.yaml    # Canary CR for Triton (if used for model rollout)
│   ├── prometheus.yaml               # Prometheus-Operator CRs (ServiceMonitor for our services)
│   └── grafana-dashboards/           # Custom Grafana dashboard JSONs (if we want to check into git)
├── docker-compose.yaml               # (Optional) Compose file to run components locally (e.g., API, Triton, Auth, Prom, Grafana)
├── README.md                         # Documentation for setting up and running the project
└── Makefile                          # (Optional) Makefile with common commands (build images, run tests, etc.)

```
