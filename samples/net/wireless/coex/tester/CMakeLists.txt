# SPDX-License-Identifier: Apache-2.0

cmake_minimum_required(VERSION 3.20.0)

LIST(APPEND QEMU_EXTRA_FLAGS -serial unix:/tmp/bt-stack-tester)

find_package(Zephyr REQUIRED HINTS $ENV{ZEPHYR_BASE})
project(tester)

zephyr_library_include_directories(${ZEPHYR_BASE}/samples/bluetooth)
zephyr_library_include_directories(${ZEPHYR_BASE}/subsys/bluetooth/mesh)
zephyr_library_include_directories(${ZEPHYR_BASE}/subsys/bluetooth/host)

set(tester_src ${ZEPHYR_BASE}/tests/bluetooth/tester/src)
set(tester_src_btp ${ZEPHYR_BASE}/tests/bluetooth/tester/src/btp)
set(tester_src_audio ${ZEPHYR_BASE}/tests/bluetooth/tester/src/audio)
set(tester_src_audio_btp ${ZEPHYR_BASE}/tests/bluetooth/tester/src/audio/btp)

FILE(GLOB main_source ${ZEPHYR_BASE}/tests/bluetooth/tester/src/main.c)
FILE(GLOB btp_source ${ZEPHYR_BASE}/tests/bluetooth/tester/src/btp.c)
FILE(GLOB btp_core_source ${ZEPHYR_BASE}/tests/bluetooth/tester/src/btp_core.c)
FILE(GLOB btp_gap_source ${ZEPHYR_BASE}/tests/bluetooth/tester/src/btp_gap.c)
FILE(GLOB btp_gatt_source ${ZEPHYR_BASE}/tests/bluetooth/tester/src/btp_gatt.c)
target_sources(app PRIVATE
    ${main_source}
    ${btp_source}
    ${btp_core_source}
    ${btp_gap_source}
    ${btp_gatt_source}
    )

if(CONFIG_BT_L2CAP_DYNAMIC_CHANNEL)
FILE(GLOB btp_l2cap_source ${ZEPHYR_BASE}/tests/bluetooth/tester/src/btp_l2cap.c)
target_sources(app PRIVATE ${btp_l2cap_source})
endif()

if(CONFIG_BT_MESH)
    FILE(GLOB btp_mesh_source ${ZEPHYR_BASE}/tests/bluetooth/tester/src/btp_l2cap.c)
    target_sources(app PRIVATE ${btp_mesh_source})
endif()

if(CONFIG_BT_IAS OR CONFIG_BT_IAS_CLIENT)
    FILE(GLOB btp_ias_source ${ZEPHYR_BASE}/tests/bluetooth/tester/src/btp_l2cap.c)
    target_sources(app PRIVATE ${btp_ias_source})
endif()

if(CONFIG_BT_OTS)
    FILE(GLOB btp_ots_source ${ZEPHYR_BASE}/tests/bluetooth/tester/src/btp_ots.c)
    target_sources(app PRIVATE ${btp_ots_source})
endif()

zephyr_library_include_directories(${tester_src} ${tester_src_btp} ${tester_src_audio_btp})
add_subdirectory_ifdef(CONFIG_BT_AUDIO ${tester_src_audio_btp})
