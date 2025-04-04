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


.. image:: https://raw.githubusercontent.com/jungsohn/geoBTS/main/docs/images/Grok_Terzaghi.png
   :alt: geoBTS Logo
   :width: 100px
