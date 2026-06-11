# ML Model Deployment in Databricks

## What You Must Know for the Exam

This module focuses on taking a trained model and making it available for real-world use. The exam heavily focuses on MLflow, Model Registry, Model Serving, Model Versions, and monitoring concepts.

## 1. MLflow Model Lifecycle (Most Important)

The Databricks ML lifecycle is:

```text
Train Model
  -> Track Experiment
  -> Register Model
  -> Deploy Model
  -> Serve Predictions
  -> Monitor Performance
```

Databricks integrates MLflow throughout the entire lifecycle.

Exam tip:

MLflow ≠ Only experiment tracking.

MLflow also supports:
- Tracking
- Model Registry
- Deployment
- Model Serving

## 2. Model Registry (Highest Priority)

A Model Registry is a centralized repository for managing model versions.

It provides:
- Versioning
- Lineage
- Governance
- Lifecycle management
- Collaboration

### Why Use It?

**Without Registry:**

```text
model_final_v1
model_final_v2
model_final_v3
model_final_latest_final_v2
```

Chaos!

**With Registry:**

```text
CustomerChurnModel
  -> Version 1
  -> Version 2
  -> Version 3
```

Everything is tracked automatically.

## 3. Registering a Model

Example:

```python
mlflow.register_model(
    "runs:/<run_id>/model",
    "CustomerChurnModel"
)
```

Once registered:

```text
Model Name
  -> Versions
  -> Deployment
```

Exam tip:

A registered model is easier to:
- Deploy
- Govern
- Monitor
- Rollback

## 4. Model Versioning

Every registration creates a new version.

Example:

```text
CustomerChurnModel
  -> Version 1
  -> Version 2
  -> Version 3
```

Benefits:
- Rollback capability
- Auditability
- Reproducibility

Exam trap:

Many questions ask: "What should be used when multiple versions of a model need to be managed?"

Answer: Model Registry (NOT experiment tracking).

## 5. Model Serving (Very Important)

Model Serving exposes a trained model through a REST API endpoint.

Workflow:

```text
Application
  -> REST API
  -> Model Serving Endpoint
  -> Prediction
```

Databricks Model Serving creates scalable REST endpoints for real-time inference.

Example use cases:
- Fraud detection
- Customer churn prediction
- Recommendation systems
- Real-time scoring
## 6. Batch Inference vs Real-Time Inference

**Batch Inference:**

```text
100,000 Records
  -> Score Together
  -> Save Results
```

Examples:
- Nightly predictions
- Monthly forecasting

**Real-Time Inference:**

```text
Single Request
  -> Immediate Prediction
```

Examples:
- Credit card fraud
- Website recommendations

Exam tip:

| Scenario | Solution |
|---|---|
| Millions of records overnight | Batch inference |
| Immediate prediction needed | Model serving |
## 7. Unity Catalog + Model Governance

Modern Databricks deployments use Unity Catalog for model governance.

Benefits:
- Centralized access control
- Cross-workspace sharing
- Model lineage
- Auditing

Exam tip:

Current Databricks best practice:

```text
Unity Catalog + MLflow Registry
```
## 8. Monitoring (Frequently Tested)

Deployment is not the end. Models must be monitored.

### Data Drift

Input data changes over time.

Example:

```text
Training Data Average Age = 30
Production Data Average Age = 55
-> Model quality may degrade
```

### Prediction Drift

Prediction distribution changes unexpectedly.

Example:

```text
Normally: 10% Positive
Suddenly: 95% Positive
-> Potential issue
```

### Performance Monitoring

Track over time:
- Accuracy
- Precision
- Recall
- F1
- RMSE

Industry MLOps practices emphasize monitoring for drift and performance degradation after deployment.

## 9. Deployment Options (Know the Difference)

**Databricks Model Serving:**

Best for:
- Real-time predictions
- Managed deployment
- REST APIs

**Batch Scoring & External Platforms:**

Best for:
- Large datasets
- Scheduled predictions
- External platforms

Examples:
- Kubernetes
- Kubeflow
- MLflow Docker images

MLflow models can be deployed beyond Databricks as REST services or containerized workloads.

## Common Exam Confusions

| Concept A | Concept B | Difference |
|---|---|---|
| Model Registry | Experiment Tracking | Model lifecycle vs run tracking |
| Model Registry | Feature Store | Models vs features |
| Batch Inference | Real-Time Serving | Large datasets vs immediate prediction |
| Model Serving | Model Registry | Deployment vs storage |
| Monitoring | Evaluation | Production vs training |

## Minimal Working Example

```python
import mlflow

# Register Model
mlflow.register_model(
    "runs:/run_id/model",
    "CustomerChurnModel"
)

# Deploy via Databricks Model Serving
# Endpoint exposes REST API
# Client sends request
# Model returns prediction
```
## 30-Second Revision

```text
Train Model
  -> MLflow Tracking
  -> Register Model
  -> Model Registry
  -> Version Management
  -> Deploy Endpoint
  -> REST API Serving
  -> Monitor Drift & Performance
```

## If You Remember Only 3 Things

1. Experiment Tracking -> Tracks runs; Model Registry -> Tracks models
2. Model Serving = Real-time REST API endpoint
3. Deployment is not the end—monitor data drift, prediction drift, and model performance

## Most Likely Exam Questions

- When should you use Model Registry?
- Difference between Model Registry and Feature Store?
- Difference between batch inference and real-time serving?
- Purpose of Model Serving?
- Why is model monitoring required after deployment?

If you can answer these 5 questions confidently, you've covered most of the high-value concepts from the ML Model Deployment module.


## One Additional Exam Trap

Many candidates confuse MLflow Experiment Tracking vs MLflow Model Registry.

Remember:

| Component | Purpose |
|---|---|
| Experiment Tracking | Tracks runs, parameters, metrics, artifacts |
| Model Registry | Tracks model versions and lifecycle |
| Model Serving | Exposes models via REST endpoint |
| Feature Store | Stores reusable features |

This distinction appears repeatedly in MLflow documentation and Databricks deployment workflows.

## Deployment Module Memory Trick

```text
Train
  -> Track
  -> Register
  -> Deploy
  -> Serve
  -> Monitor
```

If you remember this flow, most deployment questions become straightforward.