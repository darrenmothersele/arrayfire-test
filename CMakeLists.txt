cmake_minimum_required(VERSION 3.5)
project(arrayfiretest)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

FIND_PACKAGE(ArrayFire REQUIRED)
INCLUDE_DIRECTORIES(${ArrayFire_INCLUDE_DIRS})

set(SOURCE_FILES main.cpp)
#add_executable(arrayfiretest ${SOURCE_FILES})
#TARGET_LINK_LIBRARIES(arrayfiretest ${ArrayFire_LIBRARIES} ${EXTRA_LIBS})

if(${ArrayFire_CPU_FOUND})
    MESSAGE(STATUS "ArrayFire CPU backend found. Enabling CPU benchmark")
    ADD_EXECUTABLE(foo-cpu ${SOURCE_FILES})
    TARGET_LINK_LIBRARIES(foo-cpu ${ArrayFire_CPU_LIBRARIES}
            ${CMAKE_THREAD_LIBS_INIT})
ENDIF()

FIND_PACKAGE(OpenCL)
IF(${ArrayFire_OpenCL_FOUND} AND ${OpenCL_FOUND})
    # We need to find OpenCL as transitive linking is disabled on some OSes
    MESSAGE(STATUS "ArrayFire OpenCL backend found. Enabling OpenCL benchmark")
    ADD_EXECUTABLE(arrayfiretest ${SOURCE_FILES})
    TARGET_LINK_LIBRARIES(arrayfiretest ${ArrayFire_OpenCL_LIBRARIES}
            ${OpenCL_LIBRARIES} ${CMAKE_THREAD_LIBS_INIT})
ENDIF()
