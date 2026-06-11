# Machine Learning Operations (MLOps)

## What You Must Know for the Exam

MLOps is the practice of managing the complete machine learning lifecycle—from development to deployment and monitoring—using automation, governance, and operational best practices. Databricks combines DataOps + ModelOps + DevOps into a unified platform.

## 1. What is MLOps? (Highest Priority)

Think of MLOps as:

```text
DevOps + DataOps + ModelOps = MLOps
```

Goal:

```text
Build Model
  -> Deploy Model
  -> Monitor Model
  -> Retrain Model
  -> Repeat
```

MLOps focuses on automation, reproducibility, governance, and reliability of ML systems.

Exam tip:

If a question mentions:
- Automation
- Reproducibility
- CI/CD
- Monitoring
- Retraining

The answer is usually related to MLOps.

## 2. The Databricks MLOps Lifecycle

Memorize this flow:

```text
Data
  -> Feature Engineering
  -> Model Training
  -> MLflow Tracking
  -> Model Registry
  -> Deployment
  -> Monitoring
  -> Retraining
```

This is essentially the lifecycle Databricks promotes for production ML systems.

## 3. CI/CD for Machine Learning (Very Important)

**Traditional CI/CD:**

```text
Code
  -> Build
  -> Test
  -> Deploy
```

**ML CI/CD:**

```text
Code
  -> Data Validation
  -> Model Training
  -> Model Validation
  -> Deployment
```

Databricks supports CI/CD using:
- Git integration
- Databricks Asset Bundles
- GitHub Actions
- Azure DevOps
- Jenkins

Exam tip:

CI/CD in ML applies to:
- Code
- Data pipelines
- Models

Not just application code.

## 4. Git Integration

Databricks supports Git-based development.

Common providers:
- GitHub
- GitLab
- Azure DevOps

Benefits:
- Version control
- Collaboration
- Branching
- Pull requests

Exam tip:

If multiple data scientists are working together, use Git + Databricks Repos/Folders.

## 5. Environment Promotion

Typical environments:

```text
Development
  -> Staging
  -> Production
```

Purpose:
- Test before deployment
- Reduce risk
- Ensure quality

Databricks recommends separate environments/catalogs for development, staging, and production workflows.

Exam trap:

Do NOT deploy directly from development to production.

## 6. Unity Catalog (Frequently Tested)

Unity Catalog provides centralized governance.

Governed assets:
- Tables
- Features
- Models
- Endpoints

Capabilities:
- Access control
- Lineage
- Auditing
- Security

Exam tip:

Current Databricks best practice:

```text
Unity Catalog + Models + Features
= Everything governed in one place
```

## 7. Feature Engineering in Unity Catalog

Feature Store is now integrated into Unity Catalog.

Benefits:
- Shared features
- Discoverability
- Lineage
- Governance

Feature tables are Delta tables with primary keys.

Exam tip:

```text
Feature Store -> Feature Engineering in Unity Catalog
```

Databricks has been moving feature management into Unity Catalog governance.

## 8. Workflow Orchestration

Production ML requires automation.

Databricks uses:
- Lakehouse Jobs

to automate:
- Training
- Batch inference
- Monitoring
- Retraining

Example:

```text
Daily Job
  -> Retrain Model
  -> Evaluate Metrics
  -> Register New Version
```
## 9. Monitoring (High Exam Priority)

A deployed model must be monitored continuously.

### Data Quality
- Missing values
- Schema changes
- Data quality issues

### Data Drift
- Training data ≠ production data

### Model Performance
- Accuracy
- Precision
- Recall
- RMSE

### Prediction Drift
- Prediction behavior changes unexpectedly

Monitoring is a core pillar of MLOps.

## 10. Automated Retraining

When:
- Data drift occurs
- Performance drops
- New data becomes available

You may trigger:

```text
Retraining
  -> Validation
  -> Deployment
```

Exam tip:

MLOps is not just deployment. It includes continuous improvement of models over time.

## Common Exam Confusions

| Concept A | Concept B | Difference |
|---|---|---|
| DevOps | MLOps | Software vs ML lifecycle |
| Model Monitoring | Model Evaluation | Production vs training |
| Git | MLflow | Code vs experiments |
| Feature Store | Model Registry | Features vs models |
| CI/CD | Workflow Scheduling | Deployment vs execution |
## Minimal Working Example

```python
import mlflow

with mlflow.start_run():
    mlflow.log_param("max_depth", 5)
    mlflow.log_metric("accuracy", 0.91)
    mlflow.sklearn.log_model(model, "model")
```

This demonstrates:
- Experiment tracking
- Reproducibility
- Governance

which are key MLOps principles.

## 30-Second Revision

```text
Git
  -> Develop Model
  -> MLflow Tracking
  -> Register Model
  -> CI/CD Pipeline
  -> Deploy
  -> Monitor
  -> Detect Drift
  -> Retrain
  -> Redeploy
```

## If You Remember Only 3 Things

1. MLOps = DevOps + DataOps + ModelOps
2. Monitor models after deployment—not monitoring is one of the biggest ML failures.
3. Unity Catalog governs:
   - Data
   - Features
   - Models
   - Endpoints
## Most Likely Exam Questions

- What is MLOps?
- Difference between DevOps and MLOps?
- Why use CI/CD in ML?
- What does Unity Catalog govern?
- Why monitor deployed models?
- What is data drift?
- Why retrain models?
- What is the role of Git in Databricks ML workflows?