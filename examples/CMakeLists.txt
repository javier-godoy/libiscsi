#
# Example programs. Matches noinst_PROGRAMS in examples/Makefile.am: they
# are built but never installed.
#
set(ISCSI_EXAMPLES
    iscsiclient
    iscsi-dd
    iscsi-pthreads-inq
    iscsi-pthreads-readloop
    iscsi-pthreads-readloop-async)

foreach(_example IN LISTS ISCSI_EXAMPLES)
    add_executable(${_example} ${_example}.c)
    libiscsi_apply_build_settings(${_example})
    target_link_libraries(${_example} PRIVATE iscsi ${LIBISCSI_PLATFORM_LIBS})
    if(CMAKE_THREAD_LIBS_INIT)
        target_link_libraries(${_example} PRIVATE Threads::Threads)
    endif()
endforeach()
