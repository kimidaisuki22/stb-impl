cmake_minimum_required(VERSION 3.21.0)
project(stb-impl VERSION 0.0.1)

add_library(stb-impl STATIC)

file(GLOB SRC src/*.cpp)

target_sources(stb-impl  PRIVATE
    ${SRC}
)

find_package(Stb REQUIRED)
target_include_directories(stb-impl PUBLIC
    $<BUILD_INTERFACE:${Stb_INCLUDE_DIR}>
    $<INSTALL_INTERFACE:include>
)

include(CPack)
include(GNUInstallDirs)
install(TARGETS stb-impl
    EXPORT stb-impl
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION  ${CMAKE_INSTALL_BINDIR}
)

install(EXPORT stb-impl
    FILE stb-implTargets.cmake
    NAMESPACE  stb-impl::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/stb-impl
)


include(CMakeFindDependencyMacro)
find_dependency(Stb)


configure_file(${CMAKE_CURRENT_SOURCE_DIR}/cmake/stb-implConfig.cmake.in
               ${CMAKE_CURRENT_BINARY_DIR}/stb-implConfig.cmake
               @ONLY)
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/stb-implConfig.cmake
        DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/stb-impl)
