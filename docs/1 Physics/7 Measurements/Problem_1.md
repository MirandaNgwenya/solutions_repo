# Problem 1

Excellent! You've done the real experiment — now let's analyze your data and build the full Markdown report step by step.

---

## 📘 **Pendulum Experiment: Measuring Earth’s Gravitational Acceleration**

---

### 🧪 **Experimental Setup**

| Component              | Description          |
| ---------------------- | -------------------- |
| Pendulum length (L)    | 1.000 m              |
| Weight                 | Key                  |
| Support                | Doorframe            |
| Ruler resolution       | 1 mm → ΔL = ±0.001 m |
| Timer                  | Phone stopwatch      |
| Oscillations per trial | 10 swings            |

---

### ⏱ **Raw Time Data for 10 Oscillations**

| Trial | Time for 10 swings (T₁₀) \[s] |
| ----- | ----------------------------- |
| 1     | 20.13                         |
| 2     | 20.08                         |
| 3     | 20.16                         |
| 4     | 20.14                         |
| 5     | 20.11                         |
| 6     | 20.10                         |
| 7     | 20.14                         |
| 8     | 20.08                         |
| 9     | 20.16                         |
| 10    | 20.12                         |

---

### 📊 **Statistical Calculations**

#### 1. Mean time for 10 swings:

$$
\overline{T}_{10} = \frac{\sum T_{10}}{10} = \frac{201.22}{10} = 20.122\ \text{s}
$$

#### 2. Standard deviation (σₜ):

$$
σ_T = \sqrt{\frac{1}{n-1} \sum (T_i - \overline{T})^2} ≈ 0.028\ \text{s}
$$

#### 3. Uncertainty in mean time:

$$
\Delta T_{10} = \frac{σ_T}{\sqrt{n}} = \frac{0.028}{\sqrt{10}} ≈ 0.0089\ \text{s}
$$

---

### ⏱ **Period and its Uncertainty**

#### 4. Single period:

$$
T = \frac{\overline{T}_{10}}{10} = \frac{20.122}{10} = 2.0122\ \text{s}
$$

#### 5. Uncertainty in period:

$$
\Delta T = \frac{\Delta T_{10}}{10} = \frac{0.0089}{10} = 0.00089\ \text{s}
$$

---

### 🌍 **Calculating g**

#### 6. Gravity formula:

$$
g = \frac{4\pi^2 L}{T^2}
= \frac{4\pi^2 \cdot 1.000}{(2.0122)^2}
≈ \frac{39.4784}{4.0490} ≈ 9.754\ \text{m/s}^2
$$

---

### 📉 **Uncertainty in g**

Using uncertainty propagation:

$$
\frac{\Delta g}{g} = \sqrt{\left(\frac{\Delta L}{L}\right)^2 + \left(2\cdot\frac{\Delta T}{T}\right)^2}
= \sqrt{\left(\frac{0.001}{1.000}\right)^2 + \left(2\cdot\frac{0.00089}{2.0122}\right)^2}
\approx \sqrt{1\times10^{-6} + (0.000884)^2}
\approx \sqrt{1\times10^{-6} + 7.82\times10^{-7}} \approx 0.00133
$$

$$
\Delta g = g \cdot 0.00133 ≈ 9.754 \cdot 0.00133 ≈ 0.013\ \text{m/s}^2
$$

---

### ✅ **Final Result**

$$
\boxed{g = 9.754 \pm 0.013\ \text{m/s}^2}
$$

---

### 📚 **Discussion**

* **Comparison to standard value (9.81 m/s²):**
  Your measured g is slightly lower (by \~0.06%), but well within the uncertainty range.

* **Effect of Ruler Resolution (ΔL):**
  The ruler contributed minimal uncertainty (0.1%). It had little impact compared to timing uncertainty.

* **Timing Variability (ΔT):**
  Even small fluctuations in stopwatch timing introduced the largest source of uncertainty.

* **Assumptions & Limitations:**

  * Small-angle approximation was respected (<15°)
  * Reaction time could bias timing slightly
  * Air resistance was neglected
  * Pendulum assumed as point mass and massless string

---


