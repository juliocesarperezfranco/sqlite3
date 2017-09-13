# Main CMake script for the project SQLite3.
CMAKE_MINIMUM_REQUIRED(VERSION 3.0)

PROJECT(SQLite3 C CXX)

SET(SQLITE3_MAJOR_VERSION 0)
SET(SQLITE3_MINOR_VERSION 1)
SET(SQLITE3_PATCH_VERSION 0)
SET(SQLITE3_VERSION
  ${SQLITE3_MAJOR_VERSION}.${SQLITE3_MINOR_VERSION}.${SQLITE3_PATCH_VERSION})

# This option lets the programmer to use shared or static libraries.
OPTION (BUILD_SHARED_LIBS "Build Shared Libraries" OFF)

# This block sets the LIB_TYPE variable, by default is set to 'STATIC'.
SET(LIB_TYPE STATIC)
IF (BUILD_SHARED_LIBS) # If the option is ON then it will set the variable to 'SHARED'
  SET (LIB_TYPE SHARED)
ENDIF (BUILD_SHARED_LIBS)

# This option lets the programmer to also build the projects inside the tools subdirectory.
OPTION(BUILD_TOOLS "SQLite3 tools of the project" ON)
IF(BUILD_TOOLS)
   SUBDIRS(Tools)
ENDIF(BUILD_TOOLS)

# Offer the user the choice of overriding the installation directories
SET(INSTALL_LIB_DIR lib         CACHE PATH "Installation directory for libraries")
SET(INSTALL_BIN_DIR bin         CACHE PATH "Installation directory for executables")
SET(INSTALL_INCLUDE_DIR include CACHE PATH "Installation directory for header files")
if(WIN32 AND NOT CYGWIN)
  SET(DEF_INSTALL_CMAKE_DIR CMake)
else()
  SET(DEF_INSTALL_CMAKE_DIR lib/CMake/SQLite3)
endif()
SET(INSTALL_CMAKE_DIR ${DEF_INSTALL_CMAKE_DIR} CACHE PATH "Installation directory for CMake files")


# Make relative paths absolute (needed later on)
FOREACH(p LIB BIN INCLUDE CMAKE)
  SET(var INSTALL_${p}_DIR)
  IF(NOT IS_ABSOLUTE "${${var}}")
    SET(${var} "${CMAKE_INSTALL_PREFIX}/${${var}}")
  ENDIF()
ENDFOREACH()

# set up include-directories
INCLUDE_DIRECTORIES(
  "${PROJECT_SOURCE_DIR}"   # to find sqlite3/sqlite3.h
  "${PROJECT_BINARY_DIR}")  # to find sqlite3/config.h

# This line loads the CMakeLists.txt inside the src subdirectory.
ADD_SUBDIRECTORY(src)

# The interesting stuff goes here
# ===============================

# Add all targets to the build-tree export set
EXPORT(TARGETS SQLITE3 FILE "${PROJECT_BINARY_DIR}/SQLite3Targets.cmake")

# Export the package for use from the build-tree
# (this registers the build-tree with a global CMake-registry)
EXPORT(PACKAGE SQLite3)

# Create the SQLite3Config.cmake and FooBarConfigVersion files
FILE(RELATIVE_PATH REL_INCLUDE_DIR "${INSTALL_CMAKE_DIR}" "${INSTALL_INCLUDE_DIR}")
# ... for the build tree
SET(CONF_INCLUDE_DIRS "${PROJECT_SOURCE_DIR}" "${PROJECT_BINARY_DIR}")
CONFIGURE_FILE(SQLite3Config.cmake.in "${PROJECT_BINARY_DIR}/SQLite3Config.cmake" @ONLY)
# ... for the install tree
SET(CONF_INCLUDE_DIRS "\${SQLITE3_CMAKE_DIR}/${REL_INCLUDE_DIR}")
CONFIGURE_FILE(SQLite3Config.cmake.in "${PROJECT_BINARY_DIR}${CMAKE_FILES_DIRECTORY}/SQLite3Config.cmake" @ONLY)
# ... for both
CONFIGURE_FILE(SQLite3ConfigVersion.cmake.in "${PROJECT_BINARY_DIR}/SQLite3ConfigVersion.cmake" @ONLY)

# Install the FooBarConfig.cmake and FooBarConfigVersion.cmake
INSTALL(FILES
  "${PROJECT_BINARY_DIR}${CMAKE_FILES_DIRECTORY}/SQLite3Config.cmake"
  "${PROJECT_BINARY_DIR}/SQLite3ConfigVersion.cmake"
  DESTINATION "${INSTALL_CMAKE_DIR}" COMPONENT dev)

# Install the export set for use with the install-tree
INSTALL(EXPORT SQLite3Targets DESTINATION "${INSTALL_CMAKE_DIR}" COMPONENT dev)
