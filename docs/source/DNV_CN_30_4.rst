DNC CN 30.4
============

2. Axial Pile Resistance
-------------------------

- eq.2.2.2.1) R = Sum(fs * As) + qp * Ap

    - alpha method: fs = alpha * Cu::

        - qp = 9 * Cu * Fc
        - r: pile aspect ratio = L/D

    - beta method: fs = K * beta * Po::

        - beta = np.tan(np.radians(delta)) 

3. Lateral Pile Resistance
---------------------------


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
