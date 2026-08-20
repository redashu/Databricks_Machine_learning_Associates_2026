# Sampling Methods for Machine Learning

Sampling is the process of selecting a subset of records from a larger dataset for analysis, training, validation, testing, or experimentation.

## 1. Random Sampling

**Definition:** Randomly selects records from the dataset. Each record has approximately the same chance of being selected.

**Example:** From 1,000,000 customer records, randomly select 10%.

```python
sample_df = df.sample(
    withReplacement=False,
    fraction=0.10,
    seed=42
)
```

**Use when:** The dataset is reasonably representative and there is no special class or time dependency.

**Exam:** Random sampling = random records.

---

## 2. Stratified Sampling ⭐

**Definition:** Samples from different groups/classes while maintaining their desired proportions.

**Example:** A fraud dataset contains 95% normal transactions and 5% fraud transactions. Stratified sampling keeps approximately the same ratio in the sample.

```python
fractions = {
    "Normal": 0.10,
    "Fraud": 0.10
}

sample_df = df.sampleBy(
    "transaction_type",
    fractions=fractions,
    seed=42
)
```

**Use when:** The target/classes are imbalanced and you want important groups to remain represented.

**Exam:** Stratified sampling = preserve class/group distribution.

---

## 3. Systematic Sampling

**Definition:** Selects every Nth record after establishing a starting point.

**Example:** Select every 10th customer.

```text
1  2  3  4  5  6  7  8  9  10  11  12 ...
                         ↑
                      select
```

```python
from pyspark.sql.window import Window
from pyspark.sql.functions import row_number, col

window = Window.orderBy("customer_id")

sample_df = (
    df
    .withColumn("row_num", row_number().over(window))
    .filter(col("row_num") % 10 == 0)
)
```

**Use when:** You need regularly spaced observations from an ordered dataset.

**Exam:** Systematic sampling = every Nth record.

---

## 4. Time-Series Sampling / Chronological Split ⭐⭐⭐

**Definition:** Samples or splits data according to time while preserving chronological order.

**Example:** Predict December sales using historical sales.

```text
January ───────── October → Training
November                 → Validation
December                 → Test
```

```python
train_df = df.filter(col("date") < "2026-11-01")

validation_df = df.filter(
    (col("date") >= "2026-11-01") &
    (col("date") < "2026-12-01")
)

test_df = df.filter(col("date") >= "2026-12-01")
```

**Use when:** Working with stock prices, sales forecasting, demand forecasting, sensor data, or other temporal data.

**Critical exam point:** Do not randomly mix future observations into training data because it can cause **data leakage**.

**Exam:** Time-series data = preserve chronological order.

---

## 5. Cluster Sampling

**Definition:** Selects entire groups (clusters) rather than individual records.

**Example:** A company has 500 stores. Select 20 stores and use all customers from those stores.

```text
500 Stores
    ↓
Select 20 Stores
    ↓
Use all customers in those stores
```

**Use when:** Data naturally belongs to groups such as stores, schools, hospitals, regions, or departments.

**Exam:** Cluster sampling = select whole groups.

---

## 6. Bootstrap Sampling ⭐

**Definition:** Randomly samples data **with replacement**, so the same observation can appear multiple times.

**Example:**

```text
Original:  [A, B, C, D, E]

Bootstrap: [B, D, B, A, E]
                ↑
             duplicate
```

```python
sample_df = df.sample(
    withReplacement=True,
    fraction=1.0,
    seed=42
)
```

**Use when:** Estimating uncertainty, confidence intervals, statistical properties, or as a foundation for some ensemble techniques.

**Exam:** Bootstrap = sampling with replacement.

---

## 7. Random Train/Test Split

**Definition:** Randomly divides data into training and testing sets.

**Example:**

```text
100,000 records
       ↓
Random Split
       ↓
80% Training
20% Testing
```

```python
train_df, test_df = df.randomSplit(
    [0.8, 0.2],
    seed=42
)
```

**Use when:** Data is approximately IID (independent and identically distributed) and has no temporal dependency.

**Do not use blindly:** For time-series data, use a chronological split.

---

## 8. Sampling With vs Without Replacement

### Without Replacement

A record can be selected only once.

```python
df.sample(
    withReplacement=False,
    fraction=0.20,
    seed=42
)
```

```text
[A, B, C, D] → [A, C]

A cannot appear again.
```

### With Replacement

A record can be selected multiple times.

```python
df.sample(
    withReplacement=True,
    fraction=1.0,
    seed=42
)
```

```text
[A, B, C, D] → [A, C, A, D]

A appears twice.
```

---

## Quick Comparison

| Method | Main Idea | Typical ML Use |
|---|---|---|
| **Random Sampling** | Randomly select records | General datasets |
| **Stratified Sampling** | Preserve class/group proportions | Imbalanced classification |
| **Systematic Sampling** | Select every Nth record | Ordered data |
| **Time-Series Sampling** | Preserve chronological order | Forecasting |
| **Cluster Sampling** | Select entire groups | Store/region-based data |
| **Bootstrap Sampling** | Sampling with replacement | Statistical estimation |
| **Random Split** | Randomly divide train/test | Standard IID ML |

---

## Databricks ML Exam Cheat Sheet

| Scenario | Best Choice |
|---|---|
| Randomly select 10% of a large dataset | **Random Sampling** |
| Maintain 95% Normal / 5% Fraud ratio | **Stratified Sampling** |
| Select every 10th record | **Systematic Sampling** |
| Predict future sales from historical sales | **Time-Series / Chronological Split** |
| Select 20 stores and all their customers | **Cluster Sampling** |
| Allow the same record to appear multiple times | **Bootstrap Sampling** |
| Split IID data into 80% train / 20% test | **Random Split** |

## Memory Trick

```text
Random      → Random rows
Stratified  → Preserve classes/groups
Systematic  → Every Nth row
Time-Series → Preserve time order
Cluster     → Whole groups
Bootstrap   → With replacement
Random Split→ Train/Test for IID data
```
