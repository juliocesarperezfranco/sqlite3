CONFIGURE_FILE(config.h.in "${CMAKE_CURRENT_BINARY_DIR}/config.h" @ONLY)

# This block defines the varaible holding the header files.
SET(SQLite3Headers
    sqlite3.h
    sqlite3ext.h
    )

# This block defines the variable holding the source files.
SET(SQLite3Sources
    sqlite3.c
    )

# Setting the target.
# In this line we create a target called "SQLite3" which can be STATIC (generating a .lib file) or SHARED (generating a .dll or .a file) depending on the switch option provided in the main CMakeLists.txt file.
# This target also includes all the source files on the variables SQLite3Sources and SQLite3Headers.
ADD_LIBRARY(SQLITE3 ${LIB_TYPE} ${SQLite3Sources} ${SQLite3Headers} config.h)

# Naming the output files depending of the type of target.
SET_PROPERTY(TARGET SQLITE3 PROPERTY RUNTIME_OUTPUT_NAME_DEBUG   "SQLITE3d") ## This line sets the name of the debug version of the shared library (.dll or .a) with the name SQLITE3d.dll or SQLITE3.a
SET_PROPERTY(TARGET SQLITE3 PROPERTY RUNTIME_OUTPUT_NAME_RELEASE "SQLITE3")  ## This line sets the name of the release version of the shared library (.dll or .a) with the name SQLITE3.dll or sqlite.a
SET_PROPERTY(TARGET SQLITE3 PROPERTY LIBRARY_OUTPUT_NAME_DEBUG   "SQLITE3d")
SET_PROPERTY(TARGET SQLITE3 PROPERTY LIBRARY_OUTPUT_NAME_RELEASE "SQLITE3")
SET_PROPERTY(TARGET SQLITE3 PROPERTY ARCHIVE_OUTPUT_NAME_DEBUG   "SQLITE3d") ## This line sets the name of the release version of the static library (.lib) with the name SQLITE3d.dll or sqlited.a
SET_PROPERTY(TARGET SQLITE3 PROPERTY ARCHIVE_OUTPUT_NAME_RELEASE "SQLITE3")  ## This line sets the name of the release version of the static library (.lib) with the name SQLITE3d.dll or sqlited.a

SET_TARGET_PROPERTIES(SQLITE3 PROPERTIES LINKER_LANGUAGE C)

SET_TARGET_PROPERTIES(SQLITE3 PROPERTIES PUBLIC_HEADER "sqlite3.h;sqlite3ext.h;${CMAKE_CURRENT_BINARY_DIR}/config.h")

INSTALL(TARGETS SQLITE3 
        EXPORT SQLite3Targets 
        RUNTIME DESTINATION "${INSTALL_BIN_DIR}"                   COMPONENT bin
        LIBRARY DESTINATION "${INSTALL_BIN_DIR}"                   COMPONENT bin
        ARCHIVE DESTINATION "${INSTALL_LIB_DIR}"                   COMPONENT lib
        PUBLIC_HEADER DESTINATION "${INSTALL_INCLUDE_DIR}/sqlite3" COMPONENT dev)
