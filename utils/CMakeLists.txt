#
# Command line utilities. Matches bin_PROGRAMS in utils/Makefile.am.
#
set(ISCSI_UTILS
    iscsi-inq
    iscsi-ls
    iscsi-swp
    iscsi-pr
    iscsi-discard
    iscsi-md5sum
    iscsi-rtpg)

if(NOT WIN32)
    list(APPEND ISCSI_UTILS iscsi-perf iscsi-readcapacity16)
endif()

foreach(_util IN LISTS ISCSI_UTILS)
    add_executable(${_util} ${_util}.c)
    libiscsi_apply_build_settings(${_util})
    target_link_libraries(${_util} PRIVATE iscsi ${LIBISCSI_PLATFORM_LIBS})
    install(TARGETS ${_util} RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}")
endforeach()
