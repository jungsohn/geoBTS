Pipe–Soil Interaction (PSI) Handbook
====================================

.. contents::
   :depth: 3

1. Introduction to PSI
-----------------------

Pipe–soil interaction (PSI) plays a critical role in offshore pipeline design and analysis.  
It governs how the seabed soil reacts to the vertical, lateral, and axial movements of a pipeline,  
and is directly linked to problems such as buckling, walking, settlement, and fatigue.

PSI serves as the key interface between structural pipeline behavior and geotechnical soil response,  
making it a shared concern between subsea engineers and offshore geotechnical engineers.

1.1 Purpose:

This handbook provides a structured guide to understanding and applying
pipe–soil interaction (PSI) concepts in offshore engineering. It focuses on
practical methods for estimating PSI resistance using analytical and empirical
approaches, and helps engineers prepare input curves and assess design conditions
before engaging in numerical modeling.

2.1 Scope:

The handbook covers:
- Directional PSI behavior: vertical, lateral, axial
- Construction of PSI resistance curves (V–z, H–y, T–u)
- Advanced effects: cyclic degradation, stiffness variation, embedment
- Simple Python-style calculations for real-world usage


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


5. PSI Curve Construction using Analytical and Empirical Methods
------------------------------------------------------------------

Note:
This section summarizes the empirical and analytical methods used to construct PSI curves
across all directions (vertical, lateral, axial). While some equations are introduced in earlier sections,
this chapter provides a unified approach for curve generation before numerical modeling.

5.1 Vertical Resistance Curve (V–z)
.....................................

Based on undrained clay behavior (DNVGL-RP-F114):

::

    def generate_vertical_curve(Su, Nc, D, z_max=1.0, n_points=100):
        """
        Generate V–z curve for vertical pipe-soil interaction in clay.
        Su      : Undrained shear strength [kPa]
        Nc      : Bearing capacity factor (~10)
        D       : Pipe diameter [m]
        z_max   : Max displacement for curve [m]
        """
        import numpy as np

        z = np.linspace(0, z_max, n_points)
        q_ult = Nc * Su                     # [kPa]
        V_peak = q_ult * D                  # [kN/m]
        z_peak = 0.5 * z_max                # Assume peak occurs at 50%

        V = np.minimum(V_peak, V_peak * (z / z_peak))  # bilinear
        return z, V

**Example parameters:**
- Su = 5.0 kPa
- Nc = 10
- D = 0.6 m

::

    z, V = generate_vertical_curve(5.0, 10, 0.6)

5.2 Lateral Resistance Curve (H–y)
.....................................

Using empirical formulation:

::

    def generate_lateral_curve(Su, Np, D, y_max=0.2, n_points=100):
        """
        Generate H–y (lateral resistance) curve for soft clay.
        Su      : Undrained shear strength [kPa]
        Np      : Lateral resistance factor (~5–10)
        D       : Pipe diameter [m]
        y_max   : Max lateral displacement [m]
        """
        import numpy as np

        y = np.linspace(0, y_max, n_points)
        H_peak = Np * Su * D                # [kN/m]
        y_peak = 0.1 * D                    # empirical rule

        H = np.minimum(H_peak, H_peak * (y / y_peak))
        return y, H

**Example parameters:**
- Su = 5.0 kPa
- Np = 6.0
- D = 0.6 m

::

    y, H = generate_lateral_curve(5.0, 6.0, 0.6)

5.3 Axial Resistance Curve (T–u)
.....................................

Using interface friction model:

::

    def generate_axial_curve(W_prime, suction, mu, u_max=0.1, n_points=100):
        """
        Generate T–u (axial resistance) curve.
        W_prime : Submerged pipe weight [kN/m]
        suction : Suction resistance [kN/m]
        mu      : Friction factor (0.2–0.4 for clay)
        u_max   : Max axial displacement [m]
        """
        import numpy as np

        u = np.linspace(0, u_max, n_points)
        T_peak = mu * (W_prime + suction)
        u_peak = 0.05                       # [m] empirical

        T = np.minimum(T_peak, T_peak * (u / u_peak))
        return u, T

**Example parameters:**
- W' = 5.0 kN/m
- suction = 1.5 kN/m
- mu = 0.3

::

    u, T = generate_axial_curve(5.0, 1.5, 0.3)

5.4 Visualizing All PSI Curves (Optional)
.....................................

::

    import matplotlib.pyplot as plt

    def plot_psi_curve(x, y, xlabel, ylabel, title):
        plt.figure(figsize=(6, 4))
        plt.plot(x, y, label=title)
        plt.xlabel(xlabel)
        plt.ylabel(ylabel)
        plt.title(title)
        plt.grid(True)
        plt.legend()
        plt.tight_layout()
        plt.show()

    # Example usage:
    z, V = generate_vertical_curve(5.0, 10, 0.6)
    plot_psi_curve(z, V, "Displacement z [m]", "Vertical Resistance V [kN/m]", "Vertical V–z Curve")


6. Advanced PSI Considerations
---------------------------------

Note:
This section extends the concepts introduced in Sections 2–5 to include advanced and field-calibrated
behaviors observed in cyclic loading, stiffness changes, and pipeline-specific conditions. These are
critical in real-world projects, especially in long-term performance, installation design, and risk assessment.

6.1 Cyclic Axial Resistance
.....................................
- Undrained to drained transition
- Residual vs. peak friction (τ_max → τ_res)
- Use in walking models (thermal and pressure cycling)
- Factors: clay consolidation, cycle count, rate effects

6.2 Cyclic Lateral Resistance
.....................................
- Resistance degradation with repeated lateral movements
- Application to thermal buckling, dynamic response
- Recommended H_deg/H_peak ratios from DNVGL or tests

6.3 Static Vertical Stiffness
.....................................
- Definition: initial slope of V–z curve
- Used in spring stiffness in global models
- Empirical estimation from V_peak and z_peak

6.4 Light vs. Heavy Pipe Behavior
.....................................
- **Light pipe**: on-bottom without self-embedment → low resistance
- **Heavy pipe**: penetrates seabed due to weight → deeper embedment, higher resistance
- Design implication: different p–y, V–z, T–u curves

Reference:
- DNVGL-RP-F114 Appendix A
- DNVGL-RP-F109 Sec. 3.4

Python-style Calculation:

::

    def classify_pipe_behavior(W_prime, Su, D):
        """
        Classify pipeline as 'light' or 'heavy' based on normalized bearing pressure.

        References:
        - DNVGL-RP-F114 Appendix A
        - DNVGL-RP-F109 Sec. 3.4

        Parameters:
        - W_prime : Submerged weight of pipe [kN/m]
        - Su      : Undrained shear strength of seabed [kPa]
        - D       : Pipe diameter [m]

        Returns:
        - Classification string: 'Light Pipe – likely on-bottom' or 'Heavy Pipe – likely embedded'
        """

        bearing_pressure = W_prime / D                # [kPa]
        normalized_pressure = bearing_pressure / Su

        print(f"Normalized bearing pressure = {normalized_pressure:.2f}")

        if normalized_pressure < 2:
            return "Light Pipe – likely on-bottom"
        else:
            return "Heavy Pipe – likely embedded"

# Example usage:

    behavior = classify_pipe_behavior(W_prime=5.0, Su=2.5, D=0.6)
    print(behavior)


6.5 On-Bottom Stability
.....................................
- Purpose: prevent lateral sliding due to current, waves
- Governed by axial and lateral friction
- Stability checks based on DNVGL-RP-F109

Reference:
- DNVGL-RP-F109: On-bottom stability design
- DNVGL-RP-F114: Axial friction estimation

Python-style Calculation:

::

    def check_on_bottom_stability(W_prime, mu, hydrodynamic_force):
        """
        Check whether a pipeline remains stable on the seabed under lateral hydrodynamic loading.

        References:
        - DNVGL-RP-F109 Sec. 3.4
        - DNVGL-RP-F114 Sec. 4.2

        Parameters:
        - W_prime           : Submerged weight of pipe [kN/m]
        - mu                : Axial friction factor (typ. 0.2–0.6 for clay)
        - hydrodynamic_force: Expected lateral or uplift load from waves/currents [kN/m]

        Returns:
        - Stability assessment: 'Stable' or 'Unstable – pipe may slide'
        """

        T_resist = mu * W_prime
        print(f"Axial resistance = {T_resist:.2f} kN/m")
        print(f"External hydrodynamic force = {hydrodynamic_force:.2f} kN/m")

        if T_resist >= hydrodynamic_force:
            return "Stable"
        else:
            return "Unstable – pipe may slide"

# Example usage:

    result = check_on_bottom_stability(W_prime=6.0, mu=0.4, hydrodynamic_force=2.0)
    print(result)



6.6 Free Spanning Conditions
.....................................
- Gaps under pipe due to seabed irregularities or scouring
- Risks: vortex-induced vibrations (VIV), fatigue, local buckling
- PSI influence: support loss → local stiffness change

6.7 Pipeline Embedment
.....................................
- Factors: pipe weight, soil strength, installation method (lay tension)
- Influence on lateral and vertical resistance curves
- Empirical prediction methods (DNVGL-RP-F114 Table A-3)

Reference:
- DNVGL-RP-F114 Table A-3
- Empirical embedment estimation for soft clay

Python-style Calculation:

::

    def estimate_embedment(W_prime, Su, D):
        """
        Estimate pipeline embedment depth based on normalized pressure using empirical method.

        Reference:
        - DNVGL-RP-F114 Table A-3 (embedment depth based on W'/Su ratio)

        Parameters:
        - W_prime : Submerged weight of pipe [kN/m]
        - Su      : Undrained shear strength [kPa]
        - D       : Pipe diameter [m]

        Returns:
        - Estimated embedment depth [m]
        """

        normalized_pressure = (W_prime / D) / Su  # Dimensionless

        if normalized_pressure < 2:
            embedment = 0.05 * D
        elif normalized_pressure < 5:
            embedment = 0.1 * D
        else:
            embedment = 0.2 * D

        print(f"Normalized pressure = {normalized_pressure:.2f}")
        print(f"Estimated embedment depth = {embedment:.3f} m")
        return embedment

# Example usage:

    z = estimate_embedment(W_prime=6.0, Su=5.0, D=0.6)
    print(f"Embedment depth = {z:.3f} m")


6.8 Summary Table of Advanced Conditions
...........................................

+----------------------+-------------------------------------------+
| Condition            | Affects                                   |
+----------------------+-------------------------------------------+
| Cyclic axial loads   | T–u curve degradation (walking risk)     |
| Cyclic lateral loads | H–y degradation (buckling, stability)    |
| Pipe self-weight     | Embedment depth (affects all PSI curves) |
| Seabed roughness     | Free spans, contact loss                 |
+----------------------+-------------------------------------------+


6.9 Effect of Soil Consolidation
.................................

Residual axial resistance is known to increase with overconsolidation of the clay seabed.  
The following empirical model can be used to estimate residual axial force:

::

    def compute_residual_axial_resistance(Su_res, sigma_n_eff, OCR, zeta=1.0, m=0.5):
        """
        Calculate the normalized residual axial resistance considering soil consolidation.

        References:
        - Randolph & White (2008)
        - DNVGL-RP-F114 Appendix B

        Parameters:
        - Su_res        : Residual interface shear strength [kPa]
        - sigma_n_eff   : Initial effective normal stress [kPa]
        - OCR           : Overconsolidation ratio (typically >1 for stiff clay)
        - zeta, m       : Empirical coefficients

        Returns:
        - Normalized residual axial resistance F_Ares / V
        """

        ratio = Su_res / sigma_n_eff
        normalized_resistance = zeta * (ratio) * (OCR ** m)
        return normalized_resistance

# Example usage:

    FAres_over_V = compute_residual_axial_resistance(Su_res=3.0, sigma_n_eff=5.0, OCR=4.0)
    print(f"Normalized F_Ares/V = {FAres_over_V:.3f}")

6.10 Time-Dependent Axial Resistance (Hardening Model)
.......................................................

- Consolidation-driven interface hardening model
- Gradual increase from undrained to drained strength over time
- Model suitable for walking analysis and clay seabeds


Reference:
- Randolph & White (2008)
- DNVGL-RP-F114 Appendix B

Python-style Calculation:

::

    import numpy as np

    def compute_time_dependent_axial_resistance(FA_res_u, FA_res_d, t, t50, n):
        """
        Compute the normalized axial resistance as a function of time.

        Parameters:
        - FA_res_u : Initial undrained normalized axial resistance (F/V)
        - FA_res_d : Final drained normalized axial resistance (F/V)
        - t        : Time since installation [same unit as t50]
        - t50      : Time to reach 50% strength gain
        - n        : Shape parameter (typically 1–2)

        Reference:
        - Based on Randolph & White (2008), time-hardening interface model
        """

        exponent = -np.log(2) * (t / t50)**n
        FA_res_t = FA_res_d - (FA_res_d - FA_res_u) * np.exp(exponent)
        return FA_res_t

# Example usage:

    FA_res = compute_time_dependent_axial_resistance(
        FA_res_u=0.2, FA_res_d=0.6, t=24, t50=12, n=1.5)
    print(f"Normalized F_Ares(t)/V = {FA_res:.3f}")

import matplotlib.pyplot as plt

t = np.linspace(0, 48, 100)
resistances = [compute_time_dependent_axial_resistance(0.2, 0.6, ti, 12, 1.5) for ti in t]

plt.plot(t, resistances)
plt.xlabel("Time (hours)")
plt.ylabel("Normalized Axial Resistance (F/V)")
plt.title("Time-dependent Resistance Hardening")
plt.grid(True)
plt.tight_layout()
plt.show()


6.10.1 Estimating t₅₀ from Consolidation Coefficient
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Why is `c_v` used in cyclic axial resistance models?

**(KR)**  
파이프가 축 방향으로 반복해서 앞뒤로 움직이는 경우(예: 열팽창, 수축),  
점토 지반과의 인터페이스 전단 저항력이 매 싸이클 후 약해지고,  
싸이클 사이의 시간 동안에는 흙이 점차 배수(consolidation)되면서  
저항력이 회복되는 현상이 발생한다.

이때 그 회복 속도를 지배하는 핵심 파라미터가 바로 **압밀 계수 `c_v`** 이다.  
즉, `c_v` 값이 클수록 흙의 배수 속도가 빠르고, 파이프가 더 빨리 저항력을 회복한다.

**(EN)**  
In cyclic axial loading scenarios (e.g., thermal expansion/contraction),  
the interface shear resistance between pipeline and seabed degrades during loading  
but gradually recovers during the rest periods between cycles due to consolidation of the clay.  

The rate of this recovery is governed by the **coefficient of consolidation `c_v`**,  
which controls how fast excess pore pressure dissipates.  
Higher `c_v` means faster recovery of axial resistance.

---

Time to 50% recovery (`t₅₀`) can be estimated from `c_v` using the 1D consolidation equation:

::

    def compute_t50_from_cv(cv, H):
        """
        Estimate t50 from coefficient of consolidation and drainage length.

        Reference:
        - Terzaghi 1D consolidation theory
        - DNVGL-RP-F114 Appendix B
        """
        import numpy as np
        return H**2 / (np.pi**2 * cv)

# Example usage:

    cv = 5e-8  # m²/s
    H = 0.1    # m
    t50 = compute_t50_from_cv(cv, H)
    print(f"t50 = {t50:.1f} s")

This value of `t₅₀` can then be used in the time-hardening axial resistance model.


6.10.2 Example: Axial Resistance Recovery During Cyclic Rest Periods
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This example shows how the axial resistance recovers during rest periods between cyclic loading events.
The recovery is governed by the time-hardening model using `t₅₀`, which itself depends on the
soil’s consolidation coefficient (`c_v`).

The figure below plots the normalized axial resistance over time for a range of rest periods.

Python Code (no Streamlit required):
::

    import numpy as np
    import matplotlib.pyplot as plt

    def compute_t50_from_cv(cv, H):
        """
        Estimate t50 from coefficient of consolidation and drainage path.

        Reference:
        - Terzaghi 1D consolidation theory
        """
        return H**2 / (np.pi**2 * cv)

    def compute_F_Ares_t(FA_u, FA_d, t, t50, n=1.0):
        """
        Time-dependent axial resistance recovery model.
        """
        return FA_d - (FA_d - FA_u) * np.exp(-np.log(2) * (t / t50)**n)

    # Parameters
    cv = 5e-8          # m²/s
    H = 0.1            # m
    FA_u = 0.2         # Initial (undrained) F/V
    FA_d = 0.6         # Final (drained) F/V
    n = 1.0            # Shape factor

    t50 = compute_t50_from_cv(cv, H)

    t_rest = np.linspace(0, 12 * 3600, 200)  # 0 to 12 hours in seconds
    FA_rest = compute_F_Ares_t(FA_u, FA_d, t_rest, t50, n)

    # Plotting
    plt.figure(figsize=(6, 4))
    plt.plot(t_rest / 3600, FA_rest, label="Recovered F/V")
    plt.xlabel("Rest Time (hours)")
    plt.ylabel("Normalized Axial Resistance (F/V)")
    plt.title("Recovery of Axial Resistance During Rest Periods")
    plt.grid(True)
    plt.legend()
    plt.tight_layout()
    plt.show()
