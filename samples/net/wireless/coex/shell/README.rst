.. zephyr:code-sample:: wireless-coex-shell
   :name: Wireless coexistence shell
   :relevant-api: bluetooth net_stats

   Test Bluetooth, Wi-Fi, and Thread coexistence using an interactive shell.

Overview
********

This sample demonstrates the coexistence of multiple wireless protocols
(Bluetooth LE, Wi-Fi, and Thread/OpenThread) running simultaneously on the same
device. It provides shell commands to control and test each protocol stack
independently while they operate concurrently.

The sample enables:

* Bluetooth shell for BLE operations (scan, advertise, connect)
* Wi-Fi shell for Wi-Fi operations (scan, connect, disconnect)
* OpenThread shell for Thread network operations
* Network shell for network interface management

This is useful for:

* Testing multi-protocol coexistence scenarios
* Verifying that wireless protocols can operate simultaneously without interference
* Debugging coexistence issues
* Demonstrating real-world IoT device capabilities

Supported Features
********************

* Bluetooth Low Energy (BLE) via Bluetooth shell
* Bluetooth Classic (BR/EDR) with A2DP, HFP, AVRCP
* Wi-Fi (802.11) via Wi-Fi shell
* Thread/IEEE 802.15.4 via OpenThread shell
* Concurrent operation of all three protocols
* Network management via net shell

Configuration
*************

Protocol selection is controlled via CMake options:

* ``COEX_BLE`` - Enable BLE protocol (default: ON)
* ``COEX_BLE_CLASSIC`` - Enable BLE Classic/BR/EDR protocol (default: OFF)
* ``COEX_WIFI`` - Enable Wi-Fi protocol (default: ON)
* ``COEX_OT`` - Enable OpenThread protocol (default: OFF)

Vendor-specific platform configurations are provided via NXP snippets:

* ``nxp-wifi`` - NXP Wi-Fi driver, performance tuning, power management
* ``nxp-wifi-hostap`` - WPA supplicant with enterprise, DPP, WPS support
* ``nxp-ot`` - NXP OpenThread platform (RCP interface, PSA crypto)
* ``nxp-bt-a2dp`` - A2DP streaming performance tuning
* ``nxp-bt-auto-pts`` - Auto-PTS certification configurations

Building and Running
********************

Verify that the board and chip you are targeting provide Bluetooth LE, Wi-Fi,
and Thread/802.15.4 support.

Wi-Fi + BLE + Thread (full coex on RW612):

.. code-block:: console

   west build -b rd_rw612_bga samples/net/wireless/coex/shell \
     -S nxp-wifi -S nxp-wifi-hostap -S nxp-ot -- \
     -DCOEX_BLE=ON -DCOEX_WIFI=ON -DCOEX_OT=ON

Wi-Fi + BLE (default, no Thread):

.. code-block:: console

   west build -b rd_rw612_bga samples/net/wireless/coex/shell \
     -S nxp-wifi -S nxp-wifi-hostap

BLE + Thread (no Wi-Fi):

.. code-block:: console

   west build -b rd_rw612_bga samples/net/wireless/coex/shell \
     -S nxp-ot -- \
     -DCOEX_WIFI=OFF -DCOEX_OT=ON

Wi-Fi + BLE on external module (IW610 with MIMXRT1060):

.. code-block:: console

   west build -b mimxrt1060_evk@C/mimxrt1062/qspi samples/net/wireless/coex/shell \
     -S nxp-wifi -S nxp-wifi-hostap

BLE Classic + A2DP + Wi-Fi:

.. code-block:: console

   west build -b rd_rw612_bga samples/net/wireless/coex/shell \
     -S nxp-wifi -S nxp-wifi-hostap -S nxp-bt-a2dp -- \
     -DCOEX_BLE=OFF -DCOEX_BLE_CLASSIC=ON

Sample console interaction
==========================

Bluetooth operations
--------------------

.. code-block:: console

   uart:~$ bt init
   Bluetooth initialized
   uart:~$ bt scan on
   Bluetooth active scan enabled
   [DEVICE]: 01:23:45:67:89:AB (random), AD evt type 0, RSSI -45
   [DEVICE]: CD:EF:01:23:45:67 (random), AD evt type 3, RSSI -62
   uart:~$ bt scan off
   Scan successfully stopped

Wi-Fi operations
----------------

.. code-block:: console

   uart:~$ wifi scan
   Scan requested
   Num  | SSID                             (len) | Chan | RSSI | Sec
   1    | MyNetwork                        9     | 6    | -45  | WPA/WPA2
   2    | GuestWiFi                        9     | 11   | -67  | Open
   ----------
   Scan request done

   uart:~$ wifi connect "MyNetwork" 0 MyPassword
   Connection requested
   Connected
   uart:~$ wifi status
   Status: successful
   ==================
   State: COMPLETED
   Interface Mode: STATION
   SSID: MyNetwork
   BSSID: 12:34:56:78:9A:BC
   Band: 2.4GHz
   Channel: 6
   Security: WPA2-PSK
   RSSI: -45

Thread operations
-----------------

.. code-block:: console

   uart:~$ ot state
   disabled
   Done
   uart:~$ ot ifconfig up
   Done
   uart:~$ ot thread start
   Done
   uart:~$ ot state
   leader
   Done
   uart:~$ ot ipaddr
   fdde:ad00:beef:0:0:ff:fe00:fc00
   fdde:ad00:beef:0:0:ff:fe00:c00
   Done
