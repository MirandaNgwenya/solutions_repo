# Problem 1

Here is a solution for the four tasks based on the provided guidelines. I will provide a comprehensive explanation of each task, followed by a Python script implementation. The solution will be presented in the context of a Markdown document. Below is the full response, ready to be used in your project:

---

# Investigating the Range as a Function of the Angle of Projection

### Motivation:

Projectile motion is a classic topic in physics that explores the trajectory of an object launched into the air. The relationship between the launch angle and range, combined with parameters like initial velocity and gravity, is fundamental to various real-world scenarios. From sports to space exploration, understanding this relationship has wide-ranging applications.

---

## 1. Theoretical Foundation

### 1.1 Governing Equations of Motion

The fundamental equations governing projectile motion come from Newton's laws of motion, applied to the x- and y-axes separately. We start by assuming a projectile is launched from ground level (or a given height) with an initial velocity $v_0$ at an angle $\theta$.

* **Horizontal motion (x-direction)**:

  * The horizontal velocity remains constant throughout the flight since there's no horizontal acceleration (ignoring air resistance).

  $$
  x(t) = v_0 \cos(\theta) \cdot t
  $$

* **Vertical motion (y-direction)**:

  * The vertical motion is influenced by gravity, which causes a constant downward acceleration $g$.

  $$
  y(t) = v_0 \sin(\theta) \cdot t - \frac{1}{2} g t^2
  $$

The **time of flight** $T$ (the time when the projectile hits the ground) is obtained by solving for $t$ when $y(t) = 0$. This gives:

$$
T = \frac{2 v_0 \sin(\theta)}{g}
$$

* **Range (R)**:
  The horizontal range is the distance the projectile travels in the x-direction during the time of flight:

  $$
  R = v_0 \cos(\theta) \cdot T = \frac{v_0^2 \sin(2\theta)}{g}
  $$

### 1.2 Family of Solutions

The general solution to the equations of motion can be adjusted for different initial conditions (initial velocity $v_0$, gravitational acceleration $g$, and launch height $h$). For a launch from a height $h$, the trajectory equation is modified to:

$$
y(t) = v_0 \sin(\theta) \cdot t - \frac{1}{2} g t^2 + h
$$

To solve for the time of flight and the range in this case, you would solve the quadratic equation for $t$ when $y(t) = 0$, then calculate the range as before.

---

## 2. Analysis of the Range

### 2.1 Dependence of the Range on the Launch Angle

The range equation $R = \frac{v_0^2 \sin(2\theta)}{g}$ shows that:

* **Maximum range occurs at $\theta = 45^\circ$**: This angle maximizes $\sin(2\theta)$, giving the longest possible range for a given initial velocity and gravitational acceleration.
* **Symmetry**: $R(\theta) = R(90^\circ - \theta)$, meaning that the range is the same for angles symmetric around $45^\circ$.

### 2.2 Influence of Other Parameters

* **Initial velocity $v_0$**: The range is proportional to the square of the initial velocity. Doubling the initial velocity results in a fourfold increase in range.
* **Gravitational acceleration $g$**: The range is inversely proportional to $g$. In a lower-gravity environment (like on the Moon), the range would be larger for the same initial velocity and launch angle.
* **Launch height $h$**: A higher launch height increases the range since the projectile spends more time in the air.

---

## 3. Practical Applications

This model can be adapted to real-world situations with some modifications:

1. **Uneven Terrain**: If the projectile is launched from a hill or mountain, the height $h$ should be adjusted to reflect this elevation. The range equation can be adjusted by incorporating the effect of the slope of the terrain.

2. **Air Resistance (Drag)**: In real-world conditions, air resistance plays a significant role, especially at high velocities. The drag force is proportional to the square of the velocity, and it requires solving differential equations numerically. This makes the problem more complex but can be simulated using numerical methods like Euler’s method or Runge-Kutta methods.

3. **Wind**: Wind can affect the horizontal motion of the projectile, introducing a horizontal force component. To model this, you would include wind velocity in the equations of motion.

4. **Rocket Launches**: In the case of a rocket, variable acceleration and changing mass (due to fuel consumption) need to be considered.

---

## 4. Implementation

### Python Code Implementation

```python
import numpy as np
import matplotlib.pyplot as plt

# Constants
g = 9.81  # gravity (m/s^2)

def compute_range(v0, theta_deg, h=0):
    """
    Compute the range of a projectile.
    
    Parameters:
    - v0: Initial velocity (m/s)
    - theta_deg: Launch angle (degrees)
    - h: Initial height (meters), default is 0 for ground-level launches
    
    Returns:
    - Range of the projectile (meters)
    """
    theta = np.radians(theta_deg)
    if h == 0:
        # Simple case: launch and landing at the same height
        return (v0 ** 2) * np.sin(2 * theta) / g
    else:
        # Launch from an elevated height
        vy = v0 * np.sin(theta)
        vx = v0 * np.cos(theta)
        t_flight = (vy + np.sqrt(vy**2 + 2 * g * h)) / g
        return vx * t_flight

# Parameters
v0_list = [10, 20, 30]  # Initial velocities in m/s
angles = np.linspace(0, 90, 500)  # Launch angles from 0 to 90 degrees
height = 0  # Initial height (can be adjusted)

# Create plot
plt.figure(figsize=(10, 6))

# Plot range vs angle for each initial velocity
for v0 in v0_list:
    ranges = [compute_range(v0, angle, h=height) for angle in angles]
    plt.plot(angles, ranges, label=f"v₀ = {v0} m/s")

# Customize plot
plt.title("Projectile Range vs Launch Angle")
plt.xlabel("Launch Angle (degrees)")
plt.ylabel("Range (meters)")
plt.legend()
plt.grid(True)

# Show the plot
plt.show()

# Example with elevated launch (height = 5 meters)
height = 5  # Set initial height to 5 meters
plt.figure(figsize=(10, 6))

# Plot range vs angle for each initial velocity with height
for v0 in v0_list:
    ranges = [compute_range(v0, angle, h=height) for angle in angles]
    plt.plot(angles, ranges, label=f"v₀ = {v0} m/s, h = {height} m")

# Customize plot
plt.title(f"Projectile Range vs Launch Angle (With Elevation {height} m)")
plt.xlabel("Launch Angle (degrees)")
plt.ylabel("Range (meters)")
plt.grid(True)
plt.legend()

# Show the plot
plt.show()
```

### Description of the Python Code:

1. **Function `compute_range`**: This function calculates the range of a projectile given the initial velocity, launch angle, and height. It supports both ground-level launches and launches from an elevated position.
2. **Plotting**: The script generates two sets of plots:

   * The first set compares the ranges for different initial velocities at ground level.
   * The second set compares the ranges when the projectile is launched from a height of 5 meters.


![alt text](image.png)

![alt text](image-1.png)

### Limitations of the Model:

* **Idealized Assumptions**: This model assumes no air resistance and constant gravitational acceleration.
* **Uniform Terrain**: The model assumes the terrain is flat; launching from a slope requires modifications.
* **No Wind**: Wind effects are ignored, which is not realistic in many real-world applications.

### Realistic Extensions:

* To model air resistance, drag coefficients must be included in the equations.
* Wind speed and direction would need to be modeled as external forces affecting the horizontal velocity.

---

### Conclusion

In this project, we investigated how the range of a projectile depends on its launch angle and initial conditions. The theoretical model highlighted the interplay between initial velocity, gravitational acceleration, and launch height. We implemented a Python simulation to visualize the range versus launch angle for various initial velocities. While the model is idealized, real-world factors such as air resistance, wind, and terrain can further influence the projectile's trajectory, leading to more complex simulations.

---

## Colab

Visit my colab: [link](https://colab.research.google.com/drive/1QJXaB8GarSTX-KdqEcIiMGUPimhgvGS3?usp=sharing)
