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

    # Total vertical resistance of embedded pipeline (eq.4.1)
    Qv = Qv0 * (1 + d_ca) + gamma_prime * Abm  
    # Initial vertical bearing resistance (eq.4.2)
    Qv0 = F * (Nc * su0 + rho * B / 4) * B     

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

5.3 Axial
-----------

5.4 Lateral
------------

5.5 Uplift resistance
------------------------


7.1 On-bottom stability
------------------------


7.2 Free spanning pipelines
------------------------
