API RP 2GEO
============

This site provides a user-friendly interpretation of API RP 2GEO for educational purposes.  
All formulas and concepts are used under fair use for engineering education.  
For the official document, please refer to the American Petroleum Institute (API) publications.

----

7. Shallow Foundation
--------------------

7.4. Undrained: Constant Su
.................


- eq.2) Qd = Su * Nc * Kc * Ae::

        - eq3) Qo = 5.14 * Su * Ao
        - eq4) Qd = 6.05 * Su * A

        - A.10) Kc = 1 + sc + dc - ic - bc - gc
                - A.11) sc = 0.18 * (1 - 2*ic)*(Be/Le)
                - A.12) dc = 0.3*np.atan(D/Be)
                - A.13) ic = 0.5 - 0.5*(1 - He/(Ae*Su))**0.5

7.5. Undrained: Linearly increasing Su
............................

- eq.5) Qd = F * (Nc * Su0 + k * Be / 4) * Kc * Ae::

        - Su0 = SuML + k * z

        - A.17) def F_2geo(a, b, c, d, x):
                    return a + b * x - ((c + b * x)**2 + d**2)**0.5
                - x = k * Be / Su0

        - A.16) Kc = 1 + sc + dc - ic - bc - gc
                - A.18) sc = scv * (1 - w*ic)*(Be/Le)
                        - A.19) scv = 0.18 - 0.1155 * x**0.5 + 0.021 * x

                - A.20) dc = 0.3 * (SuAVE/Su2) * np.atan(D/Be)
                        - Su2 = F * (Nc * Su0 + k * Beff / 4) / Nc

                - A21) ic = 0.5 - 0.5 * (1 - He / (Ae * Su0)) ** 0.5
                - A.22) bc = 0.4 * v == 0
                - A.23) gc = 0.4 * b == 0

        - A.1) dH = Kru * SuAVE * Ah::
                - SuAVE = (SuML + Su0)
                - Kru = from 2 to 4

        - A.8) Le = L - 2*e1, Be = B - 2*e2::
                - Le = Lx - 2*ex
                - Be = By - 2*ey

7.6. Drained
............................

- eq.6) Qd' = (Pe * (Nq - 1) * Kq + 0.5 * GAMe * Be * Ng * Kg) * Ae::

        - Nq = np.exp(np.pi * np.tan(np.radians(phi)) )* np.tan(np.radians(45 + phi/2))
        - Ng = 1.5 * (Nq - 1) * np.tan(np.radians(phi))
        - phi should be relevant to triaxial conditions (phi_TX), because the phi_PS is 10 % higher than phi_TX.

        - eq.7) Qo = 0.5 * GAMe * B * Ng * Ao
        - eq.8) Qd' = 0.3 * GAMe * B * Ng * A

        - A.24) Kq = iq * sq * dq * bq * gq ,  Kg = ig * sg * dg * bg * gg
                - A.25) iq = (1 - 0.5*(H/Q))**5,  ig = (1 - 0.7*(H/Q))**5
                - A.26) sq = 1 + iq * (B/Le) * np.sin(np.radians(phi)),  sg = 1 - 0.4 * ig * (B/Le)
                - A.27) dq = 1 + 1.2 * (D/Be) * np.tan(np.radians(phi)) * (1 - np.sin(np.radians(phi)))**2, dg = 1
                - A.28) bq = np.exp(-2 * v * np.tan(np.radians(phi))),  bg = np.exp(-2.7 * v * np.tan(np.radians(phi)))
                - A.29) gq = (1 - 0.5 * np.tan(np.radians(b)))**5,  gg = gq

        - A.2) dH = 0.5 * Krd * GAMe * Db * Ah::
                - A.3) Krd = Kp - 1/Kp
                - A.4) Kp = (np.tan(np.radians(45 + 0.5*phi)))**2


7.6. Displacement
............................

- Short-term::

    - eq.9) Uv = (1 - v)/(4*G*R) * Q
    - eq.10) Uh = ((7 - 8*v)/(32 * (1 - v) * G * R)) * H
    - eq.11) THEr = (3*(1 - v)/(8 * G * R**3)) * M
    - eq.12) THEt = (3/(16 * G * R**3)) * T

- Long-term::

    - eq.13) Uv = (h*C / (1 + e0)) * np.log10((qo + dq)/qo)

- Sliding::

    - eq.14) Hd = Suo * A
    - eq.15) Hd' = Q * np.tan(np.radians(phi))

- Torsion::

    - FYI)

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
                psi = Su / P'o
        q = 9 * Su        # eq.20

- cohesionless::

        f = beta * P'o       # eq.21
        q = Nq * P'o         # eq.22

8.4. Soil reaction
...................

- Axial shear transfer: t-z curve
- End bearing resistance: Q-z curve

8.5. Lateral load
..................

- cohesive::

        - eq.23) Pu * D = 3 * Su * D + SUW * z * D + J * Su * z
                - but limited by eq.24) PuD = 9 * Su * D for z > zr
                - zr = 6 * D / (SUW * D / Su + J)
                - J ranging from 0.25 to 0.5

        - Lateral soil resistance: p-y curve

- cohessionless::

        - eq.26) Shallow: Pus = (C1 * z + C2 * D) * SUW * z
                - C1 is determined by phi
                - C2 is determined by phi

        - eq.27) Deep: Pud = C3 * D * SUE * z
                - C3 is determined by phi

        - Lateral soil resistance: p-y curve
                - p = A * Pu * np.tanh( k*z / (A*Pu) * y )

9. Soil-structure interaction
------------------------------


9.2. Steel catenary risers (SCR)
.................................

9.3. Top tension riser
.......................

- eq.32) Gmax / SuDSS = 300 / (PI/100)
