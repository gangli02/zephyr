# SPDX-License-Identifier: Apache-2.0

cmake_minimum_required(VERSION 3.20.0)

option(COEX_BLE "Enable BLE protocol" ON)
option(COEX_BLE_CLASSIC "Enable BLE Classic (BR/EDR) protocol" OFF)
option(COEX_WIFI "Enable Wi-Fi protocol" ON)
option(COEX_OT "Enable OpenThread protocol" OFF)

set(CONF_FILE ${CMAKE_CURRENT_SOURCE_DIR}/prj.conf)

if(COEX_BLE)
  list(APPEND CONF_FILE ${CMAKE_CURRENT_SOURCE_DIR}/prj-ble.conf)
elseif(COEX_BLE_CLASSIC)
  list(APPEND CONF_FILE ${CMAKE_CURRENT_SOURCE_DIR}/prj-ble-classic.conf)
endif()

if(COEX_WIFI)
  list(APPEND CONF_FILE ${CMAKE_CURRENT_SOURCE_DIR}/prj-wifi.conf)
endif()

if(COEX_OT)
  list(APPEND CONF_FILE ${CMAKE_CURRENT_SOURCE_DIR}/prj-ot.conf)
endif()

set(board_conf ${CMAKE_CURRENT_SOURCE_DIR}/boards/${BOARD}.conf)
if(EXISTS ${board_conf})
  list(APPEND CONF_FILE ${board_conf})
endif()

find_package(Zephyr REQUIRED HINTS $ENV{ZEPHYR_BASE})
project(coex_shell)

file(GLOB app_sources src/*.c)
target_sources(app PRIVATE ${app_sources})
