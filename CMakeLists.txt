# Copyright (c) 2018 Steven Hoving
#
# Permission is hereby granted, free of charge, to any person obtaining a copy
# of this software and associated documentation files (the "Software"), to deal
# in the Software without restriction, including without limitation the rights
# to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
# copies of the Software, and to permit persons to whom the Software is
# furnished to do so, subject to the following conditions:
#
# The above copyright notice and this permission notice shall be included in all
# copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
# AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
# OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
# SOFTWARE.

cmake_minimum_required(VERSION 3.10)

project(libcamcapture)

if(NOT DEFINED MSVC OR MSVC_VERSION LESS 1910)
  message(FATAL_ERROR "libcamcapture currently only builds with visual studio 2017 for now.")
endif()

set(CMAKE_CXX_STANDARD 17)

add_compile_options(/WX) # treat warnings as errors.
add_compile_options(/permissive-) # vs conformance mode.
add_compile_options(/Zc:twoPhase-) # 2 phase name lookup.

###############################################################################

set_property(GLOBAL PROPERTY USE_FOLDERS ON)

###############################################################################

set(LIB_CAM_CAPTURE_SOURCE
    include/libcamcapture/libcamcapture.h
    include/libcamcapture/icapture_source.h
    include/libcamcapture/gdi_capture_source.h
    include/libcamcapture/capture_sample.h
    include/libcamcapture/capture_sample_fwd.h
    src/gdi_capture_source.cpp
)

source_group(libcamcapture FILES
    ${LIB_CAM_CAPTURE_SOURCE}
)

add_library(libcamcapture STATIC
    ${LIB_CAM_CAPTURE_SOURCE}
)

target_include_directories(libcamcapture
  PUBLIC
    include
)

install(
    DIRECTORY include
    DESTINATION include
)

add_subdirectory(examples)
