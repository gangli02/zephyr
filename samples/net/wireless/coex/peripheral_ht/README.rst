.. zephyr:code-sample:: wifi-ble-peripheral-ht-ot
   :name: Wi-Fi Shell, Bluetooth LE Health Thermometer (Peripheral),
      and OpenThread Shell Coexistence
   :relevant-api: bt_bas bluetooth net_stats

   Demonstrates coexistence of the Wi-Fi shell, Bluetooth LE Peripheral
   role using the Health Thermometer GATT Service, and the OpenThread shell
   running concurrently on a single device.


Overview
********

This sample demonstrates the coexistence of multiple wireless protocols
(Bluetooth LE, Wi-Fi, and Thread/OpenThread) running simultaneously on the same
device. It provides shell commands to control and test each protocol stack
independently while they operate concurrently.
Except that BLE specifically exposes the HT (Health Thermometer) GATT Service.

This is useful for:

* Testing multi-protocol coexistence scenarios
* Verifying that wireless protocols can operate simultaneously without interference
* Debugging coexistence issues
* Demonstrating real-world IoT device capabilities

Supported Features
==================

* Bluetooth LE Health Thermometer GATT Service
* Wi-Fi (802.11) via Wi-Fi shell
* Thread/IEEE 802.15.4 via OpenThread shell
* Concurrent operation of all three protocols
* Network management via net shell

Requirements
************

* BlueZ running on the host, or
* A board with Bluetooth LE support

Building and Running
********************

Verify that the board and chip you are targeting provide Bluetooth LE, Wi-Fi,
and Thread/802.15.4 support.

Board-specific configuration files are provided in the boards directory:

- :file:`boards/rd_rw612_bga.conf`
  Configuration for RD RW612 BGA board with full coex support (BT + Wi-Fi + Thread).

- :file:`boards/rd_rw612_bga.overlay`
  Device tree overlay for RD RW612 BGA board.

- :file:`boards/frdm_rw612.conf`
  Configuration for FRDM RW612 board with full coex support.

- :file:`boards/frdm_rw612.overlay`
  Device tree overlay for FRDM RW612 board.

- :file:`boards/mimxrt1060_evk_mimxrt1062_qspi_C.overlay`
  Device tree overlay for MIMXRT1060 EVKC board.

Build the wifi-ble-peripheral-ht-ot application like this:

.. zephyr-app-commands::
   :zephyr-app: samples/net/wireless/coex/peripheral_ht
   :board: <board to use>
   :goals: build
   :compact:

For instance you can use NXP's RW612 RD board by selecting the rd_rw612_bga board.

.. zephyr-app-commands::
   :zephyr-app: samples/net/wireless/coex/peripheral_ht
   :board: rd_rw612_bga
   :gen-args:
      -DEXTRA_CONF_FILE="
      nxp/overlay_wifi_rw612.conf
      nxp/overlay_wifi_hostap_rw612.conf
      nxp/overlay_ot_rw612.conf"
   :goals: build
   :compact:

Sample console interaction
==========================

Bluetooth operations
--------------------

See :zephyr:code-sample-category:`bluetooth` samples for details.

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

