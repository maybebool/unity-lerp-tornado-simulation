# Unity Lerp Tornado Simulation

![Example](https://github.com/maybebool/Unity-Tornado/blob/main/Recordings/image_001_0000.jpg)
A physics-based tornado simulation in Unity that combines linear interpolation, animated pivot rotation, and rigidbody dynamics to create a convincing vortex effect.

## How It Works

The tornado effect emerges from three layered techniques:

1. **Animated pivot path** — An Animator drives a pivot point along a predefined trajectory
2. **Rotational interpolation** — A modified lerp generates a spiraling path around the moving pivot
3. **Rigidbody interaction** — Objects within the trigger volume are pulled in or pushed out based on their distance to the vortex center

### Force Model

The core mechanic uses a custom **linear interpolation remap** to modulate force direction based on distance. For an object at distance $d$ from the vortex center, the interpolation scalar $t$ is computed as:

$$t = \frac{d - a}{b - a} \cdot (t_b - t_a) + t_a$$

where $a = 0$, $b = 10$, $t_a = 1$, and $t_b = 0$. This produces a value $t \in [0, 1]$ that **decreases** as distance increases — objects closer to the center experience stronger centripetal pull.

### Direction Reversal

A threshold parameter $r$ controls the transition between pull and push behavior:

$$\vec{F} = \begin{cases} \hat{d} \cdot F_c \cdot m \cdot t & \text{if } t \leq r \\[6pt] -\hat{d} \cdot F_c \cdot m \cdot F_p & \text{if } t > r \end{cases}$$

where:
- $\hat{d}$ — unit direction vector from object to vortex center
- $F_c$ — centripetal force strength
- $F_p$ — push (counter) force
- $m$ — force multiplier
- $r$ — reverse indication threshold (default: $0.8$)

This creates a **pull-push cycle**: objects accelerate toward the center, then get ejected outward once they cross the threshold — producing the characteristic spiraling motion of a tornado.

### Parameters

| Parameter | Description | Default |
|---|---|---|
| `centripetalForce` | Inward pull strength | `10` |
| `push` | Outward counter-force | `3` |
| `forceMultiplier` | Global force scalar | `10` |
| `reverseIndicationValue` | Distance threshold for direction flip | `0.8` |

## Controls

- **Play Mode** — Click the white square buttons to spawn objects into the scene
- **Observe** — Watch the rigidbodies interact with the vortex field

## Tech Stack

- **Unity** — Game engine and physics (Rigidbody, Trigger Colliders)
- **C#** — Custom interpolation and force calculations
- **Animator** — Pivot path animation



