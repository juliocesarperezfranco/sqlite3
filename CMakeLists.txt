# Main CMake script for the project SQLite3.
CMAKE_MINIMUM_REQUIRED(VERSION 3.0)

PROJECT(SQLite3 C)

# This option lets the programmer to use shared or static libraries.
OPTION (BUILD_SHARED_LIBS "Build Shared Libraries" OFF)

# This block sets the LIB_TYPE variable, by default is set to 'STATIC'.
SET(LIB_TYPE STATIC)
IF (BUILD_SHARED_LIBS) # If the option is ON then it will set the variable to 'SHARED'
  SET (LIB_TYPE SHARED)
ENDIF (BUILD_SHARED_LIBS)

# This line loads the CMakeLists.txt inside the src subdirectory.
ADD_SUBDIRECTORY(src)