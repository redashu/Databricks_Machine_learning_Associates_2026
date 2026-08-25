# Min-Max Scaling — Real ML Dataset Example

Original dataset:

| Person | Age | Salary (₹) |
|--------|-----|------------|
| A      | 20  | 30,000     |
| B      | 25  | 40,000     |
| C      | 30  | 50,000     |
| D      | 35  | 60,000     |
| E      | 40  | 70,000     |

We want to scale **Age** and **Salary** to the range **[0, 1]**.

---

## Step 1: Scale Age

**Original Age:** `20, 25, 30, 35, 40`

### Find Minimum and Maximum

- **Minimum** = 20
- **Maximum** = 40

### Min-Max Formula

$$X' = \frac{X - \text{Min}}{\text{Max} - \text{Min}}$$

### Calculations

| Person | Age | Calculation                        | Result |
|--------|-----|------------------------------------|--------|
| A      | 20  | $\frac{20 - 20}{40 - 20} = \frac{0}{20}$  | 0.00   |
| B      | 25  | $\frac{25 - 20}{40 - 20} = \frac{5}{20}$  | 0.25   |
| C      | 30  | $\frac{30 - 20}{40 - 20} = \frac{10}{20}$ | 0.50   |
| D      | 35  | $\frac{35 - 20}{40 - 20} = \frac{15}{20}$ | 0.75   |
| E      | 40  | $\frac{40 - 20}{40 - 20} = \frac{20}{20}$ | 1.00   |

### Result

| Person | Original Age | Min-Max Age |
|--------|-------------|-------------|
| A      | 20          | 0.00        |
| B      | 25          | 0.25        |
| C      | 30          | 0.50        |
| D      | 35          | 0.75        |
| E      | 40          | 1.00        |

---

## Step 2: Scale Salary

**Original values:** `30,000 · 40,000 · 50,000 · 60,000 · 70,000`

- **Minimum** = 30,000
- **Maximum** = 70,000
- **Max − Min** = 40,000

### Calculations

| Person | Salary   | Calculation                                          | Result |
|--------|----------|------------------------------------------------------|--------|
| A      | ₹30,000  | $\frac{30{,}000 - 30{,}000}{40{,}000}$               | 0.00   |
| B      | ₹40,000  | $\frac{40{,}000 - 30{,}000}{40{,}000}$               | 0.25   |
| C      | ₹50,000  | $\frac{50{,}000 - 30{,}000}{40{,}000}$               | 0.50   |
| D      | ₹60,000  | $\frac{60{,}000 - 30{,}000}{40{,}000}$               | 0.75   |
| E      | ₹70,000  | $\frac{70{,}000 - 30{,}000}{40{,}000}$               | 1.00   |

---

## Final Dataset

| Person | Original Age | Original Salary | Min-Max Age | Min-Max Salary |
|--------|-------------|----------------|-------------|----------------|
| A      | 20          | ₹30,000        | 0.00        | 0.00           |
| B      | 25          | ₹40,000        | 0.25        | 0.25           |
| C      | 30          | ₹50,000        | 0.50        | 0.50           |
| D      | 35          | ₹60,000        | 0.75        | 0.75           |
| E      | 40          | ₹70,000        | 1.00        | 1.00           |

---

## Key Difference from Z-Score

**With Z-score:**

```
Original Age:  20     25      30     35     40
                ↓      ↓       ↓      ↓      ↓
             -1.41  -0.71    0.00   0.71   1.41
```

**With Min-Max:**

```
Original Age:  20     25      30     35     40
                ↓      ↓       ↓      ↓      ↓
              0.00   0.25    0.50   0.75   1.00
```

> **Z-score** tells you how far a value is from the mean in standard-deviation units.
>
> **Min-Max** tells you where a value lies between the minimum and maximum, on a 0-to-1 scale.