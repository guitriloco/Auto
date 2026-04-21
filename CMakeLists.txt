cmake_minimum_required(VERSION 3.10)
project(SoupLog C)

set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)

# Optimization flags
if(MSVC)
    add_compile_options(/O2)
else()
    add_compile_options(-O3 -Wall -Wextra)
endif()

include_directories(include)

set(SOURCES
    src/main.c
    src/soup_log.c
    src/storage_bin.c
)

add_executable(souplog ${SOURCES})

# Ensure data directory exists
install(DIRECTORY DESTINATION data)
