API RP 2GEO
===========

This site provides a user-friendly interpretation of API RP 2GEO for educational purposes.  
All formulas and concepts are used under fair use for engineering education.  
For the official document, please refer to the American Petroleum Institute (API) publications.

.. contents:: Table of Contents
   :depth: 2
   :local:

----

7. Shallow Foundation
--------------------

7.4. Undrained: Constant Su
............................

- **eq.2**:  
  Qd = Su * Nc * Kc * Ae

.. code-block:: python

    Qo = 5.14 * Su * Ao  # eq.3
    Qd = 6.05 * Su * A   # eq.4

    Kc = 1 + sc + dc - ic - bc - gc        # A.10
    sc = 0.18 * (1 - 2*ic)*(Be/Le)         # A.11
    dc = 0.3 * np.atan(D/Be)               # A.12
    ic = 0.5 - 0.5*(1 - He/(Ae*Su))**0.5   # A.13


7.5. Undrained: Linearly increasing Su
......................................

- **eq.5**:  
  Qd = F * (Nc * Su0 + k * Be / 4) * Kc * Ae

.. code-block:: python

    Su0 = SuML + k * z  # definition

    def F_2geo(a, b, c, d, x):  # A.17
        return a + b * x - ((c + b * x)**2 + d**2)**0.5

    x = k * Be / Su0

    Kc = 1 + sc + dc - ic - bc - gc                        # A.16
    sc = scv * (1 - w*ic)*(Be/Le)                          # A.18
    scv = 0.18 - 0.1155 * x**0.5 + 0.021 * x               # A.19
    dc = 0.3 * (SuAVE/Su2) * np.atan(D/Be)                 # A.20
    Su2 = F * (Nc * Su0 + k * Beff / 4) / Nc               # from A.20
    ic = 0.5 - 0.5 * (1 - He / (Ae * Su0)) ** 0.5          # A.21
    bc = 0.4 * v == 0                                      # A.22
    gc = 0.4 * b == 0                                      # A.23

    dH = Kru * SuAVE * Ah                                  # A.1
    SuAVE = (SuML + Su0)                                   # from A.1
    Kru = 2 to 4                                           # A.1

    Le = L - 2*e1                                          # A.8
    Be = B - 2*e2                                          # A.8
    Le = Lx - 2*ex                                         # A.8
    Be = By - 2*ey                                         # A.8


7.6. Drained
.............

- **eq.6**:  
  Qd' = (Pe * (Nq - 1) * Kq + 0.5 * GAMe * Be * Ng * Kg) * Ae

  Note: phi should be relevant to triaxial conditions (phi_TX), because the phi_PS is 10 % higher than phi_TX.

.. code-block:: python

    Nq = np.exp(np.pi * np.tan(np.radians(phi))) * np.tan(np.radians(45 + phi/2))  # from eq.6
    Ng = 1.5 * (Nq - 1) * np.tan(np.radians(phi))                                   # from eq.6

    Qo = 0.5 * GAMe * B * Ng * Ao    # eq.7
    Qd = 0.3 * GAMe * B * Ng * A     # eq.8

    Kq = iq * sq * dq * bq * gq      # A.24
    Kg = ig * sg * dg * bg * gg      # A.24

    iq = (1 - 0.5*(H/Q))**5          # A.25
    ig = (1 - 0.7*(H/Q))**5          # A.25

    sq = 1 + iq * (B/Le) * np.sin(np.radians(phi))       # A.26
    sg = 1 - 0.4 * ig * (B/Le)                           # A.26

    dq = 1 + 1.2 * (D/Be) * np.tan(np.radians(phi)) * (1 - np.sin(np.radians(phi)))**2  # A.27
    dg = 1                                               # A.27

    bq = np.exp(-2 * v * np.tan(np.radians(phi)))        # A.28
    bg = np.exp(-2.7 * v * np.tan(np.radians(phi)))      # A.28

    gq = (1 - 0.5 * np.tan(np.radians(b)))**5            # A.29
    gg = gq                                              # A.29

    dH = 0.5 * Krd * GAMe * Db * Ah                      # A.2
    Krd = Kp - 1/Kp                                      # A.3
    Kp = (np.tan(np.radians(45 + 0.5*phi)))**2           # A.4


7.6. Displacement
..................

.. code-block:: python

    # Short-term
    Uv = (1 - v)/(4*G*R) * Q            # eq.9
    Uh = ((7 - 8*v)/(32 * (1 - v) * G * R)) * H  # eq.10
    THEr = (3*(1 - v)/(8 * G * R**3)) * M        # eq.11
    THEt = (3/(16 * G * R**3)) * T               # eq.12

    # Long-term
    Uv = (h*C / (1 + e0)) * np.log10((qo + dq)/qo)  # eq.13

    # Sliding
    Hd = Suo * A                        # eq.14
    Hd_ = Q * np.tan(np.radians(phi))  # eq.15

A.7.12 Skirts
............................

- Penetration: A.30) Qr = Qf + Qp = f * As + q * Ap
- Removal: N/A

A.7.14 - 15
............

- Installation: N/A 
- Sliding: N/A 
- Torsional: N/A 


8. Pile Foundation
--------------------

8.1. Axial compression
........................

- eq.16) Qc = f * As + q * Ap

- cohesive::

        f = alpha * su    # eq.17
                alpha = 0.5 * psi**-0.5 # for psi<1
                alpha = 0.5 * psi**-0.25 # for psi>1
                psi = Su / Po
        q = 9 * Su        # eq.20

- cohesionless::

        f = beta * Po       # eq.21
        q = Nq * Po         # eq.22

8.4. Soil reaction
...................

- Axial shear transfer: t-z curve
- End bearing resistance: Q-z curve

8.5. Lateral load
..................

- cohesive::

        Pu * D = 3 * Su * D + SUW * z * D + J * Su * z        # eq.23
                # but limited by eq.24) PuD = 9 * Su * D for z > zr
                # zr = 6 * D / (SUW * D / Su + J)
                # J ranging from 0.25 to 0.5

- cohessionless::

        Pus = (C1 * z + C2 * D) * SUW * z        # eq.26 Shallow
                # C1 is determined by phi
                # C2 is determined by phi

        Pud = C3 * D * SUE * z                   # eq.27 Deep
                # C3 is determined by phi

        p = A * Pu * np.tanh( k*z / (A*Pu) * y )
                # Lateral soil resistance: p-y curve 

9. Soil-structure interaction
------------------------------


9.2. Steel catenary risers (SCR)
.................................

9.3. Top tension riser
.......................

- eq.32) Gmax / SuDSS = 300 / (PI/100)
