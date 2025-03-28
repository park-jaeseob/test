# Introduction
Examples of simulation differences based on different material properties in the O-ring simulation using Abaqus/standard.

## 1. Part
A quarter ring with a central hole, diameter, and thickness of 1 mm. Ring's mesh size set to 0.2. 
Rigid plane to press ring. Distance of two part is 0.1.

<img src="image/Ex01_Oring/Oring_part.png" alt="drawing" width="200"/>

## 2. Boundary Counditions
### 2-1. Symmetry/Antisymmetry/Encastre
Based on the picture above, a fixed value was applied to the quarter ring, and *XSYMM* was applied to the symmetric plane close to the plane and *YSYMM* was applied to the symmetric plane far away.
### 2-2. Displacement/Rotation

| Plane's Displacement/Rotation | Value |
|:------------------------------|:------|
| U1                            |   0   |
| U2                            | -0.25 |
| U3                            |   0   |
| UR1                           |   0   |
| UR2                           |   0   |
| UR3                           |   0   |

## 3. Material Properties
### 3.1 elastic & elasto-plastic

Elastic property value
| Young's Modulus  | Poisson's Ratio |
|:-----------------|:------|
| 210000    |   0.3  |

Plastic property value
| Yield Stress | Plastic Strain |
|:-----------------|:------|
|  |  |
|  |  |
|  |  |

<img src="image/Ex01_Oring/elasticity.gif" width="200">

### 3.2 only plastic

### 3.3 stress strain curve

