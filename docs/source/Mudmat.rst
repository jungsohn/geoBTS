Mudmat
===================

Background Theory (WebApp)
-------------------------

Terzaghi (1943)
................

- Equation: qu = c*Nc + GAM*Df*Nq + 0.5*GAM*B*Ng

- Feature: Only strip footing

- Limitation:

  - No Fh nor Moment
  - No shape foundation
  - Simple contrant soil

- WebApp: https://webapp-mudmat-7f78b3e2018b.herokuapp.com/Terzaghi_bearing_capacity

Meyerhof (1951,1963)
............

- Equation: qu = c*Nc*Kc + GAM*Df*Nq*Kq + 0.5*GAM*B*Ng*Kg

- Feature:

  - Apply K factors to Terzaghi's
  - Updated Nc, Nq, and Ng: For example, phi=30 --> Ng (Meyerhof) = 20, while Ng (Terzaghi) = 15


Offshore (WebApp)
--------------

- Clay: https://webapp-mudmat-7f78b3e2018b.herokuapp.com/offshore_mudmat_clay

- Sand: TBD


.. table:: Comparison of Shallow Foundation Bearing Capacity Theories

   +-----------------------------+-------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------+----------------------------------------------------------------------------------+
   | **Aspect**                  | **Terzaghi (1943)**                                                                 | **Meyerhof (1951, 1963)**                                                                | **Vesic (1973)**                                                                 | **Hansen (1970)**                                                                |
   +=============================+=====================================================================================+==========================================================================================+==================================================================================+==================================================================================+
   | **Basic Formula**           | :math:`q_u = c N_c + \gamma D N_q + 0.5 \gamma B N_\gamma`                         | :math:`q_u = c N_c s_c d_c i_c + \gamma D N_q s_q d_q i_q + 0.5 \gamma B N_\gamma s_\gamma d_\gamma i_\gamma` | Similar to Meyerhof: :math:`q_u = c N_c s_c d_c i_c + \gamma D N_q s_q d_q i_q + 0.5 \gamma B N_\gamma s_\gamma d_\gamma i_\gamma` | Similar to Meyerhof: :math:`q_u = c N_c s_c d_c i_c g_c + \gamma D N_q s_q d_q i_q g_q + 0.5 \gamma B N_\gamma s_\gamma d_\gamma i_\gamma g_\gamma` |
   +-----------------------------+-------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------+----------------------------------------------------------------------------------+
   | **Bearing Capacity Factors**| :math:`N_c, N_q, N_\gamma` based on soil friction angle (:math:`\phi`), derived theoretically | Refined :math:`N_c, N_q, N_\gamma` with broader applicability, adjusted for :math:`\phi` | Improved :math:`N_c, N_q, N_\gamma` using experimental data and soil compressibility | Uses Meyerhof’s factors but adjusts for complex conditions like sloping ground   |
   +-----------------------------+-------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------+----------------------------------------------------------------------------------+
   | **Shape Factors**           | Not included (assumes strip footing)                                               | :math:`s_c, s_q, s_\gamma`: Accounts for footing shape (rectangular, square, circular)  | Adopts Meyerhof’s shape factors with minor refinements                           | :math:`s_c, s_q, s_\gamma`: Similar to Meyerhof, with emphasis on practical use  |
   +-----------------------------+-------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------+----------------------------------------------------------------------------------+
   | **Depth Factors**           | Not explicitly included (assumes shallow depth)                                    | :math:`d_c, d_q, d_\gamma`: Increases capacity with depth, :math:`D/B` ratio considered | Refined depth factors based on soil type and compressibility                    | :math:`d_c, d_q, d_\gamma`: Enhanced for deeper shallow foundations             |
   +-----------------------------+-------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------+----------------------------------------------------------------------------------+
   | **Inclination Factors**     | Not considered (assumes vertical load)                                             | :math:`i_c, i_q, i_\gamma`: Reduces capacity for inclined loads                        | Adopts Meyerhof’s inclination factors with slight adjustments                   | :math:`i_c, i_q, i_\gamma`: Further refined for load eccentricity and inclination |
   +-----------------------------+-------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------+----------------------------------------------------------------------------------+
   | **Ground Slope Factors**    | Not included                                                               | Not included                                                                            | Not included                                                                    | :math:`g_c, g_q, g_\gamma`: Unique addition for sloping ground conditions        |
   +-----------------------------+-------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------+----------------------------------------------------------------------------------+
   | **Key Assumptions**         | - Strip footing                                                                    | - Generalizes to all footing shapes                                                     | - Similar to Meyerhof                                                           | - Complex geometries                                                             |
   |                             | - Homogeneous soil                                                                 | - Homogeneous soil                                                                      | - Considers soil compressibility and settlement                                 | - Sloping ground                                                                 |
   |                             | - Failure along a logarithmic spiral                                               | - Inclined loads allowed                                                               |                                                                                  | - Practical design focus                                                        |
   +-----------------------------+-------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------+----------------------------------------------------------------------------------+
   | **Strengths**               | Simple, foundational, easy to apply for basic cases                                | More versatile, widely used in practice due to shape and depth corrections              | High accuracy due to experimental validation, good for variable soils           | Highly flexible, ideal for non-standard conditions like slopes                  |
   +-----------------------------+-------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------+----------------------------------------------------------------------------------+
   | **Limitations**             | Limited to strip footings, no shape or depth adjustments                           | Less focus on soil compressibility or extreme conditions                               | Complex calculations, less focus on sloping ground                              | More parameters increase complexity, may overcomplicate simple cases            |
   +-----------------------------+-------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------+----------------------------------------------------------------------------------+

**Notes**:  
- :math:`q_u`: Ultimate bearing capacity  
- :math:`c`: Cohesion, :math:`\gamma`: Soil unit weight, :math:`D`: Foundation depth, :math:`B`: Foundation width, :math:`\phi`: Friction angle  
- Each subsequent theory builds on the previous one, adding complexity and applicability.
