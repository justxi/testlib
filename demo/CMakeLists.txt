add_executable(demo main.c)

target_link_libraries(demo LINK_PUBLIC testlib)

INSTALL(TARGETS demo DESTINATION ${CMAKE_INSTALL_BINDIR})
