cmake_minimum_required(VERSION 3.5)

set(EIPSCANNER_MAJOR_VERSION 1)
set(EIPSCANNER_MINOR_VERSION 1)
set(EIPSCANNER_PATCH_VERSION 0)
set(EIPSCANNER_FULL_VERSION ${EIPSCANNER_MAJOR_VERSION}.${EIPSCANNER_MINOR_VERSION}.${EIPSCANNER_PATCH_VERSION})
project(eipscanner VERSION ${EIPSCANNER_FULL_VERSION})

set(CMAKE_CXX_STANDARD 14)
option(ENABLE_VENDOR_SRC "Enable vendor source" ON)
option(TEST_ENABLED "Enable unit test" OFF)
option(EXAMPLE_ENABLED "Build examples" OFF)

# find dependencies
find_package(ament_cmake REQUIRED)

add_subdirectory(src)
if (EXAMPLE_ENABLED)
    add_subdirectory(examples)
endif()

if (TEST_ENABLED)
    add_subdirectory(test)
endif()

ament_package()
