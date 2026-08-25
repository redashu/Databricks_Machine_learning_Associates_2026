# Z-Score Standardization

Suppose this is our original dataset before standardization:

| Person | Age | Salary (₹) |
|--------|-----|------------|
| A      | 20  | 30,000     |
| B      | 25  | 40,000     |
| C      | 30  | 50,000     |
| D      | 35  | 60,000     |
| E      | 40  | 70,000     |

We want to apply Z-score standardization to **Age** and **Salary**.

---

## Step 1: Standardize Age

**Original Age:** `20, 25, 30, 35, 40`

### Calculate Mean

$$\text{Mean} = \frac{20 + 25 + 30 + 35 + 40}{5} = 30$$

### Calculate Standard Deviation

Differences from mean:

| Value | Difference | Squared |
|-------|-----------|---------|
| 20    | −10        | 100     |
| 25    | −5         | 25      |
| 30    | 0          | 0       |
| 35    | +5         | 25      |
| 40    | +10        | 100     |

**Population variance:**

$$\text{Variance} = \frac{250}{5} = 50$$

**Standard deviation:**

$$\text{Std} = \sqrt{50} = 7.071$$

### Calculate Z-Score

**Formula:**

$$Z = \frac{X - \text{Mean}}{\text{Std}}$$

**For Age = 20:**

$$Z = \frac{20 - 30}{7.071} = -1.414$$

**For Age = 25:**

$$Z = \frac{25 - 30}{7.071} = -0.707$$

And so on.

**Result:**

| Person | Original Age | Standardized Age |
|--------|-------------|-----------------|
| A      | 20          | −1.414           |
| B      | 25          | −0.707           |
| C      | 30          | 0.000            |
| D      | 35          | 0.707            |
| E      | 40          | 1.414            |

---

## Step 2: Standardize Salary

**Original values:** `30,000 · 40,000 · 50,000 · 60,000 · 70,000`

**Mean:**

$$\text{Mean} = 50{,}000$$

**Standard deviation:**

$$\text{Std} = 14{,}142.14$$

**Z-scores:**

| Person | Calculation                                       | Z-Score |
|--------|--------------------------------------------------|---------|
| A      | $\frac{30{,}000 - 50{,}000}{14{,}142.14}$        | −1.414  |
| B      | $\frac{40{,}000 - 50{,}000}{14{,}142.14}$        | −0.707  |
| C      | $\frac{50{,}000 - 50{,}000}{14{,}142.14}$        | 0.000   |
| D      | $\frac{60{,}000 - 50{,}000}{14{,}142.14}$        | +0.707  |
| E      | $\frac{70{,}000 - 50{,}000}{14{,}142.14}$        | +1.414  |

---

## Final Dataset

| Person | Original Age | Original Salary | Standardized Age | Standardized Salary |
|--------|-------------|----------------|-----------------|---------------------|
| A      | 20          | 30,000         | −1.414           | −1.414               |
| B      | 25          | 40,000         | −0.707           | −0.707               |
| C      | 30          | 50,000         | 0.000            | 0.000                |
| D      | 35          | 60,000         | 0.707            | 0.707                |
| E      | 40          | 70,000         | 1.414            | 1.414                |

---

## Key Concept

> We don't calculate one mean and one standard deviation for the whole dataset.
> We standardize **each numerical feature independently**.

```
Original Dataset
       │
       ├── Age
       │     ↓
       │   Mean + Std
       │     ↓
       │   Z-score
       │
       └── Salary
             ↓
          Mean + Std
             ↓
          Z-score
```