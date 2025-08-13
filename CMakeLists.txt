cmake_minimum_required(VERSION 3.15)
project(FusionProject LANGUAGES C)

# --- Fusion library sources ---
set(FUSION_SOURCES
    Fusion/FusionAhrs.c
    Fusion/FusionCompass.c
    Fusion/FusionOffset.c
)

# --- Fusion library headers ---
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

# --- Create the static library ---
add_library(Fusion STATIC ${FUSION_SOURCES} ${FUSION_HEADERS})

# --- Include directories for building and installing ---
target_include_directories(Fusion PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/Fusion>
    $<INSTALL_INTERFACE:include/Fusion>
)

# --- Install the library ---
install(TARGETS Fusion
    EXPORT FusionTargets
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION bin
    INCLUDES DESTINATION include/Fusion
)

# --- Install headers preserving Fusion/ prefix ---
install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/Fusion/
    DESTINATION include/Fusion
    FILES_MATCHING PATTERN "*.h"
)

# --- Export CMake targets ---
install(EXPORT FusionTargets
    FILE FusionTargets.cmake
    NAMESPACE Fusion::
    DESTINATION lib/cmake/Fusion
)

# --- Create FusionConfig.cmake and version file ---
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

# --- Install CMake config files ---
install(FILES
    "${CMAKE_CURRENT_BINARY_DIR}/FusionConfig.cmake"
    "${CMAKE_CURRENT_BINARY_DIR}/FusionConfigVersion.cmake"
    DESTINATION lib/cmake/Fusion
)

