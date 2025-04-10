Pipe–Soil Interaction (PSI) Handbook
====================================

.. contents::
   :depth: 2
   :local:

1. Introduction to PSI
-----------------------

- Engineering need for PSI in offshore pipeline design
- Role in buckling, walking, settlement, and fatigue analysis
- PSI as an interface between pipeline mechanics and seabed geomechanics

2. Vertical Resistance
----------------------

2.1 Why it matters in design
    - Pipe embedment and support
    - Settlement and vertical stiffness effects

2.2 Required input parameters
    - Pipe diameter, weight (submerged)
    - Soil undrained shear strength (Su), unit weight, penetration depth
    - Interface roughness or adhesion factor (α)

2.3 Key equations and curves
    - Bearing capacity equation (clay):
      ::
      
        q_ult = N_c * S_u

    - Initial stiffness:
      ::
      
        k_v = q_ult / z_ult

    - Resistance curve shape: bilinear or exponential

2.4 Outputs and interpretation
    - Load–displacement curve: V–z
    - Interpretation: embedment depth, bearing failure, support capacity
    - Use in global FE model: spring element definition

2.5 Worked Example: Vertical Resistance in Soft Clay
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Problem Setup:**

- Undrained shear strength: Su = 5.0  # [kPa]
- Bearing capacity factor: Nc = 10
- Pipe diameter: D = 0.6  # [m]
- Pipe weight (submerged): W_prime = 6.0  # [kN/m]

**Python-style Calculation:**

::

    Su = 5.0         # [kPa]
    Nc = 10
    D = 0.6          # [m]
    W_prime = 6.0    # [kN/m]

    # Step 1: Calculate ultimate bearing capacity
    q_ult = Nc * Su               # [kPa]
    q_ult_kNm = q_ult * D         # [kN/m]

    # Step 2: Estimate embedment depth
    z_ult = q_ult_kNm / W_prime

    print(f"q_ult = {q_ult_kNm:.2f} kN/m")
    print(f"Estimated embedment depth = {z_ult:.2f} m")

**Result:**
- Ultimate vertical resistance: 30.0 kN/m
- Estimated embedment depth: 5.0 m

**Graph (Load–Displacement Curve):**

.. code-block:: python

    import matplotlib.pyplot as plt
    import numpy as np

    z = np.linspace(0, 6, 100)  # displacement [m]
    q_ult = 30.0  # [kN/m]
    z_ult = 5.0

    V = q_ult * (z / z_ult)  # linear up to z_ult
    V[z > z_ult] = q_ult     # perfectly plastic after peak

    plt.figure(figsize=(6,4))
    plt.plot(z, V, label='V–z curve')
    plt.xlabel("Embedment depth z [m]")
    plt.ylabel("Vertical Resistance V [kN/m]")
    plt.title("Vertical Load–Displacement Curve")
    plt.grid(True)
    plt.legend()
    plt.tight_layout()
    plt.show()

3. Lateral Resistance
---------------------

3.1 Why it matters in design
    - Thermal expansion → lateral buckling
    - On-bottom stability checks

3.2 Required input parameters
    - Peak undrained shear strength (S_u)
    - Trench geometry, burial depth (H)
    - Friction factors or lateral resistance factor (p_y)

3.3 Key equations and curves
    - Peak resistance (clay):
      ::
      
        H_peak = N_p * S_u * D

    - Initial stiffness:
      ::
      
        k_h = H_peak / y_peak

    - p–y curve structure and DNVGL-RP-F114 recommendations

3.4 Outputs and interpretation
    - Displacement threshold for mobilizing full resistance
    - Use in lateral buckling analysis
    - Application in ORCAFLEX/ABAQUS: spring or nonlinear link element

4. Axial Resistance
-------------------

4.1 Why it matters in design
    - Pipeline walking due to thermal cycles
    - Axial friction effects on expansion anchors

4.2 Required input parameters
    - Friction factor (μ), contact area (A)
    - Soil type, consolidation time

4.3 Key equations and curves
    - Axial friction force:
      ::
      
        T = μ * N = μ * (W' + suction force)

    - Residual vs. peak distinction in clay
    - Rate effects and time-dependent resistance growth

4.4 Outputs and interpretation
    - T–u curve (axial force vs displacement)
    - Mobilization length and accumulated walking
    - How to input in global model

5. PSI Curve Implementation
---------------------------

5.1 Curve generation methods
    - Empirical approach using DNVGL-RP-F114
    - Laboratory testing (T-bar, CPT, box tests)
    - Numerical simulation (2D/3D FE)

5.2 Curve formatting and application
    - Input for FE software (ABAQUS/ORCAFLEX/PLAXIS)
    - Tabular or functional format
    - Guidelines for element assignment

