.. _snippet-nxp-bt-a2dp:

NXP Bluetooth A2DP (nxp-bt-a2dp)
#################################

Overview
********

This snippet provides NXP-specific Bluetooth A2DP streaming performance
tuning. It increases L2CAP TX buffers, heap, stack sizes, and HCI buffer
counts for high-throughput audio streaming.

Usage
*****

Use together with ``overlay-ble.conf`` or ``overlay-ble-classic.conf``:

.. code-block:: console

   west build -b frdm_rw612 <sample> -S nxp-bt-a2dp -- \
     -DEXTRA_CONF_FILE="overlay-ble-classic.conf overlay-wifi.conf"
