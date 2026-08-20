# Hyperparameter Tuning for Machine Learning

Hyperparameter tuning is the process of searching for the best **hyperparameter values** for an ML model to improve validation performance.

---

## 1. Parameter vs Hyperparameter ⭐

### Parameter

A parameter is **learned by the model during training**.

Example:

```text
Linear Regression
    ↓
Weights / coefficients
    ↓
Learned from training data
```

### Hyperparameter

A hyperparameter is **set before training** and controls how the model learns.

Example:

```text
Random Forest
    ├── numTrees = 100
    ├── maxDepth = 10
    └── featureSubsetStrategy = "auto"
```

### Memory Trick

> **Parameter → learned by the model**  
> **Hyperparameter → chosen before training**

---

# 2. Why Hyperparameter Tuning?

Different hyperparameter values can produce different model performance.

Example:

```text
maxDepth = 5    → Accuracy = 82%
maxDepth = 10   → Accuracy = 89%
maxDepth = 15   → Accuracy = 87%
```

The goal is to find the configuration that performs best on validation data.

```text
Hyperparameters
       ↓
Train Model
       ↓
Evaluate
       ↓
Try another configuration
       ↓
Evaluate
       ↓
Best configuration
```

---

# 3. Common Hyperparameters

Different algorithms have different hyperparameters.

| Algorithm | Common Hyperparameters |
|---|---|
| **Linear Regression** | regParam, elasticNetParam |
| **Logistic Regression** | regParam, elasticNetParam, maxIter |
| **Decision Tree** | maxDepth, maxBins, minInstancesPerNode |
| **Random Forest** | numTrees, maxDepth, maxBins |
| **XGBoost** | max_depth, learning_rate, n_estimators, subsample |
| **Neural Network** | learning_rate, batch_size, epochs, number of layers |

---

# 4. Grid Search ⭐⭐⭐

## Concept

Grid Search evaluates **every combination** in a predefined search grid.

Example:

```text
maxDepth  = [5, 10]
numTrees  = [50, 100]
```

Grid Search tests:

```text
1. maxDepth=5,  numTrees=50
2. maxDepth=5,  numTrees=100
3. maxDepth=10, numTrees=50
4. maxDepth=10, numTrees=100
```

Total combinations:

```text
2 × 2 = 4
```

### PySpark Example

```python
from pyspark.ml.tuning import ParamGridBuilder

param_grid = (
    ParamGridBuilder()
    .addGrid(model.maxDepth, [5, 10])
    .addGrid(model.numTrees, [50, 100])
    .build()
)
```

### Advantages

- Simple
- Exhaustive
- Easy to understand

### Disadvantages

- Can become expensive very quickly
- Number of experiments grows multiplicatively

### Exam Memory

> **Grid Search = ALL combinations**

---

# 5. Random Search ⭐⭐⭐

## Concept

Random Search randomly selects hyperparameter combinations from the search space instead of testing every combination.

Example:

```text
Possible combinations = 1,000

Random Search
     ↓
Randomly evaluate 50
     ↓
Find a good configuration
```

### Advantages

- More efficient than exhaustive Grid Search for large search spaces
- Can explore many values without testing every combination

### Disadvantage

- Some randomly selected configurations may not be useful

### Exam Memory

> **Random Search = RANDOM combinations**

---

# 6. Bayesian Optimization ⭐⭐⭐

## Concept

Bayesian optimization uses the results of **previous trials** to decide which hyperparameter configuration should be evaluated next.

```text
Trial 1 → Accuracy 80%
Trial 2 → Accuracy 85%
Trial 3 → Accuracy 88%
        ↓
Learn from previous results
        ↓
Choose promising configuration
        ↓
Trial 4 → Accuracy 91%
```

Unlike Grid Search and Random Search, it does not blindly select the next configuration.

### Advantages

- Can find good configurations with fewer trials
- Useful when model training is expensive

### Exam Memory

> **Bayesian Optimization = learn from previous trials**

---

# 7. Hyperopt ⭐⭐⭐

**Hyperopt** is a Python framework for hyperparameter optimization.

A typical Hyperopt workflow defines:

1. Search space
2. Objective function
3. Optimization algorithm
4. Number of evaluations

### Example

```python
from hyperopt import fmin, tpe, hp, SparkTrials

search_space = {
    "max_depth": hp.choice(
        "max_depth",
        [5, 10, 15]
    ),
    "learning_rate": hp.uniform(
        "learning_rate",
        0.01,
        0.2
    )
}

best = fmin(
    fn=objective,
    space=search_space,
    algo=tpe.suggest,
    max_evals=20,
    trials=SparkTrials()
)
```

### Important Hyperopt Concepts

| Concept | Meaning |
|---|---|
| `fmin()` | Finds the best configuration |
| `hp.choice()` | Defines categorical choices |
| `hp.uniform()` | Defines a continuous uniform range |
| `tpe.suggest` | TPE optimization algorithm |
| `max_evals` | Maximum number of trials |
| `SparkTrials` | Runs Hyperopt trials in parallel using Spark |

### TPE

**TPE = Tree-structured Parzen Estimator**

TPE is an optimization algorithm commonly used with Hyperopt.

### Exam Memory

> **Hyperopt → `fmin()` + search space + optimization algorithm**

---

# 8. Optuna ⭐⭐

Optuna is another hyperparameter optimization framework.

It uses a **trial-based** approach.

### Example

```python
import optuna

def objective(trial):

    max_depth = trial.suggest_int(
        "max_depth",
        3,
        15
    )

    learning_rate = trial.suggest_float(
        "learning_rate",
        0.01,
        0.2
    )

    # Train model
    # Evaluate model

    return validation_score


study = optuna.create_study(
    direction="maximize"
)

study.optimize(
    objective,
    n_trials=50
)
```

### Important Concepts

```text
Study
  ↓
Collection of optimization trials

Trial
  ↓
One hyperparameter configuration
```

### Exam Memory

> **Optuna → `trial.suggest_*()` + `study.optimize()`**

---

# 9. Cross-Validation + Hyperparameter Tuning ⭐⭐⭐

Cross-validation and hyperparameter tuning are related but **not the same thing**.

### Cross-Validation

Used to estimate how well a model performs across different subsets of training data.

Example:

```text
Training Data
      ↓
 ┌────┬────┬────┬────┬────┐
 │ F1 │ F2 │ F3 │ F4 │ F5 │
 └────┴────┴────┴────┴────┘

Fold 1 → Train on F2-F5, Validate on F1
Fold 2 → Train on F1,F3-F5, Validate on F2
...
Fold 5 → Train on F1-F4, Validate on F5
```

Average validation performance is calculated.

### Hyperparameter Tuning

Searches for the best hyperparameter values.

### Together

```text
Hyperparameter Combination 1
          ↓
    Cross Validation
          ↓
    Validation Score

Hyperparameter Combination 2
          ↓
    Cross Validation
          ↓
    Validation Score

          ↓
Best Hyperparameters
```

---

# 10. Train / Validation / Test

A good ML workflow keeps the final test set untouched.

```text
Complete Dataset
       ↓
 ┌─────┴─────────┐
 │               │
Training      Test
 │
Validation
```

Typical workflow:

```text
Training Data
     ↓
Hyperparameter Tuning
     ↓
Validation Performance
     ↓
Best Hyperparameters
     ↓
Final Model
     ↓
Untouched Test Data
     ↓
Final Performance
```

### ⚠️ Important

Do not repeatedly tune hyperparameters using the final test set.

The test set should provide an **unbiased final evaluation**.

---

# 11. Grid vs Random vs Bayesian

| Method | Search Strategy | Key Idea |
|---|---|---|
| **Grid Search** | Exhaustive | Try **ALL** combinations |
| **Random Search** | Random | Try randomly selected combinations |
| **Bayesian Optimization** | Intelligent | Use previous results to select promising trials |
| **Hyperopt** | Optimization framework | Supports algorithms such as TPE |
| **Optuna** | Optimization framework | Trial-based optimization |

---

# 12. Example: Tuning XGBoost

Suppose we want to tune:

```text
max_depth
learning_rate
n_estimators
```

### Grid Search

```text
max_depth     = [3, 6]
learning_rate = [0.01, 0.1]
n_estimators  = [100, 200]
```

Total:

```text
2 × 2 × 2 = 8 combinations
```

### Random Search

```text
Search space
    ↓
Randomly select configurations
    ↓
Evaluate selected trials
```

### Bayesian Optimization

```text
Initial trials
     ↓
Learn which regions are promising
     ↓
Choose next configuration
     ↓
Evaluate
     ↓
Repeat
```

---

# 13. Overfitting During Hyperparameter Tuning

A model can perform extremely well on training data but poorly on unseen data.

```text
Training Accuracy = 99%
Validation Accuracy = 80%
```

This is a sign of possible **overfitting**.

Hyperparameters such as:

```text
maxDepth
number of trees
regularization
learning rate
```

can strongly affect overfitting.

### Example

```text
Decision Tree

maxDepth = 3
    ↓
Simple model

maxDepth = 50
    ↓
Very complex model
    ↓
Potential overfitting
```

---

# 14. Important Hyperparameters to Recognize

## Decision Tree

```text
maxDepth
maxBins
minInstancesPerNode
```

Higher `maxDepth` generally allows a more complex tree.

---

## Random Forest

```text
numTrees
maxDepth
maxBins
featureSubsetStrategy
```

`numTrees` controls the number of trees.

---

## XGBoost

```text
max_depth
learning_rate
n_estimators
subsample
colsample_bytree
```

### Learning Rate

Controls how much each boosting step contributes.

```text
High learning_rate
    ↓
Faster learning
    ↓
Potentially less stable / overfitting risk

Low learning_rate
    ↓
Slower learning
    ↓
Often requires more trees
```

---

# 15. Early Stopping

Early stopping prevents unnecessary training when validation performance stops improving.

```text
Epoch 1 → Validation = 80%
Epoch 2 → Validation = 85%
Epoch 3 → Validation = 89%
Epoch 4 → Validation = 91%
Epoch 5 → Validation = 91%
Epoch 6 → Validation = 90%
```

Training can stop when the validation metric stops improving.

### Benefit

- Reduces unnecessary computation
- Can help prevent overfitting

---

# 16. Databricks ML Experimentation

In Databricks, hyperparameter tuning is commonly combined with **MLflow**.

Typical workflow:

```text
Hyperparameter Trial
        ↓
Train Model
        ↓
Evaluate
        ↓
MLflow logs:
    - Parameters
    - Metrics
    - Model
        ↓
Compare Experiments
        ↓
Select Best Run
```

Example:

```python
import mlflow

with mlflow.start_run():

    mlflow.log_param("max_depth", 10)
    mlflow.log_param("learning_rate", 0.1)

    mlflow.log_metric("accuracy", 0.91)
```

### Important Distinction

> **Hyperparameter tuning searches for the best configuration.**

> **MLflow tracks and manages the experiments/runs and their results.**

---

# 17. Scenario-Based Exam Questions

### Scenario 1

An engineer wants to evaluate **every possible combination** of `maxDepth`
and `numTrees`.

**Answer: Grid Search**

---

### Scenario 2

An engineer has 10,000 possible configurations but wants to evaluate only
100 randomly selected configurations.

**Answer: Random Search**

---

### Scenario 3

An engineer wants the optimization algorithm to use results from previous
experiments when selecting the next hyperparameters.

**Answer: Bayesian Optimization**

---

### Scenario 4

The code contains:

```python
fmin(
    fn=objective,
    space=search_space,
    algo=tpe.suggest
)
```

**Answer: Hyperopt**

---

### Scenario 5

The code contains:

```python
trial.suggest_int(...)
study.optimize(...)
```

**Answer: Optuna**

---

### Scenario 6

An engineer evaluates each hyperparameter configuration using 5 folds of
the training data.

**Answer: Cross-Validation + Hyperparameter Tuning**

---

### Scenario 7

An engineer repeatedly changes hyperparameters based on the final test
set performance.

**Answer: Incorrect — this can lead to test-set overfitting.**

The test set should remain untouched until final evaluation.

---

# 18. ⭐ Databricks ML Exam Memory Sheet

```text
PARAMETER
    ↓
Learned by the model

HYPERPARAMETER
    ↓
Set before training


GRID SEARCH
    ↓
ALL combinations


RANDOM SEARCH
    ↓
RANDOM combinations


BAYESIAN OPTIMIZATION
    ↓
Learn from previous trials


HYPEROPT
    ↓
fmin()
hp.*
tpe.suggest
SparkTrials


OPTUNA
    ↓
trial.suggest_*
study.optimize()


CROSS-VALIDATION
    ↓
Evaluate model across multiple folds


MLFLOW
    ↓
Track parameters
Track metrics
Track models
Compare runs


TEST SET
    ↓
Keep untouched
Use for final evaluation
```

---

# 19. One-Line Final Comparison

> **Grid Search = exhaustive**

> **Random Search = random exploration**

> **Bayesian Optimization = learn from previous trials**

> **Hyperopt = optimization framework commonly using TPE**

> **Optuna = trial-based optimization framework**

> **Cross-Validation = evaluate robustness across folds**

> **MLflow = track and compare tuning experiments**

