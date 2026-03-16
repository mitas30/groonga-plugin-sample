cmake_minimum_required(VERSION 3.14)

project(groonga-plugin-sample
  VERSION 1.0.0
  LANGUAGES C
)

# --- Build options ---
option(ENABLE_DEBUG "Build with debug flags (-O0 -g3)" OFF)

# --- Compiler flags (configure.ac の CFLAGS に相当) ---
if(CMAKE_C_COMPILER_ID MATCHES "GNU|Clang")
  add_compile_options(
    -Wall
    -Wextra
    -Wmissing-declarations
    -Wmissing-prototypes
  )
endif()

if(ENABLE_DEBUG)
  add_compile_options(-O0 -g3)
endif()

# --- Find Groonga via pkg-config ---
find_package(PkgConfig REQUIRED)
pkg_check_modules(GROONGA REQUIRED groonga>=8.0.0)

execute_process(
  COMMAND ${PKG_CONFIG_EXECUTABLE} --variable=pluginsdir groonga
  OUTPUT_VARIABLE GROONGA_PLUGINS_DIR
  OUTPUT_STRIP_TRAILING_WHITESPACE
)
execute_process(
  COMMAND ${PKG_CONFIG_EXECUTABLE} --variable=groonga groonga
  OUTPUT_VARIABLE GROONGA_BINARY
  OUTPUT_STRIP_TRAILING_WHITESPACE
)

if(NOT GROONGA_PLUGINS_DIR)
  message(FATAL_ERROR "groonga の pluginsdir を pkg-config から取得できませんでした")
endif()
if(NOT GROONGA_BINARY)
  message(FATAL_ERROR "groonga バイナリのパスを pkg-config から取得できませんでした")
endif()

message(STATUS "Groonga version:     ${GROONGA_VERSION}")
message(STATUS "Groonga plugins dir: ${GROONGA_PLUGINS_DIR}")
message(STATUS "Groonga binary:      ${GROONGA_BINARY}")

enable_testing()

add_subdirectory(functions)
add_subdirectory(test)
