add_library(testlib SHARED testlib.c)
set_target_properties(testlib PROPERTIES PUBLIC_HEADER testlib.h)

# Set version of the shared library
set_target_properties(testlib PROPERTIES VERSION ${LIB_VERSION_MAJOR}.${LIB_VERSION_MINOR}.${LIB_VERSION_TWEAK})
# Create symlink "libtestlib.${LIB_VERSION_MAJOR} ->"
set_target_properties(testlib PROPERTIES SOVERSION ${LIB_VERSION_MAJOR})

# If desired, set different output name
set_target_properties(testlib
    PROPERTIES
      OUTPUT_NAME "testlib"
)

# Let the executable "demo" find the header files
target_include_directories(testlib
	PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
	PUBLIC $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}/testlib>
)

# Use variables from "GNUInstallDirs" to let cmake where to install the library and header files
install(TARGETS testlib EXPORT testlibTarget
	LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
	PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/testlib
)

# Install files to use "find_package(testlib )"
configure_file(testlibConfig-Version.cmake.in ${CMAKE_CURRENT_BINARY_DIR}/testlibConfig-Version.cmake @ONLY)
install(FILES testlibConfig.cmake ${CMAKE_CURRENT_BINARY_DIR}/testlibConfig-Version.cmake DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/testlib)

set(INSTALL_CONFIGDIR ${CMAKE_INSTALL_LIBDIR}/cmake/testlib)

install(EXPORT testlibTarget
	FILE testlibTargets.cmake
	NAMESPACE testlib::
	DESTINATION ${INSTALL_CONFIGDIR}
)
