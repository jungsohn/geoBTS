API RP 2GEO
============

Shallow Foundation
-------------------

Equations

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

    geo_Qv = F * (Nc * Cu0 + k * Beff / 4) * geo_Kc * Aeff # (eq.5)



Deep Foundation
----------------

Equations
