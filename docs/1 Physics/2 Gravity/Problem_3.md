# Problem 3

---

# **Escape Velocities and Cosmic Velocities**

## **Motivation**

Understanding the speeds required to enter orbit, escape planetary gravity, or even leave the Solar System is foundational to space exploration. These speeds—called **cosmic velocities**—determine how we launch satellites, send missions to other planets, and plan interstellar probes.

---

## **1. Definitions of Cosmic Velocities**

| Cosmic Velocity    | Description                                                     | Formula                                     |
| ------------------ | --------------------------------------------------------------- | ------------------------------------------- |
| **First** ($v_1$)  | Minimum speed to achieve stable circular orbit near the surface | $v_1 = \sqrt{\frac{GM}{R}}$                 |
| **Second** ($v_2$) | Minimum speed to escape the planet’s gravity                    | $v_2 = \sqrt{\frac{2GM}{R}}$                |
| **Third** ($v_3$)  | Speed to escape the star system from a planet’s orbit           | $v_3 = \sqrt{v_2^2 + v_{\text{orbital}}^2}$ |

Where:

* $G = 6.674 \times 10^{-11} \, \text{m}^3\,\text{kg}^{-1}\,\text{s}^{-2}$
* $M$: Mass of the celestial body
* $R$: Radius of the celestial body

---

## **2. Mathematical Derivations**

### **First Cosmic Velocity**

For a stable circular orbit at radius $R$:

$$
v_1 = \sqrt{\frac{GM}{R}}
$$

### **Second Cosmic Velocity (Escape Velocity)**

Set total energy to zero (kinetic + gravitational potential):

$$
\frac{1}{2}mv^2 - \frac{GMm}{R} = 0 \Rightarrow v = \sqrt{\frac{2GM}{R}}
$$

### **Third Cosmic Velocity**

Escape velocity from the Sun starting from Earth’s orbit:

$$
v_3 = \sqrt{v_2^2 + v_{\text{Earth orbit}}^2}
$$

Where $v_{\text{Earth orbit}} \approx 29.78 \times 10^3$ m/s.

---

## **3. Python Code: Computing Cosmic Velocities**

```python
import numpy as np
import matplotlib.pyplot as plt

# Constants
G = 6.67430e-11  # gravitational constant, m^3 kg^-1 s^-2

# Celestial bodies: name -> (mass in kg, radius in meters)
bodies = {
    "Earth": (5.972e24, 6.371e6),
    "Mars": (6.417e23, 3.3895e6),
    "Jupiter": (1.898e27, 6.9911e7)
}

def compute_cosmic_velocities(mass, radius):
    v1 = np.sqrt(G * mass / radius)
    v2 = np.sqrt(2) * v1
    return v1, v2

# Compute velocities
results = {}
for planet, (mass, radius) in bodies.items():
    v1, v2 = compute_cosmic_velocities(mass, radius)
    results[planet] = {'v1': v1, 'v2': v2}

# Display results
for planet, v in results.items():
    print(f"{planet}:")
    print(f"  First Cosmic Velocity (v1): {v['v1']:.2f} m/s")
    print(f"  Second Cosmic Velocity (v2): {v['v2']:.2f} m/s\n")
```

Earth:
  First Cosmic Velocity (v1): 7909.68 m/s
  Second Cosmic Velocity (v2): 11185.98 m/s

Mars:
  First Cosmic Velocity (v1): 3554.68 m/s
  Second Cosmic Velocity (v2): 5027.08 m/s

Jupiter:
  First Cosmic Velocity (v1): 42567.51 m/s
  Second Cosmic Velocity (v2): 60199.54 m/s
---

## **4. Visualization**

### **Bar Chart of Velocities**

```python
labels = list(results.keys())
v1_vals = [results[planet]['v1'] for planet in labels]
v2_vals = [results[planet]['v2'] for planet in labels]

x = np.arange(len(labels))
width = 0.35

plt.figure(figsize=(10, 6))
plt.bar(x - width/2, v1_vals, width, label='1st Cosmic Velocity')
plt.bar(x + width/2, v2_vals, width, label='2nd Cosmic Velocity')
plt.xticks(x, labels)
plt.ylabel('Velocity (m/s)')
plt.title('Cosmic Velocities for Earth, Mars, and Jupiter')
plt.legend()
plt.grid(True, linestyle='--', alpha=0.5)
plt.tight_layout()
plt.show()
```

---

## **5. Third Cosmic Velocity (Escape the Solar System)**

From Earth:

* $v_2 \approx 11.2 \times 10^3 \, \text{m/s}$
* $v_{\text{Earth orbit}} \approx 29.78 \times 10^3 \, \text{m/s}$

$$
v_3 = \sqrt{(11.2 \times 10^3)^2 + (29.78 \times 10^3)^2} \approx 42.1 \times 10^3 \, \text{m/s}
$$

---

## **6. Importance in Space Exploration**

| Application                   | Required Velocity |
| ----------------------------- | ----------------- |
| Satellite Orbiting            | $v_1$             |
| Escape to Moon/Mars           | $v_2$             |
| Voyager/Interstellar Missions | $v_3$             |

* **Launch Vehicles**: Must reach $v_2$ to leave Earth.
* **Transfer Orbits**: Use combinations of $v_1$ and gravity assists.
* **Deep Space Missions**: Require planning to reach $v_3$ using multiple planetary flybys.

---

## **7. Summary Table**

| Planet  | 1st Cosmic Velocity (m/s) | 2nd Cosmic Velocity (m/s) |
| ------- | ------------------------- | ------------------------- |
| Earth   | \~7,905                   | \~11,186                  |
| Mars    | \~3,550                   | \~5,030                   |
| Jupiter | \~42,000                  | \~59,540                  |

---

## **Conclusion**

Cosmic velocities are fundamental for any space mission:

* **v₁**: for stable orbits,
* **v₂**: for escape trajectories,
* **v₃**: for interstellar travel.

These concepts are essential in mission planning, propulsion design, and the future of human spaceflight.

--
