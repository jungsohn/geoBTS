Suction Pile
=============

This section provides interactive tools and theoretical references for suction pile design,  
including bearing capacity, installation, retrieval, and chain configuration considerations.

Web Apps
-----------------

1. **Suction Pile Calculator**: `Link <https://your-suction-app-url>`_

.. image:: https://raw.githubusercontent.com/jungsohn/geoBTS/main/docs/images/suction_pile_main.jpg
   :width: 600px

Design Reference
-----------------

**α, β, λ Methods**
....................

These empirical methods are used to estimate shaft friction and end bearing for suction piles in cohesive soils.

- **α-method** (for cohesive soils):

  .. math::

     f = \alpha \cdot S_u

  where:
  - :math:`f` is unit shaft resistance
  - :math:`\alpha` is adhesion factor (typically 0.5 to 1.0)
  - :math:`S_u` is undrained shear strength

- **β-method** (for cohesionless soils):

  .. math::

     f = \beta \cdot \sigma'_v

  where:
  - :math:`\sigma'_v` is effective vertical stress
  - :math:`\beta` depends on soil type and installation method

- **λ-method**:

  .. math::

     f = \lambda \cdot \sqrt{S_u \cdot \sigma'_v}

  where :math:`\lambda` is an empirical fitting parameter.

---

Chain Configuration
....................

Proper chain angle and padeye position are essential for maintaining vertical alignment of the suction pile during loading.

- Uplift force :math:`T` acting on the padeye induces both vertical and horizontal components.
- Design often requires :math:`\theta < 30^\circ` for safe axial load transfer.

.. image:: https://raw.githubusercontent.com/jungsohn/geoBTS/main/docs/images/suction_pile_chain_config.jpg
   :width: 600px

---

Installation
.............

Installation is achieved by applying suction pressure (negative head) to draw the pile into the seabed.

- Driving force:

  .. math::

     \Delta p = \frac{W - R_s}{A}

  where:
  - :math:`\Delta p` is required suction pressure
  - :math:`W` is self-weight + penetration force
  - :math:`R_s` is skin resistance
  - :math:`A` is internal area of the pile

- Excessive suction can cause soil failure or loss of seal at the pile tip.

---

Retrieval
.............

Suction pile retrieval is usually performed by reversing the suction or applying external upward force.

- Common method: **water jetting** to reduce skin friction
- Reversal suction is limited by external hydrostatic pressure and structural capacity

.. note::

   Retrieval can become difficult in stiff clay due to soil reconsolidation and setup effects.
