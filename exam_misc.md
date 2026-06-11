# Databricks ML Associate Exam – Final Revision Sheet

**Purpose:** This is not a course summary. This is a high-value revision guide focused on concepts that commonly appear in Databricks ML Associate exam scenarios and real-world Databricks MLOps implementations.

## Big Picture

MLOps combines:

```text
DevOps + DataOps + ModelOps = MLOps
```

The objective is to automate and govern the complete ML lifecycle. Databricks recommends a structured workflow across development → staging → production environments.

## The Most Important Workflow to Remember

```text
Data
  -> Feature Engineering
  -> Train Model
  -> Hyperparameter Tuning
  -> MLflow Tracking
  -> Model Validation
  -> Register Model
  -> Champion vs Challenger
  -> Deploy
  -> Serve Predictions
  -> Monitor
  -> Retrain
```

If you understand this flow, you can answer most MLOps questions in the exam.

---

## 1. Development → Staging → Production

Databricks strongly recommends separate environments.

**Development:**
- Data scientists
- Perform EDA
- Build features
- Train models
- Tune hyperparameters
- Run experiments

**Staging:**
- ML engineers
- Execute unit tests
- Execute integration tests
- Validate pipelines

**Production:**
- Automated training
- Automated validation
- Automated deployment
- Monitoring

Exam tip:
- Development = Experiment
- Staging = Validate
- Production = Operate

## 2. MLflow Is Everywhere

Many candidates think MLflow = experiment tracking.

**Wrong.**

For Databricks, MLflow includes:
- Tracking
- Models
- Registry
- Serving
- Governance

Exam question: "What service tracks parameters, metrics, artifacts and models?"

Answer: **MLflow**

## 3. MLflow Tracking

Tracks:
- Parameters
- Metrics
- Artifacts
- Models

Example:

```python
with mlflow.start_run():
    mlflow.log_param("max_depth", 5)
    mlflow.log_metric("accuracy", 0.92)
    mlflow.sklearn.log_model(model, "model")
```

## 4. MLflow Autologging

One of the easiest exam questions.

```python
mlflow.autolog()
```

Automatically captures:
- Parameters
- Metrics
- Artifacts
- Models

Exam tip: Manual logging vs autologging — autologging reduces boilerplate code.

## 5. MLflow Artifacts

MLflow stores more than models.

Examples:
- Confusion matrix
- ROC curve
- SHAP plot
- Reports
- Feature importance

Exam trap: MLflow ≠ Only models.

Databricks explicitly recommends storing governance artifacts such as explanations and evaluation outputs.

## 6. Hyperparameter Tuning

Production models are rarely trained once.

Workflow:

```text
Train -> Tune -> Evaluate -> Register
```

Databricks production training pipelines include training and tuning activities.

**Grid Search:**
- Tests every combination
- Pros: Exhaustive
- Cons: Expensive

**Optuna:**

```python
study = optuna.create_study(direction="maximize")
study.optimize(objective, n_trials=100)
```

- Pros: Intelligent search, faster convergence

Exam memory:
- Grid search = Brute force
- Optuna = Smart search

## 7. Experiment → Best Run → Registry

Common Databricks pattern:

```text
Multiple Runs
  -> Compare Metrics
  -> Select Best Run
  -> Register Model
```

MLflow tracking helps identify the best-performing run before registration.

## 8. Model Validation (Very Important)

A trained model should NEVER go directly into production.

Workflow:

```text
Train -> Validate -> Deploy
```

Validation checks:
- Performance thresholds
- Metadata validation
- Compliance checks
- Documentation checks
- Slice-based evaluation

Databricks recommends a dedicated validation pipeline before deployment.

## 9. Champion vs Challenger (Highest Probability Topic)

Databricks now recommends model aliases.

**Current Production Model → Champion**

**New Candidate Model → Challenger**

Comparison:

```text
Champion VS Challenger
```

If Challenger performs better:

```text
Promote Challenger -> New Champion
```

Databricks uses champion and challenger aliases to support model promotion workflows.

## 10. Model Aliases (Unity Catalog)

**Legacy thinking:**
- Staging
- Production
- Archived

**Modern Unity Catalog:**
- Champion
- Challenger

Example:

```python
client.set_registered_model_alias(
    "prod.fraud.model",
    "Champion",
    7
)
```

Benefits:
- Human readable
- Easier promotions
- No hardcoded versions

## 11. Deploy Code, Not Models

One of the most important Databricks-specific concepts.

**Most beginners think:**

```text
Train Model -> Move Model To Prod
```

**Databricks recommendation:**

```text
Promote Code -> Retrain -> Validate -> Deploy
```

**Why?**
- Better governance
- Better reproducibility
- Better CI/CD
- Environment consistency

This is known as the **Deploy Code Pattern**.

### Exam Scenario

**Question:** Data scientists trained a model in development and want to move the model artifact to production.

**Best Databricks answer:** Promote training code, not model artifact.

## 12. CI / CD / CT

**CI — Continuous Integration:**

```text
Merge Code -> Run Tests
```

**CD — Continuous Deployment:**

```text
Validated Pipeline -> Deploy
```

**CT — Continuous Training:**

```text
New Data -> Retrain -> Deploy
```

MLOps extends traditional DevOps by including retraining workflows.

## 13. Unit Tests vs Integration Tests

**Unit Test:**
- Tests a single component
- Example: `calculate_customer_age()`

**Integration Test:**
- Tests complete workflow
- Example:
  - Feature pipeline
  - Training pipeline
  - Validation pipeline

Databricks staging environments are primarily used for these tests.

## 14. Model Registry

**Purpose:**
- Versioning
- Governance
- Lifecycle management

**Stores:**
- Versions
- Metadata
- Aliases

**Not used for:**
- Feature storage
- Experiment tracking

Exam trap:

| Service | Purpose |
|---|---|
| MLflow Tracking | Experiments |
| Model Registry | Models |
| Feature Store | Features |

## 15. Unity Catalog Governance

Unity Catalog governs:
- Tables
- Features
- Models
- Endpoints

Benefits:
- Access control
- Lineage
- Auditing
- Cross-workspace governance

## 16. Model Serving

Real-time inference.

```text
Application -> REST API -> Model Serving Endpoint -> Prediction
```

Supports:
- Real-time scoring
- Online inference
- Multi-model endpoints
- Traffic splitting

## 17. Inference Tables

Frequently missed topic.

Databricks can automatically collect inference request/response data from serving endpoints.

Stores:
- Requests
- Responses
- Metadata

Used for:
- Monitoring
- Auditing
- Drift detection

Exam tip: **Inference Tables = Production prediction logs**

## 18. Monitoring & Drift Detection

Deployment is NOT the end.

### Data Drift

Training data ≠ production data.

Example:

```text
Training Age Avg = 30
Production Age Avg = 55
```

### Prediction Drift

Prediction distribution changes.

Example:

```text
Normally: 10% Positive
Suddenly: 95% Positive
```

### Performance Drift

Metrics degrade.

Examples:
- Accuracy
- Precision
- Recall
- RMSE

## 19. Data Profiling

Databricks explicitly recommends profiling production data.

Monitor:
- Feature statistics
- Missing values
- Distribution changes
- Prediction statistics

Workflow:

```text
Input Data -> Profile -> Detect Drift
```

## 20. Automated Retraining

Trigger retraining when:
- Drift detected
- Performance drops
- New data arrives

Workflow:

```text
Monitor -> Retrain -> Validate -> Deploy
```

Production training pipelines are commonly triggered by scheduled retraining jobs.

---

## Common Exam Confusions

| Concept A | Concept B | Difference |
|---|---|---|
| MLflow Tracking | Model Registry | Experiments vs models |
| Feature Store | Model Registry | Features vs models |
| Champion | Challenger | Current vs candidate |
| Deploy Code | Deploy Model | Recommended vs traditional |
| Validation | Monitoring | Pre-deployment vs post-deployment |
| Data Drift | Performance Drift | Inputs vs metrics |
| Unit Test | Integration Test | Component vs workflow |
| Alias | Version | Logical name vs numeric version |

## 30-Second Revision

```text
Develop
  -> Train
  -> Tune
  -> Track
  -> Validate
  -> Register
  -> Champion vs Challenger
  -> Deploy
  -> Serve
  -> Monitor
  -> Retrain
```

## If You Remember Only 5 Things

1. Databricks MLOps = MLflow everywhere
2. Champion = Current production model; Challenger = Candidate model
3. Deploy code, NOT models
4. Development → Staging → Production
5. Monitor after deployment—not monitoring is one of the biggest ML failures

## One-Line Memory Trick

Experiment → Tune → Track → Validate → Register → Challenge → Deploy → Monitor → Retrain
