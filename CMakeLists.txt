cmake_minimum_required(VERSION 3.25)
project(lmdb VERSION 1.0
        DESCRIPTION "Lightning Memory-Mapped Database"
        LANGUAGES C)

include(CTest)

set(CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}" ${CMAKE_MODULE_PATH})

set(CMAKE_EXPORT_COMPILE_COMMANDS TRUE)
set(CMAKE_POSITION_INDEPENDENT_CODE TRUE)
set(CMAKE_INTERPROCEDURAL_OPTIMIZATION TRUE)
set(CMAKE_VERBOSE_MAKEFILE ON)
set(CMAKE_USE_FOLDERS TRUE)

set(CMAKE_C_STANDARD 23)
set(CMAKE_C_STANDARD_REQUIRED TRUE)
set(CMAKE_C_KNOWN_FEATURES c_std_23)
set(CMAKE_C_EXTENSIONS TRUE)

set(CMAKE_BUILD_TYPE Release)

add_compile_options(
  -W
  -Wall
  -Wno-unused-parameter
  -Wbad-function-cast
  -Wuninitialized
  $<$<CONFIG:RELEASE>:-Ofast>
  $<$<CONFIG:RELEASE>:-march=native>
  $<$<CONFIG:RELEASE>:-ffast-math>
  $<$<CONFIG:RELEASE>:-fverbose-asm>
  $<$<CONFIG:RELEASE>:-ftree-vectorize>
  $<$<CONFIG:RELEASE>:-pthread>

  $<$<CONFIG:DEBUG>:-Ofast>
  $<$<CONFIG:DEBUG>:-march=native>
  $<$<CONFIG:DEBUG>:-ffast-math>
  $<$<CONFIG:DEBUG>:-fverbose-asm>
  $<$<CONFIG:DEBUG>:-ftree-vectorize>
  $<$<CONFIG:DEBUG>:-ggdb3>
  $<$<CONFIG:DEBUG>:-pthread>
)

include_directories("${PROJECT_SOURCE_DIR}")

add_library(lmdb INTERFACE)
target_sources(lmdb INTERFACE "lmdb.h" "mdb.c" "midl.c")
target_compile_definitions(lmdb INTERFACE MDB_USE_POSIX_MUTEX=1)
target_compile_definitions(lmdb INTERFACE MDB_FDATASYNC_WORKS=1)
target_compile_definitions(lmdb INTERFACE MDB_USE_PWRITEV=1)
target_compile_definitions(lmdb INTERFACE MDB_USE_ROBUST=1)
target_compile_definitions(lmdb INTERFACE MDB_MAXKEYSIZE=2047)
target_link_libraries(lmdb INTERFACE c rt)

# add_executable(mdb_stat "mdb_stat.c")
# add_executable(mdb_copy "mdb_copy.c")
# add_executable(mdb_dump "mdb_dump.c")
# add_executable(mdb_load "mdb_load.c")
# add_executable(mdb_drop "mdb_drop.c")

# target_link_libraries(mdb_stat PRIVATE lmdb)
# target_link_libraries(mdb_copy PRIVATE lmdb)
# target_link_libraries(mdb_dump PRIVATE lmdb)
# target_link_libraries(mdb_load PRIVATE lmdb)
# target_link_libraries(mdb_drop PRIVATE lmdb)

# enable_testing()
# add_subdirectory(tests)
