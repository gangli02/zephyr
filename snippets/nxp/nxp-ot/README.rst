.. _snippet-nxp-ot:

NXP OpenThread (nxp-ot)
#######################

Overview
********

This snippet enables NXP OpenThread platform support including:

- PSA crypto for OpenThread security
- Board-specific radio interface configuration

Supported Boards
****************

- ``frdm_rw612`` / ``rd_rw612_bga``
- ``frdm_mcxw72``

Usage
*****

.. code-block:: console

   west build -b frdm_rw612 <sample> -S nxp-ot
