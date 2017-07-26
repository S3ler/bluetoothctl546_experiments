cmake_minimum_required(VERSION 3.7)
project(bluetoothctl546_experiments)

set(CMAKE_CXX_STANDARD 11)

find_package(PkgConfig REQUIRED)

# thank you: https://stackoverflow.com/questions/38515316/how-to-configure-dbus-dependencies-with-cmake
set(CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR})
find_package(DBus REQUIRED)
include_directories(${DBUS_INCLUDE_DIRS})

pkg_search_module(GLIB REQUIRED glib-2.0)
include_directories(${GLIB_INCLUDE_DIRS})

find_package(Readline REQUIRED)
include_directories(${Readline_INCLUDE_DIR})

include_directories(src/shared)

set(SOURCE_FILES
        client/advertising.c
        client/advertising.h
        client/agent.c
        client/agent.h
        client/display.c
        client/display.h
        client/gatt.c
        client/gatt.h

        gdbus/client.c
        gdbus/gdbus.h
        gdbus/mainloop.c
        gdbus/object.c
        gdbus/watch.c
        gdbus/polkit.c

        src/shared/io-glib.c
        src/shared/io.h
        src/shared/queue.c
        src/shared/queue.h
        src/shared/util.c
        src/shared/util.h

        monitor/uuid.c
        monitor/uuid.h
        )

set(MAIN_FILE client/main.c)

add_definitions(-DVERSION="0.0")


add_executable(bluetoothctl546_experiments ${SOURCE_FILES} ${MAIN_FILE})
target_link_libraries(bluetoothctl546_experiments  ${DBUS_LIBRARIES} ${GLIB_LIBRARIES} ${Readline_LIBRARY})

add_library(libbluetoothctl546_experiments ${SOURCE_FILES})
target_link_libraries(libbluetoothctl546_experiments  ${DBUS_LIBRARIES} ${GLIB_LIBRARIES} ${Readline_LIBRARY})

