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


Deep Foundation
----------------

Equations
