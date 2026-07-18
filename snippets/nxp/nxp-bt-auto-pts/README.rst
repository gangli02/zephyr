.. _snippet-nxp-bt-auto-pts:

NXP Bluetooth Auto-PTS (nxp-bt-auto-pts)
#########################################

Overview
********

This snippet provides NXP-specific configurations for Auto-PTS Bluetooth
certification testing. It enables privacy and MITM enforcement required
for various PTS test cases.

Usage
*****

Use together with ``overlay-ble.conf``:

.. code-block:: console

   west build -b frdm_rw612 <sample> -S nxp-bt-auto-pts -- \
     -DEXTRA_CONF_FILE="overlay-ble.conf"
