cmake_minimum_required(VERSION 3.23)


######## Project information ########
set(Project_Title       ClickerHeroesCheats)
set(Project_Author      ClaraCF)


# Configure project
project(${Project_Title} LANGUAGES C CXX VERSION ${Project_Version})


# PkgConfig to find libraries on Unix systems
if(NOT WIN32)
    # This will throw an error and exit if PkgConfig
    # is not installed in the user's computer
    find_package(PkgConfig REQUIRED)
endif()

# Find required libraries
find_package(ZLIB REQUIRED) # For compression of save files


# Set the CMake Build Type to Release by default
set(CMAKE_BUILD_TYPE "Release")

# Define some flags for increased performance
set(SYSTEM_TUNING_FLAGS "-march=native -mtune=native -O3 -pipe")

# Enable Link Time Optimizations (LTO) on Release build type. On by Default.
# This MAY cause problems on some systems.
# If it does, it can be disabled with -DENABLE_LTO=OFF.
option(ENABLE_LTO "Enable Link Time Optimizations" ON)


# Add additional flags for performance to Release build type
if(CMAKE_BUILD_TYPE STREQUAL "Release")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${SYSTEM_TUNING_FLAGS}")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${SYSTEM_TUNING_FLAGS}")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${SYSTEM_TUNING_FLAGS}")

    # Add LTO flags if enabled
    if(ENABLE_LTO)
        set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -flto")
    endif()
endif()


# Add project sources
set(PROJECT_HEADERS
)

set(PROJECT_SOURCES
)


# Target binary
add_executable(${PROJECT_NAME} ${PROJECT_HEADERS} ${PROJECT_SOURCES})

# Specify include directories for libraries
target_include_directories(${PROJECT_NAME} PUBLIC ${ZLIB_INCLUDE_DIR})

# Link libraries
target_link_libraries(${PROJECT_NAME} PUBLIC ${ZLIB_LIBRARIES})

