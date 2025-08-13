cmake_minimum_required(VERSION 3.15)
project(FusionProject LANGUAGES C)

# Set the Fusion library source files
set(FUSION_SOURCES
    Fusion/FusionAhrs.c
    Fusion/FusionCompass.c
    Fusion/FusionOffset.c
)

# Set the Fusion library headers
set(FUSION_HEADERS
    Fusion/FusionAhrs.h
    Fusion/FusionAxes.h
    Fusion/FusionCalibration.h
    Fusion/FusionCompass.h
    Fusion/FusionConvention.h
    Fusion/Fusion.h
    Fusion/FusionMath.h
    Fusion/FusionOffset.h
)

# Create the library
add_library(Fusion STATIC ${FUSION_SOURCES} ${FUSION_HEADERS})

# Specify include directories
target_include_directories(Fusion PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/Fusion>
    $<INSTALL_INTERFACE:include>
)

# Install the library
install(TARGETS Fusion
    EXPORT FusionTargets
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION bin
    INCLUDES DESTINATION include
)

# Install headers
install(FILES ${FUSION_HEADERS} DESTINATION include)

# Export CMake config
install(EXPORT FusionTargets
    FILE FusionTargets.cmake
    NAMESPACE Fusion::
    DESTINATION lib/cmake/Fusion
)

# Create a FusionConfig.cmake file for find_package
include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    "${CMAKE_CURRENT_BINARY_DIR}/FusionConfigVersion.cmake"
    VERSION 1.0.0
    COMPATIBILITY AnyNewerVersion
)

configure_package_config_file(
    "${CMAKE_CURRENT_LIST_DIR}/FusionConfig.cmake.in"
    "${CMAKE_CURRENT_BINARY_DIR}/FusionConfig.cmake"
    INSTALL_DESTINATION lib/cmake/Fusion
)

install(FILES
    "${CMAKE_CURRENT_BINARY_DIR}/FusionConfig.cmake"
    "${CMAKE_CURRENT_BINARY_DIR}/FusionConfigVersion.cmake"
    DESTINATION lib/cmake/Fusion
)
