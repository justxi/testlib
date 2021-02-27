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
set(INSTALL_CONFIGDIR ${CMAKE_INSTALL_LIBDIR}/cmake/testlib)

include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    ${CMAKE_CURRENT_BINARY_DIR}/testlibConfigVersion.cmake
    VERSION ${LIB_VERSION}
    COMPATIBILITY AnyNewerVersion
)

configure_package_config_file(
    ${CMAKE_CURRENT_LIST_DIR}/cmake/testlibConfig.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/testlibConfig.cmake
    INSTALL_DESTINATION ${INSTALL_CONFIGDIR}
)

install(
	FILES
		${CMAKE_CURRENT_BINARY_DIR}/testlibConfig.cmake
		${CMAKE_CURRENT_BINARY_DIR}/testlibConfigVersion.cmake
	DESTINATION ${INSTALL_CONFIGDIR}
)

install(EXPORT testlibTarget
	FILE testlibTargets.cmake
	NAMESPACE testlib::
	DESTINATION ${INSTALL_CONFIGDIR}
)

add_library(testlib::testlib ALIAS testlib)
