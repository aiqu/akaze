project(AKAZECU)

cmake_minimum_required (VERSION 2.6)
if (COMMAND cmake_policy)
  cmake_policy(SET CMP0003 NEW)
endif ()

# ============================================================================ #
# Build Type. By default is release
set(CMAKE_BUILD_TYPE "Release" CACHE STRING "Choose the type of build, options are: Debug, Release" FORCE)

# ============================================================================ #
# Where to output binaries and libraries
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/lib")
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/lib")
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/bin")

# ============================================================================ #
# Where to install the AKAZE library
set(AKAZE_INSTALL_PREFIX "/usr/local/lib" CACHE PATH "Installation Directory")
set(AKAZE_INCLUDE_PREFIX "/usr/local/include" CACHE PATH "Includes Directory")

# ============================================================================ #
# Installation related variables
set(TARGETS_EXPORT_NAME CudaAKAZE)
set(namespace "CudaAKAZE::")
set(config_install_dir "share/${TARGETS_EXPORT_NAME}/cmake")
set(generated_dir "${CMAKE_CURRENT_BINARY_DIR}/generated")
set(project_config "${generated_dir}/${TARGETS_EXPORT_NAME}Config.cmake")
set(CUDAAKAZE_LIBRARIES
  ${namespace}AKAZE
  ${namespace}AKAZE_CUDA
)

# ============================================================================ #
# CPP sources
message(STATUS ">>> Adding src subdirectory")
add_subdirectory(src)

# ============================================================================ #
# Installation

include(CMakePackageConfigHelpers)
configure_package_config_file(
  "cmake/Config.cmake.in"
  "${project_config}"
  INSTALL_DESTINATION "${config_install_dir}"
  )

install(
  FILES "${project_config}"
  DESTINATION "${config_install_dir}"
  )

install(
  EXPORT ${TARGETS_EXPORT_NAME}
  NAMESPACE ${namespace}
  DESTINATION ${config_install_dir}
)
