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

    fs = alpha * Cu
    qp = 9 * Cu * Fc

Note: 
   - r = pile aspect ratio = L/D

**Beta method**  
fs = K * β * Po  

.. code-block:: python

    beta = np.tan(np.radians(delta))
    K = 1 - np.sin(np.radians(phi))
    fs = K * beta * Po

**Lambda method**  
Rs = λ * (Sm + 2 * Cm) * As


.. code-block:: python

    Rs = lam * (Sm + 2 * Cm) * As

Note:
   - Sm: mean effectiver overburden pressure
   - Cm: mean undrained shear strength

3. Lateral Pile Resistance
---------------------------

3.2 Cohesive soils
...................

- eq.3.2.1.2a) Pu = (3 * Cu + SUW * X) * D + J * Cu * X
- eq.3.2.2.1) P/Pu = 0.5 * (y/yc)**(1/3)

3.3 Cohesionless soils
.......................

- eq.3.3.2.1) P = A * Pu * np.tanh(k*X / (A*Pu) * y)

4. Stability of Gracity Base Foundations
-----------------------------------------

4.4 Bearing capacity formulae
..............................

- eq.4.4.2.1) qu = 0.5 * SUW * Be * Ng * sg *dg * ig + (Po + a) * Nq * sq * dq * iq::

    - a = c * cot(phi)
    - eq.4.4.2.4a) iq = (1 - 0.5 * Fh / (Fv + 0))**5
    - eq.4.4.2.4b) ig = (1 - 0.7 * Fh / (Fv + 0))**5
    - eq.4.4.2.5a) sq = 1 + iq * Be / L * sin(phi)
    - eq.4.4.2.5b) sg = 1 - 0.4 * ig * Be / L

- eq.4.4.4.1) qu = F * (5.14 * Su0 + k * Be / 4) * (1 + sc + dc - ic)::

    - eq.4.4.3.2) ic = 0.5 - 0.5 * np.sqrt(1 - FH1 / (Ae * su) )
    - Fh1 = Fh - Rho - Rhp
    - eq.4.4.3.3) sc = 0.2 * (1-2 * ic) * Be/L
    - eq.4.4.3.4) dc = 0.3 * Su1/Su2 * np.atan(D/Be)
