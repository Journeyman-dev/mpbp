# SPDX-FileCopyrightText: 2022 Daniel Valcour <fosssweeper@gmail.com>
#
# SPDX-License-Identifier: MIT

find_package(Doxygen)

if (NOT DOXYGEN_FOUND)
    message(SEND_ERROR "Unable to generate mpbp documentation: Doxygen not installed.")
else()
    set(MPBP_DOCUMENTATION_SOURCES
        "main.dox"
    )
    list(
        TRANSFORM MPBP_DOCUMENTATION_SOURCES
        PREPEND "${CMAKE_CURRENT_SOURCE_DIR}/"
    )
    set(MPBP_DOCUMENTATION_SOURCES
        ${MPBP_DOCUMENTATION_SOURCES}
        ${MPBP_INCLUDE_FILES}
    )
    foreach(PATH IN LISTS MPBP_DOCUMENTATION_SOURCES)
        set(MPBP_DOXYGEN_INPUT "${MPBP_DOXYGEN_INPUT} \"${PATH}\"")
    endforeach()
    set(MPBP_DOXYFILE_IN "${CMAKE_CURRENT_SOURCE_DIR}/Doxyfile.in")
    set(MPBP_DOXYFILE_OUT "${CMAKE_CURRENT_BINARY_DIR}/Doxyfile.out")
    configure_file("${MPBP_DOXYFILE_IN}" "${MPBP_DOXYFILE_OUT}" @ONLY)
    add_custom_target(mpbp_documentation ALL 
        COMMAND ${DOXYGEN_EXECUTABLE} ${MPBP_DOXYFILE_OUT}
        WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
        COMMENT "Generating mpbp documentation using Doxygen."
        VERBATIM)
endif()
