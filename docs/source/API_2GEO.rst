API RP 2GEO
============

7. Shallow Foundation
--------------------

7.4. Undrained: Constant Su
.................

    eq2) Qd = Su * Nc * Kc * Ae

        eq3) Qo = 5.14 * Su * Ao

        eq4) Qd = 6.05 * Su * A

7.5. Undrained: Linearly increasing Su
............................

    eq5) Qd = F * (Nc * Cu0 + k * Beff / 4) * Kc * Ae

7.6. Drained
............................

    eq6) Qd' = (Pe * (Nq - 1) * Kq + 0.5 * GAMe * Be * Ng * Kg) * Ae

        eq7) Qo = 0.5 * GAMe * B * Ng * Ao

        eq8) Qd' = 0.3 * GAMe * B * Ng * A

7.6. Displacement
............................

Short-term

    eq9) Uv = (1 - v)/(4*G*R) * Q

    eq10) Uh = ((7 - 8*v)/(32 * (1 - v) * G * R)) * H

    eq11) THEr = (3*(1 - v)/(8 * G * R**3)) * M

    eq12) THEt = (3/(16 * G * R**3)) * T

Long-term

    eq13) Uv = (h*C / (1 + e0)) * np.log10((qo + dq)/qo)



    Sc = 1 + (Beff / Leff) * (Nq / Nc)  # p243 (C6.13.1-7)

    mL = (2 + Leff / Beff) / (1 + Leff / Beff)

    mB = (2 + Beff / Leff) / (1 + Beff / Leff)

    m = mL * (np.cos(θ * np.pi / 180))**2 + mB * (np.sin(θ * np.pi / 180))**2

    api_ic = 1 - m * api_H / (Beff * Leff * Su_fail * Nc)  # p243, (C6.13.1-6)

    api_Kc = api_ic * Sc  # p243 (C6.13.1-3)

    api_Qv = Aeff * (Su_fail * Nc * api_Kc + (SUW + UWw) * D)

    def F_2geo(a, b, c, d, x):
        return a + b * x - ((c + b * x)**2 + d**2)**0.5  # 계산식 (A.17)

    x = k * Beff / Cu0

    Scv = np.interp(x, list_kBeff_over_Su0, list_Scv)

    Su2 = F * (Nc * Cu0 + k * Beff / 4) / Nc

    geo_ic = 0.5 - 0.5 * (1 - geo_H / (Aeff * Cu0)) ** 0.5 #(A.21)




Deep Foundation
----------------

Equations
