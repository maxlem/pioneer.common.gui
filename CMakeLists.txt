cmake_minimum_required (VERSION 3.1)
project(PyBVH)
find_package (Eigen3 3.3 REQUIRED NO_MODULE)
find_package (pybind11 2.4 REQUIRED NO_MODULE)

if(WIN32)
    find_package (TBB REQUIRED NO_MODULE)
    message (STATUS "TBB: ${TBB_FOUND}")
endif(WIN32)

pybind11_add_module(PyBVH src/main.cpp)
if(WIN32)
    target_link_libraries (PyBVH PRIVATE Eigen3::Eigen TBB::tbb)
    target_link_libraries(PyBVH PRIVATE wsock32 ws2_32)
elseif(UNIX OR APPLE)
    target_link_libraries (PyBVH PRIVATE Eigen3::Eigen tbb)
    target_compile_options(PyBVH PRIVATE -std=c++1y -O3 -Wall -fmessage-length=0 -fPIC)
endif()
