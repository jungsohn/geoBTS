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

**Problem Setup:**

- Undrained shear strength: Su = 5.0  # [kPa]
- Bearing capacity factor: Nc = 10
- Pipe diameter: D = 0.6  # [m]
- Submerged pipe weight: W_prime = 6.0  # [kN/m]

**Python-style Calculation:**

::

    def calculate_vertical_resistance(Su, Nc, D, W_prime):
        """
        Calculate ultimate vertical resistance and estimated embedment depth.
        Su        : Undrained shear strength [kPa]
        Nc        : Bearing capacity factor (typically 10 for clay)
        D         : Pipe diameter [m]
        W_prime   : Submerged pipe weight [kN/m]
        """
        q_ult_kPa = Nc * Su
        q_ult_kNm = q_ult_kPa * D  # Convert to kN/m
        z_ult = q_ult_kNm / W_prime  # Estimated embedment depth

        print(f"Ultimate vertical resistance q_ult = {q_ult_kNm:.2f} kN/m")
        print(f"Estimated embedment depth z_ult = {z_ult:.2f} m")
        return q_ult_kNm, z_ult

    # Example input
    Su = 5.0         # [kPa]
    Nc = 10
    D = 0.6          # [m]
    W_prime = 6.0    # [kN/m]

    q_ult, z_ult = calculate_vertical_resistance(Su, Nc, D, W_prime)

**Result:**
- Ultimate vertical resistance: 30.0 kN/m
- Estimated embedment depth: 5.0 m

**Graph (Load–Displacement Curve):**

.. code-block:: python

    import matplotlib.pyplot as plt
    import numpy as np

    def plot_vz_curve(q_ult, z_ult):
        """
        Plot V–z curve for vertical resistance.
        q_ult : Ultimate vertical resistance [kN/m]
        z_ult : Displacement at full mobilization [m]
        """
        z = np.linspace(0, z_ult * 1.5, 100)
        V = np.minimum(q_ult, q_ult * (z / z_ult))

        plt.figure(figsize=(6, 4))
        plt.plot(z, V, label="V–z curve")
        plt.axhline(q_ult, color='gray', linestyle='--', linewidth=0.8)
        plt.xlabel("Embedment depth z [m]")
        plt.ylabel("Vertical Resistance V [kN/m]")
        plt.title("Vertical Load–Displacement Curve")
        plt.grid(True)
        plt.legend()
        plt.tight_layout()
        plt.show()

    # Plot the curve
    plot_vz_curve(q_ult, z_ult)



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

3.5 Worked Example: Lateral Resistance in Soft Clay

**Problem Setup:**

- Undrained shear strength: Su = 5.0  # [kPa]
- Lateral resistance factor: Np = 5.0
- Pipe diameter: D = 0.6  # [m]
- Peak displacement (y_peak): 0.1  # [m]

**Python-style Calculation:**

::

    def calculate_lateral_resistance(Su, Np, D, y_peak):
        """
        Calculate peak lateral resistance and initial stiffness.
        Su      : Undrained shear strength [kPa]
        Np      : Lateral bearing capacity factor
        D       : Pipe diameter [m]
        y_peak  : Displacement at peak resistance [m]
        """
        H_peak = Np * Su * D  # Peak lateral resistance [kN/m]
        k_h = H_peak / y_peak  # Initial stiffness [kN/m²]

        print(f"Peak lateral resistance H_peak = {H_peak:.2f} kN/m")
        print(f"Initial lateral stiffness k_h = {k_h:.2f} kN/m²")
        return H_peak, k_h

    # Example input
    Su = 5.0         # [kPa]
    Np = 5.0
    D = 0.6          # [m]
    y_peak = 0.1     # [m]

    H_peak, k_h = calculate_lateral_resistance(Su, Np, D, y_peak)

**Result:**
- Peak lateral resistance: 15.0 kN/m
- Initial stiffness: 150.0 kN/m²

**Graph (Lateral Load–Displacement Curve):**

.. code-block:: python

    import matplotlib.pyplot as plt
    import numpy as np

    def plot_py_curve(H_peak, y_peak):
        """
        Plot p–y curve for lateral resistance.
        H_peak : Peak lateral resistance [kN/m]
        y_peak : Displacement at H_peak [m]
        """
        y = np.linspace(0, y_peak * 2, 100)
        H = np.minimum(H_peak, (H_peak / y_peak) * y)

        plt.figure(figsize=(6, 4))
        plt.plot(y, H, label="p–y curve")
        plt.axhline(H_peak, color='gray', linestyle='--', linewidth=0.8)
        plt.xlabel("Lateral displacement y [m]")
        plt.ylabel("Lateral resistance H [kN/m]")
        plt.title("Lateral Load–Displacement (p–y) Curve")
        plt.grid(True)
        plt.legend()
        plt.tight_layout()
        plt.show()

    # Plot the curve
    plot_py_curve(H_peak, y_peak)



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

4.5 Worked Example: Axial Resistance in Soft Clay

**Problem Setup:**

- Submerged pipe weight: W_prime = 5.0  # [kN/m]
- Suction resistance: suction = 1.5  # [kN/m]
- Friction factor (μ): 0.3
- Peak displacement (u_peak): 0.05  # [m]

**Python-style Calculation:**

::

    def calculate_axial_resistance(W_prime, suction, mu, u_peak):
        """
        Calculate axial resistance and initial stiffness.
        W_prime : Submerged weight of pipe [kN/m]
        suction : Suction resistance from clay [kN/m]
        mu      : Axial friction factor
        u_peak  : Displacement at full mobilization [m]
        """
        T_peak = mu * (W_prime + suction)
        k_t = T_peak / u_peak

        print(f"Peak axial resistance T_peak = {T_peak:.2f} kN/m")
        print(f"Initial axial stiffness k_t = {k_t:.2f} kN/m²")
        return T_peak, k_t

    # Example input
    W_prime = 5.0      # [kN/m]
    suction = 1.5      # [kN/m]
    mu = 0.3
    u_peak = 0.05      # [m]

    T_peak, k_t = calculate_axial_resistance(W_prime, suction, mu, u_peak)

**Result:**
- Peak axial resistance: 1.95 kN/m
- Initial axial stiffness: 39.00 kN/m²

**Graph (Axial Load–Displacement Curve):**

.. code-block:: python

    import matplotlib.pyplot as plt
    import numpy as np

    def plot_tu_curve(T_peak, u_peak):
        """
        Plot T–u curve for axial resistance.
        T_peak : Peak axial resistance [kN/m]
        u_peak : Displacement at T_peak [m]
        """
        u = np.linspace(0, u_peak * 2, 100)
        T = np.minimum(T_peak, (T_peak / u_peak) * u)

        plt.figure(figsize=(6, 4))
        plt.plot(u, T, label="T–u curve")
        plt.axhline(T_peak, color='gray', linestyle='--', linewidth=0.8)
        plt.xlabel("Axial displacement u [m]")
        plt.ylabel("Axial resistance T [kN/m]")
        plt.title("Axial Load–Displacement (T–u) Curve")
        plt.grid(True)
        plt.legend()
        plt.tight_layout()
        plt.show()

    # Plot the curve
    plot_tu_curve(T_peak, u_peak)



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

