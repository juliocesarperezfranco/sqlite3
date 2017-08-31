# Script for the SQLite3 library project.

# This block defines de varaible holding the header files.
SET(SQLite3Headers
    sqlite3.h
    sqlite3ext.h
    )

# This block defines de varaible holding the source files.
SET(SQLite3Sources
    sqlite3.c
    )

# Setting the target.
# In this line we create a target called "SQLite3" which can be STATIC (generating a .lib file) or SHARED (generating a .dll or .a file) depending on the switch option provided in the main CMakeLists.txt file.
# This target also includes all the source files on the variables SQLite3Sources and SQLite3Headers.
ADD_LIBRARY(SQLite3 ${LIB_TYPE} ${SQLite3Sources} ${SQLite3Headers})

# Naming the output files depending of the type of target.
SET_PROPERTY(TARGET SQLite3 PROPERTY RUNTIME_OUTPUT_NAME_DEBUG   "sqlite3d") ## This line sets the name of the debug version of the shared library (.dll or .a) with the name sqlite3d.dll or sqlite3.a
SET_PROPERTY(TARGET SQLite3 PROPERTY RUNTIME_OUTPUT_NAME_RELEASE "sqlite3")  ## This line sets the name of the release version of the shared library (.dll or .a) with the name sqlite3.dll or sqlite.a
SET_PROPERTY(TARGET SQLite3 PROPERTY LIBRARY_OUTPUT_NAME_DEBUG   "sqlite3d")
SET_PROPERTY(TARGET SQLite3 PROPERTY LIBRARY_OUTPUT_NAME_RELEASE "sqlite3")
SET_PROPERTY(TARGET SQLite3 PROPERTY ARCHIVE_OUTPUT_NAME_DEBUG   "sqlite3d") ## This line sets the name of the release version of the static library (.lib) with the name sqlite3d.dll or sqlited.a
SET_PROPERTY(TARGET SQLite3 PROPERTY ARCHIVE_OUTPUT_NAME_RELEASE "sqlite3")  ## This line sets the name of the release version of the static library (.lib) with the name sqlite3d.dll or sqlited.a


SET(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)

INSTALL(TARGETS SQLite3
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib)

INSTALL(FILES sqlite3.h  DESTINATION include/sqlite3)
