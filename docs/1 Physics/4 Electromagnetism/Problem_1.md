# Problem 1

---

# **Simulating Particle Motion Under the Lorentz Force**

## **Motivation**

The **Lorentz force**

$$
\vec{F} = q(\vec{E} + \vec{v} \times \vec{B})
$$

governs the dynamics of charged particles in electromagnetic fields. It underpins technologies such as:

* **Particle accelerators**: where magnetic and electric fields steer and accelerate particles.
* **Mass spectrometers**: using curved trajectories in magnetic fields to separate ions by mass-to-charge ratio.
* **Plasma confinement systems**: like tokamaks, which use magnetic fields to confine high-temperature plasmas.

---

## **1. Exploration of Applications**

### **Key Systems Using the Lorentz Force**

| System                       | Role of Lorentz Force                                                         |
| ---------------------------- | ----------------------------------------------------------------------------- |
| **Cyclotron**                | Magnetic fields induce circular motion; electric fields accelerate particles. |
| **Mass Spectrometer**        | Charged ions deflect differently based on their mass/charge ratio.            |
| **Tokamak (Fusion Reactor)** | Magnetic fields guide and confine hot plasma.                                 |
| **Cathode Ray Tube (CRT)**   | Electric and magnetic fields steer electrons to specific screen positions.    |

### **Role of Electric and Magnetic Fields**

* **Electric Fields ($\vec{E}$)**: Accelerate charged particles in straight lines.
* **Magnetic Fields ($\vec{B}$)**: Cause particles to move in circular or helical paths due to the $\vec{v} \times \vec{B}$ force.
* **Crossed Fields**: Can create **drift motions**, such as **E × B** drift.
---

## **2. Simulating Particle Motion**

We’ll use **Euler’s method** for simplicity, though **Runge-Kutta** is better for accuracy.

```python

import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D

# Constants
q = 1.0       # charge
m = 1.0       # mass
B = np.array([0, 0, 1])  # magnetic field in z-direction
E = np.array([1, 0, 0])  # electric field in x-direction
dt = 0.01
steps = 2000

def lorentz_force(v, E, B):
    return q * (E + np.cross(v, B))

def simulate_motion(v0, r0, E_field, B_field):
    r = np.zeros((steps, 3))
    v = np.zeros((steps, 3))
    r[0], v[0] = r0, v0

    for i in range(1, steps):
        a = lorentz_force(v[i-1], E_field, B_field) / m
        v[i] = v[i-1] + a * dt
        r[i] = r[i-1] + v[i] * dt
    return r

# Plot settings
fig = plt.figure(figsize=(10, 15))

# 1. Circular trajectory
r1 = simulate_motion(v0=[1, 0, 0], r0=[0, 0, 0], E_field=[0, 0, 0], B_field=B)
ax1 = fig.add_subplot(311, projection='3d')
ax1.plot(r1[:,0], r1[:,1], r1[:,2])
ax1.set_title("Circular Trajectory in Magnetic Field")
ax1.set_xlabel("x")
ax1.set_ylabel("y")
ax1.set_zlabel("z")

# 2. E × B Drift
r2 = simulate_motion(v0=[0, 1, 0], r0=[0, 0, 0], E_field=E, B_field=B)
ax2 = fig.add_subplot(312, projection='3d')
ax2.plot(r2[:,0], r2[:,1], r2[:,2])
ax2.set_title("E × B Drift of Charged Particle")
ax2.set_xlabel("x")
ax2.set_ylabel("y")
ax2.set_zlabel("z")

# 3. Helical / Spiral trajectory
r3 = simulate_motion(v0=[1, 0, 0.5], r0=[0, 0, 0], E_field=[0, 0, 0], B_field=B)
ax3 = fig.add_subplot(313, projection='3d')
ax3.plot(r3[:,0], r3[:,1], r3[:,2])
ax3.set_title("Stable Spiral Trajectory in Magnetic Field")
ax3.set_xlabel("x")
ax3.set_ylabel("y")
ax3.set_zlabel("z")

plt.tight_layout()
plt.show()
```
![alt text](image-2.png)


## **4. Visualizing and Interpreting Results**

* **Larmor Radius**:

  $$
  r_L = \frac{mv_{\perp}}{qB}
  $$

  where $v_\perp$ is the velocity component perpendicular to $\vec{B}$.

* **Drift Velocity**:

  $$
  \vec{v}_d = \frac{\vec{E} \times \vec{B}}{B^2}
  $$

### **Summary of Observed Motions**

| Scenario                        | Motion Type | Notable Features                                     |
| ------------------------------- | ----------- | ---------------------------------------------------- |
| Uniform $\vec{B}$, no $\vec{E}$ | Circle      | Larmor radius from speed, B                          |
| Uniform $\vec{B}$, $v_z \neq 0$ | Helix       | Constant z-velocity, circular xy-plane               |
| $\vec{E} \perp \vec{B}$         | Drift       | Straight drift in $\vec{E} \times \vec{B}$ direction |

---

## **5. Real-World Implications**

| Application           | Observed Effect                               | Simulation Parallel            |
| --------------------- | --------------------------------------------- | ------------------------------ |
| **Cyclotron**         | Circular motion with increasing energy        | Uniform B with perpendicular E |
| **Tokamak**           | Charged particle spiraling around field lines | Helical motion                 |
| **Mass Spectrometer** | Radius depends on m/q                         | Larmor radius relation         |

---

## **6. Extensions and Further Exploration**

* **Non-uniform fields**: Vary $B(x, y, z)$ and $E(x, y, z)$.
* **Runge-Kutta methods**: More accurate integration for high-energy or long-duration simulations.
* **Relativistic effects**: Include Lorentz factor for very high speeds.
* **Particle interactions**: Simulate plasma behavior by adding Coulomb forces.

---

## **Conclusion**

This simulation demonstrates how the Lorentz force governs charged particle trajectories in electromagnetic fields. The simple case of a 1 C charge and 1 g mass illustrates:

* Perfect circular motion in magnetic fields.
* Helical motion when there's a velocity component along $\vec{B}$.
* Drift in crossed fields showing real phenomena like **E × B drift**.

---
