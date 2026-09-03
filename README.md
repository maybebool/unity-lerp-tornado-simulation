# Unity Lerp Tornado Simulation

<p align="center">
  <img src="Recordings/image_001_0000.jpg" alt="Tornado Simulation" height="500">
</p>

[![Unity](https://img.shields.io/badge/Unity-2022.3+-000000?style=flat-square&logo=unity&logoColor=white)](https://unity.com/)
[![C#](https://img.shields.io/badge/C%23-Rigidbody_Physics-512BD4?style=flat-square&logo=dotnet&logoColor=white)](https://dotnet.microsoft.com/)
[![Physics](https://img.shields.io/badge/Physics-Vortex_Dynamics-FF6F61?style=flat-square)](https://docs.unity3d.com/Manual/PhysicsSection.html)
[![Lerp](https://img.shields.io/badge/Lerp-Interpolation_Remap-77B829?style=flat-square)](https://docs.unity3d.com/ScriptReference/Vector3.Lerp.html)

A physics-based tornado simulation combining linear interpolation remapping, animated pivot rotation, and rigidbody dynamics to create a convincing vortex with pull-push force cycling.

## Objective

Implement a tornado effect built entirely from first-principles physics, no particle systems or pre-baked animations. Objects within the vortex volume experience distance-dependent forces computed via a custom interpolation remap, producing a natural spiraling motion where objects are pulled toward the centre, then ejected outward past a configurable threshold.

## Methodology

The tornado effect emerges from three layered techniques working together:

**1. Animated Pivot Path.** An Animator drives a pivot point along a predefined trajectory, defining the tornado's movement through the scene.

**2. Rotational Interpolation.** A modified lerp generates a spiraling path around the moving pivot, creating the characteristic funnel geometry.

**3. Rigidbody Interaction.** Objects within the trigger volume are pulled in or pushed out based on their distance to the vortex centre, using the force model described below.

## Force Model

### Distance-Based Interpolation

For an object at distance $d$ from the vortex centre, the interpolation scalar $t$ is computed via a linear remap:

$$t = \frac{d - a}{b - a} \cdot (t_b - t_a) + t_a$$

where $a = 0$, $b = 10$, $t_a = 1$, $t_b = 0$. This produces $t \in [0, 1]$ that decreases with distance; objects closer to the centre experience stronger centripetal pull.

### Direction Reversal

A threshold parameter $r$ controls the transition between pull and push behaviour:

$$\vec{F} = \begin{cases} \hat{d} \cdot F_c \cdot m \cdot t & \text{if } t \leq r \\ -\hat{d} \cdot F_c \cdot m \cdot F_p & \text{if } t > r \end{cases}$$

| Symbol | Meaning |
|:---|:---|
| $\hat{d}$ | Unit direction vector from object to vortex centre |
| $F_c$ | Centripetal force strength |
| $F_p$ | Push (counter) force |
| $m$ | Force multiplier |
| $r$ | Reverse indication threshold (default: 0.8) |

This creates a pull-push cycle: objects accelerate toward the centre, then get ejected outward once they cross the threshold, producing the characteristic spiraling motion of a tornado.

## Parameters

| Parameter | Description | Default |
|:---|:---|:---:|
| `centripetalForce` | Inward pull strength | `10` |
| `push` | Outward counter-force | `3` |
| `forceMultiplier` | Global force scalar | `10` |
| `reverseIndicationValue` | Distance threshold for direction flip | `0.8` |

## Controls

| Action | Input |
|:---|:---|
| Spawn objects | Click white square buttons in Play Mode |
| Observe | Watch rigidbodies interact with the vortex field |


## Getting Started

```bash
git clone https://github.com/maybebool/Unity-Tornado.git
```

1. Open the project in Unity 2022.3+.
2. Open the Tornado scene.
3. Press Play and click the spawn buttons to add objects into the vortex.

**Prerequisites:** Unity 2022.3+.

## Tech Stack

[![Unity](https://img.shields.io/badge/Unity-000000?style=flat-square&logo=unity&logoColor=white)](https://unity.com/)
[![.NET](https://img.shields.io/badge/.NET-512BD4?style=flat-square&logo=dotnet&logoColor=white)](https://dotnet.microsoft.com/)

| Category | Technology |
|:---|:---|
| Engine | Unity 2022.3+ (Rigidbody, Trigger Colliders) |
| Language | C# — custom interpolation and force calculations |
| Animation | Unity Animator - pivot path trajectory |
| Physics | Rigidbody force application, trigger volume detection |

## Limitations & Future Work

The current simulation uses a simplified radial force model. Possible extensions include:

- Vertical lift component for upward funnel motion
- Turbulence noise layered onto the base force for less uniform trajectories
- Debris fragmentation on collision with the ground plane
- Wind field visualisation using GPU particle trails
- Multiple simultaneous vortices with interaction forces
- Audio integration with wind intensity mapped to vortex proximity
