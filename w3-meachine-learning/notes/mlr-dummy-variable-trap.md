# Dummy Variable Trap in Multiple Linear Regression (MLR)

## 📌 What is Dummy Variable Trap?

The **Dummy Variable Trap** occurs when we use **all dummy variables** created from a categorical feature in a regression model.

This causes **perfect multicollinearity**, where one dummy variable can be predicted using the others.

As a result:
- \(X^T X\) becomes **singular (non-invertible)**
- MLR cannot find **unique coefficients**
- Model becomes **unstable**

---

## 📊 Example

### Categorical Feature: State
- TamilNadu  
- Kerala  
- Karnataka  

### One-Hot Encoding (❌ Trap)

| TamilNadu | Kerala | Karnataka |
|-----------|--------|-----------|
| 1 | 0 | 0 |
| 0 | 1 | 0 |
| 0 | 0 | 1 |

Here:

TamilNadu = 1 − (Kerala + Karnataka)  
Kerala = 1 − (TamilNadu + Karnataka)  
Karnataka = 1 − (TamilNadu + Kerala)

👉 One column is dependent on the others → **perfect multicollinearity**

---

## ⚠️ Why is this a problem in MLR?

MLR formula:

\[
Y = b_0 + b_1X_1 + b_2X_2 + \dots + b_nX_n
\]

MLR assumes **independent features**.

If one feature = combination of others:
- Infinite coefficient solutions
- Matrix inversion fails
- Model cannot train properly

---

## ✅ Solution — k − 1 Rule

If a categorical variable has **k categories**, use only **k − 1 dummy variables**.

Drop one column → it becomes the **reference category**.

### After Fix (✔)

Drop **TamilNadu**

| Kerala | Karnataka |
|--------|-----------|
| 0 | 0 → TamilNadu (reference) |
| 1 | 0 → Kerala |
| 0 | 1 → Karnataka |

Now:
- No multicollinearity  
- Features are independent  
- MLR works correctly  

---

## 🧠 Mental Model

Think of dummy variables as **switches**.

For 3 categories:
- Only **one switch can be ON**
- So one switch is always predictable from others

➡️ Remove one switch → problem solved

---

## 🧪 Python Implementation

### Using pandas

```python
pd.get_dummies(df, drop_first=True)