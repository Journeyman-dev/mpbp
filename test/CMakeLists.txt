# SPDX-FileCopyrightText: 2022 Daniel Valcour <fosssweeper@gmail.com>
#
# SPDX-License-Identifier: MIT

cmake_minimum_required(VERSION 3.19)
Include(FetchContent)
FetchContent_Declare(
    Catch2
    GIT_REPOSITORY https://github.com/catchorg/Catch2.git
    GIT_TAG        v3.1.0
)
FetchContent_MakeAvailable(Catch2)
set(MPBP_TEST_SOURCES
    "space_test.cpp"
    "rect_test.cpp"
    "packer_test.cpp"
)
list(
    TRANSFORM MPBP_TEST_SOURCES
    PREPEND "${CMAKE_CURRENT_SOURCE_DIR}/src/"
)
source_group("sources"
    FILES ${MPBP_TEST_SOURCESs}
)
add_executable(mpbp_test_auto
    ${MPBP_TEST_SOURCES}
)
set_target_properties(mpbp_test_auto
    PROPERTIES
    OUTPUT_NAME "mpbp example"
    CXX_STANDARD ${MPBP_CXX_STANDARD}
    CXX_STANDARD_REQUIRED TRUE
)
target_link_libraries(mpbp_test_auto
    PRIVATE
        Catch2::Catch2WithMain
        ${PROJECT_NAME}
)
include(CTest)
include(Catch)
catch_discover_tests(
  mpbp_test_auto
  TEST_PREFIX "unittests."
  REPORTER junit
  OUTPUT_DIR ${CMAKE_BINARY_DIR}
  OUTPUT_PREFIX "unittests."
  OUTPUT_SUFFIX .xml
)
