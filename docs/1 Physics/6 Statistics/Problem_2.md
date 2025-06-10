# Problem 2

---

# 🧠 Estimating π Using Monte Carlo Methods

## 🎯 Motivation

Monte Carlo methods offer a fascinating approach to solving complex mathematical problems using random sampling. Estimating π is a classic example that combines geometry, probability, and computation. We'll explore two approaches:

* **Circle-based simulation**: Estimating π using a unit square and an inscribed circle.
* **Buffon’s Needle**: A probabilistic geometric method using randomly dropped "needles."

---

## Part 1: Estimating π Using a Circle

### 📚 1. Theoretical Foundation

* Consider a **unit circle** (radius = 1) inscribed in a **square** of side 2.
* The area of the circle: $A_{\text{circle}} = \pi r^2 = \pi$
* The area of the square: $A_{\text{square}} = (2r)^2 = 4$
* Therefore, the ratio of points inside the circle to total points in the square ≈ $\frac{\pi}{4}$

**Formula**:

$$
\pi \approx 4 \cdot \frac{\text{Number of points inside circle}}{\text{Total number of points}}
$$

---

### 🧪 2. Simulation

```python
import numpy as np
import matplotlib.pyplot as plt

def estimate_pi_circle(n_points):
    x = np.random.uniform(-1, 1, n_points)
    y = np.random.uniform(-1, 1, n_points)
    inside_circle = x**2 + y**2 <= 1
    pi_estimate = 4 * np.mean(inside_circle)
    return pi_estimate, x, y, inside_circle
```

---

### 📊 3. Visualization

```python
def plot_circle_simulation(x, y, inside_circle, n_points):
    plt.figure(figsize=(6,6))
    plt.scatter(x[inside_circle], y[inside_circle], color='blue', s=1, label='Inside Circle')
    plt.scatter(x[~inside_circle], y[~inside_circle], color='red', s=1, label='Outside Circle')
    plt.title(f'Pi Estimation with {n_points} Points')
    plt.legend()
    plt.axis('equal')
    plt.show()
```

---

### 📈 4. Analysis

```python
n_values = [100, 1000, 10000, 100000]
estimates = []

for n in n_values:
    pi_estimate, x, y, inside = estimate_pi_circle(n)
    estimates.append(pi_estimate)
    plot_circle_simulation(x, y, inside, n)
    print(f"Points: {n}, π Estimate: {pi_estimate:.6f}")
```

**Observation**:

* As the number of points increases, the estimate of π becomes more accurate.
* Convergence is relatively fast and visually intuitive.

---

## Part 2: Estimating π Using Buffon’s Needle

### 📚 1. Theoretical Foundation

* Drop a needle of length $L$ onto a plane with parallel lines spaced distance $d$ apart.
* If $L \leq d$, the probability that the needle crosses a line is:

$$
P = \frac{2L}{\pi d}
\Rightarrow \pi \approx \frac{2L \cdot N}{d \cdot C}
$$

Where:

* $N$: number of needle drops
* $C$: number of crossings

---

### 🧪 2. Simulation

```python
def buffon_needle(n_drops, L=1.0, d=2.0):
    crosses = 0
    angles = np.random.uniform(0, np.pi, n_drops)
    centers = np.random.uniform(0, d / 2, n_drops)
    crosses = centers <= (L / 2) * np.sin(angles)
    n_crosses = np.sum(crosses)
    if n_crosses == 0:
        return None, crosses, centers, angles
    pi_estimate = (2 * L * n_drops) / (d * n_crosses)
    return pi_estimate, crosses, centers, angles
```

---

### 📊 3. Visualization

```python
def plot_buffon_needle(crosses, centers, angles, L=1.0, d=2.0):
    plt.figure(figsize=(8, 4))
    for i in range(len(crosses)):
        x_center = centers[i]
        theta = angles[i]
        x0 = x_center - (L / 2) * np.cos(theta)
        x1 = x_center + (L / 2) * np.cos(theta)
        y0 = i
        color = 'red' if crosses[i] else 'blue'
        plt.plot([x0, x1], [y0, y0], color=color)
    for i in range(0, int(max(centers) + d), int(d)):
        plt.axvline(i, color='gray', linestyle='--')
    plt.title('Buffon’s Needle Simulation')
    plt.show()
```

---

### 📈 4. Analysis

```python
n_values = [100, 1000, 10000, 50000]
buffon_estimates = []

for n in n_values:
    pi_est, crosses, centers, angles = buffon_needle(n)
    if pi_est:
        buffon_estimates.append(pi_est)
        print(f"Throws: {n}, π Estimate: {pi_est:.6f}")
    else:
        print(f"Throws: {n}, No crossings occurred (increase throws).")
```

---

## 📊 Comparative Analysis

```python
import pandas as pd

df = pd.DataFrame({
    "Number of Points": n_values,
    "π Estimate (Circle Method)": estimates,
    "π Estimate (Buffon Method)": buffon_estimates + [None] * (len(estimates) - len(buffon_estimates))
})

print(df)
```

**Discussion**:

* **Circle Method** converges faster and more consistently than Buffon’s Needle.
* **Buffon’s Needle** requires many iterations for stability and can fail with low drop counts (no crossings).
* Buffon’s method demonstrates the deep relationship between **probability and geometry**, historically significant in the study of π.

---
