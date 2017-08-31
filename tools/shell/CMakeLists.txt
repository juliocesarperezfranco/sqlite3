# CMake project for the SQLite3 Shell program.
PROJECT(Shell C)

SET(ShellSourceFiles
    src/shell.c)

INCLUDE_DIRECTORIES(
  ../../src
)

ADD_EXECUTABLE(Shell ${ShellSourceFiles})
TARGET_LINK_LIBRARIES(Shell SQLite3)

SET_PROPERTY(TARGET Shell PROPERTY RUNTIME_OUTPUT_NAME_DEBUG "shelld")   ## This line sets the name of the debug version of the program.
SET_PROPERTY(TARGET Shell PROPERTY RUNTIME_OUTPUT_NAME_RELEASE "shell")  ## This line sets the name of the release version of the program.

INSTALL(TARGETS Shell
        RUNTIME DESTINATION bin
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib)