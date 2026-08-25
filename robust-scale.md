# Robust Scaling — Real ML Dataset Example

Original dataset:

| Person | Age | Salary (₹) |
|--------|-----|------------|
| A      | 20  | 30,000     |
| B      | 25  | 40,000     |
| C      | 30  | 50,000     |
| D      | 35  | 60,000     |
| E      | 40  | 70,000     |
| F      | 100 | 500,000    |

> **100** (age) and **₹500,000** (salary) are potential outliers.

Robust scaling uses:

- **Median** instead of Mean
- **IQR** instead of Standard Deviation

### Formula

$$X' = \frac{X - \text{Median}}{\text{IQR}}$$

where $\text{IQR} = Q3 - Q1$

---

## Step 1: Robust Scaling for Age

**Original Age:** `20, 25, 30, 35, 40, 100`

### 1. Find Median

6 values, so:

$$\text{Median} = \frac{30 + 35}{2} = 32.5$$

### 2. Find Q1 and Q3

| Half        | Values       | Quartile |
|-------------|-------------|----------|
| Lower half  | 20, 25, 30  | Q1 = 25  |
| Upper half  | 35, 40, 100 | Q3 = 40  |

### 3. Calculate IQR

$$\text{IQR} = Q3 - Q1 = 40 - 25 = 15$$

### 4. Calculate Robust Scaled Values

| Person | Age | Calculation                            | Result  |
|--------|-----|----------------------------------------|---------|
| A      | 20  | $\frac{20 - 32.5}{15}$                 | −0.833  |
| B      | 25  | $\frac{25 - 32.5}{15}$                 | −0.500  |
| C      | 30  | $\frac{30 - 32.5}{15}$                 | −0.167  |
| D      | 35  | $\frac{35 - 32.5}{15}$                 | 0.167   |
| E      | 40  | $\frac{40 - 32.5}{15}$                 | 0.500   |
| F      | 100 | $\frac{100 - 32.5}{15}$                | 4.500   |

### Result

| Person | Original Age | Robust Scaled Age |
|--------|-------------|------------------|
| A      | 20          | −0.833            |
| B      | 25          | −0.500            |
| C      | 30          | −0.167            |
| D      | 35          | 0.167             |
| E      | 40          | 0.500             |
| F      | 100         | 4.500             |

> **Notice:** The outlier is still visible. Robust scaling doesn't *remove* outliers — it simply prevents them from distorting the center and scale calculations the way mean/std would.

---

## Step 2: Robust Scaling for Salary

**Values:** `30K, 40K, 50K, 60K, 70K, 500K`

| Statistic | Value                                  |
|-----------|----------------------------------------|
| Median    | $\frac{50K + 60K}{2} = 55K$            |
| Q1        | 40K                                    |
| Q3        | 70K                                    |
| IQR       | $70K - 40K = 30K$                      |

**Key calculations:**

$$\text{Person A:} \quad \frac{30K - 55K}{30K} = -0.833$$

$$\text{Person F:} \quad \frac{500K - 55K}{30K} = 14.83$$

> The outlier remains large in scaled form, but it did **not** distort the median/IQR calculation.

---

## Comparing the Three Techniques

| Technique | Center | Scale | Best When                          |
|-----------|--------|-------|------------------------------------|
| Z-score   | Mean   | Std   | Data without significant outliers  |
| Min-Max   | Min    | Range | Need values in a bounded range     |
| Robust    | Median | IQR   | Data contains outliers             |

---

## Exam Memory Trick

```
Z-score              Min-Max              Robust
   ↓                    ↓                   ↓
Mean + Std          Min + Max          Median + IQR
   ↓                    ↓                   ↓
Sensitive to       Very sensitive      Less sensitive
 outliers           to outliers         to outliers
```

> **Key point:** Robust scaling does **NOT** eliminate outliers. It uses statistics (median and IQR) that are less *affected* by outliers.