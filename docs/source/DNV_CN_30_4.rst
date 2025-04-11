DNV CN 30.4 (1992)
==================

This document provides a user-friendly summary of selected formulas from DNV CN 30.4, focused on pile and gravity base foundation design.  
It is intended for educational use only. Please refer to the official DNV publication for authoritative guidance.

.. contents:: Table of Contents
   :depth: 2
   :local:

----

2. Axial Pile Resistance
-------------------------

- **eq.2.2.2.1**  
  R = Σ(fs * As) + qp * Ap

**Alpha method**  

.. code-block:: python

    fs = alpha * Cu    # 2.2.2.4
    qp = 9 * Cu * Fc   # 2.2.2.7

Note: 
   - r = pile aspect ratio = L/D

**Beta method**  

.. code-block:: python

    beta = np.tan(np.radians(delta))
    K = 1 - np.sin(np.radians(phi))
    fs = K * beta * Po      # 2.2.2.11

**Lambda method**  

.. code-block:: python

    Rs = lam * (Sm + 2 * Cm) * As   # 2.2.2.13

Note:
   - Sm: mean effectiver overburden pressure
   - Cm: mean undrained shear strength

----

3. Lateral Pile Resistance
---------------------------

3.2 Cohesive soils
...................

.. code-block:: python

    Pu = (3 * Cu + SUW * X) * D + J * Cu * X  # eq.3.2.1.2a
    P_ratio = 0.5 * (y / yc)**(1/3)           # eq.3.2.2.1

3.3 Cohesionless soils
.......................

.. code-block:: python

    P = A * Pu * np.tanh(k * X / (A * Pu) * y)   # eq.3.3.2.1

----

4. Stability of Gravity Base Foundations
-----------------------------------------

4.4 Bearing capacity formulae
..............................

- 4.4.2 Fully drained

.. code-block:: python

    Nq = np.exp( np.pi * np.tan(np.radian(phi)) ) * (np.tan(np.radian(45 + phi/2)) )**2
    Ng = 1.5 * (Nq - 1) * np.tan(np.radian(phi))   # Hansen
    Ng = 2 * (Nq + 1 ) * np.tan(np.radian(phi))    # ?

    a = c / np.tan(np.radians(phi))
    iq = (1 - 0.5 * Fh / (Fv + 1e-9))**5
    ig = (1 - 0.7 * Fh / (Fv + 1e-9))**5
    sq = 1 + iq * Be / L * np.sin(np.radians(phi))
    sg = 1 - 0.4 * ig * Be / L

    qu = 0.5 * SUW * Be * Ng * sg * dg * ig + (Po + a) * Nq * sq * dq * iq # eq.4.4.2.1

- 4.4.4 Undrained linearly increasing Su

.. code-block:: python

    Fh1 = Fh - Rho - Rhp
    ic = 0.5 - 0.5 * np.sqrt(1 - Fh1 / (Ae * su))
    sc = scv * (1 - 2 * ic) * Be / L
    dc = 0.3 * Su1 / Su2 * np.arctan(D / Be)

    qu = F * (5.14 * Su0 + k * Be / 4) * (1 + sc + dc - ic) # eq.4.4.4.1

