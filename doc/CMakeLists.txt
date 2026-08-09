#
# Manpages. Matches doc/Makefile.am: pre-built manpages are shipped in the
# tree and installed unconditionally; ENABLE_MANPAGES regenerates them from
# the docbook sources with xsltproc.
#
set(ISCSI_MANPAGES
    iscsi-inq.1
    iscsi-ls.1
    iscsi-swp.1
    iscsi-md5sum.1
    iscsi-test-cu.1)

set(DOCBOOK_MANPAGES_XSL
    "http://docbook.sourceforge.net/release/xsl/current/manpages/docbook.xsl"
    CACHE STRING "Stylesheet used to build the manpages")

if(ENABLE_MANPAGES)
    find_program(XSLTPROC_EXECUTABLE xsltproc)
    if(NOT XSLTPROC_EXECUTABLE)
        message(FATAL_ERROR
            "ENABLE_MANPAGES is on but xsltproc was not found.")
    endif()

    set(_generated_manpages "")
    foreach(_man IN LISTS ISCSI_MANPAGES)
        get_filename_component(_base "${_man}" NAME_WE)
        set(_out "${CMAKE_CURRENT_BINARY_DIR}/${_man}")
        add_custom_command(OUTPUT "${_out}"
            COMMAND "${XSLTPROC_EXECUTABLE}" -o "${_out}"
                    "${DOCBOOK_MANPAGES_XSL}"
                    "${CMAKE_CURRENT_SOURCE_DIR}/${_base}.xml"
            DEPENDS "${CMAKE_CURRENT_SOURCE_DIR}/${_base}.xml"
            COMMENT "Generating ${_man}"
            VERBATIM)
        list(APPEND _generated_manpages "${_out}")
    endforeach()

    add_custom_target(manpages ALL DEPENDS ${_generated_manpages})
    install(FILES ${_generated_manpages}
            DESTINATION "${CMAKE_INSTALL_MANDIR}/man1")
else()
    foreach(_man IN LISTS ISCSI_MANPAGES)
        install(FILES "${CMAKE_CURRENT_SOURCE_DIR}/${_man}"
                DESTINATION "${CMAKE_INSTALL_MANDIR}/man1")
    endforeach()
endif()
