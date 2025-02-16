# SPDX-License-Identifier: Apache-2.0

# Add the board
list(APPEND BOARD_ROOT ${CMAKE_CURRENT_LIST_DIR}/)
#list(APPEND DTS_ROOT ${CMAKE_CURRENT_LIST_DIR}/)

# Add out-off-tree drivers
list(APPEND ZEPHYR_EXTRA_MODULES ${CMAKE_CURRENT_SOURCE_DIR}/drivers/sensor/pcf8653)
list(APPEND ZEPHYR_EXTRA_MODULES ${CMAKE_CURRENT_SOURCE_DIR}/drivers/sensor/bma423)
list(APPEND ZEPHYR_EXTRA_MODULES ${CMAKE_CURRENT_SOURCE_DIR}/drivers/sensor/axp202)

cmake_minimum_required(VERSION 3.13.1)
find_package(Zephyr REQUIRED HINTS $ENV{ZEPHYR_BASE})
project(twatch_v3 VERSION 0.0.1)

zephyr_include_directories(include)

add_subdirectory(src/apps)
add_subdirectory(src/backlight)
add_subdirectory(src/event_service)
add_subdirectory(src/storage)
add_subdirectory(src/vibes)

target_sources(app PRIVATE src/main.c)

execute_process(COMMAND git rev-parse --short HEAD
                WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
                OUTPUT_VARIABLE PROJECT_GIT_COMMIT_HASH
                RESULT_VARIABLE PROJECT_GIT_COMMIT_HASH_SUCCESS)

message("---------------------------------")
message("    * Version : " ${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}.${PROJECT_VERSION_PATCH})
message("    * GitRef(S) :" ${PROJECT_GIT_COMMIT_HASH})
message("---------------------------------")
