DNVGL-RP-F114 (2019)
=====================

This handbook provides simplified access to the analytical models and guidelines in DNVGL-RP-F114  
for anchor penetration and pipe-soil interaction. All content is for educational purposes.  
Refer to the official DNV publications for complete and authoritative guidelines.

.. contents:: Table of Contents
   :depth: 2
   :local:

----

4. Exposed Pipelines
---------------------

4.2. Pipe Embedment
....................

This section provides empirical and analytical formulas to evaluate the vertical resistance  
of partially embedded pipelines when subjected to external vertical loading (e.g., from dragging anchors).

- Undrained: Model 1
.. code-block:: python

    Qv = Qv0 * (1 + d_ca) + gamma_prime * Abm  # Total vertical resistance of embedded pipeline (eq.4.1)
    Qv0 = F * (Nc * su0 + rho * B / 4) * B     # Initial vertical bearing resistance (eq.4.2)

- Undrained: Model 2
.. code-block:: python

    # eq.4.8: Vertical force required to penetrate the pipe to embedment depth z
    term1 = min(6 * (z / D)**0.25, 3.4 * (10 * z / D)**0.5)
    term2 = 1.5 * (gamma_prime * Abm / (D * su)) * D * su
    Qv = (term1 + term2) * D * su

- Drained
.. code-block:: python

    # eq.4.9: Vertical force required to penetrate the pipe to embedment depth z
    Qv = 0.5 * gamma_prime * Ny * B**2 + z0 * gamma_prime * Nq * dq * B


----

4.3. Axial PSI
....................

4.4. Lateral PSI
....................

----

5. Buried and covered pipelines
---------------------------------

5.3. Axial PSI
....................

- Undrained

.. code-block:: python

    # eq.5.3: Deep axial resistance
    Fa_deep_u = alpha * su_reconsolidated * np.pi * D

    # eq.5.4: Shallow axial resistance
    Fa_shallow_u = (alpha * su_bottom_recon * np.pi * D / 2 + 2 * su_backfill_recon * (H + D / 2)




5.4. Lateral PSI
....................

5.5. Uplift resistance
....................




----

7. Special considerations
----------------------------


7.1 On-bottom stability
.........................


7.2 Free spanning pipelines
.........................

- Soil Stiffness


.. code-block:: python

    # eq.7.1: Mean effective stress beneath the pipe span
    sigma_s = 0.5 * (1 + K0) * B * gamma_prime + (V / (3 * B)) * (1 + L / (2 * Lsh))

    # eq.7.2: Vertical dynamic stiffness
    Kv_d = (Cv / (1 - nu)) * ((2/3) * (rho_s / rho) + 1/3) * D**0.5

    # eq.7.3: Lateral dynamic stiffness
    Kl_d = Cl * (1 + nu) * ((2/3) * (rho_s / rho) + 1/3) * D**0.5
